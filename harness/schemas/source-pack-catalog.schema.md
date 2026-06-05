# Source Pack Catalog Schema

`artifacts/catalog/`는 Source Pack의 기계용 원장이다.

이 schema는 다음 파일의 형식과 의미를 정의한다.

- `artifacts/catalog/entities.jsonl`
- `artifacts/catalog/documents.jsonl`
- `artifacts/catalog/files.jsonl`
- `artifacts/catalog/runs.jsonl`
- `artifacts/catalog/ir-taxonomy-candidates.jsonl`

Source Pack catalog는 링크 북마크가 아니다.  
Source Pack catalog는 로컬에 저장된 원자료와 그 메타데이터의 원장이다.

## 하네스 유형과 품질 축

하네스 유형: `수집형`

산출물 역할: `raw 원자료 저장소 / catalog 원장 / 다음 하네스 입력 패키지`

수준 선언: `수집 엄격도 - 표준 수집`

주요 품질 축: `출처 추적성`, `로컬 파일 존재성`, `catalog 일관성`, `다음 하네스 전달성`

금지되는 내용:

- 원자료 내용 해석
- 투자 thesis 작성
- valuation 의견
- 매수/매도/보유 판단
- transcript 번역, 요약, Q&A 구조화, 투자 관점 분석

## 공통 JSONL 규칙

- 모든 catalog 파일은 JSONL 형식이다.
- 한 줄은 하나의 JSON object다.
- 빈 줄을 만들지 않는다.
- 필드명은 `snake_case`를 사용한다.
- 날짜는 `YYYY-MM-DD` 형식을 사용한다.
- 시각은 ISO 8601 형식을 사용한다. 예: `2026-06-02T10:00:00+09:00`
- 확인되지 않은 값은 필드를 생략하지 말고 `null`을 사용한다.
- 사람이 확인해야 하는 내용은 `notes` 또는 해당 run의 `qa.md`에 `[확인 필요: {이유}]`로 남긴다.
- `link_only` 상태는 사용하지 않는다.
- `catalog/*.jsonl`은 upsert 장부다. 같은 고유 key의 record를 중복 append하지 않고 갱신한다.
- `download-log.jsonl`은 append 전용 실행 일지다. 같은 다운로드 시도가 반복되더라도 시도 기록을 보존한다.

## 원장 우선순위

| 원장 | 역할 | 우선순위 |
|---|---|---|
| `documents.jsonl` | 문서 단위 단일 원장 | 문서 존재와 상태의 기준 |
| `files.jsonl` | 실제 로컬 파일 단위 확정 원장 | 파일 존재와 경로의 기준 |
| `entities.jsonl` | 회사/entity 원장 | ticker, CIK, 회사 메타데이터 기준 |
| `runs.jsonl` | 실행 이력 기계용 인덱스 | 실행 delta와 run 요약 위치 기준 |
| `ir-taxonomy-candidates.jsonl` | IR `document_type` 후보 관찰 원장 | schema 확장 검토용 보조 원장 |
| `companies/{TICKER}/index.md` | 사람용 회사별 지도 | catalog와 충돌하면 catalog 우선 |

`companies/{TICKER}/sources.jsonl`은 만들지 않는다.

## 식별자 규칙

### `entity_id`

SEC 회사:

```text
sec-cik-{10-digit-cik}
```

예:

```text
sec-cik-0000320193
```

### `document_id`

SEC 문서:

```text
sec:{cik}:{accession_no}:{document_type_slug}
```

예:

```text
sec:0000320193:0000320193-24-000123:10-k
```

IR 문서:

```text
ir-{ticker}-{document_type_slug}-{period_or_date}
```

예:

```text
ir-aapl-earnings-release-fy2026-q2
ir-aapl-financial-supplement-fy2026-q2
```

Transcript:

```text
transcript:{ticker}:{date}:{source_slug}:{slug}
```

예:

```text
transcript:AAPL:2025-01-30:motley-fool:q1-earnings-call
```

### `file_id`

파일 ID는 파일 내용의 SHA-256 hash를 사용한다.

