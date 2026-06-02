# Source Pack Index Schema

회사별 사람용 지도 `artifacts/companies/{TICKER}/index.md`는 아래 형식을 따른다.

이 파일은 catalog 원장을 사람이 빠르게 읽기 위한 요약 지도다.
기계용 원본 장부는 `artifacts/catalog/*.jsonl`이며, `index.md`와 catalog가 충돌하면 catalog를 우선한다.

## 하네스 유형과 품질 축

하네스 유형: `수집형`

산출물 역할: `사람용 회사별 원자료 지도 / 다음 하네스 입력 안내`

수준 선언: `수집 엄격도 - 표준 수집`

주요 품질 축: `출처 추적성`, `로컬 파일 존재성`, `catalog 일관성`, `누락 관리`, `다음 하네스 전달성`

이 schema는 분석 리포트가 아니라 다음 하네스가 Source Pack catalog와 raw/derived 파일을 찾기 쉽게 만드는 사람용 index 형식을 정의한다.
여기서 `요약`은 수집 상태와 경로 안내의 요약을 뜻하며, 원자료 내용 요약을 뜻하지 않는다.

금지되는 내용:

- 원자료 내용 해석
- 투자 thesis 작성
- valuation 의견
- 매수/매도/보유 판단
- 경영진 또는 사업 품질에 대한 분석 결론
- transcript 번역, 요약, Q&A 구조화, 투자 관점 분석

## 파일 경로

```text
artifacts/companies/{TICKER}/index.md
```

이전 구조인 `artifacts/{TICKER}/phase2/step3-source-pack/index.md`는 사용하지 않는다.

`companies/{TICKER}/sources.jsonl`은 만들지 않는다.
회사별 기계용 문서 목록은 `catalog/documents.jsonl`에서 `ticker`, `cik`, `entity_id`로 필터링해 얻는다.

## 필수 섹션

```md
# Source Pack - {TICKER} ({company_name})

last_updated: YYYY-MM-DD
last_run_id: {이 index를 마지막으로 갱신한 run-id}
collection_mode: new_collection | incremental_update | partial_recheck | comparison
catalog_status: valid | partial | failed | unverified
ticker: {TICKER}
entity_id: sec-cik-{10자리 CIK} or [확인 필요: reason]
cik: {10-digit CIK or [확인 필요: reason]}
exchange: {exchange or [확인 필요: reason]}
sector: {sicDescription or [확인 필요: reason]}
fiscal_year_end: {MM-DD or [확인 필요: reason]}
ir_site: {url or [확인 필요: reason]}
source_priority: SEC EDGAR -> Company IR -> Transcript optional -> 수동 확인
collection_scope: {summary}

---

## Catalog 참조
| 원장 | 경로 | 이 회사 조회 기준 | 상태 |
|---|---|---|---|
| entities | artifacts/catalog/entities.jsonl | entity_id={entity_id}, ticker={TICKER} | valid/partial/failed/unverified |
| documents | artifacts/catalog/documents.jsonl | ticker={TICKER}, cik={CIK} | valid/partial/failed/unverified |
| files | artifacts/catalog/files.jsonl | ticker={TICKER} | valid/partial/failed/unverified |
| runs | artifacts/catalog/runs.jsonl | run_id={last_run_id} | valid/partial/failed/unverified |

## 수집 현황 요약
| 항목 | 요청 | 수집 성공 | 실패 | 보류/스킵 | raw 파일 | text 추출 | 비고 |
|---|---:|---:|---:|---:|---:|---:|---|

## SEC 메타데이터
| 항목 | 값 | catalog 필드 | 출처 |
|---|---|---|---|

## 10-K (Annual Reports)
| 회계연도 | 제출일 | period_end | document_id | raw/text 경로 | 상태 | 비고 |
|---|---|---|---|---|---|---|

## 10-Q (Quarterly Reports)
| 회계연도/분기 | 제출일 | period_end | document_id | raw/text 경로 | 상태 | 비고 |
|---|---|---|---|---|---|---|

## Proxy Statement (DEF 14A)
| 연도 | 제출일 | document_id | raw/text 경로 | 상태 | 비고 |
|---|---|---|---|---|---|

## 실적 발표 자료 (8-K Item 2.02)
| 분기 | 제출일 | items | document_id | raw/text 경로 | Exhibit 확인 | 상태 | 비고 |
|---|---|---|---|---|---|---|---|

## 8-K 주요 이벤트
| 날짜 | items | 제목 | document_id | raw/text 경로 | 상태 | 비고 |
|---|---|---|---|---|---|---|

## IR 자료
| 날짜 | 제목 | document_id | raw/text 경로 | 상태 | 비고 |
|---|---|---|---|---|---|

## 실적 발표 대본 원문 (Earnings Transcript, optional)
| 분기 | 출처 | document_id | raw/text 경로 | 상태 | 비고 |
|---|---|---|---|---|---|

## 산업·경쟁 자료 후보
| 자료 | 출처 | document_id 또는 URL | raw/text 경로 | 상태 | 용도 |
|---|---|---|---|---|---|

## 수동 확인 필요 목록
| 항목 | 기간/대상 | 이유 | 관련 catalog/run | 권장 확인 방법 |
|---|---|---|---|---|

## 실패 및 보류 요약
| 항목 | 상태 | 이유 | 마지막 시도 run | 다음 조치 |
|---|---|---|---|---|

## 다음 하네스 전달 요약
- Industry Primer가 먼저 읽을 자료:
- Value Chain 하네스가 먼저 읽을 자료:
- Business Model 하네스가 먼저 읽을 자료:
- Market/Share 하네스가 먼저 읽을 자료:
- Competition 하네스가 먼저 읽을 자료:
- Financial Statement 하네스가 먼저 읽을 자료:
- Transcript 하네스가 참고할 원문:
- 확인 필요:
```

