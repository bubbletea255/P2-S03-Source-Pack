# Source Pack 실행 요약

## 실행 정보
- 실행 모드: test_collection
- 대상 티커: AAPL
- run-id: run-20260603-aapl-sec-smoke-v2
- run_scope: test only: rerun AAPL small SEC scope; verify skipped_existing for latest 10-K, 10-Q, DEF 14A, 8-K Item 2.02 primary filings; no downloads, no exhibits, no IR, no transcript, no derived text
- 설정 파일: config.md
- 회사별 index: artifacts/companies/AAPL/index.md
- catalog 원장: artifacts/catalog/*.jsonl
- raw 저장: no new downloads
- derived/text 생성: no

## 결과
| 티커 | 상태 | 문서 원장 | 파일 원장 | raw 파일 | derived/text | Transcript | IR 자료 | QA |
|---|---|---:|---:|---:|---:|---|---|---|
| AAPL | 성공 | 4 | 4 | 기존 보유 | 0 | 제외 | 제외 | 성공 |

## Incremental Sync 카운트
| 티커 | collected_new | skipped_existing | repair_required | failed | files_collected_new |
|---|---:|---:|---:|---:|---:|
| AAPL | 0 | 4 | 0 | 0 | 0 |

## 주요 산출물
| 티커 | 파일 | 경로 |
|---|---|---|
| AAPL | download log | artifacts/runs/run-20260603-aapl-sec-smoke-v2/download-log.jsonl |
| AAPL | QA | artifacts/runs/run-20260603-aapl-sec-smoke-v2/qa.md |

## 실패와 확인 필요
| 티커 | 항목 | 상태 | 이유 | 권장 조치 |
|---|---|---|---|---|
| AAPL | 전체 Source Pack 범위 | 확인 필요 | test_collection은 동일 소규모 범위의 skip 검증 | 전체 수집은 별도 new_collection으로 실행 |
| AAPL | skipped_existing 검증 | 성공 | 4건 모두 fast path 통과 | 다음 단계에서 repair_required 테스트 가능 |

## 다음 하네스 입력
| 티커 | 입력 파일 | 사용 조건 |
|---|---|---|
| AAPL | artifacts/catalog/documents.jsonl | run_scope가 테스트 범위임을 확인 |
| AAPL | artifacts/catalog/files.jsonl | file_status=available 확인 |
| AAPL | artifacts/raw/sec-edgar/.../primary.html | 기존 primary 원문 확인 용도 |

## 다음 단계
- repair_required 테스트
- 8-K exhibit 처리 검증
- 문제가 없으면 AAPL 전체 SEC 범위로 확장
