# Source Pack QA - NTRA

run_id: run-20260604-ntra-ir-pilot
qa_date: 2026-06-04
overall_status: fail

## 요약
- 통과: 44th Annual J.P. Morgan Healthcare Conference presentation PDF 1건은 raw 파일과 metadata가 존재한다.
- 실패: Q1 2026 earnings press release HTML은 HTTP 403으로 수집 실패했다. Q1 2026 earnings presentation PDF는 download-log에 success로 기록됐으나, 사용자 Bitdefender 알림에 따르면 동일 local_path의 `document.pdf`가 위험 요소로 탐지되어 격리됐다.
- 미검증: 운영 catalog/index 병합은 사용자 승인 전 범위 밖이라 수행하지 않았다.
- 사람 승인 필요: 운영 catalog/index 반영 전 raw 3건 재검증과 보안 격리 사유 분리 필요

## 1단계: run 범위 확인
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| run 폴더 존재 | pass | artifacts/runs/run-20260604-ntra-ir-pilot 존재 | 없음 |
| run_scope 준수 | pass | 승인된 NTRA IR 3건만 대상으로 시도 | 없음 |
| 금지 범위 | pass | SEC 다운로드, SEC filings harvesting, webcast/audio/video, transcript, archive-wide crawling 없음 | 없음 |
| 운영 병합 | skipped_out_of_scope | 사용자 승인 전 catalog/index 병합 금지 | 없음 |

## 2단계: catalog JSONL 구조 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| 운영 catalog 변경 | skipped_out_of_scope | 이번 run은 run-local raw test_collection | 운영 반영 전 별도 승인 필요 |

## 3단계: schema 허용값 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| document_type | pass | ir-earnings-release, ir-deck 사용 | 없음 |
| source_type | pass | company-ir | 없음 |
| candidate_document_type | pass | schema 미추가, notes/summary 후보로만 기록 | 없음 |

## 4단계: entity 관계 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| NTRA entity | informational | metadata에 sec-cik-0001604821 사용 | 운영 catalog 반영 전 entities.jsonl과 재확인 |

## 5단계: document 원장 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| documents.jsonl | skipped_out_of_scope | 운영 catalog 미반영 | 없음 |
| document_id 규칙 | pass | ir-ntra-earnings-release-fy2026-q1, ir-ntra-deck-fy2026-q1, ir-ntra-deck-2026-01-13-jpm-healthcare-conference | 없음 |

## 6단계: file 원장과 raw 파일 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| earnings release HTML raw | fail | artifacts/raw/company-ir/NTRA/2026-05-07_fy2026-q1-earnings-release/document.html 없음 | 재수집 필요 |
| earnings presentation PDF raw | fail | metadata는 있으나 artifacts/raw/company-ir/NTRA/2026-05-07_fy2026-q1-earnings-presentation/document.pdf 없음. 사용자 Bitdefender 알림에서 해당 파일 격리 확인 | 일반 작업공간에서 복구/열람 금지, 보안 검토 후 처리 |
| JPM presentation PDF raw | pass | artifacts/raw/company-ir/NTRA/2026-01-13_jpm-healthcare-conference-presentation/document.pdf 존재, 1719820 bytes | 없음 |

## 7단계: download-log와 승격 관계 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| download-log 존재 | pass | artifacts/runs/run-20260604-ntra-ir-pilot/download-log.jsonl 존재 | 없음 |
| failed attempt 기록 | pass | attempt-001 HTTP 403 기록 | 없음 |
| success attempt 검증 | fail | attempt-002 success 기록 후 Bitdefender가 local_path 파일을 격리한 것으로 확인됨 | download-log는 보존하고, 격리 파일은 후속 catalog/index에 승격하지 않음 |
| success attempt 검증 | pass | attempt-003 success 기록과 JPM raw 파일 존재 | 없음 |

