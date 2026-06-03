# Source Pack QA Procedure

## 목적

Source Pack 실행 결과가 다음 가치투자 하네스에서 안정적으로 읽힐 수 있는지 검증한다.

QA의 대상은 회사별 `index.md` 하나가 아니다.
QA는 `catalog/*.jsonl`, `raw/`, `derived/text/`, run 기록, 회사별 `index.md`의 관계를 함께 검증한다.

QA는 원자료 내용을 분석하지 않는다.
QA는 원자료가 어디에 있고, catalog와 파일 관계가 일관되는지, 실패와 확인 필요가 정직하게 기록됐는지만 판단한다.

## 참조 파일

- `harness/contracts/source-pack.contract.md`
- `harness/schemas/source-pack-catalog.schema.md`
- `harness/schemas/source-pack-index.schema.md`
- `harness/procedures/source-pack-collector.md`
- `harness/rubrics/source-pack-qa.rubric.md`

## 입력

QA는 아래 입력을 받는다.

- 대상 ticker
- `run_id`
- `artifacts/companies/{TICKER}/index.md`
- `artifacts/catalog/entities.jsonl`
- `artifacts/catalog/documents.jsonl`
- `artifacts/catalog/files.jsonl`
- `artifacts/catalog/runs.jsonl`
- `artifacts/runs/{run-id}/download-log.jsonl`
- `artifacts/runs/{run-id}/run-summary.md`
- `artifacts/runs/{run-id}/qa.md`
- `artifacts/raw/`
- `artifacts/derived/text/`

## 출력

QA 결과는 아래 파일에 저장한다.

```text
artifacts/runs/{run-id}/qa.md
```

QA는 대화 보고로 끝나지 않는다.
후속 하네스가 읽으면 위험한 결과는 반드시 `qa.md`, 관련 catalog record의 `notes`, 회사별 `index.md`의 확인 필요 목록 중 적어도 한 곳에 남긴다.

## 판정 상태

| 판정 | 의미 |
|---|---|
| `pass` | 다음 하네스가 사용할 수 있음 |
| `partial_pass` | 일부 확인 필요가 있으나 핵심 자료는 사용할 수 있음 |
| `unverified` | catalog 또는 파일 관계 검증이 부족해 후속 사용 전 확인 필요 |
| `fail` | 후속 하네스가 사용하면 위험함 |
| `stopped` | User-Agent, 사람 승인, 외부 조건 등으로 QA 완료 불가 |

## 표준 단계 원칙

`test_collection`을 포함한 모든 run의 QA는 표준 14단계 제목과 순서를 유지한다.
run_scope 특화 검증은 단계 제목을 바꾸지 말고 해당 표준 단계 내부의 추가 행으로 기록한다.
run_scope 밖 항목은 생략하지 않고 `skipped_out_of_scope`로 명시한다.

## 1단계: run 범위 확인

1. `run_id`가 있는지 확인한다.
2. `artifacts/runs/{run-id}/` 폴더가 있는지 확인한다.
3. `download-log.jsonl`, `run-summary.md`, `qa.md` 경로가 맞는지 확인한다.
4. `catalog/runs.jsonl`에 해당 `run_id` record가 있는지 확인한다.
5. `runs.jsonl.run_summary_path`와 `qa_path`가 실제 경로와 맞는지 확인한다.
6. `run_mode`와 `status`가 schema 허용값인지 확인한다.
7. `run_mode: test_collection`이면 `runs.jsonl.run_scope`가 비어 있지 않은지 확인한다.

실패 기준:

- `run_id`가 없거나 run 폴더가 없으면 `fail`
- `runs.jsonl` record가 없으면 `unverified`
- `run_summary_path` 또는 `qa_path`가 잘못되면 `partial_pass` 또는 `unverified`
- `test_collection`인데 `run_scope`가 없거나 비어 있으면 `unverified`

## 2단계: catalog JSONL 구조 검증

대상 파일:

```text
artifacts/catalog/entities.jsonl
artifacts/catalog/documents.jsonl
artifacts/catalog/files.jsonl
artifacts/catalog/runs.jsonl
```

확인:

1. 파일이 존재하는가?
2. 빈 줄이 없는가?
3. 한 줄이 하나의 JSON object인가?
4. 필드명이 `snake_case`인가?
5. 날짜는 `YYYY-MM-DD`, 시각은 ISO 8601 형식인가?
6. 확인되지 않은 값이 필드 누락 대신 `null` 또는 `[확인 필요: {이유}]`로 기록됐는가?

실패 기준:

- JSONL 파싱 불가 파일이 있으면 `fail`
- 필수 catalog 파일이 없으면 `fail`
- 빈 줄이나 필드명 문제만 있으면 영향 범위에 따라 `partial_pass` 또는 `unverified`

## 3단계: schema 허용값 검증

아래 필드가 schema 허용값만 사용하는지 확인한다.

| 파일 | 필드 |
|---|---|
| `entities.jsonl` | `entity_status` |
| `documents.jsonl` | `source_type`, `document_type`, `collection_status`, `text_status` |
| `files.jsonl` | `source_type`, `file_role`, `file_format`, `file_status` |
| `runs.jsonl` | `run_mode`, `status`, 조건부 `run_scope` |
| `download-log.jsonl` | `attempt_status` |

자동 실패 또는 미검증:

- schema에 없는 `document_type`이 있으면 `fail`
- schema에 없는 `collection_status` 또는 `file_status`가 있으면 `fail`
- `link_only` 또는 동등한 링크-only 운영 상태가 있으면 `fail`

## 4단계: entity 관계 검증

확인:

1. 대상 ticker의 `entities.jsonl` record가 있는가?
2. `entity_id`가 `sec-cik-{10자리 CIK}` 형식인가?
3. `cik`가 10자리 문자열인가?
4. `documents.jsonl.entity_id`가 `entities.jsonl.entity_id`와 연결되는가?
5. `files.jsonl.entity_id`가 `entities.jsonl.entity_id`와 연결되는가?
6. 같은 ticker가 여러 CIK에 연결되면 확인 필요로 표시됐는가?

실패 기준:

- CIK가 없고 실패 사유도 없으면 `fail`
- `document` 또는 `file` record가 존재하는데 대응 `entity`가 없으면 `fail`
- ticker-CIK 불일치가 설명 없이 남아 있으면 `unverified`

## 5단계: document 원장 검증

확인:

1. Tier 1 자료 후보가 `documents.jsonl`에 기록됐는가?
2. `document_id`가 source type별 규칙을 따르는가?
3. SEC 문서는 `accession_no`, `form_type`, `primary_doc`가 가능한 범위에서 기록됐는가?
4. `documents.jsonl.source_url`은 가능한 경우 primary document URL인가?
5. filing folder URL과 primary document URL을 혼동하지 않았는가?
6. `collection_status: collected`인 문서의 `primary_file_id`가 null이 아닌가?
7. `collection_status: failed` 또는 `skipped`인 문서에 이유가 있는가?
8. `text_status`가 text 추출 실행 여부와 일치하는가?

Tier 1 기본 범위:

| 자료 | 기본 범위 |
|---|---|
| 10-K | 최대 10년 |
| 10-Q | 최대 12분기 |
| DEF 14A | 최대 5년 |
| 8-K Item 2.02 | 최대 12분기 |

`test_collection` 예외:

- `run_mode: test_collection`이면 `runs.jsonl.run_scope`에 선언된 범위를 document 검증 기준으로 삼는다.
- `run_scope` 밖의 Tier 1 기본 범위 누락은 실패나 부분 성공의 원인으로 삼지 않는다.
- `test_collection`의 `pass`는 선언된 테스트 범위 안에서의 통과를 뜻하며, 해당 ticker의 전체 Source Pack 완료를 뜻하지 않는다.
- `qa.md`에는 실행 범위 밖 단계도 생략하지 않고 `skipped_out_of_scope`로 표시한다.

실패 기준:

- `collected`인데 `primary_file_id`가 없으면 `fail`
- 필수 자료가 누락됐는데 이유가 없으면 `fail`
- source URL이 접근 불가능한 폴더 URL만 있고 primary document 정보가 없으면 `unverified`

## 6단계: file 원장과 raw 파일 검증

확인:

1. `files.jsonl.file_id`가 `sha256:{hash}` 형식인가?
2. `files.jsonl.local_path`가 실제 존재하는가?
3. `size_bytes`가 0보다 큰가?
4. `sha256`이 비어 있지 않은가?
5. 가능하면 실제 파일 hash와 `files.jsonl.sha256`이 일치하는가?
6. `file_status: available`이면 파일이 접근 가능한가?
7. `files.jsonl.document_id`가 `documents.jsonl.document_id`와 연결되는가?
8. `primary_file_id`가 `files.jsonl.file_id`에 대응하는가?
9. `content_type`이 있으면 MIME type처럼 보이는가? 예: `text/html`, `application/pdf`

실패 기준:

- `available` 파일의 `local_path`가 없으면 `fail`
- `size_bytes <= 0`이면 `fail`
- `sha256`이 없으면 `fail`
- `primary_file_id`가 대응 file record 없이 존재하면 `fail`

## 7단계: download-log와 승격 관계 검증

확인:

1. 다운로드 시도마다 `download-log.jsonl` record가 있는가?
2. 성공한 attempt가 `files.jsonl`에 승격됐는가?
3. 실패한 attempt가 `files.jsonl`에 잘못 승격되지 않았는가?
4. `attempt_status`가 schema 허용값인가?
5. 실패 attempt에 `error` 또는 실패 이유가 있는가?
6. `target_path`, `size_bytes`, `sha256`가 성공/실패 상태와 논리적으로 맞는가?

실패 기준:

- 성공 attempt가 있으나 SHA-256, size, content type, 파일 접근 검증 실패 사유가 기록되어 승격되지 않았으면 `partial_pass` 또는 `unverified`
- 성공 attempt가 있고 검증 실패 사유도 없는데 `files.jsonl`에 승격되지 않았으면 `fail`
- 실패 다운로드가 `available` 파일로 승격됐으면 `fail`
- 실패 이유가 없는 반복 실패는 `partial_pass` 또는 `unverified`

## 8단계: SEC 실적 발표 자료 검증

확인:

1. 실적 발표 자료는 `8-K Item 2.02`를 주 기준으로 삼았는가?
2. `items`에 `2.02`, `9.01` 등이 배열로 기록됐는가?
3. `EX-99.1`은 보조 기준으로만 사용됐는가?
4. EX-99.1 또는 관련 exhibit이 있으면 `files.jsonl.file_role: exhibit`으로 기록됐는가?
5. Exhibit이 누락되거나 확인되지 않으면 `[확인 필요: exhibit 누락 여부]`가 남아 있는가?
6. `run_scope` 또는 `config.md`에 포함된 exhibit의 `files.jsonl.local_path`가 없으면 `repair_required`로 표시됐는가?
7. 과거에 수집한 적 없는 optional exhibit은 `repair_required`가 아니라 신규 파일 후보 또는 `skipped_out_of_scope`로 처리됐는가?

실패 기준:

- Item 2.02와 EX-99.1을 같은 기준처럼 혼동하면 `unverified` 또는 `fail`
- 실적 발표 자료 누락을 이유 없이 생략하면 `fail`
- `available`로 기록된 검증 대상 exhibit의 local_path가 없는데 `skipped_existing`으로 처리하면 `unverified` 또는 `fail`

## 9단계: IR 자료 검증

확인:

1. IR 자료가 있으면 `source_type: company-ir`인가?
2. IR presentation은 `document_type: ir-deck`인가?
3. IR presentation 파일은 `file_role: ir_deck`인가?
4. shareholder letter, annual report, 기타 IR 문서는 기본적으로 `file_role: primary`인가?
5. 제목, 날짜, source URL이 불확실하면 `notes`에 확인 필요가 있는가?
6. IR 자동 탐색 실패가 조용히 생략되지 않았는가?

