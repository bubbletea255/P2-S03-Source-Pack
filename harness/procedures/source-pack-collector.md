# Source Pack Collector Procedure

## 목적

한 회사의 SEC 공시, IR 자료, 실적 발표 자료, 가능한 transcript 원문을 수집해 `raw/`에 저장하고, `catalog/` 원장과 회사별 `index.md`를 갱신한다.

Source Pack Collector는 링크 북마크를 만들지 않는다.
Collector의 핵심 임무는 다음 하네스가 외부 사이트를 반복 방문하지 않아도 되도록 원자료 파일, catalog record, 실행 기록을 남기는 것이다.

금지:

- 원자료 내용 해석
- 투자 thesis 작성
- valuation 의견
- 매수/매도/보유 판단
- transcript 번역, 요약, Q&A 구조화, 투자 관점 분석
- Cloudflare, bot 차단, 로그인, 유료벽 우회

## 참조 파일

Collector는 아래 공통 원장을 따른다.

- `harness/contracts/source-pack.contract.md`
- `harness/schemas/source-pack-catalog.schema.md`
- `harness/schemas/source-pack-index.schema.md`
- `harness/rubrics/source-pack-qa.rubric.md`

## 0단계: 모드 결정과 사전 점검

1. 대상 ticker를 대문자로 정규화한다.
2. `config.md`에서 수집 범위, 속도 제한, SEC User-Agent를 확인한다.
3. SEC User-Agent가 비어 있으면 실행을 중단하고 사람 확인을 요청한다.
4. 기존 산출물 존재 여부를 확인한다.

새 운영 경로:

```text
artifacts/companies/{TICKER}/index.md
artifacts/catalog/entities.jsonl
artifacts/catalog/documents.jsonl
artifacts/catalog/files.jsonl
artifacts/catalog/runs.jsonl
```

모드:

| 조건 | 모드 |
|---|---|
| 회사별 index와 catalog record가 없음 | `new_collection` |
| 기존 catalog가 있고 새 자료만 확인 | `incremental_update` |
| 특정 자료 또는 실패 항목만 다시 확인 | `partial_recheck` |
| Claude/Codex 비교 실행 | `comparison` |

주의:

- 이전 구조 `artifacts/{TICKER}/phase2/step3-source-pack/index.md`에는 쓰지 않는다.
- 이전 링크-only artifacts는 운영 catalog로 억지 이전하지 않는다.
- 기존 산출물 삭제, archive, 대규모 덮어쓰기는 사람 승인 후에만 한다.
- `companies/{TICKER}/sources.jsonl`은 만들지 않는다.

## 1단계: run 초기화

1. `run_id`를 만든다.

권장 형식:

```text
run-{YYYYMMDD}-{ticker-lower}
```

같은 날짜와 ticker의 run이 이미 있으면 `-02`, `-03`처럼 suffix를 붙인다.

2. run 폴더를 만든다.

```text
artifacts/runs/{run-id}/
```

3. run 폴더에 아래 파일을 초기화한다.

```text
artifacts/runs/{run-id}/download-log.jsonl
artifacts/runs/{run-id}/run-summary.md
artifacts/runs/{run-id}/qa.md
```

4. `download-log.jsonl`은 run 중 모든 다운로드 시도와 transcript 탐색 제한 판단을 기록한다.
5. `catalog/runs.jsonl`은 run 종료 시 최종 상태로 갱신한다. 실행 중 조기 중단이 발생하면 `status: stopped` 또는 `failed`로 기록한다.
6. `run-summary.md`에는 대상 ticker, run mode, 시작 시각, config 요약, 사람 승인 필요 항목을 남긴다.

## 2단계: CIK 조회와 entity record 갱신

CIK 조회 우선순위:

1. `https://www.sec.gov/files/company_tickers_exchange.json`
2. `https://www.sec.gov/files/company_tickers.json`
3. `https://efts.sec.gov/LATEST/search-index?q="{TICKER}"`
4. `https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK={TICKER}`