## 8단계: SEC 실적 발표 자료 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| SEC 다운로드 | skipped_out_of_scope | 새 SEC 다운로드 없음 | 없음 |
| SEC filings harvesting | skipped_out_of_scope | 새 SEC filings page harvesting 없음 | 없음 |
| SEC overlap | unverified | earnings-related HTML은 HTTP 403, earnings presentation PDF는 Bitdefender 격리로 사후 raw 검증 불가 | 보안 검토 없이 quarantined PDF를 운영 catalog/index에 반영하지 않음 |

## 9단계: IR 자료 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| 공식 출처 | pass | Natera 공식 IR 및 Q4 CDN 공식 PDF URL 사용 | 없음 |
| 범위 준수 | pass | 승인된 3건 외 대량 수집 없음 | 없음 |
| webcast/audio/video 제외 | pass | 수집하지 않음 | 없음 |

## 10단계: Transcript optional 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| transcript | skipped_out_of_scope | no transcript | 없음 |

## 11단계: derived text 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| derived/text | skipped_out_of_scope | text 추출 미실행 | 필요 시 별도 승인 |

## 12단계: 회사별 index.md 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| NTRA index | skipped_out_of_scope | 운영 index 미반영 | raw 3건 성공 후 승인 받아 반영 |

## 13단계: 금지 내용 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| 투자 분석/요약/번역 | pass | 원자료 내용 해석 없음 | 없음 |
| 접근 우회 | pass | 로그인, 유료벽, 봇 차단 우회 없음 | 없음 |

## 14단계: QA 결과 작성
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| qa.md 작성 | pass | artifacts/runs/run-20260604-ntra-ir-pilot/qa.md 작성 | 없음 |

## Candidate document_type 판단
| 자료 | 현재 document_type | candidate_document_type | 판단 |
|---|---|---|---|
| Q1 2026 earnings press release HTML | ir-earnings-release | 없음 | 기존 schema 유지 |
| Q1 2026 earnings presentation PDF | ir-deck | ir-earnings-presentation | 반복 등장 여부를 더 본 뒤 schema 확장 판단 |
| J.P. Morgan Healthcare Conference presentation PDF | ir-deck | ir-investor-conference-presentation | 반복 등장 여부를 더 본 뒤 schema 확장 판단 |

## 다음 하네스 사용 가능 여부
| 하네스 | 사용 가능 여부 | 이유 |
|---|---|---|
| 후속 하네스 | 불가 | NTRA pilot raw 3건 중 1건은 HTTP 403, 1건은 Bitdefender 격리, 운영 catalog/index 미반영 |

## Rubric Score

total_score: 45
overall_status: fail

| 항목 | 가중치 | 점수(1-5) | 환산점 | 근거 |
|---|---:|---:|---:|---|
| Catalog 구조와 schema 준수 | 20 | 3 | 12 | 운영 catalog 미반영은 범위 밖이나 document_type 사용은 적절 |
| Raw/File 무결성 | 20 | 1 | 4 | 목표 3건 중 1건만 raw 파일 완전 |
| 수집 완전성과 출처 추적성 | 15 | 2 | 6 | 공식 출처는 맞지만 2건 실패/불일치 |
| Run log와 실패 처리 | 15 | 3 | 9 | download-log 존재, attempt-002는 보안 격리로 원인 보정 |
| Index와 다음 하네스 전달성 | 10 | 1 | 2 | 운영 index 미반영, 후속 사용 불가 |
| IR/Transcript optional 처리 | 10 | 5 | 10 | transcript/audio/video 제외 범위 준수 |
| 금지 내용과 모델 중립성 | 10 | 1 | 2 | 분석은 없지만 raw 실패로 후속 전달 불가 |

## Rubric 판정 근거
- 자동 실패 조건: `available`이어야 할 raw 파일이 없고, 그중 1건은 보안 제품에 의해 격리됐다.
- fail 근거: 목표 3건 중 1건은 HTTP 403, 1건은 Bitdefender 격리라 후속 하네스가 사용하면 위험하다.
- 사람 승인 필요: 운영 catalog/index 반영 전 HTML 접근 문제와 Bitdefender 격리 사유를 분리해 처리해야 한다.
