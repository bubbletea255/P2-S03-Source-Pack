# Source Pack 실행 요약

## 실행 정보
- 실행 모드: partial_recheck
- 대상 티커: APP
- run-id: run-20260614-app-sec-sector-topup
- 설정 파일: config.md
- 회사별 index: artifacts/companies/APP/index.md 갱신
- catalog 원장: artifacts/catalog/entities.jsonl, artifacts/catalog/documents.jsonl, artifacts/catalog/files.jsonl, artifacts/catalog/runs.jsonl 갱신
- raw 저장: artifacts/raw/sec-edgar/cik-0001751008/accession-0001751008-26-000010, artifacts/raw/sec-edgar/cik-0001751008/accession-0001751008-26-000044
- derived/text 생성: 없음
- run_scope: APP S04 SEC top-up only - latest 10-K + latest 10-Q accession/date pinned at retrieval, plus SEC entity/sector metadata. No DEF 14A, no 8-K backlog, no transcript, no product pages, no PDF table extraction. Not a full SEC backfill.

## 요청 item_id 정산
| item_id | 요청 항목 | 결과 | document_id 또는 catalog 필드 | accession / filing_date | 비고 |
|---|---|---|---|---|---|
| APP-S03-REQ-20260613-001 | Latest APP Form 10-K | collected | sec:0001751008:0001751008-26-000010:10-k | 0001751008-26-000010 / 2026-02-19 | primary HTML 및 filing metadata raw 저장 |
| APP-S03-REQ-20260613-002 | Latest APP Form 10-Q | collected | sec:0001751008:0001751008-26-000044:10-q | 0001751008-26-000044 / 2026-05-06 | primary HTML 및 filing metadata raw 저장 |
| APP-S03-REQ-20260613-003 | SEC entity/sector metadata | collected | entities.sector | SEC submissions API checked 2026-06-14 | sector: Services-Computer Programming, Data Processing, Etc.; sic=7370 |

## 결과
| 티커 | 상태 | 문서 원장 | 파일 원장 | raw 파일 | derived/text | Transcript | IR 자료 | QA |
|---|---|---:|---:|---:|---:|---|---|---|
| APP | 성공 | 2 SEC 신규 + entity metadata 갱신 | 2 SEC primary 신규 | 4 SEC raw files | 0 | out_of_scope | out_of_scope | pass |

## Incremental Sync 카운트
| 티커 | collected_new | skipped_existing | repair_required | failed | files_collected_new |
|---|---:|---:|---:|---:|---:|
| APP | 2 | 0 | 0 | 0 | 2 |

## SEC 후보 확인
| item_id | form | accession | filing_date | period_end | primary | 범위 판정 |
|---|---|---|---|---|---|---|
| APP-S03-REQ-20260613-001 | 10-K | 0001751008-26-000010 | 2026-02-19 | 2025-12-31 | app-20251231.htm | 이번 run_scope에 포함 |
| APP-S03-REQ-20260613-002 | 10-Q | 0001751008-26-000044 | 2026-05-06 | 2026-03-31 | app-20260331.htm | 이번 run_scope에 포함 |

## 주요 산출물
| 티커 | 파일 | 경로 |
|---|---|---|
| APP | latest 10-K primary HTML | artifacts/raw/sec-edgar/cik-0001751008/accession-0001751008-26-000010/primary.html |
| APP | latest 10-K filing metadata | artifacts/raw/sec-edgar/cik-0001751008/accession-0001751008-26-000010/metadata.json |
| APP | latest 10-Q primary HTML | artifacts/raw/sec-edgar/cik-0001751008/accession-0001751008-26-000044/primary.html |
| APP | latest 10-Q filing metadata | artifacts/raw/sec-edgar/cik-0001751008/accession-0001751008-26-000044/metadata.json |
| APP | download log | artifacts/runs/run-20260614-app-sec-sector-topup/download-log.jsonl |
| APP | QA | artifacts/runs/run-20260614-app-sec-sector-topup/qa.md |

## 실패와 확인 필요
| 티커 | 항목 | 상태 | 이유 | 권장 조치 |
|---|---|---|---|---|
| APP | product-level official docs/pages | deferred | Phase 3 company-official 최소 규칙과 Phase 4 page_key/item_id preflight 전까지 수집하지 않음 | Phase 2-5에서 별도 진행 |
| APP | DEF 14A / 8-K backlog / transcript / PDF table extraction | out_of_scope | 이번 Phase 1 run_scope에서 명시 제외 | 별도 승인 전 수집하지 않음 |
| APP | full SEC backfill | out_of_scope | 이번 run은 latest 10-K/10-Q + sector metadata top-up이며 full collection이 아님 | 필요 시 별도 full collection 승인 |

## 다음 하네스 입력
| 티커 | 입력 파일 | 사용 조건 |
|---|---|---|
| APP | artifacts/companies/APP/index.md | Phase 1 SEC/sector top-up 결과와 남은 product pages deferred 상태를 함께 확인 |
| APP | sec:0001751008:0001751008-26-000010:10-k | APP-S03-REQ-20260613-001, latest 10-K pinned at retrieval |
| APP | sec:0001751008:0001751008-26-000044:10-q | APP-S03-REQ-20260613-002, latest 10-Q pinned at retrieval |
| APP | artifacts/catalog/entities.jsonl | APP-S03-REQ-20260613-003, SEC sector metadata resolved |

## 다음 단계
- Phase 2: `company-official` 최소 반영 계획 작성.
- Phase 3 전까지 product-level official pages는 운영 catalog에 넣지 않는다.
- 이번 run 결과를 APP full Source Pack collection 완료로 표시하지 않는다.

## 선택 섹션: 하네스 운영 관찰
- instructions_files_consulted: 11
- instructions_lines_consulted_estimate: approximate
- bottleneck_note: Phase 1 itself was small; most work was preflight and strict catalog/index consistency.
- trim_candidate: SEC-only latest filing top-up could be scripted after company-official Phase 5 pilot is complete.
