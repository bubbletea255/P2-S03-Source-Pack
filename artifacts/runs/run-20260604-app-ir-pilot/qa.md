# Source Pack QA - APP

run_id: run-20260604-app-ir-pilot
qa_date: 2026-06-05
overall_status: partial_pass

## 요약
- 통과: APP IR raw 2건을 운영 documents/files/runs/index에 반영했고, local_path 존재, size, SHA-256 hash, document-file-entity 관계를 검증했다.
- 실패: final document failures 0건.
- 미검증: APP SEC 자료가 아직 로컬 catalog/raw에 없어 SEC-IR overlap은 `not_found_in_local_catalog`와 `future_recheck_required: true`로 남겼다.
- 사람 승인 필요: 없음. 다음 재확인은 APP SEC 수집 이후 수행한다.

## 1단계: run 범위 확인
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| run 폴더 존재 | pass | artifacts/runs/run-20260604-app-ir-pilot 존재 | 없음 |
| run_scope 준수 | pass | 승인된 APP IR HTML 1건과 PDF 1건만 운영 반영 | 없음 |
| 금지 범위 | pass | SEC 다운로드, SEC filings harvesting, webcast/audio/video, transcript, archive-wide crawling 없음 | 없음 |
| runs.jsonl record | pass | artifacts/catalog/runs.jsonl에 run-20260604-app-ir-pilot 추가 | 없음 |

## 2단계: catalog JSONL 구조 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| JSONL 구조 | pass | entities/documents/files/runs 모두 파싱 가능 | 없음 |
| 빈 줄 | pass | catalog JSONL에 빈 줄 없음 | 없음 |
| APP 신규 record | pass | entity 1건, document 2건, file 2건, run 1건 추가 | 없음 |

## 3단계: schema 허용값 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| document_type | pass | ir-earnings-release, ir-financial-supplement 사용 | 없음 |
| source_type | pass | company-ir | 없음 |
| file_role/file_format/file_status | pass | primary/html/pdf/available 모두 schema 허용값 | 없음 |
| run_mode/status | pass | test_collection/partial_success 모두 schema 허용값 | 없음 |
| source_label/handled_as | pass | `Financial Update`는 source_label로, 실제 document_type은 `ir-financial-supplement`로 기록 | 없음 |

## 4단계: entity 관계 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| APP entity | pass | entity_id=sec-cik-0001751008, cik=0001751008 추가 | 없음 |
| documents/files entity 연결 | pass | APP document/file 2건 모두 sec-cik-0001751008과 연결 | 없음 |
| sector metadata | partial | SEC collection 미실행으로 sector는 null 및 index 확인 필요로 기록 | APP SEC metadata 확인 시 갱신 |

## 5단계: document 원장 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| IR document_id | pass | ir-app-earnings-release-fy2026-q1, ir-app-financial-supplement-fy2026-q1 | 없음 |
| collected primary_file_id | pass | collected 2건 모두 primary_file_id가 files.jsonl file_id와 연결 | 없음 |
| SEC overlap notes | pass | 두 document notes에 not_found_in_local_catalog, exact_hash_match=false, future_recheck_required=true 기록 | APP SEC 수집 후 재확인 |
| financial update source label | pass | financial update PDF notes에 source_label: Financial Update; handled_as: ir-financial-supplement 기록 | 없음 |

## 6단계: file 원장과 raw 파일 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| earnings release HTML raw | pass | artifacts/raw/company-ir/APP/2026-05-06_fy2026-q1-earnings-release/document.html 존재, 256924 bytes | 없음 |
| earnings release HTML hash | pass | sha256 700b8775dac1e211ff10b27feaf4d5511df87badecffefcac330ebf2421f4e9e 일치 | 없음 |
| financial update PDF raw | pass | artifacts/raw/company-ir/APP/2026-05-06_fy2026-q1-financial-update/document.pdf 존재, 807036 bytes | 없음 |
| financial update PDF hash | pass | sha256 bf35ed7b995a09bc999f251dcb2b885d8338435c2dc7e8ec5302379ece4f0ce8 일치 | 없음 |
| file_id 관계 | pass | file_id가 sha256:{hash} 형식이고 documents.primary_file_id와 대응 | 없음 |

## 7단계: download-log와 승격 관계 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| download-log 존재 | pass | artifacts/runs/run-20260604-app-ir-pilot/download-log.jsonl 존재 | 없음 |
| attempt-001 | pass | HTTP 200, success, local_path 존재, files.jsonl 승격 | 없음 |
| attempt-002 | pass | HTTP 200, success, local_path 존재, files.jsonl 승격 | 없음 |
| 실패 attempt | pass | 실패 attempt 없음 | 없음 |

