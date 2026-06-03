# Source Pack 실행 요약

## 실행 정보
- 실행 모드: test_collection
- 대상 티커: AAPL
- run-id: run-20260603-aapl-8k-exhibit-repair-test
- run_scope: test only: detect missing local file for latest AAPL 8-K Item 2.02 EX-99.1 exhibit; no downloads, no automatic repair, no IR, no transcript, no derived text
- 설정 파일: config.md
- 회사별 index: artifacts/companies/AAPL/index.md
- catalog 원장: artifacts/catalog/*.jsonl
- raw 저장: no new downloads
- derived/text 생성: no

## 결과
| 티커 | 상태 | 문서 원장 | 파일 원장 | raw 파일 | derived/text | Transcript | IR 자료 | QA |
|---|---|---:|---:|---:|---:|---|---|---|
| AAPL | 확인 필요 | 4 | 5 | EX-99.1 missing | 0 | 제외 | 제외 | 부분 성공 |

## Incremental Sync 카운트
| 티커 | collected_new | skipped_existing | repair_required | failed | files_collected_new |
|---|---:|---:|---:|---:|---:|
| AAPL | 0 | 0 | 1 | 0 | 0 |

## 주요 산출물
| 티커 | 파일 | 경로 |
|---|---|---|
| AAPL | download log | artifacts/runs/run-20260603-aapl-8k-exhibit-repair-test/download-log.jsonl |
| AAPL | QA | artifacts/runs/run-20260603-aapl-8k-exhibit-repair-test/qa.md |
| AAPL | 확인 필요 파일 | artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-26-000011/exhibits/99.1_a8-kex991q2202603282026.htm |

## 실패와 확인 필요
| 티커 | 항목 | 상태 | 이유 | 권장 조치 |
|---|---|---|---|---|
| AAPL | EX-99.1 exhibit local_path | repair_required | missing_local_file: catalog files.jsonl에는 available record가 있으나 local_path 파일이 없음 | 숨긴 파일을 원래 이름으로 복구한 뒤 같은 범위를 재확인 |
| AAPL | 자동 재다운로드 | 금지 | 기존 수집 성공 파일의 손상/누락은 사람 확인 후 복구해야 함 | 사람 승인 전 자동 다운로드하지 않음 |
| AAPL | 전체 Source Pack 범위 | 확인 필요 | test_collection은 repair_required 감지 테스트 | 전체 수집은 별도 new_collection으로 실행 |

## 다음 하네스 입력
| 티커 | 입력 파일 | 사용 조건 |
|---|---|---|
| AAPL | artifacts/catalog/documents.jsonl | repair_required 해소 전 해당 EX-99.1 사용 금지 |
| AAPL | artifacts/catalog/files.jsonl | file_role=exhibit local_path 복구 후 사용 |
| AAPL | artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-26-000011/exhibits/99.1_a8-kex991q2202603282026.htm | 현재 missing_local_file 상태 |

## 다음 단계
- 숨긴 EX-99.1 파일을 원래 이름으로 복구
- 같은 EX-99.1 범위를 재실행해 skipped_existing으로 돌아오는지 확인