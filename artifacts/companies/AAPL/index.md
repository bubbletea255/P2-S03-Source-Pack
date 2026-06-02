# Source Pack - AAPL (Apple Inc.)

last_updated: 2026-06-03
last_run_id: run-20260603-aapl-test
collection_mode: test_collection
catalog_status: valid
ticker: AAPL
entity_id: sec-cik-0000320193
cik: 0000320193
exchange: Nasdaq
sector: Electronic Computers
fiscal_year_end: [확인 필요: Apple fiscal year end varies; use documents.period_end for filing-level matching]
ir_site: https://investor.apple.com/
source_priority: SEC EDGAR -> Company IR -> Transcript optional -> manual check
collection_scope: test only: latest AAPL 10-K primary SEC filing, no exhibits, no IR, no transcript

---

## Catalog 참조
| 원장 | 경로 | 이 회사 조회 기준 | 상태 |
|---|---|---|---|
| entities | artifacts/catalog/entities.jsonl | entity_id=sec-cik-0000320193, ticker=AAPL | valid |
| documents | artifacts/catalog/documents.jsonl | ticker=AAPL, cik=0000320193 | valid |
| files | artifacts/catalog/files.jsonl | ticker=AAPL | valid |
| runs | artifacts/catalog/runs.jsonl | run_id=run-20260603-aapl-test | valid |

## 수집 현황 요약
| 항목 | 요청 | 수집 성공 | 실패 | 보류/스킵 | raw 파일 | text 추출 | 비고 |
|---|---:|---:|---:|---:|---:|---:|---|
| SEC 10-K primary document | 1 | 1 | 0 | 0 | 1 | 0 | test_collection 범위 |
| SEC exhibits | 0 | 0 | 0 | 0 | 0 | 0 | run_scope에서 제외 |
| Company IR | 0 | 0 | 0 | 0 | 0 | 0 | run_scope에서 제외 |
| Transcript | 0 | 0 | 0 | 0 | 0 | 0 | run_scope에서 제외 |

## SEC 메타데이터
| 항목 | 값 | catalog 필드 | 출처 |
|---|---|---|---|
| CIK | 0000320193 | entities.cik | SEC submissions API |
| Company | Apple Inc. | entities.company_name | SEC submissions API |
| Form | 10-K | documents.form_type | SEC submissions API |
| Accession | 0000320193-25-000079 | documents.accession_no | SEC submissions API |
| Filing date | 2025-10-31 | documents.filing_date | SEC submissions API |
| Period end | 2025-09-27 | documents.period_end | SEC submissions API |
| Primary document | aapl-20250927.htm | documents.primary_doc | SEC submissions API |

## 10-K (Annual Reports)
| Filing date | Period end | Accession | Primary document | Raw file | SHA-256 |
|---|---|---|---|---|---|
| 2025-10-31 | 2025-09-27 | 0000320193-25-000079 | aapl-20250927.htm | artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-25-000079/primary.html | 548ae59778cf08ee0f2ee088e7ece20d947076c3c01f74d2d65db4c2777e436a |

## 확인 필요
| 항목 | 상태 | 이유 | 권장 조치 |
|---|---|---|---|
| 전체 Source Pack 범위 | not complete | 이번 run은 최신 10-K primary 1건만 검증한 test_collection | 운영 수집은 별도 new_collection으로 실행 |

## 다음 하네스 입력
| 입력 | 경로 | 사용 조건 |
|---|---|---|
| SEC 10-K primary raw HTML | artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-25-000079/primary.html | 최신 10-K primary 원문 확인에만 사용 |
| catalog documents | artifacts/catalog/documents.jsonl | run_mode와 run_scope를 확인한 뒤 사용 |
| catalog files | artifacts/catalog/files.jsonl | file_status=available, sha256 검증 후 사용 |