```text
sha256:{hash}
```

같은 파일이 다른 경로에서 발견되어도 같은 `file_id`를 가질 수 있다.
`files.jsonl`의 upsert key는 `file_id + document_id + file_role` 조합이다.

### slug 규칙

- 소문자 사용
- 공백은 `-`로 변경
- 알파벳, 숫자, 하이픈만 사용
- 연속 하이픈은 하나로 축소
- 80자 이하 권장
- 제목이 없으면 `document`, `presentation`, `transcript` 같은 일반 이름 사용

## `entities.jsonl`

회사 또는 분석 대상 entity의 원장이다.

경로:

```text
artifacts/catalog/entities.jsonl
```

필수 필드:

| 필드 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `entity_id` | string | entity 고유 ID | `sec-cik-0000320193` |
| `ticker` | string | 현재 티커 | `AAPL` |
| `cik` | string or null | 10자리 SEC CIK | `0000320193` |
| `company_name` | string | 회사명 | `Apple Inc.` |
| `exchange` | string or null | 거래소 | `Nasdaq` |
| `country` | string | 국가 | `US` |
| `sector` | string or null | SEC SIC description 또는 수집된 sector | `Electronic Computers` |
| `fiscal_year_end` | string or null | 회계연도 종료 월일 | `09-28` |
| `entity_status` | string | entity 상태 | `active` |
| `last_updated` | string | 마지막 갱신일 | `2026-06-02` |

선택 필드:

| 필드 | 타입 | 설명 |
|---|---|---|
| `former_tickers` | array[string] | 과거 티커 배열 |
| `ir_site` | string or null | IR 사이트 URL |
| `notes` | string or null | 확인 필요 또는 수동 메모 |

허용 값:

| 필드 | 값 |
|---|---|
| `entity_status` | `active`, `inactive`, `merged`, `delisted`, `unknown` |

예시:

```json
{"entity_id":"sec-cik-0000320193","ticker":"AAPL","cik":"0000320193","company_name":"Apple Inc.","exchange":"Nasdaq","country":"US","sector":"Electronic Computers","fiscal_year_end":"09-28","entity_status":"active","last_updated":"2026-06-02","former_tickers":[],"ir_site":"https://investor.apple.com/","notes":null}
```

## `documents.jsonl`

논리적 문서의 원장이다.

문서란 SEC filing, IR presentation, earnings transcript처럼 다른 하네스가 분석 단위로 인식하는 자료를 뜻한다.

경로:

```text
artifacts/catalog/documents.jsonl
```

필수 필드:

필수 필드는 record마다 key가 있어야 한다.  
아직 값이 확정되지 않은 경우에는 필드를 생략하지 말고 `null`을 사용한다.

| 필드 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `document_id` | string | 문서 고유 ID | `sec:0000320193:0000320193-24-000123:10-k` |
| `entity_id` | string | 연결 entity | `sec-cik-0000320193` |
| `ticker` | string | 티커 | `AAPL` |
| `cik` | string or null | CIK | `0000320193` |
| `source_type` | string | 자료 출처 유형 | `sec-edgar` |
| `document_type` | string | 문서 유형 | `10-K` |
| `title` | string | 문서 제목 | `Form 10-K` |
| `filing_date` | string or null | 제출일 또는 게시일 | `2024-11-01` |
| `period_end` | string or null | 대상 기간 종료일 | `2024-09-28` |
| `fiscal_year` | string or null | 회계연도 | `2024` |
| `source_url` | string | 원출처 URL | `https://www.sec.gov/...` |
| `collection_status` | string | 문서 수집 상태 | `collected` |
| `primary_file_id` | string or null | 대표 파일 ID. `collected`일 때 필수 | `sha256:...` |
| `raw_root` | string or null | raw 문서 폴더 | `artifacts/raw/sec-edgar/...` |
| `text_root` | string or null | derived text 폴더 | `artifacts/derived/text/sec-edgar/...` |
| `first_collected_run_id` | string or null | 최초 수집 run | `run-20260602-aapl` |
| `last_checked_run_id` | string | 마지막 확인 run | `run-20260602-aapl` |

