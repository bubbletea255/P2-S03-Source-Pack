# Source Pack 실행 요약

## 실행 정보
- 실행 모드: test_collection
- 대상 티커: AAPL
- run-id: run-20260603-aapl-8k-exhibit-test-v2
- run_scope: test only: rerun latest AAPL 8-K Item 2.02 EX-99.1 exhibit scope; verify skipped_existing for existing primary and exhibit; no downloads, no additional SEC filings, no IR, no transcript, no derived text
- 설정 파일: config.md
- 회사별 index: artifacts/companies/AAPL/index.md
- catalog 원장: artifacts/catalog/*.jsonl
- raw 저장: no new downloads
- derived/text 생성: no

## 결과
| 티커 | 상태 | 문서 원장 | 파일 원장 | raw 파일 | derived/text | Transcript | IR 자료 | QA |
|---|---|---:|---:|---:|---:|---|---|---|
| AAPL | 성공 | 4 | 5 | 기존 보유 | 0 | 제외 | 제외 | 성공 |

## Incremental Sync 카운트
| 티커 | collected_new | skipped_existing | repair_required | failed | files_collected_new |
|---|---:|---:|---:|---:|---:|
| AAPL | 0 | 1 | 0 | 0 | 0 |

## 주요 산출물
| 티커 | 파일 | 경로 |
|---|---|---|
| AAPL | 회사별 index | artifacts/companies/AAPL/index.md |
| AAPL | download log | artifacts/runs/run-20260603-aapl-8k-exhibit-test-v2/download-log.jsonl |
| AAPL | QA | artifacts/runs/run-20260603-aapl-8k-exhibit-test-v2/qa.md |

## 실패와 확인 필요
| 티커 | 항목 | 상태 | 이유 | 권장 조치 |
|---|---|---|---|---|
| AAPL | 전체 Source Pack 범위 | 확인 필요 | test_collection은 최신 8-K Item 2.02 EX-99.1 exhibit 재사용만 검증 | 전체 수집은 별도 new_collection으로 실행 |
| AAPL | skipped_existing 검증 | 성공 | primary와 EX-99.1 exhibit 모두 fast path 통과 | 다음 단계에서 repair_required 테스트 가능 |

## 다음 하네스 입력
| 티커 | 입력 파일 | 사용 조건 |
|---|---|---|
| AAPL | artifacts/catalog/documents.jsonl | run_scope가 테스트 범위임을 확인 |
| AAPL | artifacts/catalog/files.jsonl | file_role=exhibit, file_status=available 확인 |
| AAPL | artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-26-000011/exhibits/99.1_a8-kex991q2202603282026.htm | 최신 8-K Item 2.02 EX-99.1 원문 확인 용도 |

## 다음 단계
- repair_required 테스트
- 문제가 없으면 AAPL 전체 SEC 범위로 확장