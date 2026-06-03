# Source Pack 실행 요약

## 실행 정보
- 실행 모드: test_collection
- 대상 티커: AAPL
- run-id: run-20260603-aapl-sec-full-scope-test-v2
- 설정 파일: config.md
- 회사별 index: artifacts/companies/AAPL/index.md
- catalog 원장: artifacts/catalog/*.jsonl
- raw 저장: 신규 다운로드 없음
- derived/text 생성: 미실행
- run_scope: test only: rerun AAPL SEC Tier 1 full-range scope; verify skipped_existing for 10-K 10 years, 10-Q 12 quarters, DEF 14A 5 years, 8-K Item 2.02 12 quarters and existing EX-99.1 exhibits; no downloads, no IR, no transcript, no derived text

## 결과
| 티커 | 상태 | 문서 원장 | 파일 원장 | raw 파일 | derived/text | Transcript | IR 자료 | QA |
|---|---|---:|---:|---:|---:|---|---|---|
| AAPL | success | 39 | 51 | 51 | 0 | 제외 | 제외 | pass |

## Incremental Sync 카운트
| 티커 | collected_new | skipped_existing | repair_required | failed | files_collected_new |
|---|---:|---:|---:|---:|---:|
| AAPL | 0 | 39 | 0 | 0 | 0 |

## SEC 범위 카운트
| 범위 | 요청 | 기존 재사용 | 비고 |
|---|---:|---:|---|
| 10-K primary | 10 | 10 | config 기준 10년 |
| 10-Q primary | 12 | 12 | config 기준 12분기 |
| DEF 14A primary | 5 | 5 | config 기준 5년 |
| 8-K Item 2.02 primary | 12 | 12 | config 기준 12분기 |
| 8-K Item 2.02 EX-99.1 exhibit | 12 | 12 | 기존 exhibit fast path |

## 주요 산출물
| 티커 | 파일 | 경로 |
|---|---|---|
| AAPL | run summary | artifacts/runs/run-20260603-aapl-sec-full-scope-test-v2/run-summary.md |
| AAPL | QA | artifacts/runs/run-20260603-aapl-sec-full-scope-test-v2/qa.md |
| AAPL | download log | artifacts/runs/run-20260603-aapl-sec-full-scope-test-v2/download-log.jsonl |
| AAPL | runs catalog | artifacts/catalog/runs.jsonl |

## 실패와 확인 필요
| 티커 | 항목 | 상태 | 이유 | 권장 조치 |
|---|---|---|---|---|
| AAPL | idempotency | 정상 | 모든 후보가 skipped_existing fast path 통과 | 추가 조치 없음 |
| AAPL | SEC raw preflight | not_applicable | 새 raw 다운로드 후보가 없어 preflight 미실행 | 정상 |
| AAPL | IR / Transcript / derived text | 제외 | 이번 SEC idempotency test_collection 범위 밖 | 후속 별도 테스트에서 확인 |

## 다음 하네스 입력
| 티커 | 입력 파일 | 사용 조건 |
|---|---|---|
| AAPL | artifacts/catalog/documents.jsonl | ticker=AAPL, collection_status=collected 기준 |
| AAPL | artifacts/catalog/files.jsonl | file_status=available, local_path 존재 기준 |
| AAPL | artifacts/companies/AAPL/index.md | 사람용 지도, catalog 우선 원칙 유지 |

## 다음 단계
- SEC idempotency는 통과했다. 이후 IR 자료 또는 transcript optional 범위를 별도로 테스트한다.