`primary_file_id` 규칙:

- `collection_status: collected`이면 `primary_file_id`는 null이면 안 된다.
- `collection_status: pending`, `failed`, `skipped`이면 `primary_file_id`는 `null`일 수 있다.
- `primary_file_id`가 존재하면 `files.jsonl`에 같은 `file_id` record가 있어야 한다.

SEC 전용 필드:

| 필드 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `accession_no` | string or null | SEC accession number | `0000320193-24-000123` |
| `form_type` | string or null | SEC form type | `10-K` |
| `items` | array[string] | 8-K item 목록 | `["2.02","9.01"]` |
| `primary_doc` | string or null | SEC primary document 이름 | `aapl-20240928.htm` |

공통 선택 필드:

| 필드 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `text_status` | string or null | 텍스트 추출 상태 | `pending` |
| `notes` | string or null | 확인 필요, 수동 메모, 예외 사항 | `[확인 필요: exhibit 누락 여부 확인]` |

IR/transcript 선택 필드:

| 필드 | 타입 | 설명 |
|---|---|---|
| `event_date` | string or null | 발표 또는 이벤트 날짜 |
| `source_name` | string or null | 회사 IR, Quartr, AlphaStreet 등 |
| `slug` | string or null | 경로에 사용하는 제목 slug |
| `language` | string or null | 문서 언어 |

허용 값:

| 필드 | 값 |
|---|---|
| `source_type` | `sec-edgar`, `company-ir`, `transcripts`, `industry-source`, `manual` |
| `document_type` | `10-K`, `10-Q`, `DEF 14A`, `8-K`, `transcript`, `ir-deck`, `ir-earnings-release`, `ir-financial-supplement`, `industry-source`, `other` |
| `collection_status` | `pending`, `collected`, `failed`, `skipped` |
| `text_status` | `pending`, `extracted`, `failed`, `not_applicable`, null |

`document_type` 의미:

| 값 | 의미 |
|---|---|
| `10-K` | Annual Report |
| `10-Q` | Quarterly Report |
| `DEF 14A` | Proxy Statement |
| `8-K` | Current Report |
| `transcript` | Earnings Call Transcript |
| `ir-deck` | IR Presentation |
| `ir-earnings-release` | 회사 IR 또는 Newsroom의 공식 실적 발표 자료 |
| `ir-financial-supplement` | 회사 IR 또는 Newsroom의 실적 관련 재무 보충자료 |
| `industry-source` | 산업/경쟁 자료 |
| `other` | 기타 |

IR `document_type` 운영 원칙:

- IR `document_type`은 controlled but extensible vocabulary다.
- 새 IR 자료 유형을 기존 값에 억지로 넣지 않는다.
- 새 공식 IR 자료 유형이 반복 발견되면 preflight 또는 run-summary에 `candidate_document_type`으로 기록하고, 사용자 승인 후 schema에 추가한다.
- 새 IR `document_type`을 추가할 때 의미, 제외 기준, 기본 `file_role`, earnings-related 여부를 함께 정한다.
- earnings-related로 명시된 IR `document_type`은 SEC overlap hash 비교 대상이다.

운영 원칙:

- `link_only` 상태는 사용하지 않는다.
- 실제 로컬 파일 존재 여부는 `files.jsonl`에서 확인한다.
- transcript는 optional source다.
- transcript 접근 차단, 유료벽, 검색 실패는 Source Pack 전체 실패가 아니다.
- transcript 번역, 요약, Q&A 구조화, 투자 관점 분석은 이 catalog에 포함하지 않는다.

예시:

```json
{"document_id":"sec:0000320193:0000320193-24-000123:10-k","entity_id":"sec-cik-0000320193","ticker":"AAPL","cik":"0000320193","source_type":"sec-edgar","document_type":"10-K","title":"Form 10-K","filing_date":"2024-11-01","period_end":"2024-09-28","fiscal_year":"2024","source_url":"https://www.sec.gov/Archives/edgar/data/320193/000032019324000123/","collection_status":"collected","primary_file_id":"sha256:example","raw_root":"artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-24-000123","text_root":"artifacts/derived/text/sec-edgar/cik-0000320193/accession-0000320193-24-000123","first_collected_run_id":"run-20260602-aapl","last_checked_run_id":"run-20260602-aapl","accession_no":"0000320193-24-000123","form_type":"10-K","items":[],"primary_doc":"aapl-20240928.htm","text_status":"pending","notes":null}
```

## `files.jsonl`

실제 로컬에 존재하는 파일의 확정 원장이다.

`files.jsonl`에는 검증 후 로컬에 존재한다고 확인된 파일만 기록한다.  
실패한 다운로드 시도는 `runs/{run-id}/download-log.jsonl`과 `qa.md`에 남긴다.

경로:

```text
artifacts/catalog/files.jsonl
```

필수 필드:

| 필드 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `file_id` | string | 파일 고유 ID | `sha256:...` |
| `document_id` | string | 연결 문서 ID | `sec:0000320193:...` |
| `entity_id` | string | 연결 entity | `sec-cik-0000320193` |
| `ticker` | string | 티커 | `AAPL` |
| `source_type` | string | 출처 유형 | `sec-edgar` |
| `file_role` | string | 파일 역할 | `primary` |
| `file_format` | string | 파일 형식 | `html` |
| `local_path` | string | 로컬 파일 경로 | `artifacts/raw/sec-edgar/.../primary.html` |
| `source_url` | string or null | 원출처 URL | `https://www.sec.gov/...` |
| `sha256` | string | 파일 해시 | `...` |
| `size_bytes` | number | 파일 크기 | `123456` |
| `retrieved_at` | string | 다운로드 또는 생성 시각 | `2026-06-02T10:00:00+09:00` |
| `run_id` | string | 생성 run | `run-20260602-aapl` |
| `file_status` | string | 파일 상태 | `available` |

선택 필드:

| 필드 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `content_type` | string or null | 파일 MIME type. 서버 응답 header 또는 파일 검사로 확인한 값 | `text/html`, `application/pdf`, `text/plain` |
| `notes` | string or null | 중복 hash 관계, 경로 예외, 확인 필요 메모 | `same_content_as: sha256:...` |

`file_format`은 저장된 파일의 형식 또는 확장자 기준 분류이고, `content_type`은 가능한 경우 확인한 MIME type이다.  
MIME type을 확인할 수 없으면 `content_type`은 `null`로 둔다.

허용 값:

| 필드 | 값 |
|---|---|
| `source_type` | `sec-edgar`, `company-ir`, `transcripts`, `industry-source`, `manual` |
| `file_role` | `metadata`, `primary`, `exhibit`, `ir_deck`, `transcript`, `text`, `other` |
| `file_format` | `json`, `html`, `txt`, `pdf`, `xml`, `xbrl`, `csv`, `other` |
| `file_status` | `available`, `missing`, `replaced` |

승격 조건:

run 완료 후 아래 조건을 만족한 파일만 `catalog/files.jsonl`에 승격한다.

- 로컬 파일이 실제 존재한다.
- 파일 크기가 0보다 크다.
- SHA-256 hash가 계산됐다.
- `document_id`가 존재한다.
- `file_role`이 정해졌다.
- source URL 또는 원출처 metadata가 남아 있다.

실패한 다운로드는 기본적으로 `files.jsonl`에 올리지 않는다.

Upsert와 중복 hash 규칙:

- 같은 `file_id + document_id + file_role` 조합이 이미 있으면 중복 append하지 않고 기존 record를 갱신한다.
- 같은 `file_id`지만 `document_id`가 다르면 별도 record를 허용한다.
- SEC 자료에서 accession이 다르면 각 accession `raw_root`의 자기완결성을 위해 별도 `local_path`를 허용한다.
- 같은 `file_id`, 같은 `document_id`, 다른 `file_role`이면 기존 `local_path` 재사용을 우선한다.
- 역할 구분상 별도 파일명이 필요하면 별도 `local_path`를 허용하고 `notes`에 이유를 남긴다.
- 같은 hash를 가진 별도 record에는 가능한 경우 `notes`에 `same_content_as` 또는 `duplicate_hash_of` 관계를 남긴다.

