# Source Pack - APP (AppLovin Corporation)

last_updated: 2026-06-14
last_run_id: run-20260614-app-sec-sector-topup
collection_mode: partial_recheck
catalog_status: partial
ticker: APP
entity_id: sec-cik-0001751008
cik: 0001751008
exchange: Nasdaq
sector: Services-Computer Programming, Data Processing, Etc.
fiscal_year_end: 12-31
ir_site: https://investors.applovin.com/
source_priority: SEC EDGAR -> Company IR -> Transcript optional -> manual check
collection_scope: APP S04 Phase 1 SEC/sector top-up; latest 10-K, latest 10-Q, SEC entity/sector metadata only; no APP full SEC backfill; no DEF 14A, no 8-K backlog, no transcript/webcast/audio/video, no product pages, no PDF table extraction

---

## Catalog 참조
| 원장 | 경로 | 이 회사 조회 기준 | 상태 |
|---|---|---|---|
| entities | artifacts/catalog/entities.jsonl | entity_id=sec-cik-0001751008, ticker=APP | valid |
| documents | artifacts/catalog/documents.jsonl | ticker=APP, cik=0001751008 | partial |
| files | artifacts/catalog/files.jsonl | ticker=APP | partial |
| runs | artifacts/catalog/runs.jsonl | run_id=run-20260614-app-sec-sector-topup | valid |

## 수집 현황 요약
| 항목 | 요청 | 수집 성공 | 실패 | 보류/스킵 | raw 파일 | text 추출 | 비고 |
|---|---:|---:|---:|---:|---:|---:|---|
| SEC 10-K/10-Q Phase 1 top-up | 2 | 2 | 0 | 0 | 4 | 0 | latest 10-K/10-Q primary HTML과 filing metadata JSON 수집; item_id APP-S03-REQ-20260613-001~002 |
| SEC entity metadata | 1 | 1 | 0 | 0 | 0 | - | SEC submissions API로 sector metadata resolved; item_id APP-S03-REQ-20260613-003 |
| SEC FY2026 Q1 8-K overlap recheck | 1 | 1 | 0 | 0 | 2 | 0 | 이전 run에서 FY2026 Q1 8-K Item 2.02 primary와 EX-99.1 제한 수집 |
| Company IR | 2 | 2 | 0 | 0 | 2 | 0 | 기존 Q1 2026 earnings release HTML, financial update PDF; 재수집 없이 overlap notes 갱신 |
| Transcript | 0 | 0 | 0 | 1 | 0 | 0 | run_scope에서 제외 |
| Webcast/audio/video | 0 | 0 | 0 | 1 | 0 | 0 | run_scope에서 제외 |

## SEC 메타데이터
| 항목 | 값 | catalog 필드 | 출처 |
|---|---|---|---|
| company_name | AppLovin Corp | entities.company_name | SEC submissions API |
| CIK | 0001751008 | entities.cik | SEC submissions API |
| exchange | Nasdaq | entities.exchange | 기존 APP metadata 유지 |
| sector | Services-Computer Programming, Data Processing, Etc. | entities.sector | SEC submissions API sicDescription, sic=7370 |
| fiscal_year_end | 12-31 | entities.fiscal_year_end | SEC submissions API fiscalYearEnd=1231 |

## 10-K (Annual Reports)
| 회계연도 | 제출일 | period_end | document_id | raw/text 경로 | 상태 | 비고 |
|---|---|---|---|---|---|---|
| 2025 | 2026-02-19 | 2025-12-31 | sec:0001751008:0001751008-26-000010:10-k | raw: artifacts/raw/sec-edgar/cik-0001751008/accession-0001751008-26-000010 | collected | item_id=APP-S03-REQ-20260613-001; accession/date pinned at retrieval; primary_doc=app-20251231.htm |

## 10-Q (Quarterly Reports)
| 회계연도/분기 | 제출일 | period_end | document_id | raw/text 경로 | 상태 | 비고 |
|---|---|---|---|---|---|---|
| FY2026 Q1 | 2026-05-06 | 2026-03-31 | sec:0001751008:0001751008-26-000044:10-q | raw: artifacts/raw/sec-edgar/cik-0001751008/accession-0001751008-26-000044 | collected | item_id=APP-S03-REQ-20260613-002; accession/date pinned at retrieval; primary_doc=app-20260331.htm |

## Proxy Statement (DEF 14A)
| 연도 | 제출일 | document_id | raw/text 경로 | 상태 | 비고 |
|---|---|---|---|---|---|
| - | - | - | - | skipped | run_scope에서 제외 |

## 실적 발표 자료 (8-K Item 2.02)
| 분기 | 제출일 | items | document_id | raw/text 경로 | Exhibit 확인 | 상태 | 비고 |
|---|---|---|---|---|---|---|---|
| FY2026 Q1 | 2026-05-06 | 2.02,9.01 | sec:0001751008:0001751008-26-000042:8-k | raw: artifacts/raw/sec-edgar/cik-0001751008/accession-0001751008-26-000042 | EX-99.1 collected | collected | 제한 재확인 범위: primary 8-K와 EX-99.1만 수집 |

