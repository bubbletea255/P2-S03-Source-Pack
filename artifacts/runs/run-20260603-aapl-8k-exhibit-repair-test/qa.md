# Source Pack QA - AAPL

overall_status: partial_pass
run_id: run-20260603-aapl-8k-exhibit-repair-test
run_mode: test_collection
run_scope: test only: detect missing local file for latest AAPL 8-K Item 2.02 EX-99.1 exhibit; no downloads, no automatic repair, no IR, no transcript, no derived text
checked_at: 2026-06-03T11:05:45+09:00

## 1단계: run 범위 확인
| 항목 | 결과 | 비고 |
|---|---|---|
| run 폴더 | pass | artifacts/runs/run-20260603-aapl-8k-exhibit-repair-test |
| runs.jsonl record | pass | run_id=run-20260603-aapl-8k-exhibit-repair-test |
| run_mode 허용값 | pass | test_collection |
| run_scope | pass | missing_local_file repair_required 감지 테스트 |

## 2단계: catalog JSONL 구조 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| entities.jsonl | pass | JSONL 파싱 가능 |
| documents.jsonl | pass | JSONL 파싱 가능, 기존 8-K document record 존재 |
| files.jsonl | pass | JSONL 파싱 가능, EX-99.1 exhibit file record 존재 |
| runs.jsonl | pass | JSONL 파싱 가능, run_id=run-20260603-aapl-8k-exhibit-repair-test |
| download-log.jsonl | pass | 0건, 실제 다운로드 없음 |

## 3단계: schema 허용값 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| document_type | pass | 8-K |
| collection_status | pass | collected |
| file_role | pass | primary, exhibit |
| file_status | pass | catalog record는 available이나 local_path missing으로 repair_required |
| run_mode | pass | test_collection |
| attempt_status | pass | download-log 비어 있음, 실제 I/O 시도 없음 |

## 4단계: entity 관계 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| entity_id | pass | sec-cik-0000320193 |
| ticker-CIK | pass | AAPL / 0000320193 |
| document 연결 | pass | 8-K document.entity_id matches |
| file 연결 | pass | EX-99.1 exhibit files.entity_id matches |

## 5단계: document 원장 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| 선언 범위 | pass | 최신 AAPL 8-K Item 2.02 EX-99.1 exhibit missing_local_file 감지 |
| existing document | pass | document_id=sec:0000320193:0000320193-26-000011:8-k |
| primary fast path | pass | 기존 primary.html 존재, size=37639 |
| exhibit relation | partial_pass | 기존 files.jsonl exhibit record는 있으나 local_path 파일이 없음 |
| Tier 1 전체 범위 누락 | pass | test_collection run_scope 밖이므로 실패 사유 아님 |

## 6단계: file 원장과 raw 파일 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| primary file | pass | 기존 primary file record와 local_path 정상 |
| EX-99.1 local_path | repair_required | missing_local_file: artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-26-000011/exhibits/99.1_a8-kex991q2202603282026.htm |
| EX-99.1 hidden file marker | pass | hiddenExists=True, 삭제가 아니라 임시 숨김 상태로 확인 |
| EX-99.1 catalog record | pass | file_role=exhibit, sha256=f909dffd7a353847e37c33eb5e666fbf0ffd09b8a6f9535127eb1a2dc0e5e913 |
| 자동 hash 재검증 | skipped_out_of_scope | local_path가 없어 hash 재계산 불가, 자동 재다운로드 금지 |

## 7단계: download-log와 승격 관계 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| download-log attempt | pass | 0건, 실제 다운로드 없음 |
| 자동 재다운로드 | pass | 수행하지 않음 |
| collected_new | pass | 0 |
| skipped_existing | pass | 0, run_scope의 EX-99.1 fast path가 실패했으므로 skip 처리하지 않음 |
| repair_required | pass | 1, missing_local_file |
| files_collected_new | pass | 0 |
| failed | pass | 0, 실패가 아니라 사람 확인 필요 상태 |

## 8단계: SEC 실적 발표 자료 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| 8-K Item 2.02 primary | pass | 기존 수집 파일 재사용 가능 |
| EX-99.1 exhibit | repair_required | catalog record는 있으나 raw local_path가 missing |

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
| 확인 필요 표시 | pass | repair_required와 missing_local_file 기록 |

## 13단계: 금지 내용 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| 투자 판단/valuation | pass | 작성되지 않음 |
| 원자료 해석/요약/번역 | pass | 작성되지 않음 |
| 유료벽/로그인/봇 차단 우회 | pass | 시도하지 않음 |

## 14단계: QA 결과 작성
| 항목 | 결과 | 비고 |
|---|---|---|
| overall_status | partial_pass | repair_required 감지는 성공했으나 해당 EX-99.1은 복구 전 사용 금지 |
| skipped 단계 명시 | pass | 범위 밖 단계는 skipped_out_of_scope로 기록 |
| 전체 완료 오해 방지 | pass | run-summary와 index에 명시 |

## 결론
숨겨진 AAPL 8-K Item 2.02 EX-99.1 exhibit local_path 누락이 missing_local_file repair_required로 감지됐다.
이번 run에서는 자동 재다운로드가 발생하지 않았다.
숨긴 파일을 원래 이름으로 복구한 뒤 같은 범위를 재확인해야 한다.