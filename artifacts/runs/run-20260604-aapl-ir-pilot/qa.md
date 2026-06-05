# Source Pack QA - AAPL

run_id: run-20260604-aapl-ir-pilot
qa_date: 2026-06-04
overall_status: pass

## 요약
- 통과: 승인된 AAPL IR 2파일 pilot raw를 운영 documents/files/index에 반영했고, raw 파일 hash/size/local_path 관계를 검증했다.
- 실패: 0 final document failures. 초기 transport 실패 attempt 2건은 retry_success로 보존했다.
- 미검증: 없음. SEC overlap은 같은 hash 없음, 같은 실적자료 후보 있음으로 notes에 기록했다.
- 사람 승인 필요: 없음

## 1단계: run 범위 확인
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| run 폴더 존재 | pass | artifacts/runs/run-20260604-aapl-ir-pilot 존재 | 없음 |
| run_scope 준수 | pass | Apple Newsroom HTML 1건과 PDF 1건만 운영 반영 | 없음 |
| runs.jsonl record | pass | artifacts/catalog/runs.jsonl에 run-20260604-aapl-ir-pilot 추가 | 없음 |

## 2단계: catalog JSONL 구조 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| JSONL 구조 | pass | documents.jsonl, files.jsonl, runs.jsonl에 한 줄당 JSON object 유지 | 없음 |
| 빈 줄 | pass | 새로 추가한 record에 빈 줄 없음 | 없음 |

## 3단계: schema 허용값 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| IR document_type | pass | ir-earnings-release, ir-financial-supplement는 schema 허용값 | 없음 |
| source_type | pass | company-ir | 없음 |
| file_role/file_format/file_status | pass | primary/html/pdf/available 모두 schema 허용값 | 없음 |
| run_mode/status | pass | test_collection/success 모두 schema 허용값 | 없음 |

## 4단계: entity 관계 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| AAPL entity | pass | entity_id=sec-cik-0000320193, cik=0000320193 사용 | 없음 |
| documents/files entity 연결 | pass | IR 2건 모두 기존 AAPL entity_id와 연결 | 없음 |

## 5단계: document 원장 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| IR document_id | pass | ir-aapl-earnings-release-fy2026-q2, ir-aapl-financial-supplement-fy2026-q2 | 없음 |
| collected primary_file_id | pass | collected 2건 모두 primary_file_id가 files.jsonl file_id와 연결 | 없음 |
| SEC overlap notes | pass | 두 document notes에 sec_overlap: likely, canonical_source: sec-edgar, related SEC 8-K/EX-99.1 기록 | 없음 |

## 6단계: file 원장과 raw 파일 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| raw 파일 존재 | pass | company-ir HTML/PDF 2건 local_path 존재 | 없음 |
| size_bytes | pass | HTML 140373 bytes, PDF 110402 bytes | 없음 |
| sha256 | pass | files.jsonl sha256이 실제 파일 hash와 일치 | 없음 |
| file_id 관계 | pass | file_id가 sha256:{hash} 형식이고 documents.primary_file_id와 대응 | 없음 |

## 7단계: download-log와 승격 관계 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| download-log 기록 | pass | 실패 attempt 2건과 성공 retry 2건 보존 | 없음 |
| 성공 attempt 승격 | pass | attempt-003, attempt-004가 files.jsonl에 승격됨 | 없음 |
| 실패 attempt 미승격 | pass | attempt-001, attempt-002는 files.jsonl에 승격되지 않음 | 없음 |

## 8단계: SEC 실적 발표 자료 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| SEC 다운로드 | skipped_out_of_scope | 이번 반영 작업에서 새 SEC 다운로드 없음 | 없음 |
| SEC filings harvesting | skipped_out_of_scope | 새 SEC filings page harvesting 없음 | 없음 |
| SEC overlap | pass | 기존 SEC 8-K Item 2.02/9.01 및 EX-99.1 후보와 overlap likely 기록, exact_hash_match=false | 없음 |

## 9단계: IR 자료 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| 공식 출처 | pass | Apple Newsroom 공식 HTML/PDF URL | 없음 |
| 범위 준수 | pass | 승인된 2건만 catalog/index 반영 | 없음 |
| webcast/audio 제외 | pass | 수집/반영하지 않음 | 없음 |

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
| index 경로 | pass | artifacts/companies/AAPL/index.md | 없음 |
| IR 섹션 | pass | IR 자료 2건이 document_id와 raw 경로로 기록됨 | 없음 |
| 다음 하네스 전달 요약 | pass | company-ir FY2026 Q2 records 포함 | 없음 |

## 13단계: 금지 내용 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| 투자 분석/요약/번역 | pass | 원자료 내용 해석 없음 | 없음 |
| 접근 우회 | pass | 로그인, 유료벽, 봇 차단 우회 없음 | 없음 |

## 14단계: QA 결과 작성
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| qa.md 작성 | pass | artifacts/runs/run-20260604-aapl-ir-pilot/qa.md 갱신 | 없음 |

## 다음 하네스 사용 가능 여부
| 하네스 | 사용 가능 여부 | 읽을 자료 | 주의 |
|---|---|---|---|
| 모든 후속 하네스 | 가능 | artifacts/catalog/documents.jsonl, artifacts/catalog/files.jsonl, artifacts/companies/AAPL/index.md | IR 2건은 SEC EX-99.1과 sec_overlap: likely 관계가 있음 |

## 다음 조치
- 다른 회사 IR 1곳을 소규모 pilot으로 테스트해 새 IR 자료 유형 후보를 확인한다.
- 반복 등장하는 새 IR 유형은 candidate_document_type으로 기록한 뒤 schema 확장 여부를 결정한다.

## Rubric Score

total_score: 96
overall_status: pass

| 항목 | 가중치 | 점수(1-5) | 환산점 | 근거 |
|---|---:|---:|---:|---|
| Catalog 구조와 schema 준수 | 20 | 5 | 20 | documents/files/runs JSONL 관계 통과 |
| Raw/File 무결성 | 20 | 5 | 20 | raw 파일 2건 존재, SHA-256/size 일치 |
| 수집 완전성과 출처 추적성 | 15 | 5 | 15 | 승인된 공식 URL 2건만 반영 |
| Run log와 실패 처리 | 15 | 5 | 15 | download-log, run-summary, qa와 retry 기록 보존 |
| Index와 다음 하네스 전달성 | 10 | 5 | 10 | AAPL index IR 섹션과 전달 요약 반영 |
| IR/Transcript optional 처리 | 10 | 5 | 10 | IR 범위 준수, transcript/audio 제외 |
| 금지 내용과 모델 중립성 | 10 | 3 | 6 | 분석/요약은 없고 overlap은 수집 메타데이터로만 기록 |

## Rubric 판정 근거
- 자동 실패 조건: 없음
- pass 근거: pilot 범위 raw 2건이 운영 catalog/index에 반영됐고 파일 관계가 검증됨
- 사람 승인 필요: 없음