## 8단계: SEC 실적 발표 자료 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| SEC 다운로드 | skipped_out_of_scope | 새 SEC 다운로드 없음 | 없음 |
| SEC filings harvesting | skipped_out_of_scope | 새 SEC filings page harvesting 없음 | 없음 |
| SEC overlap | partial | existing_local_catalog_files_raw_only 범위에서 APP SEC 후보 없음. 중복 없음 확정은 아님 | APP SEC 수집 후 동일 ticker 범위에서 재비교 |

## 9단계: IR 자료 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| 공식 출처 | pass | AppLovin 공식 IR HTML과 공식 Q4 CDN PDF 사용 | 없음 |
| 범위 준수 | pass | 승인된 2건 외 대량 수집 없음 | 없음 |
| webcast/audio/video 제외 | pass | 수집하지 않음 | 없음 |
| Bitdefender 격리 | pass | 다운로드 후 local_path 존재와 hash 검증 완료, 격리 관찰 없음 | 없음 |

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
| APP index | pass | artifacts/companies/APP/index.md 생성 | 없음 |
| 필수 섹션 | pass | schema 주요 섹션 포함 | 없음 |
| catalog 참조 | pass | entity/documents/files/runs 경로와 조회 기준 명시 | 없음 |
| 확인 필요 표시 | pass | SEC overlap recheck와 sector metadata 확인 필요 기록 | 없음 |

## 13단계: 금지 내용 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| 투자 분석/요약/번역 | pass | 원자료 내용 해석 없음 | 없음 |
| 접근 우회 | pass | 로그인, 유료벽, 봇 차단 우회 없음 | 없음 |
| source-pack-collector.md | pass | SEC collector 수정 없음 | 없음 |
| NTRA 격리 파일 | pass | 복구/열람/승격하지 않음 | 없음 |

## 14단계: QA 결과 작성
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| qa.md 작성 | pass | artifacts/runs/run-20260604-app-ir-pilot/qa.md 갱신 | 없음 |
| overall_status | pass | SEC overlap future recheck가 남아 있어 보수적으로 partial_pass | APP SEC 수집 후 재검토 가능 |

## 자료명과 document_type 판단
| 자료 | 현재 document_type | source_label / handled_as | 판단 |
|---|---|---|---|
| Q1 2026 earnings press release HTML | ir-earnings-release | 없음 | 기존 schema 유지 |
| Q1 2026 financial update PDF | ir-financial-supplement | source_label: Financial Update; handled_as: ir-financial-supplement | 별도 document_type 후보가 아니라 회사 측 자료명을 기존 type으로 처리 |

## 다음 하네스 사용 가능 여부
| 하네스 | 사용 가능 여부 | 읽을 자료 | 주의 |
|---|---|---|---|
| 후속 하네스 | 제한적으로 가능 | artifacts/companies/APP/index.md, documents/files catalog, APP company-ir raw 2건 | APP SEC canonical 자료는 아직 없음. future_recheck_required notes 확인 |

## Rubric Score

total_score: 88
overall_status: partial_pass

| 항목 | 가중치 | 점수(1-5) | 환산점 | 근거 |
|---|---:|---:|---:|---|
| Catalog 구조와 schema 준수 | 20 | 5 | 20 | APP entity/document/file/run 관계 생성 및 JSONL 검증 |
| Raw/File 무결성 | 20 | 5 | 20 | 목표 2건 모두 raw 파일 존재, size/hash 일치 |
| 수집 완전성과 출처 추적성 | 15 | 5 | 15 | 공식 출처 2건만 반영 |
| Run log와 실패 처리 | 15 | 5 | 15 | download-log와 run-summary/QA 갱신 |
| Index와 다음 하네스 전달성 | 10 | 4 | 8 | APP index 생성. SEC overlap future recheck 주의 필요 |
| IR/Transcript optional 처리 | 10 | 5 | 10 | webcast/audio/video/transcript 제외 범위 준수 |
| 금지 내용과 모델 중립성 | 10 | 5 | 10 | 분석/요약/번역/우회 없음 |

## Rubric 판정 근거
- 자동 실패 조건: 없음
- partial_pass 근거: APP IR raw 2건은 운영 반영됐지만, local catalog에 APP SEC 자료가 없어 SEC-IR overlap은 future recheck로 남는다.
- 다음 조치: APP SEC 수집이 수행되면 동일 ticker 범위에서 SEC file hash와 APP IR file hash를 재비교한다.
