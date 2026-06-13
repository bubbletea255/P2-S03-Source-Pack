# Source Pack QA - APP Phase 1 SEC/sector top-up

run_id: run-20260614-app-sec-sector-topup  
target: APP  
status: pass  
qa_date: 2026-06-14  
run_scope: APP S04 SEC top-up only - latest 10-K + latest 10-Q accession/date pinned at retrieval, plus SEC entity/sector metadata. No DEF 14A, no 8-K backlog, no transcript, no product pages, no PDF table extraction. Not a full SEC backfill.

## 1단계: run 범위 확인

| 확인 항목 | 결과 | 비고 |
|---|---|---|
| run 폴더 | pass | artifacts/runs/run-20260614-app-sec-sector-topup |
| download-log/run-summary/qa | pass | 세 파일 모두 존재 |
| runs.jsonl record | pass | run_mode=partial_recheck, status=success |
| run_scope | pass | Phase 1 범위와 제외 항목 명시 |

## 2단계: catalog JSONL 구조 검증

| 파일 | 결과 | 비고 |
|---|---|---|
| entities.jsonl | pass | APP entity record 갱신 |
| documents.jsonl | pass | APP 10-K/10-Q SEC records 추가 |
| files.jsonl | pass | APP 10-K/10-Q primary file records 추가 |
| runs.jsonl | pass | run record 추가 |

## 3단계: schema 허용값 검증

| 필드 | 결과 | 비고 |
|---|---|---|
| documents.source_type | pass | sec-edgar |
| documents.document_type | pass | 10-K, 10-Q |
| documents.collection_status | pass | collected |
| documents.text_status | pass | not_applicable |
| files.file_role/file_format/file_status | pass | primary/html/available |
| runs.run_mode/status | pass | partial_recheck/success |
| download-log.attempt_status | pass | success |

## 4단계: entity 관계 검증

| 확인 항목 | 결과 | 비고 |
|---|---|---|
| entity_id | pass | sec-cik-0001751008 |
| CIK | pass | 0001751008 |
| sector metadata | pass | Services-Computer Programming, Data Processing, Etc.; sic=7370 |
| document/file entity 연결 | pass | 모든 신규 record가 sec-cik-0001751008과 연결 |

## 5단계: document 원장 검증

| item_id | document_id | 결과 | 비고 |
|---|---|---|---|
| APP-S03-REQ-20260613-001 | sec:0001751008:0001751008-26-000010:10-k | pass | accession 0001751008-26-000010, filing_date 2026-02-19, primary app-20251231.htm |
| APP-S03-REQ-20260613-002 | sec:0001751008:0001751008-26-000044:10-q | pass | accession 0001751008-26-000044, filing_date 2026-05-06, primary app-20260331.htm |

Run scope 밖의 DEF 14A, 8-K backlog, transcript, product pages, PDF table extraction은 `skipped_out_of_scope`로 판정한다.

## 6단계: file 원장과 raw 파일 검증

| file_id | local_path | size_bytes | hash check | 결과 |
|---|---|---:|---|---|
| sha256:114ab8ef7252fef33b54814923119b50d633669cb97a8b1ebd3d4ffd997a2197 | artifacts/raw/sec-edgar/cik-0001751008/accession-0001751008-26-000010/primary.html | 2009739 | match | pass |
| sha256:38285e1f1d70cb8c83dc96a74e778aeceb38fcdba12e4acefdb61e0f57c24353 | artifacts/raw/sec-edgar/cik-0001751008/accession-0001751008-26-000044/primary.html | 1036582 | match | pass |

Metadata JSON files are stored in each raw_root and listed in download-log, but primary_file_id points to the primary HTML file.

## 7단계: download-log와 승격 관계 검증

| attempt | 대상 | 결과 | 비고 |
|---|---|---|---|
| attempt-001 | 10-K primary HTML | pass | success, promoted to files.jsonl |
| attempt-002 | 10-K metadata JSON | pass | success, raw support file only |
| attempt-003 | 10-Q primary HTML | pass | success, promoted to files.jsonl |
| attempt-004 | 10-Q metadata JSON | pass | success, raw support file only |

## 8단계: SEC 실적 발표 자료 검증

skipped_out_of_scope. 이번 run은 latest 10-K/10-Q + sector metadata만 승인됐고 8-K Item 2.02 또는 EX-99.1 수집은 수행하지 않았다.

## 9단계: IR 자료 검증

skipped_out_of_scope. 이번 run은 IR 재수집 또는 SEC-IR overlap recheck를 포함하지 않았다.

## 10단계: transcript 검증

skipped_out_of_scope. Transcript, webcast, audio, video 수집은 명시 제외됐다.

## 11단계: 회사별 index 검증

| 확인 항목 | 결과 | 비고 |
|---|---|---|
| last_run_id | pass | run-20260614-app-sec-sector-topup |
| sector 확인 필요 해소 | pass | SEC submissions metadata로 resolved |
| 10-K/10-Q 표 | pass | 최신 10-K/10-Q document_id와 raw_root 반영 |
| full collection 오표기 방지 | pass | collection_scope와 확인 필요 항목에 full SEC backfill 아님을 명시 |

## 12단계: run-summary 검증

| 확인 항목 | 결과 | 비고 |
|---|---|---|
| item_id 정산 | pass | APP-S03-REQ-20260613-001~003 매핑 완료 |
| 실패/확인 필요 | pass | product pages deferred, excluded scopes recorded |
| 다음 하네스 입력 | pass | index/catalog/raw 경로 안내 |

## 13단계: 다음 하네스 전달성 검증

| 입력 | 결과 | 비고 |
|---|---|---|
| latest 10-K | pass | raw/catalog/index 사용 가능 |
| latest 10-Q | pass | raw/catalog/index 사용 가능 |
| sector metadata | pass | entities.sector resolved |
| product official pages | deferred | Phase 2-5에서 별도 처리 필요 |

## 14단계: 최종 판정

QA status: pass

이번 Phase 1 범위 안에서는 필수 항목 3개가 모두 처리됐다. 이 결과는 APP full SEC collection 완료가 아니며, product-level official docs/pages 수집 완료도 아니다.