규칙:

- CIK는 10자리로 패딩한다. 예: `320193` -> `0000320193`.
- `entity_id`는 `sec-cik-{10자리 CIK}` 형식으로 만든다.
- 조회에 사용한 출처와 fallback 사용 여부를 `run-summary.md`와 회사별 `index.md`의 확인 필요 영역에 남긴다.

CIK 확보 후 아래 API를 호출한다.

```text
GET https://data.sec.gov/submissions/CIK{10자리CIK}.json
```

추출 항목:

- `name`
- `tickers`
- `exchanges`
- `sic`
- `sicDescription`
- `fiscalYearEnd`
- 최근/과거 filings

`artifacts/catalog/entities.jsonl`에 upsert한다.

필수:

- 같은 `entity_id` record가 있으면 갱신한다.
- 같은 ticker가 다른 CIK로 발견되면 `[확인 필요: ticker-CIK 불일치]`를 남긴다.
- CIK 조회 실패 시 run을 실패 또는 중단 상태로 기록하고, 가능한 범위의 `run-summary.md`, `qa.md`, 회사별 `index.md`를 남긴다.

## 3단계: SEC 문서 후보 탐색과 `documents.jsonl` pending 기록

`filings.recent`와 `filings.files`를 함께 확인한다.

수집 대상:

| 계층 | 자료 | 기본 범위 |
|---|---|---|
| Tier 1 | 10-K | 최대 10년 |
| Tier 1 | 10-Q | 최대 12분기 |
| Tier 1 | DEF 14A / Proxy | 최대 5년 |
| Tier 1 | 8-K Item 2.02 | 최대 12분기 |
| Tier 2 | 주요 8-K 이벤트 | `config.md`의 Item/키워드 필터 |

각 후보 문서마다 `documents.jsonl` record를 만든다.

SEC `document_id` 형식:

```text
sec:{cik}:{accession_no}:{document_type_slug}
```

SEC filing folder URL 형식:

```text
https://www.sec.gov/Archives/edgar/data/{CIK숫자}/{accessionNumber하이픈제거}/
```

SEC primary document URL 형식:

```text
https://www.sec.gov/Archives/edgar/data/{CIK숫자}/{accessionNumber하이픈제거}/{primary_doc}
```

`documents.jsonl.source_url`에는 가능한 경우 primary document URL을 기록한다.
filing folder URL은 exhibit 탐색, metadata, raw folder 구성에 사용하되 primary document URL과 혼동하지 않는다.

초기 record 규칙:

- `collection_status`: `pending`
- `primary_file_id`: `null`
- `raw_root`: 예상 raw folder
- `text_root`: text 추출이 예정되어 있으면 예상 text folder, 아니면 `null`
- `last_checked_run_id`: 현재 `run_id`
- `source_type`: `sec-edgar`
- `document_type`: schema 허용값만 사용한다.
- `form_type`, `accession_no`, `items`, `primary_doc`를 가능한 범위에서 채운다.
- `primary_doc`가 확인되면 filing folder URL과 결합해 `source_url`을 primary document URL로 갱신한다.

주의:

- `documents.jsonl`은 문서 단위 원장이다.
- SEC exhibit 파일은 보통 같은 문서의 `files.jsonl` record로 처리한다.
- 동일 `document_id`가 이미 있으면 중복 추가하지 않고 갱신한다.

## 4단계: SEC raw 다운로드와 `files.jsonl` 승격

각 SEC 후보 문서에 대해 아래 raw 경로를 사용한다.

```text
artifacts/raw/sec-edgar/cik-{CIK}/accession-{ACCESSION}/
```

권장 파일:

| 파일 | 역할 |
|---|---|
| `metadata.json` | filing metadata |
| `primary.html` | primary document |
| `primary.txt` | SEC 원문 txt가 있는 경우 |
| `exhibits/{sequence}_{filename}` | exhibit 파일 |

다운로드 루프:

1. 목표 `document_id`, `source_url`, `target_path`를 정한다.
2. `download-log.jsonl`에 attempt 시작 정보를 남긴다.
3. SEC User-Agent와 속도 제한을 지켜 요청한다.
4. 성공하면 파일을 `target_path`에 저장한다.
5. 저장 후 파일 존재, `size_bytes > 0`, SHA-256 hash를 확인한다.
6. 가능하면 `content_type`을 서버 header 또는 파일 검사로 기록한다.
7. `file_id`는 `sha256:{hash}` 형식으로 만든다.
8. 검증된 파일만 `catalog/files.jsonl`에 승격한다.
9. 대표 파일은 `file_role: primary`로 기록하고, 해당 `file_id`를 `documents.jsonl.primary_file_id`에 연결한다.
10. exhibit 파일은 `file_role: exhibit`으로 기록한다.
11. 실패하면 `files.jsonl`에 올리지 않고 `download-log.jsonl`, `documents.jsonl.notes`, `qa.md`에 실패 사유를 남긴다.

`files.jsonl` 승격 조건:

- 로컬 파일이 실제 존재한다.
- 파일 크기가 0보다 크다.
- SHA-256 hash가 계산됐다.
- `document_id`가 `documents.jsonl`에 존재한다.
- `entity_id`가 `entities.jsonl`에 존재한다.
- `file_role`, `file_format`, `file_status`가 schema 허용값이다.

다운로드 성공 후 `documents.jsonl` 갱신:

- 대표 파일이 있으면 `collection_status: collected`
- 대표 파일이 없고 실패가 확정되면 `collection_status: failed`
- 범위 밖 또는 설정상 제외면 `collection_status: skipped`
- `primary_file_id`, `raw_root`, `last_checked_run_id`, `notes`를 갱신한다.

## 5단계: 실적 발표 자료 처리

실적 발표 자료의 주 기준은 `8-K Item 2.02`다.

`EX-99.1`은 실적 발표 보도자료, shareholder letter, presentation이 첨부되었는지 확인하는 보조 기준이다.

처리 규칙:

- `8-K Item 2.02` filing은 `document_type: 8-K` 문서로 기록한다.
- `items`에는 `2.02`, `9.01` 등 SEC item을 배열로 기록한다.
- primary document와 관련 exhibit을 raw로 저장한다.
- EX-99.1이 있으면 `files.jsonl`에 `file_role: exhibit`으로 기록한다.
- EX-99.1이 없거나 일부 exhibit이 누락되면 `[확인 필요: exhibit 누락 여부]`를 남긴다.
- Item 2.02와 EX-99.1을 같은 기준으로 혼동하지 않는다.

## 6단계: IR 자료 탐색과 다운로드

IR 사이트는 아래 경로로 찾는다.

1. SEC submissions 응답의 website 또는 company metadata
2. 기존 `entities.jsonl.ir_site`
3. 회사 홈페이지의 investor relations 링크
4. 검색 후보

탐색 대상:

- investor presentation
- shareholder letter
- annual report
- investor day
- earnings presentation

IR 문서 `document_id` 형식:

```text
ir:{ticker}:{date}:{slug}
```

raw 경로:

```text
artifacts/raw/company-ir/{TICKER}/{YYYY-MM-DD}_{slug}/
```

처리 규칙:

- 발견한 IR 문서는 `documents.jsonl`에 `source_type: company-ir`로 기록한다.
- IR presentation은 기본적으로 `document_type: ir-deck`을 사용한다.
- IR presentation 파일은 `files.jsonl`에 기본적으로 `file_role: ir_deck`으로 기록한다.
- shareholder letter, annual report, 기타 IR 문서는 기본적으로 `file_role: primary`로 기록한다.
- 제목이나 날짜가 불확실하면 `notes`에 `[확인 필요: {이유}]`를 남긴다.
- 다운로드 가능한 PDF/HTML만 raw 저장을 시도한다.
- 자동 탐색 실패는 조용히 생략하지 않고 `download-log.jsonl`, `run-summary.md`, 회사별 `index.md`의 확인 필요 목록에 남긴다.