예시:

```json
{"file_id":"sha256:example","document_id":"sec:0000320193:0000320193-24-000123:10-k","entity_id":"sec-cik-0000320193","ticker":"AAPL","source_type":"sec-edgar","file_role":"primary","file_format":"html","local_path":"artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-24-000123/primary.html","source_url":"https://www.sec.gov/Archives/edgar/data/320193/000032019324000123/aapl-20240928.htm","sha256":"example","size_bytes":123456,"retrieved_at":"2026-06-02T10:00:00+09:00","run_id":"run-20260602-aapl","file_status":"available","content_type":"text/html"}
```

## `runs.jsonl`

기계용 실행 이력 인덱스다.

상세 실행 기록은 `artifacts/runs/{run-id}/`에 둔다.  
`catalog/runs.jsonl`은 그 상세 기록으로 가는 짧은 포인터다.
`catalog/runs.jsonl`은 실행별 delta를 기록하며 실제 보유 원자료의 source of truth가 아니다.
실제 보유 문서와 파일의 기준은 `documents.jsonl`과 `files.jsonl`이다.

경로:

```text
artifacts/catalog/runs.jsonl
```

필수 필드:

| 필드 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `run_id` | string | 실행 ID | `run-20260602-aapl` |
| `target` | string | 실행 대상 | `AAPL` |
| `run_mode` | string | 실행 모드 | `new_collection` |
| `started_at` | string | 시작 시각 | `2026-06-02T10:00:00+09:00` |
| `ended_at` | string or null | 종료 시각 | `2026-06-02T10:15:00+09:00` |
| `status` | string | 실행 상태 | `success` |
| `collected_new` | number | 이번 run에서 새로 수집 성공한 문서 수. 문서 단위 delta | `40` |
| `skipped_existing` | number | fast path 조건을 통과해 다시 다운로드하지 않은 기존 문서 수. 문서 단위 delta | `2` |
| `repair_required` | number | 과거에 수집 성공했으나 현재 catalog/file 관계 확인이 필요한 문서 수. 문서 단위 delta | `0` |
| `failed` | number | 이번 run에서 수집 시도했지만 실패했거나 후속 사용이 위험한 문서 수. 문서 단위 delta | `2` |
| `files_collected_new` | number | 이번 run에서 `files.jsonl`에 새로 승격한 파일 record 수. 파일 단위 delta | `55` |
| `run_summary_path` | string | 사람용 실행 요약 | `artifacts/runs/run-20260602-aapl/run-summary.md` |
| `qa_path` | string | QA 파일 | `artifacts/runs/run-20260602-aapl/qa.md` |

조건부 필드:

| 필드 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `run_scope` | string or null | 이번 run에서 승인된 수집/평가 범위. `test_collection`일 때 필수 | `test only: latest AAPL 10-K primary SEC filing, no exhibits, no IR, no transcript` |

허용 값:

| 필드 | 값 |
|---|---|
| `run_mode` | `new_collection`, `incremental_update`, `partial_recheck`, `test_collection`, `comparison` |
| `status` | `success`, `partial_success`, `failed`, `stopped` |

규칙:

- `run_mode: test_collection`이면 `run_scope`를 비워 두지 않는다.
- `test_collection`의 `success` 또는 QA `pass`는 `run_scope`에 선언된 범위 안에서의 성공을 뜻한다.
- `test_collection` 결과를 해당 ticker의 전체 Source Pack 완료로 해석하지 않는다.
- `collected_new`, `skipped_existing`, `repair_required`, `failed`는 문서 단위 실행 결과다.
- `files_collected_new`는 파일 원장 record 단위 실행 결과다. SEC exhibit 때문에 `collected_new`와 다를 수 있다.
- 다음 하네스는 실제 입력 자료 존재 여부를 `runs.jsonl`이 아니라 `documents.jsonl`과 `files.jsonl`로 판단한다.

