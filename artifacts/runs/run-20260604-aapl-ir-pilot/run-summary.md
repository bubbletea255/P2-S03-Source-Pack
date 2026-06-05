# Source Pack 실행 요약

## 실행 정보
- 실행 모드: test_collection
- 대상 티커: AAPL
- run-id: run-20260604-aapl-ir-pilot
- 설정 파일: config.md
- 회사별 index: artifacts/companies/AAPL/index.md 갱신
- catalog 원장: artifacts/catalog/documents.jsonl, artifacts/catalog/files.jsonl, artifacts/catalog/runs.jsonl 갱신
- raw 저장: artifacts/raw/company-ir/AAPL/
- derived/text 생성: 미실행
- run_scope: test only: AAPL company-ir official Apple FY2026 Q2 earnings press release HTML and consolidated financial statements PDF; no SEC download; no SEC filings page harvesting; no webcast/audio; no transcript; operating catalog/index merge approved after SEC overlap check

## 결과
| 티커 | 상태 | 문서 원장 | 파일 원장 | raw 파일 | derived/text | Transcript | IR 자료 | QA |
|---|---|---:|---:|---:|---:|---|---|---|
| AAPL | 성공 | 2 | 2 | 2 | 0 | out_of_scope | 2/2 collected | pass |

## Incremental Sync 카운트
| 티커 | collected_new | skipped_existing | repair_required | failed | files_collected_new |
|---|---:|---:|---:|---:|---:|
| AAPL | 2 | 0 | 0 | 0 | 2 |

주의: 이번 run은 AAPL IR 2파일 pilot을 운영 catalog/index에 반영하는 제한 범위다. SEC 다운로드, SEC filings page harvesting, webcast/audio, transcript, derived/text 생성은 수행하지 않았다.

## SEC overlap 확인
| IR document_id | SEC overlap | 관련 SEC 후보 | hash 동일 여부 | 처리 |
|---|---|---|---|---|
| ir-aapl-earnings-release-fy2026-q2 | likely | sec:0000320193:0000320193-26-000011:8-k / EX-99.1 | false | company-ir 별도 raw로 catalog 반영 |
| ir-aapl-financial-supplement-fy2026-q2 | likely | sec:0000320193:0000320193-26-000011:8-k / EX-99.1 | false | company-ir 별도 raw로 catalog 반영 |

## 주요 산출물
| 티커 | 파일 | 경로 |
|---|---|---|
| AAPL | company index | artifacts/companies/AAPL/index.md |
| AAPL | documents catalog | artifacts/catalog/documents.jsonl |
| AAPL | files catalog | artifacts/catalog/files.jsonl |
| AAPL | runs catalog | artifacts/catalog/runs.jsonl |
| AAPL | download-log | artifacts/runs/run-20260604-aapl-ir-pilot/download-log.jsonl |
| AAPL | run-summary | artifacts/runs/run-20260604-aapl-ir-pilot/run-summary.md |
| AAPL | qa | artifacts/runs/run-20260604-aapl-ir-pilot/qa.md |
| AAPL | earnings release raw | artifacts/raw/company-ir/AAPL/2026-04-30_fy2026-q2-earnings-release/document.html |
| AAPL | financial supplement raw | artifacts/raw/company-ir/AAPL/2026-04-30_fy2026-q2-financial-supplement/document.pdf |

## 실패와 확인 필요
| 티커 | 항목 | 상태 | 이유 | 권장 조치 |
|---|---|---|---|---|
| AAPL | 초기 transport 시도 | retry_success | Invoke-WebRequest가 2건 모두 Object reference 오류를 냈으나 curl.exe 재시도로 2건 성공 | 최초 실패는 download-log에 보존됨 |
| AAPL | SEC overlap | recorded | 기존 SEC catalog/files/raw 안에서 같은 hash는 없고 같은 실적자료 후보는 있음 | notes에 sec_overlap: likely, canonical_source: sec-edgar 기록 |

## 다음 하네스 입력
| 티커 | 입력 파일 | 사용 조건 |
|---|---|---|
| AAPL | artifacts/catalog/documents.jsonl | IR document_type: ir-earnings-release, ir-financial-supplement |
| AAPL | artifacts/catalog/files.jsonl | company-ir raw 2건의 local_path, hash, size 확인 |
| AAPL | artifacts/companies/AAPL/index.md | 사람용 IR 자료 위치 확인 |

## 하네스 운영 관찰

이 섹션은 하네스 감량과 운영 개선을 위한 선택 메모다.
비어 있거나 누락되어도 QA 실패 또는 run 실패로 보지 않는다.

- instructions_files_consulted: 10
- instructions_lines_consulted_estimate: 대략 2600
- bottleneck_note: pilot raw를 운영 catalog/index에 반영할 때 run-summary, QA, runs.jsonl까지 같이 맞춰야 전체 관계가 안정적이다.
- trim_candidate: IR pilot raw 승격 시 사용할 documents/files/index/run-summary/qa 체크리스트 템플릿.

## 다음 단계
- 다른 회사 IR 1곳을 소규모 pilot으로 테스트해 Apple보다 다양한 IR 자료 유형이 나타나는지 확인한다.
- 반복 등장하는 새 IR 자료 유형은 candidate_document_type으로 기록한 뒤 schema 확장 여부를 결정한다.