## 7단계: Transcript 원문 수집

Transcript는 optional source다. 실패해도 Source Pack 전체 실패로 보지 않는다.

Transcript에서 허용되는 작업:

- transcript 원문 후보 발견
- transcript 원문 파일 다운로드
- transcript metadata, source_url, raw path, text path catalog 등록

금지되는 작업:

- transcript 번역
- transcript 요약
- Q&A 주제 구조화
- 투자 관점 해석
- 경영진 발언 평가

수집 전 냉각 기간 확인:

1. 이전 `artifacts/runs/*/download-log.jsonl`을 읽는다.
2. 동일 `ticker`, `quarter`, `source_name` 조합을 찾는다.
3. `quarter`는 `YYYYQ{n}` 형식만 사용한다. 예: `2026Q1`.
4. 90일 이내 `blocked`, `paywalled`, `not_found`, `skipped_budget` 기록이 있으면 자동 재시도하지 않는다.
5. 이 경우 현재 run의 `download-log.jsonl`에 `attempt_status: skipped_budget` 또는 `skipped`로 기록한다.
6. 사용자가 명시적으로 재시도를 요청한 경우에만 냉각 기간을 무시한다.

탐색 제한:

- 티커당 transcript 탐색에 사용할 `source_name` 또는 `source_type` 종류는 최대 3개다.
- 각 분기에서는 후보 출처 중 최대 2개 출처만 시도한다.
- 출처당 자동 접근은 최대 1회다.
- Cloudflare, bot 차단, 로그인 요구, 유료벽이 감지되면 우회하지 않고 즉시 중단한다.

우선순위:

1. SEC 8-K 또는 exhibit에 transcript 원문이 있는지 확인
2. 회사 IR 사이트의 earnings call, conference call, webcast, transcript 후보 확인
3. 공개 접근 가능한 transcript 후보 확인
4. 없으면 `[확인 필요: transcript 원문 미수집]`로 기록

Transcript 문서 `document_id` 형식:

```text
transcript:{ticker}:{date}:{source_slug}:{slug}
```

raw 경로:

```text
artifacts/raw/transcripts/{TICKER}/{YYYY-MM-DD}_{source_slug}_{slug}/
```

성공 시:

- `documents.jsonl`에 `source_type: transcripts`, `document_type: transcript`를 기록한다.
- `files.jsonl`에 `file_role: transcript`로 승격한다.
- `index.md`에는 원문 위치와 상태만 남긴다.

실패 시:

- `download-log.jsonl`에 `blocked`, `paywalled`, `not_found`, `skipped_budget`, `failed` 중 하나로 기록한다.
- `documents.jsonl`에는 확정 문서 후보가 있을 때만 `failed` 또는 `skipped` 상태로 남긴다.
- 회사별 `index.md`의 확인 필요 목록과 실패 및 보류 요약에 남긴다.

## 8단계: derived text 생성 또는 상태 기록

텍스트 추출이 실행 범위에 포함된 경우에만 `derived/text/`를 만든다.

경로 규칙:

```text
artifacts/derived/text/{raw 하위 구조와 동일한 구조}/
```

규칙:

- derived text는 원문을 해석하지 않는다.
- 번역, 요약, 투자 관점 분석을 넣지 않는다.
- raw 파일에서 추출한 plain text 또는 normalized text만 저장한다.
- text 파일도 검증 후 `files.jsonl`에 `file_role: text`로 기록한다.
- `documents.jsonl.text_status`는 `extracted`, `failed`, `pending`, `not_applicable` 중 하나로 기록한다.
- text 추출을 실행하지 않은 run에서는 회사별 `index.md`의 `text 추출` 칸에 `-`를 쓴다.

## 9단계: catalog 최종 검증

index를 쓰기 전에 catalog를 먼저 검증한다.