예시:

```json
{"run_id":"run-20260602-aapl","target":"AAPL","run_mode":"new_collection","started_at":"2026-06-02T10:00:00+09:00","ended_at":"2026-06-02T10:15:00+09:00","status":"partial_success","collected_new":40,"skipped_existing":0,"repair_required":0,"failed":2,"files_collected_new":55,"run_summary_path":"artifacts/runs/run-20260602-aapl/run-summary.md","qa_path":"artifacts/runs/run-20260602-aapl/qa.md"}
```

`test_collection` 예시:

```json
{"run_id":"run-20260602-aapl-test","target":"AAPL","run_mode":"test_collection","run_scope":"test only: latest AAPL 10-K primary SEC filing, no exhibits, no IR, no transcript","started_at":"2026-06-02T10:00:00+09:00","ended_at":"2026-06-02T10:03:00+09:00","status":"success","collected_new":1,"skipped_existing":0,"repair_required":0,"failed":0,"files_collected_new":1,"run_summary_path":"artifacts/runs/run-20260602-aapl-test/run-summary.md","qa_path":"artifacts/runs/run-20260602-aapl-test/qa.md"}
```

## `ir-taxonomy-candidates.jsonl`

IR `document_type` 후보 관찰 원장이다.

경로:

```text
artifacts/catalog/ir-taxonomy-candidates.jsonl
```

역할:

- 새 IR 자료 유형 후보를 흩어진 run-summary 메모가 아니라 한 곳에 남긴다.
- 같은 후보가 여러 ticker 또는 여러 run에서 반복되는지 확인한다.
- 사용자에게 schema 확장 승인 요청을 할 근거를 제공한다.

중요:

```text
이 원장은 schema를 자동 변경하지 않는다.
새 document_type 추가는 사용자 승인 후 별도 schema 수정으로만 진행한다.
```

필수 필드:

| 필드 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `candidate_id` | string | 후보 관찰 고유 ID | `candidate:ir-investor-conference-presentation:ntra:2026-06-04:jpm-healthcare` |
| `candidate_document_type` | string | 제안된 IR document_type 후보 | `ir-investor-conference-presentation` |
| `ticker` | string | 관찰된 회사 티커 | `NTRA` |
| `entity_id` | string or null | 연결 entity | `sec-cik-...` |
| `observed_at` | string | 관찰일 | `2026-06-05` |
| `observed_in_run_id` | string or null | 관찰한 run 또는 preflight | `run-20260604-ntra-ir-pilot` |
| `source_url` | string or null | 후보를 발견한 원출처 URL | `https://...` |
| `evidence_title` | string | 관찰된 자료 제목 또는 설명 | `44th Annual J.P. Morgan Healthcare Conference presentation` |
| `handled_as` | string or null | 현재 schema 안에서 임시 처리한 document_type | `ir-deck` |
| `reason` | string | 후보로 본 이유 | `conference presentation이 반복 관찰될 수 있음` |
| `status` | string | 후보 상태 | `observed` |

선택 필드:

| 필드 | 타입 | 설명 |
|---|---|---|
| `evidence_count_hint` | number or null | 현재 run에서 파악한 대략 관찰 수 |
| `distinct_ticker_count_hint` | number or null | 현재 run에서 파악한 대략 ticker 수 |
| `review_trigger_reason` | string or null | 사용자 승인 요청을 제안하는 이유 |
| `recommended_action` | string or null | `keep_observing`, `request_user_review`, `merge_into_existing_type`, `reject_candidate` 등 |
| `notes` | string or null | 수동 메모 |

허용 값:

| 필드 | 값 |
|---|---|
| `status` | `observed`, `needs_review`, `approved`, `rejected`, `merged` |
| `recommended_action` | `keep_observing`, `request_user_review`, `merge_into_existing_type`, `reject_candidate`, null |

Upsert key:

```text
candidate_id
```

권장 `candidate_id`:

```text
candidate:{candidate_document_type}:{ticker-lower}:{YYYY-MM-DD}:{short-slug}
```

