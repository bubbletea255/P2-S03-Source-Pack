# Source Pack - AAPL (Apple Inc.)

last_updated: 2026-06-04
last_run_id: run-20260604-aapl-ir-pilot
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
collection_scope: SEC Tier 1 full-range test records retained; IR pilot added AAPL FY2026 Q2 earnings release HTML and consolidated financial statements PDF; no new SEC download, no SEC filings page harvesting, no webcast/audio, no transcript, no derived text

---

## Catalog 참조
| 원장 | 경로 | 이 회사 조회 기준 | 상태 |
|---|---|---|---|
| entities | artifacts/catalog/entities.jsonl | entity_id=sec-cik-0000320193, ticker=AAPL | valid |
| documents | artifacts/catalog/documents.jsonl | ticker=AAPL, cik=0000320193 | valid |
| files | artifacts/catalog/files.jsonl | ticker=AAPL | valid |
| runs | artifacts/catalog/runs.jsonl | run_id=run-20260604-aapl-ir-pilot | valid |

## 수집 현황 요약
| 항목 | 요청 | 새 수집 | 기존 보유 | 복구 필요 | 실패 | raw 파일 | text 추출 | 비고 |
|---|---:|---:|---:|---:|---:|---:|---:|---|
| SEC 10-K primary document | 10 | 0 | 10 | 0 | 0 | 10 | 0 | rerun fast path |
| SEC 10-Q primary document | 12 | 0 | 12 | 0 | 0 | 12 | 0 | rerun fast path |
| SEC DEF 14A primary document | 5 | 0 | 5 | 0 | 0 | 5 | 0 | rerun fast path |
| SEC 8-K Item 2.02 primary document | 12 | 0 | 12 | 0 | 0 | 12 | 0 | rerun fast path |
| SEC 8-K Item 2.02 EX-99.1 exhibit | 12 | 0 | 12 | 0 | 0 | 12 | 0 | rerun exhibit fast path |
| Company IR | 2 | 2 | 0 | 0 | 0 | 2 | 0 | FY2026 Q2 IR pilot; sec_overlap: likely with SEC 8-K/EX-99.1 |
| Transcript | 0 | 0 | 0 | 0 | 0 | 0 | 0 | run_scope에서 제외 |

## SEC 메타데이터
| 항목 | 값 | catalog 필드 | 출처 |
|---|---|---|---|
| company_name | Apple Inc. | entities.company_name | SEC submissions API |
| CIK | 0000320193 | entities.cik | SEC submissions API |
| SIC | 3571 | entities.sector | SEC submissions API |
| fiscal_year_end | filing별 period_end 사용 | documents.period_end | SEC submissions API |

## 10-K (Annual Reports)
| 회계연도 | 제출일 | period_end | document_id | raw/text 경로 | 상태 | 비고 |
|---|---|---|---|---|---|---|
| 2025 | 2025-10-31 | 2025-09-27 | sec:0000320193:0000320193-25-000079:10-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-25-000079 | collected | primary=aapl-20250927.htm |
| 2024 | 2024-11-01 | 2024-09-28 | sec:0000320193:0000320193-24-000123:10-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-24-000123 | collected | primary=aapl-20240928.htm |
| 2023 | 2023-11-03 | 2023-09-30 | sec:0000320193:0000320193-23-000106:10-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-23-000106 | collected | primary=aapl-20230930.htm |
| 2022 | 2022-10-28 | 2022-09-24 | sec:0000320193:0000320193-22-000108:10-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-22-000108 | collected | primary=aapl-20220924.htm |
| 2021 | 2021-10-29 | 2021-09-25 | sec:0000320193:0000320193-21-000105:10-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-21-000105 | collected | primary=aapl-20210925.htm |
| 2020 | 2020-10-30 | 2020-09-26 | sec:0000320193:0000320193-20-000096:10-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-20-000096 | collected | primary=aapl-20200926.htm |
| 2019 | 2019-10-31 | 2019-09-28 | sec:0000320193:0000320193-19-000119:10-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-19-000119 | collected | primary=a10-k20199282019.htm |
| 2018 | 2018-11-05 | 2018-09-29 | sec:0000320193:0000320193-18-000145:10-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-18-000145 | collected | primary=a10-k20189292018.htm |
| 2017 | 2017-11-03 | 2017-09-30 | sec:0000320193:0000320193-17-000070:10-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-17-000070 | collected | primary=a10-k20179302017.htm |
| 2016 | 2016-10-26 | 2016-09-24 | sec:0000320193:0001628280-16-020309:10-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0001628280-16-020309 | collected | primary=a201610-k9242016.htm |