구조 QA:

- `entities.jsonl`, `documents.jsonl`, `files.jsonl`, `runs.jsonl`이 JSONL 형식이다.
- 한 줄은 하나의 JSON object다.
- 필드명은 `snake_case`다.
- schema 허용값 밖의 `document_type`, `source_type`, `collection_status`, `text_status`, `file_role`, `file_format`, `file_status`가 없다.
- `link_only` 상태를 사용하지 않는다.
- `companies/{TICKER}/sources.jsonl` 같은 이중 원장을 만들지 않는다.

관계 QA:

- `documents.jsonl.primary_file_id`가 존재하면 `files.jsonl.file_id`에 대응 record가 있다.
- `collection_status: collected`인 문서의 `primary_file_id`는 `null`이면 안 된다.
- `files.jsonl.document_id`는 `documents.jsonl.document_id`에 대응한다.
- `files.jsonl.entity_id`는 `entities.jsonl.entity_id`에 대응한다.
- `runs.jsonl.run_summary_path`와 `qa_path`는 실제 파일 경로와 맞는다.

파일 QA:

- `files.jsonl.local_path` 파일이 실제로 존재한다.
- `files.jsonl.size_bytes`는 0보다 크다.
- `files.jsonl.sha256`은 비어 있지 않다.
- 가능하면 실제 파일 hash와 `files.jsonl.sha256`이 일치한다.
- `file_status: available`이면 파일 접근이 가능해야 한다.

문제가 있으면:

- `qa.md`에 실패 항목을 남긴다.
- 관련 record의 `notes`에 `[확인 필요: {이유}]`를 남긴다.
- 후속 하네스가 사용하면 위험한 자료는 `unverified` 또는 `failed`로 표시한다.

## 10단계: 회사별 `index.md` 작성

`harness/schemas/source-pack-index.schema.md`를 따른다.

경로:

```text
artifacts/companies/{TICKER}/index.md
```

작성 원칙:

- `index.md`는 catalog를 사람이 읽기 쉽게 보여주는 지도다.
- 원본 장부는 `catalog/*.jsonl`이다.
- catalog와 충돌하면 catalog를 우선한다.
- `last_run_id`에는 현재 run id를 쓴다.
- `Catalog 참조` 섹션을 포함한다.
- 표에는 `document_id`, `raw/text 경로`, `상태`, `비고`를 중심으로 쓴다.
- 원자료 URL은 필요하면 비고에 넣을 수 있지만, 링크만 있는 문서를 `collected`로 표시하지 않는다.
- `다음 하네스 전달 요약`에는 다음 하네스가 먼저 읽을 catalog record와 raw/derived 경로를 안내한다.
- 여기서 `요약`은 수집 상태와 경로 안내의 요약이며, 원자료 내용 요약이 아니다.

필수:

- 필수 섹션 모두 포함
- Markdown 표 파싱 가능
- 누락, 실패, 보류 건수 음수 금지
- 수동 확인 필요 항목 명시
- 실패 및 보류 요약 포함
- 다음 하네스 전달 요약 포함

## 11단계: run 마감, `runs.jsonl` 갱신, QA 저장

run 종료 시 아래를 작성한다.

```text
artifacts/runs/{run-id}/run-summary.md
artifacts/runs/{run-id}/qa.md
artifacts/catalog/runs.jsonl
```

`runs.jsonl` 기록:

- `run_id`
- `target`
- `run_mode`
- `started_at`
- `ended_at`
- `status`
- `documents_attempted`
- `documents_collected`
- `files_available`
- `run_summary_path`
- `qa_path`

상태 기준:

| status | 의미 |
|---|---|
| `success` | 필수 Tier 1 자료와 catalog QA가 통과 |
| `partial_success` | 핵심 수집은 진행됐지만 일부 실패 또는 확인 필요가 있음 |
| `failed` | CIK 실패, SEC 핵심 수집 실패, catalog QA 실패 등으로 후속 사용이 위험 |
| `stopped` | 사람 승인, User-Agent 누락, 속도 제한, 외부 조건으로 중단 |

