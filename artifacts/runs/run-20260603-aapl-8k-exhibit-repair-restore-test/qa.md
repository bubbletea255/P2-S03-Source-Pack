# Source Pack QA - AAPL

overall_status: pass
run_id: run-20260603-aapl-8k-exhibit-repair-restore-test
run_mode: test_collection
run_scope: test only: restore latest AAPL 8-K Item 2.02 EX-99.1 exhibit local file and verify skipped_existing; no downloads, no additional SEC filings, no IR, no transcript, no derived text
checked_at: 2026-06-03T11:29:08+09:00

## 1단계: run 범위 확인
| 항목 | 결과 | 비고 |
|---|---|---|
| run 폴더 | pass | artifacts/runs/run-20260603-aapl-8k-exhibit-repair-restore-test |
| runs.jsonl record | pass | run_id=run-20260603-aapl-8k-exhibit-repair-restore-test |
| run_mode 허용값 | pass | test_collection |
| run_scope | pass | EX-99.1 local file 복구 후 fast path 재확인 |

## 2단계: catalog JSONL 구조 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| entities.jsonl | pass | JSONL 파싱 가능 |
| documents.jsonl | pass | JSONL 파싱 가능, 기존 8-K document record 존재 |
| files.jsonl | pass | JSONL 파싱 가능, EX-99.1 exhibit file record 존재 |
| runs.jsonl | pass | JSONL 파싱 가능, run_id=run-20260603-aapl-8k-exhibit-repair-restore-test |
| download-log.jsonl | pass | 0건, 실제 다운로드 없음 |

## 3단계: schema 허용값 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| document_type | pass | 8-K |
| collection_status | pass | collected |
| file_role | pass | primary, exhibit |
| file_format | pass | html |
| file_status | pass | available |
| run_mode | pass | test_collection |
| attempt_status | pass | download-log 비어 있음, 실제 I/O 시도 없음 |

## 4단계: entity 관계 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| entity_id | pass | sec-cik-0000320193 |
| ticker-CIK | pass | AAPL / 0000320193 |
| document 연결 | pass | 8-K document.entity_id matches |
| file 연결 | pass | primary와 EX-99.1 exhibit files.entity_id matches |

## 5단계: document 원장 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| 선언 범위 | pass | 최신 AAPL 8-K Item 2.02 EX-99.1 exhibit 복구 확인 |
| existing document | pass | document_id=sec:0000320193:0000320193-26-000011:8-k |
| primary fast path | pass | 기존 primary.html 존재, size=37639 |
| exhibit relation | pass | files.jsonl exhibit record와 local_path 복구 확인 |
| Tier 1 전체 범위 누락 | pass | test_collection run_scope 밖이므로 실패 사유 아님 |

## 6단계: file 원장과 raw 파일 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| primary file | pass | 기존 primary file record와 local_path 정상 |
| EX-99.1 local_path | pass | artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-26-000011/exhibits/99.1_a8-kex991q2202603282026.htm |
| EX-99.1 size_bytes | pass | 168815 |
| EX-99.1 sha256 | pass | f909dffd7a353847e37c33eb5e666fbf0ffd09b8a6f9535127eb1a2dc0e5e913 |
| EX-99.1 fast path | pass | files.jsonl record + local_path exists + size > 0 + hash matches |

## 7단계: download-log와 승격 관계 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| download-log attempt | pass | 0건, 실제 다운로드 없음 |
| 자동 재다운로드 | pass | 수행하지 않음 |
| collected_new | pass | 0 |
| skipped_existing | pass | 1, 기존 8-K/EX-99.1 범위 재사용 |
| repair_required | pass | 0, local_path 복구 확인 |
| files_collected_new | pass | 0 |
| failed | pass | 0 |

## 8단계: SEC 실적 발표 자료 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| 8-K Item 2.02 primary | pass | 기존 수집 파일 재사용 가능 |
| EX-99.1 exhibit | pass | local_path 복구 후 기존 수집 파일 재사용 가능 |

## 9단계: IR 자료 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| Company IR | skipped_out_of_scope | test_collection run_scope에서 제외 |

## 10단계: Transcript optional 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| Transcript 원문 | skipped_out_of_scope | test_collection run_scope에서 제외 |
| Transcript 우회 금지 | pass | transcript 접근 시도 없음 |

## 11단계: derived text 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| derived/text 생성 | skipped_out_of_scope | test_collection run_scope에서 제외 |
| documents.text_status | pass | not_applicable 유지 |

## 12단계: 회사별 index.md 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| index 경로 | pass | artifacts/companies/AAPL/index.md |
| catalog_status | pass | valid로 복귀 |
| exhibit 목록 | pass | SEC Exhibit 목록에 EX-99.1 raw 경로 유지 |

## 13단계: 금지 내용 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| 투자 판단/valuation | pass | 작성되지 않음 |
| 원자료 해석/요약/번역 | pass | 작성되지 않음 |
| 유료벽/로그인/봇 차단 우회 | pass | 시도하지 않음 |

## 14단계: QA 결과 작성
| 항목 | 결과 | 비고 |
|---|---|---|
| overall_status | pass | 복구 후 EX-99.1 fast path 재확인 기준 |
| skipped 단계 명시 | pass | 범위 밖 단계는 skipped_out_of_scope로 기록 |
| 전체 완료 오해 방지 | pass | run-summary와 index에 명시 |

## 결론
숨겨졌던 AAPL 8-K Item 2.02 EX-99.1 exhibit local_path가 복구됐고, 동일 범위가 skipped_existing으로 정상 처리됐다.
이번 run에서는 신규 다운로드가 발생하지 않았다.
이번 run은 AAPL 전체 Source Pack 완료를 뜻하지 않는다.