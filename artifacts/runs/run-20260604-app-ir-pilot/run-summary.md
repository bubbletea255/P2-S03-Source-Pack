# Source Pack 실행 요약

## 실행 정보
- 실행 모드: test_collection
- 대상 티커: APP
- run-id: run-20260604-app-ir-pilot
- 설정 파일: config.md
- 회사별 index: artifacts/companies/APP/index.md 생성
- catalog 원장: artifacts/catalog/entities.jsonl, documents.jsonl, files.jsonl, runs.jsonl 갱신
- raw 저장: artifacts/raw/company-ir/APP/
- derived/text 생성: 미실행
- run_scope: test only: APP company-ir official AppLovin Q1 2026 earnings press release HTML and Q1 2026 financial update PDF; no SEC download; no SEC filings page harvesting; no webcast/audio/video; no transcript; no archive-wide crawling; operating catalog/index merge approved after local SEC overlap check

## 결과
| 티커 | 상태 | 문서 원장 | 파일 원장 | raw 파일 | 보안 격리 | derived/text | Transcript | IR 자료 | QA |
|---|---|---:|---:|---:|---:|---:|---|---|---|
| APP | 부분 성공 | 2 | 2 | 2 | 0 | 0 | out_of_scope | 2/2 collected | partial_pass |

## 대상 자료별 결과
| document_id | document_type | 대상 | 상태 | 근거 |
|---|---|---|---|---|
| ir-app-earnings-release-fy2026-q1 | ir-earnings-release | Q1 2026 earnings press release HTML | collected | 운영 documents/files/index 반영, local_path 존재, hash 검증 성공 |
| ir-app-financial-supplement-fy2026-q1 | ir-financial-supplement | Q1 2026 financial update PDF | collected | 운영 documents/files/index 반영, local_path 존재, hash 검증 성공, 보안 격리 관찰 없음 |

## Incremental Sync 카운트
| 티커 | collected_new | skipped_existing | repair_required | failed | security_quarantined | files_collected_new |
|---|---:|---:|---:|---:|---:|---:|
| APP | 2 | 0 | 0 | 0 | 0 | 2 |

주의: 이번 run은 APP IR run-local raw 2건을 사용자 승인 후 운영 catalog/index에 반영한 제한 범위다. SEC 다운로드, SEC filings page harvesting, webcast/audio/video, transcript, archive-wide crawling은 수행하지 않았다.

## SEC overlap 확인
| IR document_id | SEC overlap 상태 | 확인 범위 | hash 동일 여부 | 처리 |
|---|---|---|---|---|
| ir-app-earnings-release-fy2026-q1 | not_found_in_local_catalog | existing_local_catalog_files_raw_only | false | company-ir raw를 운영 catalog에 반영하고 future_recheck_required=true 기록 |
| ir-app-financial-supplement-fy2026-q1 | not_found_in_local_catalog | existing_local_catalog_files_raw_only | false | company-ir raw를 운영 catalog에 반영하고 source_label/handled_as 및 future_recheck_required=true 기록 |

해석 주의:

```text
not_found_in_local_catalog는 "중복 없음 확정"이 아니다.
현재 로컬 catalog/files/raw 안에서 같은 hash 또는 APP SEC 실적자료 후보를 찾지 못했다는 뜻이다.
나중에 APP SEC 8-K/EX-99.1을 수집하면 동일 ticker 범위에서 SEC-IR overlap을 재확인해야 한다.
```

## 자료명과 document_type 판단
| 자료 | 현재 document_type | source_label / handled_as | 판단 |
|---|---|---|---|
| Q1 2026 earnings press release HTML | ir-earnings-release | 없음 | 기존 schema로 충분 |
| Q1 2026 financial update PDF | ir-financial-supplement | source_label: Financial Update; handled_as: ir-financial-supplement | 별도 document_type 후보가 아니라 회사 측 자료명을 기존 type으로 처리 |

## 보안 격리 확인
| 파일 | 상태 | 근거 |
|---|---|---|
| earnings release HTML | not_quarantined_observed | 운영 반영 시 local_path 존재, size/hash 일치 |
| financial update PDF | not_quarantined_observed | 운영 반영 시 local_path 존재, size/hash 일치. NTRA와 같은 격리 현상은 관찰되지 않음 |

## 주요 산출물
| 티커 | 파일 | 경로 |
|---|---|---|
| APP | company index | artifacts/companies/APP/index.md |
| APP | entities catalog | artifacts/catalog/entities.jsonl |
| APP | documents catalog | artifacts/catalog/documents.jsonl |
| APP | files catalog | artifacts/catalog/files.jsonl |
| APP | runs catalog | artifacts/catalog/runs.jsonl |
| APP | download-log | artifacts/runs/run-20260604-app-ir-pilot/download-log.jsonl |
| APP | run-summary | artifacts/runs/run-20260604-app-ir-pilot/run-summary.md |
| APP | qa | artifacts/runs/run-20260604-app-ir-pilot/qa.md |
| APP | earnings release raw | artifacts/raw/company-ir/APP/2026-05-06_fy2026-q1-earnings-release/document.html |
| APP | financial update raw | artifacts/raw/company-ir/APP/2026-05-06_fy2026-q1-financial-update/document.pdf |

## 실패와 확인 필요
| 티커 | 항목 | 상태 | 이유 | 권장 조치 |
|---|---|---|---|---|
| APP | SEC overlap | pending_recheck | 현재 local catalog/files/raw에는 APP SEC 후보가 없음 | APP SEC 수집 시 같은 ticker의 SEC files hash와 재비교 |
| APP | sector metadata | 확인 필요 | SEC collection을 실행하지 않아 sector 미확정 | SEC entity metadata 확인 시 갱신 |

## 다음 하네스 입력
| 티커 | 입력 가능 여부 | 이유 |
|---|---|---|
| APP | 제한적으로 가능 | 운영 catalog/index에 APP IR raw 2건 반영 완료. 단, SEC canonical 자료와 overlap recheck는 APP SEC 수집 후 필요 |

## 하네스 운영 관찰

이 섹션은 하네스 감량과 운영 개선을 위한 선택 메모다.
비어 있거나 누락되어도 QA 실패 또는 run 실패로 보지 않는다.

- instructions_files_consulted: 7
- instructions_lines_consulted_estimate: 대략 2300
- bottleneck_note: APP 운영 반영에서 가장 시간이 걸린 부분은 run-local metadata를 운영 documents/files/runs/index/QA 관계로 일관되게 맞추는 작업이었다.
- trim_candidate: IR pilot raw 승격 체크리스트. documents/files/runs/index/qa를 한 번에 점검하는 작은 검증 도구가 있으면 반복 작업을 줄일 수 있다.

## 다음 단계
- APP SEC 자료를 나중에 수집하면 `not_found_in_local_catalog` 상태였던 IR 2건을 동일 ticker 범위에서 다시 hash 비교한다.
- APP/NTRA/AAPL pilot 결과 기준으로 `ir-financial-update`는 source_label로 정리했으며, `ir-earnings-presentation`, `ir-shareholder-letter` 등은 별도 targeted pilot 후 schema 승격 여부를 논의한다.
