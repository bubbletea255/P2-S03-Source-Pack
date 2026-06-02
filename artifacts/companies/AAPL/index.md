# Source Pack - AAPL (Apple Inc.)

last_updated: 2026-06-03
last_run_id: run-20260603-aapl-sec-smoke
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
collection_scope: test only: AAPL latest 10-Q, latest DEF 14A, latest 8-K Item 2.02 primary SEC filings; reuse existing latest 10-K; no exhibits, no IR, no transcript, no derived text

---

## Catalog 참조
| 원장 | 경로 | 이 회사 조회 기준 | 상태 |
|---|---|---|---|
| entities | artifacts/catalog/entities.jsonl | entity_id=sec-cik-0000320193, ticker=AAPL | valid |
| documents | artifacts/catalog/documents.jsonl | ticker=AAPL, cik=0000320193 | valid |
| files | artifacts/catalog/files.jsonl | ticker=AAPL | valid |
| runs | artifacts/catalog/runs.jsonl | run_id=run-20260603-aapl-sec-smoke | valid |

## 수집 현황 요약
| 항목 | 요청 | 새 수집 | 기존 보유 | 복구 필요 | 실패 | raw 파일 | text 추출 | 비고 |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| SEC 10-K primary document | 1 | 0 | 1 | 0 | 0 | 1 | 0 | 기존 test_collection 결과 재사용 |
| SEC 10-Q primary document | 1 | 1 | 0 | 0 | 0 | 1 | 0 | test_collection 범위 |
| SEC DEF 14A primary document | 1 | 1 | 0 | 0 | 0 | 1 | 0 | test_collection 범위 |
| SEC 8-K Item 2.02 primary document | 1 | 1 | 0 | 0 | 0 | 1 | 0 | exhibits 제외 |
| Company IR | 0 | 0 | 0 | 0 | 0 | 0 | 0 | run_scope에서 제외 |
| Transcript | 0 | 0 | 0 | 0 | 0 | 0 | 0 | run_scope에서 제외 |

## SEC 문서 목록
| 유형 | Filing date | Period end | Accession | Primary document | Raw file |
|---|---|---|---|---|---|
| 10-Q | 2026-05-01 | 2026-03-28 | 0000320193-26-000013 | aapl-20260328.htm | artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-26-000013/primary.html |
| 8-K | 2026-04-30 | 2026-04-30 | 0000320193-26-000011 | aapl-20260430.htm | artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-26-000011/primary.html |
| DEF 14A | 2026-01-08 | 2026-02-24 | 0001308179-26-000008 | aapl014016-def14a.htm | artifacts/raw/sec-edgar/cik-0000320193/accession-0001308179-26-000008/primary.html |
| 10-K | 2025-10-31 | 2025-09-27 | 0000320193-25-000079 | aapl-20250927.htm | artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-25-000079/primary.html |

## 확인 필요
| 항목 | 상태 | 이유 | 권장 조치 |
|---|---|---|---|
| 전체 Source Pack 범위 | not complete | 이번 run은 주요 SEC 문서 유형별 primary 일부만 검증한 test_collection | 운영 수집은 별도 new_collection으로 실행 |
| 8-K exhibits | out_of_scope | 이번 run은 primary document만 다운로드 | 다음 SEC 확장 테스트에서 exhibit 처리 검증 |

## 다음 하네스 입력
| 입력 | 경로 | 사용 조건 |
|---|---|---|
| catalog documents | artifacts/catalog/documents.jsonl | run_mode와 run_scope를 확인한 뒤 사용 |
| catalog files | artifacts/catalog/files.jsonl | file_status=available, sha256 검증 후 사용 |
| SEC raw HTML | artifacts/raw/sec-edgar/.../primary.html | 수집된 primary 원문 확인 용도 |