`qa.md`에는 아래를 남긴다.

- 통과 항목
- 실패 항목
- 확인 필요 항목
- 사람이 승인해야 할 항목
- 다음 실행에서 재시도할 항목

## 실패 처리

실패는 대화 보고로 끝내지 않는다. 가능한 범위에서 아래 파일에 남긴다.

- `download-log.jsonl`
- `run-summary.md`
- `qa.md`
- `catalog/documents.jsonl`
- `catalog/runs.jsonl`
- `artifacts/companies/{TICKER}/index.md`

주요 실패 처리:

| 상황 | 처리 |
|---|---|
| CIK 조회 실패 | run 실패 또는 중단, 수동 확인 필요 기록 |
| SEC 429 반복 | 30초 대기 후 최대 3회 재시도, 마지막 성공 지점 기록 |
| raw 다운로드 실패 | `download-log.jsonl`에 실패 기록, `files.jsonl` 승격 금지 |
| hash 검증 실패 | 파일을 `available`로 승격하지 않음, QA 실패 기록 |
| catalog schema 실패 | `qa.md`와 notes에 필드/값/관계 문제 기록 |
| IR 접근 실패 | 수동 확인 필요와 후보 URL 기록 |
| transcript 차단/유료벽 | 우회 금지, `blocked` 또는 `paywalled` 기록 |
| transcript 반복 실패 | 90일 냉각 기간 적용, 자동 재시도 금지 |

## 자체 점검 질문

`index.md`, `catalog/*.jsonl`, run 파일 저장 전 아래 질문에 답한다.
하나라도 "아니오"이면 산출물에 `[확인 필요: {이유}]`를 남기거나 QA 실패로 표시한다.

1. SEC User-Agent와 속도 제한을 확인했는가?
2. `run_id`와 `artifacts/runs/{run-id}/`를 만들었는가?
3. CIK 조회 출처와 fallback 사용 여부를 기록했는가?
4. `entity_id`가 `sec-cik-{10자리 CIK}` 형식인가?
5. Tier 1 문서 후보가 `documents.jsonl`에 기록됐는가?
6. 다운로드 시도마다 `download-log.jsonl` record가 있는가?
7. 성공한 raw 파일만 `files.jsonl`에 승격했는가?
8. `collection_status: collected`인 문서의 `primary_file_id`가 `files.jsonl.file_id`와 연결되는가?
9. 10-K, 10-Q, Proxy, 실적 발표 자료의 요청/수집/실패/보류 건수가 논리적으로 맞는가?
10. Item 2.02와 EX-99.1을 혼동하지 않고 주 기준과 보조 확인을 분리했는가?
11. Transcript 수집 전 이전 `download-log.jsonl`의 90일 냉각 기간을 확인했는가?
12. Transcript 시도 제한을 지켰고 차단/유료벽을 우회하지 않았는가?
13. IR 자료 미수집을 조용히 생략하지 않고 확인 필요 목록에 연결했는가?
14. 모든 Markdown 표가 구분선과 데이터 행을 분리해 파싱 가능한가?
15. 누락, 실패, 보류 건수가 음수로 계산된 항목이 없는가?
16. 후속 하네스가 먼저 읽을 catalog record와 raw/derived 경로를 `다음 하네스 전달 요약`에 남겼는가?
17. 원자료 내용 요약이나 해석을 분석 리포트처럼 작성하지 않았는가?
18. 투자 판단, 목표주가, 매수/매도 의견을 쓰지 않았는가?

## 속도 제한

- SEC EDGAR API 호출 간격: 최소 0.5초
- SEC raw 다운로드 요청 간격: 최소 0.5초
- IR 사이트 요청 간격: 최소 2.0초
- 429 오류: 30초 대기 후 최대 3회 재시도

모든 SEC 요청에는 `config.md`의 User-Agent를 포함한다.