실패 기준:

- IR 문서를 확정처럼 표시했지만 출처나 파일이 없으면 `unverified`
- IR 실패를 아무 기록 없이 생략하면 `partial_pass` 또는 `fail`

## 10단계: Transcript optional 검증

Transcript는 optional source다. 실패 자체는 Source Pack 전체 실패가 아니다.

확인:

1. transcript 수집 전 이전 `artifacts/runs/*/download-log.jsonl`을 확인했는가?
2. `quarter`가 `YYYYQ{n}` 형식인가? 예: `2026Q1`
3. 90일 이내 동일 `ticker`, `quarter`, `source_name`의 `blocked`, `paywalled`, `not_found`, `skipped_budget` 기록이 있으면 자동 재시도하지 않았는가?
4. 티커당 `source_name` 또는 `source_type` 종류가 3개 이하인가?
5. 각 분기에서 후보 출처를 2개 이하로 시도했는가?
6. 출처당 자동 접근은 1회 이하인가?
7. Cloudflare, bot 차단, 로그인 요구, 유료벽을 우회하지 않았는가?
8. transcript 성공 파일은 `source_type: transcripts`, `document_type: transcript`, `file_role: transcript`인가?
9. transcript 실패는 `download-log.jsonl`, `index.md`, `qa.md` 중 적절한 위치에 기록됐는가?

자동 실패:

- transcript 번역, 요약, Q&A 구조화, 투자 관점 분석이 catalog/raw/index/qa에 들어가 있으면 `fail`
- Cloudflare, paywall, login 우회 시도가 있으면 `fail`

## 11단계: derived text 검증

텍스트 추출이 실행된 경우에만 검증한다.

확인:

1. `derived/text/`가 `raw/` 하위 구조를 mirror하는가?
2. text 파일은 `files.jsonl.file_role: text`로 기록됐는가?
3. `documents.jsonl.text_status`가 `extracted`, `failed`, `pending`, `not_applicable` 중 하나인가?
4. text 추출 미실행 run에서는 index의 `text 추출` 칸이 `-`로 표시됐는가?
5. derived text에 번역, 요약, 투자 분석이 들어가지 않았는가?

실패 기준:

- derived text에 원자료 해석이나 투자 판단이 들어가면 `fail`
- text 파일이 있는데 `files.jsonl`에 기록되지 않았으면 `unverified`

## 12단계: 회사별 `index.md` 검증

`harness/schemas/source-pack-index.schema.md`를 따른다.

확인:

1. 경로가 `artifacts/companies/{TICKER}/index.md`인가?
2. 예전 경로 `artifacts/{TICKER}/phase2/step3-source-pack/index.md`를 운영 산출물로 쓰지 않았는가?
3. 필수 섹션이 모두 있는가?
4. header의 `last_run_id`, `ticker`, `entity_id`, `cik`, `catalog_status`가 catalog와 맞는가?
5. `Catalog 참조` 섹션이 있는가?
6. 표가 Markdown으로 파싱 가능한가?
7. 표 구분선과 첫 데이터 행이 같은 줄에 붙어 있지 않은가?
8. 누락, 실패, 보류 건수가 음수가 아닌가?
9. `raw/text 경로`가 catalog의 `raw_root`, `text_root`, `files.jsonl.local_path`와 맞는가?
10. `다음 하네스 전달 요약`이 있는가?
11. `index.md`가 `catalog/documents.jsonl`을 중복 원장처럼 복제하지 않는가?
12. `index.md`와 catalog가 충돌하면 `[확인 필요: catalog 불일치]`가 남아 있는가?

자동 실패:

- 필수 섹션 누락
- Markdown 표 파싱 불가
- 누락, 실패, 보류 건수 음수
- `link_only` 상태 사용
- 링크만 있고 raw 파일이 없는 문서를 `collected`로 표시
- 다음 하네스 전달 요약 누락