## 8-K 주요 이벤트
| 날짜 | items | 제목 | document_id | raw/text 경로 | 상태 | 비고 |
|---|---|---|---|---|---|---|
| - | - | - | - | - | skipped | run_scope에서 제외 |

## IR 자료
| 날짜 | 제목 | document_id | raw/text 경로 | 상태 | 비고 |
|---|---|---|---|---|---|
| 2026-05-06 | AppLovin Announces First Quarter 2026 Financial Results | ir-app-earnings-release-fy2026-q1 | raw: artifacts/raw/company-ir/APP/2026-05-06_fy2026-q1-earnings-release | collected | overlap_status: different_hash_from_sec_candidate; related_sec_document_id=sec:0001751008:0001751008-26-000042:8-k; related_sec_file_id=sha256:498f7a0986a47990501cbc57e5b107f9da82940116b8ccb5da18d526eb7c4441; exact_hash_match=false; future_recheck_required=false |
| 2026-05-06 | AppLovin Q1 2026 Financial Update | ir-app-financial-supplement-fy2026-q1 | raw: artifacts/raw/company-ir/APP/2026-05-06_fy2026-q1-financial-update | collected | source_label: Financial Update; handled_as: ir-financial-supplement; overlap_status: sec_equivalent_not_found_in_scoped_8k; canonical_source=company-ir; related_sec_document_id=sec:0001751008:0001751008-26-000042:8-k; exact_hash_match=false; sec_separate_financial_update_exhibit=not_found_in_scoped_8k; future_recheck_required=false |

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
| SEC overlap recheck | FY2026 Q1 earnings-related IR 2건 | APP FY2026 Q1 8-K Item 2.02 / EX-99.1 후보 확인 완료. IR 2건 모두 exact hash match 없음 | run-20260605-app-sec-ir-overlap-recheck | 완료. 향후 full SEC collection 때는 이 관계를 재사용 |
| product-level official docs/pages | MAX/AXON/AppDiscovery/mediation/advertising/measurement 등 | Phase 3 company-official 최소 규칙과 Phase 4 page_key/item_id preflight 전까지 운영 catalog 수집 보류 | APP-S03-REQ-20260613-004부터 예정 | Phase 2~5에서 별도 진행 |
| full SEC backfill | APP 10-K 10년 / 10-Q 12분기 / DEF 14A / 8-K backlog | 이번 Phase 1은 latest 10-K/10-Q + sector metadata top-up이며 full collection이 아님 | run-20260614-app-sec-sector-topup | 필요 시 별도 승인 후 full collection |

## 실패 및 보류 요약
| 항목 | 상태 | 이유 | 마지막 시도 run | 다음 조치 |
|---|---|---|---|---|
| APP SEC overlap | checked | APP FY2026 Q1 8-K Item 2.02 / EX-99.1 제한 수집 후 hash 비교 완료. same hash 없음 | run-20260605-app-sec-ir-overlap-recheck | 추가 조치 없음 |
| APP SEC/sector Phase 1 top-up | checked | latest 10-K, latest 10-Q, sector metadata collected/resolved | run-20260614-app-sec-sector-topup | Phase 2 company-official 최소 반영 계획으로 진행 |
| product-level official docs/pages | deferred | company-official 운영 규칙 반영 전 수집 금지 | pending Phase 2~5 | Phase 3 이후 page_key/canonical_url 승인 후 수집 |
| derived/text | skipped | run_scope에서 제외 | run-20260604-app-ir-pilot | 필요 시 별도 승인 |
| transcript/webcast/audio/video | skipped | run_scope에서 제외 | run-20260604-app-ir-pilot | 별도 transcript/audio 설계에서 처리 |

## 다음 하네스 전달 요약
- Industry Primer가 먼저 읽을 자료: latest APP 10-K `sec:0001751008:0001751008-26-000010:10-k`, latest APP 10-Q `sec:0001751008:0001751008-26-000044:10-q`, 기존 APP IR raw 2건, SEC FY2026 Q1 8-K/EX-99.1 raw. 단, product-level official docs/pages는 아직 deferred다.
- Value Chain 하네스가 먼저 읽을 자료: 현재 Source Pack 범위에서는 해당 자료 없음.
- Business Model 하네스가 먼저 읽을 자료: latest APP 10-K/10-Q SEC raw와 artifacts/catalog/documents.jsonl에서 ticker=APP, source_type=company-ir인 IR 2건.
- Market/Share 하네스가 먼저 읽을 자료: 현재 Source Pack 범위에서는 해당 자료 없음.
- Competition 하네스가 먼저 읽을 자료: 현재 Source Pack 범위에서는 해당 자료 없음.
- Financial Statement 하네스가 먼저 읽을 자료: ir-app-financial-supplement-fy2026-q1 raw PDF. `source_label: Financial Update; handled_as: ir-financial-supplement`, `canonical_source=company-ir`, `sec_equivalent_not_found_in_scoped_8k` 메모 확인 필요.
- Transcript 하네스가 참고할 원문: 없음. transcript는 run_scope에서 제외.
- 확인 필요: APP 전체 SEC collection은 아직 수행하지 않았다. 이번 Phase 1은 latest 10-K/10-Q + sector metadata top-up만 수행했다. product-level official docs/pages는 Phase 2~5에서 별도 처리한다.