## 상태값 규칙

`index.md`의 상태값은 사람이 읽기 위한 요약이다.
기계용 정확한 상태는 `catalog/documents.jsonl`, `catalog/files.jsonl`, `artifacts/runs/{run-id}/download-log.jsonl`을 따른다.

권장 상태값:

| 상태 | 의미 |
|---|---|
| `collected` | 문서와 하나 이상의 raw 파일이 catalog에 등록됨 |
| `pending` | 문서 후보는 있으나 수집이 아직 끝나지 않음 |
| `failed` | 수집 또는 검증 실패 |
| `skipped` | 설정, 범위, 냉각 기간, optional source 정책으로 건너뜀 |
| `unverified` | catalog 또는 로컬 파일 검증이 끝나지 않음 |
| `not_applicable` | 해당 회사나 기간에 적용되지 않음 |

Transcript 상태는 optional source로 다룬다. `blocked`, `paywalled`, `not_found`, `skipped_budget` 같은 attempt 상태는 `download-log.jsonl`에 남기고, `index.md`에는 요약과 다음 조치만 남긴다.

## raw/text 경로 표기 규칙

`raw/text 경로` 칸에는 가능한 경우 아래 순서로 적는다.

```text
text: artifacts/derived/text/...
raw: artifacts/raw/...
```

텍스트 추출물이 없으면 `raw:`만 적는다.
raw 파일도 없으면 `[확인 필요: raw 파일 없음 - {이유}]`로 적는다.

경로는 `catalog/documents.jsonl`의 `raw_root`, `text_root`와 `catalog/files.jsonl`의 `local_path`에서 가져온다.
index에서 임의로 경로를 만들어내지 않는다.

## Catalog 참조 규칙

- `document_id`는 `catalog/documents.jsonl`의 값을 그대로 쓴다.
- `file_id`, `sha256`, `size_bytes`, `content_type` 같은 파일 세부 정보는 필요할 때만 비고에 요약한다.
- `primary_file_id`가 있는 문서는 `catalog/files.jsonl`에 대응 record가 있어야 한다.
- `index.md`는 `catalog/documents.jsonl`을 복제하는 파일이 아니다. 사람에게 필요한 상태와 다음 행동만 요약한다.
- catalog와 충돌하는 표기나 숫자가 발견되면 `index.md`에 `[확인 필요: catalog 불일치]`를 남기고 QA 실패 후보로 표시한다.

## 형식 규칙

- Markdown 표 구분선과 첫 데이터 행은 반드시 다른 줄이어야 한다.
- 누락, 실패, 보류 건수는 음수일 수 없다.
- 요청보다 많이 수집한 경우 실패/누락은 `0`이고 비고에 `추가 수집`을 쓴다.
- `text 추출` 칸은 `derived/text/` 생성이 실행된 경우에만 숫자를 기입하고, 미실행 시 `-`로 표시한다.
- 확인되지 않은 정보는 `[확인 필요: {이유}]` 형식으로 쓴다.
- 추정 정보는 `(추정: {근거})` 형식으로 쓴다.
- 사람 승인 필요 항목은 `▶ 사람 승인 필요: {내용}` 형식으로 쓴다.
- `link_only` 상태를 사용하지 않는다.
- 원자료 URL은 필요하면 비고에 넣을 수 있지만, 링크만 있고 raw 파일이 없는 문서를 `collected`로 표시하지 않는다.

## 다음 하네스 읽기 규칙

다음 하네스는 `index.md`만 읽고 결론을 내리지 않는다.
`index.md`는 길 안내 역할이고, 실제 입력 파일은 catalog와 raw/derived 경로에서 찾는다.

권장 순서:

1. `artifacts/companies/{TICKER}/index.md`를 읽어 수집 상태와 확인 필요 항목을 파악한다.
2. `artifacts/catalog/entities.jsonl`에서 `entity_id`, `ticker`, `cik`를 확인한다.
3. `artifacts/catalog/documents.jsonl`에서 필요한 `document_type`, `fiscal_year`, `period_end` 문서를 찾는다.
4. `artifacts/catalog/files.jsonl`에서 실제 `local_path`를 찾는다.
5. `derived/text/`가 있으면 텍스트를 먼저 읽고, 없으면 `raw/`를 읽는다.
6. raw와 derived가 모두 없으면 Source Pack 재실행 또는 사람 확인을 요청한다.