## 10-Q (Quarterly Reports)
| 회계연도/분기 | 제출일 | period_end | document_id | raw/text 경로 | 상태 | 비고 |
|---|---|---|---|---|---|---|
| 2026 | 2026-05-01 | 2026-03-28 | sec:0000320193:0000320193-26-000013:10-q | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-26-000013 | collected | primary=aapl-20260328.htm |
| 2025 | 2026-01-30 | 2025-12-27 | sec:0000320193:0000320193-26-000006:10-q | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-26-000006 | collected | primary=aapl-20251227.htm |
| 2025 | 2025-08-01 | 2025-06-28 | sec:0000320193:0000320193-25-000073:10-q | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-25-000073 | collected | primary=aapl-20250628.htm |
| 2025 | 2025-05-02 | 2025-03-29 | sec:0000320193:0000320193-25-000057:10-q | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-25-000057 | collected | primary=aapl-20250329.htm |
| 2024 | 2025-01-31 | 2024-12-28 | sec:0000320193:0000320193-25-000008:10-q | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-25-000008 | collected | primary=aapl-20241228.htm |
| 2024 | 2024-08-02 | 2024-06-29 | sec:0000320193:0000320193-24-000081:10-q | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-24-000081 | collected | primary=aapl-20240629.htm |
| 2024 | 2024-05-03 | 2024-03-30 | sec:0000320193:0000320193-24-000069:10-q | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-24-000069 | collected | primary=aapl-20240330.htm |
| 2023 | 2024-02-02 | 2023-12-30 | sec:0000320193:0000320193-24-000006:10-q | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-24-000006 | collected | primary=aapl-20231230.htm |
| 2023 | 2023-08-04 | 2023-07-01 | sec:0000320193:0000320193-23-000077:10-q | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-23-000077 | collected | primary=aapl-20230701.htm |
| 2023 | 2023-05-05 | 2023-04-01 | sec:0000320193:0000320193-23-000064:10-q | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-23-000064 | collected | primary=aapl-20230401.htm |
| 2022 | 2023-02-03 | 2022-12-31 | sec:0000320193:0000320193-23-000006:10-q | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-23-000006 | collected | primary=aapl-20221231.htm |
| 2022 | 2022-07-29 | 2022-06-25 | sec:0000320193:0000320193-22-000070:10-q | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-22-000070 | collected | primary=aapl-20220625.htm |

## Proxy Statement (DEF 14A)
| 연도 | 제출일 | document_id | raw/text 경로 | 상태 | 비고 |
|---|---|---|---|---|---|
| 2026 | 2026-01-08 | sec:0000320193:0001308179-26-000008:def-14a | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0001308179-26-000008 | collected | primary=aapl014016-def14a.htm |
| 2025 | 2025-01-10 | sec:0000320193:0001308179-25-000008:def-14a | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0001308179-25-000008 | collected | primary=aapl4359751-def14a.htm |
| 2024 | 2024-01-11 | sec:0000320193:0001308179-24-000010:def-14a | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0001308179-24-000010 | collected | primary=laapl2024_def14a.htm |
| 2023 | 2023-01-12 | sec:0000320193:0001308179-23-000019:def-14a | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0001308179-23-000019 | collected | primary=laap2023_def14a.htm |
| 2022 | 2022-01-06 | sec:0000320193:0001193125-22-003583:def-14a | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0001193125-22-003583 | collected | primary=d222670ddef14a.htm |

## 실적 발표 자료 (8-K Item 2.02)
| 분기 | 제출일 | items | document_id | raw/text 경로 | Exhibit 확인 | 상태 | 비고 |
|---|---|---|---|---|---|---|---|
| 2026-04-30 | 2026-04-30 | 2.02,9.01 | sec:0000320193:0000320193-26-000011:8-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-26-000011 | available | collected | primary=aapl-20260430.htm |
| 2026-01-29 | 2026-01-29 | 2.02,9.01 | sec:0000320193:0000320193-26-000005:8-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-26-000005 | available | collected | primary=aapl-20260129.htm |
| 2025-10-30 | 2025-10-30 | 2.02,9.01 | sec:0000320193:0000320193-25-000077:8-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-25-000077 | available | collected | primary=aapl-20251030.htm |
| 2025-07-31 | 2025-07-31 | 2.02,9.01 | sec:0000320193:0000320193-25-000071:8-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-25-000071 | available | collected | primary=aapl-20250731.htm |
| 2025-05-01 | 2025-05-01 | 2.02,9.01 | sec:0000320193:0000320193-25-000055:8-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-25-000055 | available | collected | primary=aapl-20250501.htm |
| 2025-01-30 | 2025-01-30 | 2.02,9.01 | sec:0000320193:0000320193-25-000007:8-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-25-000007 | available | collected | primary=aapl-20250130.htm |
| 2024-10-31 | 2024-10-31 | 2.02,9.01 | sec:0000320193:0000320193-24-000120:8-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-24-000120 | available | collected | primary=aapl-20241031.htm |
| 2024-08-01 | 2024-08-01 | 2.02,9.01 | sec:0000320193:0000320193-24-000080:8-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-24-000080 | available | collected | primary=aapl-20240801.htm |
| 2024-05-02 | 2024-05-02 | 2.02,9.01 | sec:0000320193:0000320193-24-000067:8-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-24-000067 | available | collected | primary=aapl-20240502.htm |
| 2024-02-01 | 2024-02-01 | 2.02,9.01 | sec:0000320193:0000320193-24-000005:8-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-24-000005 | available | collected | primary=aapl-20240201.htm |
| 2023-11-02 | 2023-11-02 | 2.02,9.01 | sec:0000320193:0000320193-23-000104:8-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-23-000104 | available | collected | primary=aapl-20231102.htm |
| 2023-08-03 | 2023-08-03 | 2.02,9.01 | sec:0000320193:0000320193-23-000075:8-k | raw: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-23-000075 | available | collected | primary=aapl-20230803.htm |

