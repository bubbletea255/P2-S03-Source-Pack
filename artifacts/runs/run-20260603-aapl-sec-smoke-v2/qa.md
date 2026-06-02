# Source Pack QA - AAPL

overall_status: pass
run_id: run-20260603-aapl-sec-smoke-v2
run_mode: test_collection
run_scope: test only: rerun AAPL small SEC scope; verify skipped_existing for latest 10-K, 10-Q, DEF 14A, 8-K Item 2.02 primary filings; no downloads, no exhibits, no IR, no transcript, no derived text
checked_at: 2026-06-03T03:19:31+09:00

## 1단계: run 범위 확인
| 항목 | 결과 | 비고 |
|---|---|---|
| run 폴더 | pass | artifacts/runs/run-20260603-aapl-sec-smoke-v2 |
| runs.jsonl record | pass | run_id=run-20260603-aapl-sec-smoke-v2 |
| run_mode 허용값 | pass | test_collection |
| run_scope | pass | test only: rerun AAPL small SEC scope; verify skipped_existing for latest 10-K, 10-Q, DEF 14A, 8-K Item 2.02 primary filings; no downloads, no exhibits, no IR, no transcript, no derived text |

## 2단계: catalog JSONL 구조 검증
| 파일 | 결과 | 비고 |
|---|---|---|
| entities.jsonl | pass | 기존 record 사용 |
| documents.jsonl | pass | 대상 문서 4건 |
| files.jsonl | pass | 대상 file record 4건 |
| runs.jsonl | pass | run record 추가 |

## 3단계: schema 허용값 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| document_type | pass | 10-K, 10-Q, DEF 14A, 8-K |
| collection_status | pass | collected |
| file_status | pass | available |
| run_mode | pass | test_collection |
| attempt_status | pass | download-log 비어 있음, 실제 I/O 시도 없음 |

## 4단계: entity 관계 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| entity_id | pass | sec-cik-0000320193 |
| ticker-CIK | pass | AAPL / 0000320193 |
| document 연결 | pass | documents.entity_id matches |
| file 연결 | pass | files.entity_id matches |

## 5단계: document 원장 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| 선언 범위 | pass | 기존 10-K, 10-Q, DEF 14A, 8-K Item 2.02 primary 4건 재확인 |
| fast path | pass | collected + primary_file_id + file record + local_path + size > 0 |
| Tier 1 전체 범위 누락 | pass | test_collection run_scope 밖이므로 실패 사유 아님 |

## 6단계: file 원장과 raw 파일 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| 10-K | pass | exists=True, size=1520208, local_path=artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-25-000079/primary.html |
| 10-Q | pass | exists=True, size=999810, local_path=artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-26-000013/primary.html |
| DEF 14A | pass | exists=True, size=1248425, local_path=artifacts/raw/sec-edgar/cik-0000320193/accession-0001308179-26-000008/primary.html |
| 8-K | pass | exists=True, size=37639, local_path=artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-26-000011/primary.html |

## 7단계: download-log와 승격 관계 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| download-log attempt | pass | 0건, 실제 다운로드 없음 |
| skipped_existing | pass | 4건을 run-summary 카운트로 기록 |
| 신규 승격 | pass | 없음 |

## 8단계: SEC 실적 발표 자료 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| 8-K Item 2.02 primary | pass | 기존 수집 파일 fast path 통과 |
| EX-99.1 exhibit | skipped_out_of_scope | 이번 run은 primary document만 검증 |

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
| documents.text_status | pass | not_applicable |

## 12단계: 회사별 index.md 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| index 경로 | pass | artifacts/companies/AAPL/index.md |
| 다음 하네스 입력 조건 | pass | 기존 catalog/index 유지 |

## 13단계: 금지 내용 검증
| 항목 | 결과 | 비고 |
|---|---|---|
| 투자 판단/valuation | pass | 작성되지 않음 |
| 원자료 해석/요약/번역 | pass | 작성되지 않음 |
| 유료벽/로그인/봇 차단 우회 | pass | 시도하지 않음 |

## 14단계: QA 결과 작성
| 항목 | 결과 | 비고 |
|---|---|---|
| overall_status | pass | 선언된 skipped_existing 테스트 범위 기준 |
| skipped 단계 명시 | pass | 범위 밖 단계는 skipped_out_of_scope로 기록 |
| 전체 완료 오해 방지 | pass | 결론에 명시 |

## 결론
동일 AAPL SEC 소규모 범위 4건이 모두 skipped_existing으로 처리됐다.
이번 run에서는 신규 다운로드가 발생하지 않았다.
이 결과는 AAPL 전체 Source Pack 완료를 뜻하지 않는다.