사용자 승인 요청 기준:

- 같은 `candidate_document_type`이 3개 이상 distinct ticker에서 관찰됐다.
- 같은 `candidate_document_type`이 5건 이상 관찰됐다.
- 기존 `document_type`으로 처리하면 의미 왜곡이 반복된다고 QA 또는 run-summary가 2회 이상 기록했다.

이 기준에 도달하면 run-summary와 QA에 `[사용자 승인 필요: IR document_type 승격 검토]`를 남긴다.
그래도 schema는 자동으로 변경하지 않는다.

## 관련 실행 로그: `download-log.jsonl`

`download-log.jsonl`은 catalog 원장이 아니라 run별 작업 일지다.

경로:

```text
artifacts/runs/{run-id}/download-log.jsonl
```

관계:

| 파일 | 역할 |
|---|---|
| `download-log.jsonl` | 실행 중 모든 다운로드 시도 기록 |
| `files.jsonl` | 검증 후 승격된 로컬 파일 원장 |

필수 필드:

| 필드 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `run_id` | string | 실행 ID | `run-20260602-aapl` |
| `attempt_id` | string | 시도 ID | `attempt-001` |
| `document_id` | string or null | 연결 문서 ID | `sec:...` |
| `ticker` | string | 티커 | `AAPL` |
| `quarter` | string or null | transcript 냉각 기간 확인용 분기. 형식은 `YYYYQ{n}` | `2026Q1` |
| `source_name` | string or null | transcript 또는 IR 출처명 | `Motley Fool` |
| `source_url` | string | 원출처 URL | `https://www.sec.gov/...` |
| `target_path` | string or null | 저장 목표 경로 | `artifacts/raw/.../primary.html` |
| `attempt_status` | string | 시도 상태 | `success` |
| `http_status` | number or null | HTTP status | `200` |
| `size_bytes` | number or null | 파일 크기 | `123456` |
| `sha256` | string or null | 파일 해시 | `...` |
| `started_at` | string | 시작 시각 | `2026-06-02T10:00:00+09:00` |
| `ended_at` | string or null | 종료 시각 | `2026-06-02T10:00:03+09:00` |
| `error` | string or null | 오류 메시지 | `Cloudflare blocked` |

허용 값:

| 필드 | 값 |
|---|---|
| `attempt_status` | `success`, `failed`, `retry`, `skipped`, `blocked`, `paywalled`, `not_found`, `skipped_budget` |

Transcript 재시도 규칙:

- collector는 transcript 수집 전 이전 `artifacts/runs/*/download-log.jsonl`을 확인한다.
- `quarter`는 `YYYYQ{n}` 형식만 사용한다. 예: `2026Q1`, `2026Q2`, `2026Q3`, `2026Q4`.
- 동일 `ticker`, `quarter`, `source_name` 조합의 `blocked`, `paywalled`, `not_found`, `skipped_budget` 기록이 90일 이내에 있으면 자동 재시도하지 않는다.
- 사용자가 명시적으로 재시도를 요청한 경우에만 90일 냉각 기간을 무시할 수 있다.

예시:

```json
{"run_id":"run-20260602-aapl","attempt_id":"attempt-001","document_id":"transcript:AAPL:2026-01-30:motley-fool:q1-earnings-call","ticker":"AAPL","quarter":"2026Q1","source_name":"Motley Fool","source_url":"https://example.com/transcript","target_path":null,"attempt_status":"paywalled","http_status":403,"size_bytes":null,"sha256":null,"started_at":"2026-06-02T10:00:00+09:00","ended_at":"2026-06-02T10:00:03+09:00","error":"paywall or login required"}
```

## raw 경로 규칙

### SEC EDGAR

```text
artifacts/raw/sec-edgar/cik-{CIK}/accession-{ACCESSION}/
```

권장 파일명:

| 파일 | 역할 |
|---|---|
| `metadata.json` | filing metadata |
| `primary.html` | primary document |
| `primary.txt` | SEC 원문 txt가 있는 경우 |
| `exhibits/{sequence}_{filename}` | exhibit 파일 |