## 8-K 주요 이벤트
| 날짜 | items | 제목 | document_id | raw/text 경로 | 상태 | 비고 |
|---|---|---|---|---|---|---|
| - | - | - | - | - | skipped | run_scope에서 제외 |

## IR 자료
| 날짜 | 제목 | document_id | raw/text 경로 | 상태 | 비고 |
|---|---|---|---|---|---|
| 2026-04-30 | Apple reports second quarter results | ir-aapl-earnings-release-fy2026-q2 | raw: artifacts/raw/company-ir/AAPL/2026-04-30_fy2026-q2-earnings-release | collected | sec_overlap: likely; related SEC 8-K/EX-99.1; exact_hash_match=false |
| 2026-04-30 | FY26 Q2 Consolidated Financial Statements | ir-aapl-financial-supplement-fy2026-q2 | raw: artifacts/raw/company-ir/AAPL/2026-04-30_fy2026-q2-financial-supplement | collected | sec_overlap: likely; related SEC 8-K/EX-99.1; exact_hash_match=false |

## 실적 발표 대본 원문 (Earnings Transcript, optional)
| 분기 | 출처 | document_id | raw/text 경로 | 상태 | 비고 |
|---|---|---|---|---|---|
| - | - | - | - | skipped | run_scope에서 제외 |

## 산업·경쟁 자료 후보
| 자료 | 출처 | document_id 또는 URL | raw/text 경로 | 상태 | 용도 |
|---|---|---|---|---|---|
| - | - | - | - | skipped | run_scope에서 제외 |

## 수동 확인 필요 목록
| 항목 | 기간/대상 | 이유 | 관련 catalog/run | 권장 확인 방법 |
|---|---|---|---|---|
| 없음 | - | SEC 확장 테스트 범위에서 확인 필요 없음 | - | - |

## 실패 및 보류 요약
| 항목 | 상태 | 이유 | 마지막 시도 run | 다음 조치 |
|---|---|---|---|---|
| SEC 확장 테스트 | success | idempotency passed; skipped_existing=39, download-log=0 | run-20260603-aapl-sec-full-scope-test-v2 | 추가 조치 없음 |
| IR pilot | success | FY2026 Q2 company-ir raw 2건을 catalog/index에 반영; SEC overlap likely 기록 | run-20260604-aapl-ir-pilot | 추가 조치 없음 |
| Transcript / derived text | skipped | 이번 run_scope에서 제외 | run-20260604-aapl-ir-pilot | 별도 실행에서 수집 |

## 다음 하네스 전달 요약
- Industry Primer가 먼저 읽을 자료: artifacts/catalog/documents.jsonl, artifacts/catalog/files.jsonl의 SEC 10-K/10-Q/DEF 14A/8-K records와 company-ir FY2026 Q2 records
- Value Chain 하네스가 먼저 읽을 자료: SEC 10-K raw primary files
- Business Model 하네스가 먼저 읽을 자료: SEC 10-K raw primary files와 DEF 14A raw primary files
- Market/Share 하네스가 먼저 읽을 자료: SEC 10-K raw primary files
- Competition 하네스가 먼저 읽을 자료: SEC 10-K raw primary files
- Financial Statement 하네스가 먼저 읽을 자료: SEC 10-K/10-Q raw primary files
- Transcript 하네스가 참고할 원문: 이번 run에서는 transcript 원문 미수집
- 확인 필요: transcript와 derived text는 이번 IR pilot 범위 밖
