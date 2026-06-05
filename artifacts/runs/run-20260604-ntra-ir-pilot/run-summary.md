# Source Pack 실행 요약

## 실행 정보
- 실행 모드: test_collection
- 대상 티커: NTRA
- run-id: run-20260604-ntra-ir-pilot
- 설정 파일: config.md
- 회사별 index: 운영 index 미반영
- catalog 원장: 운영 catalog 미반영
- raw 저장: artifacts/raw/company-ir/NTRA/
- derived/text 생성: 미실행
- run_scope: test only: NTRA company-ir official Natera IR Q1 2026 earnings press release HTML, Q1 2026 earnings presentation PDF, and 44th Annual J.P. Morgan Healthcare Conference presentation PDF; no SEC download; no SEC filings page harvesting; no webcast/audio/video; no transcript; no archive-wide crawling; no operating catalog/index merge until user approval

## 결과
| 티커 | 상태 | 목표 raw | 완전 수집 | 실패/불일치 | derived/text | Transcript | IR 자료 | QA |
|---|---|---:|---:|---:|---:|---|---|---|
| NTRA | 부분 실패 | 3 | 1 | 2 | 0 | out_of_scope | 1/3 collected | fail |

## 대상 자료별 결과
| document_id | document_type | 대상 | 상태 | 근거 |
|---|---|---|---|---|
| ir-ntra-earnings-release-fy2026-q1 | ir-earnings-release | Q1 2026 earnings press release HTML | failed | download-log attempt-001: HTTP 403 |
| ir-ntra-deck-fy2026-q1 | ir-deck | Q1 2026 earnings presentation PDF | downloaded_then_quarantined_by_bitdefender | download-log attempt-002는 success였고, 사용자 Bitdefender 알림에서 동일 local_path의 document.pdf 격리 확인 |
| ir-ntra-deck-2026-01-13-jpm-healthcare-conference | ir-deck | 44th Annual J.P. Morgan Healthcare Conference presentation PDF | collected | raw PDF와 metadata 존재 |

## Incremental Sync 카운트
| 티커 | collected_new | skipped_existing | repair_required | failed | security_quarantined | files_collected_new |
|---|---:|---:|---:|---:|---:|---:|
| NTRA | 1 | 0 | 1 | 1 | 1 | 1 |

주의: 이번 run은 NTRA IR pilot 2의 run-local test_collection이다. SEC 다운로드, SEC filings page harvesting, webcast/audio/video, transcript, archive-wide crawling, 운영 catalog/index 병합은 수행하지 않았다.

## Candidate document_type 판단
| 자료 | 현재 document_type | candidate_document_type | 판단 |
|---|---|---|---|
| Q1 2026 earnings press release HTML | ir-earnings-release | 없음 | 기존 schema로 충분 |
| Q1 2026 earnings presentation PDF | ir-deck | ir-earnings-presentation | earnings deck이 반복되면 별도 타입 후보로 검토 |
| J.P. Morgan Healthcare Conference presentation PDF | ir-deck | ir-investor-conference-presentation | 컨퍼런스 발표가 반복되면 별도 타입 후보로 검토 |

## SEC overlap 처리
| 자료 | earnings-related | SEC overlap 처리 | 비고 |
|---|---|---|---|
| Q1 2026 earnings press release HTML | true | likely로 볼 수 있으나 raw 실패로 hash 비교 불가 | 새 SEC 다운로드 없음 |
| Q1 2026 earnings presentation PDF | true | likely로 볼 수 있으나 Bitdefender 격리로 사후 raw 검증 불가 | download-log hash는 있으나 local raw 파일 없음, 새 SEC 다운로드 없음 |
| J.P. Morgan Healthcare Conference presentation PDF | false | overlap 검사 비대상 | IR-native 자료 |

## 주요 산출물
| 티커 | 파일 | 경로 |
|---|---|---|
| NTRA | download-log | artifacts/runs/run-20260604-ntra-ir-pilot/download-log.jsonl |
| NTRA | run-summary | artifacts/runs/run-20260604-ntra-ir-pilot/run-summary.md |
| NTRA | qa | artifacts/runs/run-20260604-ntra-ir-pilot/qa.md |
| NTRA | quarantined earnings presentation metadata | artifacts/raw/company-ir/NTRA/2026-05-07_fy2026-q1-earnings-presentation/metadata.json |
| NTRA | JPM presentation raw | artifacts/raw/company-ir/NTRA/2026-01-13_jpm-healthcare-conference-presentation/document.pdf |
| NTRA | JPM presentation metadata | artifacts/raw/company-ir/NTRA/2026-01-13_jpm-healthcare-conference-presentation/metadata.json |

## 실패와 확인 필요
| 티커 | 항목 | 상태 | 이유 | 권장 조치 |
|---|---|---|---|---|
| NTRA | earnings release HTML | failed | Natera IR HTML endpoint가 curl 요청에 HTTP 403 반환 | 운영 반영 전 브라우저형 요청 또는 공식 대체 URL 확인 |
| NTRA | Q1 earnings presentation PDF | security_quarantined | Bitdefender가 `artifacts/raw/company-ir/NTRA/2026-05-07_fy2026-q1-earnings-presentation/document.pdf`를 위험 요소로 탐지해 격리함. 알림상 탐지 ID는 `SuspiciousBehavior.182793FD17B38920` | 일반 작업공간에서 복구/열람하지 말고, 필요 시 격리 상태 그대로 보존한 뒤 별도 보안 검토 또는 샌드박스에서만 확인 |
| NTRA | 운영 catalog/index | skipped_out_of_scope | 사용자 승인 전 병합 금지 | raw 3건 성공 후 별도 승인 요청 |

## 다음 하네스 입력
| 티커 | 입력 가능 여부 | 이유 |
|---|---|---|
| NTRA | 아직 불가 | 3건 중 2건 실패/불일치이며 운영 catalog/index 미반영 |

## 다음 단계
- NTRA pilot 2는 분류체계 판단에는 유효하지만, raw 수집 결과는 실패로 본다.
- 다음에는 동일 run_scope를 무작정 재시도하지 말고, 먼저 HTML 403과 Bitdefender 격리 원인을 분리해서 처리한다.
- Q1 earnings presentation PDF는 일반 작업공간에서 복구하거나 열지 않는다. 필요한 경우 공식 출처 재확인, 보안 제품 로그 보존, 격리 환경 검토를 먼저 한다.
- 새 document_type은 아직 추가하지 않고, `ir-earnings-presentation`, `ir-investor-conference-presentation`를 candidate로만 유지한다.
