# Source Pack 실행 요약

## 실행 정보
- 실행 모드: test_collection
- 대상 티커: AAPL
- run-id: run-20260603-aapl-test
- run_scope: test only: latest AAPL 10-K primary SEC filing, no exhibits, no IR, no transcript
- 설정 파일: config.md
- 회사별 index: artifacts/companies/AAPL/index.md
- catalog 원장: artifacts/catalog/*.jsonl
- raw 저장: yes, artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-25-000079/primary.html
- derived/text 생성: no

## 결과
| 티커 | 상태 | 문서 원장 | 파일 원장 | raw 파일 | derived/text | Transcript | IR 자료 | QA |
|---|---|---:|---:|---:|---:|---|---|---|
| AAPL | 성공 | 1 | 1 | 1 | 0 | 제외 | 제외 | 성공 |

## 주요 산출물
| 티커 | 파일 | 경로 |
|---|---|---|
| AAPL | 회사별 index | artifacts/companies/AAPL/index.md |
| AAPL | SEC 10-K primary HTML | artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-25-000079/primary.html |
| AAPL | download log | artifacts/runs/run-20260603-aapl-test/download-log.jsonl |
| AAPL | QA | artifacts/runs/run-20260603-aapl-test/qa.md |

## 실패와 확인 필요
| 티커 | 항목 | 상태 | 이유 | 권장 조치 |
|---|---|---|---|---|
| AAPL | 전체 Source Pack 범위 | 확인 필요 | test_collection은 최신 10-K primary 1건만 검증 | 전체 수집은 별도 new_collection으로 실행 |

## 다음 하네스 입력
| 티커 | 입력 파일 | 사용 조건 |
|---|---|---|
| AAPL | artifacts/catalog/documents.jsonl | run_scope가 테스트 범위임을 확인 |
| AAPL | artifacts/catalog/files.jsonl | file_status=available 확인 |
| AAPL | artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-25-000079/primary.html | 최신 10-K primary 원문 확인 용도 |

## 다음 단계
- QA 결과 확인
- 문제가 없으면 전체 수집 범위를 별도 실행으로 확장