### Company IR

```text
artifacts/raw/company-ir/{TICKER}/{YYYY-MM-DD}_{slug}/
```

권장 파일명:

| 파일 | 역할 |
|---|---|
| `metadata.json` | 출처, 제목, URL, 접근 시각 |
| `document.pdf` | IR PDF |
| `document.html` | IR HTML |
| `assets/` | 필요 시 이미지 또는 부속 파일 |

### Transcripts

```text
artifacts/raw/transcripts/{TICKER}/{YYYY-MM-DD}_{source_slug}_{slug}/
```

Transcript는 optional source다.

- 접근 차단, 유료벽, 로그인 요구, 미공개, 검색 실패가 흔하므로 transcript 수집 실패는 Source Pack 전체 실패로 보지 않는다.
- 티커당 transcript 탐색에 사용할 `source_name` 또는 `source_type` 종류는 최대 3개로 제한한다.
- 각 분기에서는 후보 출처 중 최대 2개 출처만 시도한다.
- 출처당 자동 접근은 최대 1회로 제한한다.
- Cloudflare, bot 차단, 로그인 요구, 유료벽이 감지되면 우회하지 않고 즉시 중단한다.

## derived text 경로 규칙

`derived/text/`는 `raw/`의 하위 구조를 가능한 한 그대로 mirror한다.

SEC:

```text
artifacts/derived/text/sec-edgar/cik-0000320193/accession-0000320193-24-000123/
```

Company IR:

```text
artifacts/derived/text/company-ir/AAPL/2025-09-09_iphone-event-presentation/
```

Transcript:

```text
artifacts/derived/text/transcripts/AAPL/2025-01-30_motley-fool_q1-earnings-call/
```

derived text는 원문을 해석하지 않는다.

## QA 규칙

catalog 산출물은 아래 조건을 만족해야 한다.

### 구조 QA

- `entities.jsonl`, `documents.jsonl`, `files.jsonl`, `runs.jsonl`이 JSONL 형식을 따른다.
- `companies/{TICKER}/sources.jsonl` 같은 이중 원장을 만들지 않는다.
- `link_only` 상태를 사용하지 않는다.

### 관계 QA

- `documents.jsonl.primary_file_id`가 존재하면 `files.jsonl.file_id`에 대응 record가 있다.
- `files.jsonl.document_id`는 `documents.jsonl.document_id`에 대응해야 한다.
- `files.jsonl.entity_id`는 `entities.jsonl.entity_id`에 대응해야 한다.
- `runs.jsonl.run_summary_path`는 존재하거나 생성 예정 경로로 명시되어야 한다.

### 파일 QA

- `files.jsonl.local_path` 파일이 실제로 존재해야 한다.
- `files.jsonl.size_bytes`는 0보다 커야 한다.
- `files.jsonl.sha256`은 비어 있으면 안 된다.
- `files.jsonl.file_status: available`이면 파일 접근이 가능해야 한다.

### Transcript QA

- transcript 실패는 전체 Source Pack 실패로 처리하지 않는다.
- `blocked`, `paywalled`, `not_found`, `skipped_budget` 기록은 `download-log.jsonl`에 남긴다.
- 90일 이내 같은 `ticker`, `quarter`, `source_name` 실패 기록이 있으면 자동 재시도하지 않는다.
- transcript 번역, 요약, Q&A 구조화, 투자 관점 분석은 catalog나 raw에 넣지 않는다.

## 다음 하네스 읽기 규칙

다른 가치투자 하네스는 원칙적으로 외부 사이트를 다시 방문하지 않는다.

권장 읽기 순서:

1. `artifacts/companies/{TICKER}/index.md`
2. `artifacts/catalog/entities.jsonl`
3. `artifacts/catalog/documents.jsonl`
4. `artifacts/catalog/files.jsonl`
5. `artifacts/derived/text/`
6. `artifacts/raw/`

raw와 derived가 모두 없으면 Source Pack 재실행 또는 사람 확인을 요청한다.
