# Source Pack 실행 요약

## 실행 정보
- 실행 모드: test_collection
- 대상 티커: AAPL
- run-id: run-20260603-aapl-sec-smoke
- run_scope: test only: AAPL latest 10-Q, latest DEF 14A, latest 8-K Item 2.02 primary SEC filings; reuse existing latest 10-K; no exhibits, no IR, no transcript, no derived text
- 설정 파일: config.md
- 회사별 index: artifacts/companies/AAPL/index.md
- catalog 원장: artifacts/catalog/*.jsonl
- raw 저장: yes, 3 primary SEC documents added
- derived/text 생성: no

## 결과
| 티커 | 상태 | 문서 원장 | 파일 원장 | raw 파일 | derived/text | Transcript | IR 자료 | QA |
|---|---|---:|---:|---:|---:|---|---|---|
| AAPL | 성공 | 4 | 4 | 4 | 0 | 제외 | 제외 | 성공 |

## Incremental Sync 카운트
| 티커 | collected_new | skipped_existing | repair_required | failed | files_collected_new |
|---|---:|---:|---:|---:|---:|
| AAPL | 3 | 1 | 0 | 0 | 3 |

## 주요 산출물
| 티커 | 파일 | 경로 |
|---|---|---|
| AAPL | 회사별 index | artifacts/companies/AAPL/index.md |
| AAPL | download log | artifacts/runs/run-20260603-aapl-sec-smoke/download-log.jsonl |
| AAPL | QA | artifacts/runs/run-20260603-aapl-sec-smoke/qa.md |

## 실패와 확인 필요
| 티커 | 항목 | 상태 | 이유 | 권장 조치 |
|---|---|---|---|---|
| AAPL | 전체 Source Pack 범위 | 확인 필요 | test_collection은 주요 SEC 문서 primary 일부만 검증 | 전체 수집은 별도 new_collection으로 실행 |
| AAPL | 8-K exhibits | out_of_scope | 이번 run은 primary document만 다운로드 | exhibit 처리 검증은 별도 확장 테스트에서 수행 |

## 다음 하네스 입력
| 티커 | 입력 파일 | 사용 조건 |
|---|---|---|
| AAPL | artifacts/catalog/documents.jsonl | run_scope가 테스트 범위임을 확인 |
| AAPL | artifacts/catalog/files.jsonl | file_status=available 확인 |
| AAPL | artifacts/raw/sec-edgar/.../primary.html | primary 원문 확인 용도 |

## 다음 단계
- 같은 범위를 다시 실행해 skipped_existing 카운트가 증가하는지 확인
- 8-K exhibit 처리 검증
- 문제가 없으면 AAPL 전체 SEC 범위로 확장