## 13단계: 금지 내용 검증

아래 내용이 있으면 실패로 본다.

- 원자료 내용 해석
- 투자 thesis 작성
- valuation 의견
- 목표주가
- 매수/매도/보유 판단
- 경영진 또는 사업 품질에 대한 분석 결론
- transcript 번역
- transcript 요약
- transcript Q&A 구조화
- 투자 관점 transcript 분석
- 유료벽, 로그인, Cloudflare, bot 차단 우회 시도

단, 수집 상태와 경로 안내의 요약은 허용된다.

## 14단계: QA 결과 작성

`qa.md`는 아래 형식을 따른다.

```md
# Source Pack QA - {TICKER}

run_id: {run-id}
qa_date: YYYY-MM-DD
overall_status: pass | partial_pass | unverified | fail | stopped

## 요약
- 통과:
- 실패:
- 미검증:
- 사람 승인 필요:

## 1단계: run 범위 확인
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|

## 2단계: catalog JSONL 구조 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|

## 3단계: schema 허용값 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|

## 4단계: entity 관계 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|

## 5단계: document 원장 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|

## 6단계: file 원장과 raw 파일 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|

## 7단계: download-log와 승격 관계 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|

## 8단계: SEC 실적 발표 자료 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|

## 9단계: IR 자료 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|

## 10단계: Transcript optional 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|

## 11단계: derived text 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|

## 12단계: 회사별 index.md 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|

## 13단계: 금지 내용 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|

## 14단계: QA 결과 작성
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|

## 다음 하네스 사용 가능 여부
| 하네스 | 사용 가능 여부 | 읽을 자료 | 주의 |
|---|---|---|---|

## 다음 조치
- 
```

## 판정 규칙

자동 실패 조건이 하나라도 있으면 전체 `overall_status`는 `fail`이다.

그 외에는 아래 기준을 적용한다.

| 조건 | overall_status |
|---|---|
| catalog, raw/file, index, run QA 모두 통과 | `pass` |
| transcript 또는 IR optional source 실패만 있음 | `partial_pass` |
| catalog 관계 또는 파일 검증 일부가 불완전함 | `unverified` |
| 필수 Tier 1 자료, CIK, catalog schema, raw 파일 관계가 깨짐 | `fail` |
| User-Agent, 사람 승인, 외부 조건으로 QA 완료 불가 | `stopped` |

여러 조건이 동시에 적용되면 더 보수적인 판정을 우선한다.
예를 들어 optional source 실패는 `partial_pass`에 해당하더라도 catalog 관계 또는 파일 검증이 불완전하면 `unverified`를 우선한다.

## 상태 매핑

QA의 `overall_status`, `catalog/runs.jsonl.status`, 회사별 `index.md.catalog_status`는 서로 다른 schema를 사용하므로 아래처럼 대응시킨다.

| QA `overall_status` | `runs.jsonl.status` | `index.md.catalog_status` |
|---|---|---|
| `pass` | `success` | `valid` |
| `partial_pass` | `partial_success` | `partial` |
| `unverified` | `partial_success` 또는 `failed` | `unverified` |
| `fail` | `failed` | `failed` |
| `stopped` | `stopped` | `unverified` |

`unverified`는 영향 범위에 따라 `runs.jsonl.status`를 `partial_success` 또는 `failed`로 둘 수 있다.
후속 하네스가 사용하면 위험한 경우에는 `failed`를 우선한다.

## QA 후 처리

QA 완료 후:

1. `artifacts/runs/{run-id}/qa.md`를 저장한다.
2. `catalog/runs.jsonl.status`가 상태 매핑과 심하게 불일치하면 `[확인 필요: runs status 불일치]`를 남긴다.
3. 회사별 `index.md`의 `catalog_status`가 상태 매핑과 맞는지 확인한다.
4. 후속 하네스가 사용하면 위험한 자료는 `unverified` 또는 `failed`로 표시한다.
5. 사람 승인 필요 항목은 `▶ 사람 승인 필요: {내용}` 형식으로 남긴다.
