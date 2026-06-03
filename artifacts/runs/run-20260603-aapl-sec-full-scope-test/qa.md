# Source Pack QA - AAPL

run_id: run-20260603-aapl-sec-full-scope-test
qa_date: 2026-06-03
overall_status: pass

## 요약
- 통과: SEC full-range test scope processed; collected_new=35, skipped_existing=4, files_collected_new=46
- 실패: failed=0
- 미검증: IR, transcript, derived text는 run_scope 밖
- 사람 승인 필요: repair_required=0

## 1단계: run 범위 확인
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| run_id와 run folder | pass | run-20260603-aapl-sec-full-scope-test 생성 | 없음 |
| run_mode/run_scope | pass | test_collection run_scope 기록 | 없음 |

## 2단계: catalog JSONL 구조 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| JSONL 구조 | pass | entities/documents/files/runs JSONL 파싱 가능 | 없음 |
| source of truth | pass | 실제 보유 자료 판단은 documents/files 기준 | 없음 |

## 3단계: schema 허용값 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| document_type/source_type/status | pass | SEC 문서는 10-K, 10-Q, DEF 14A, 8-K 허용값 사용 | 없음 |
| file_role/file_status | pass | primary/exhibit, available 사용 | 없음 |

## 4단계: entity 관계 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| entity_id | pass | sec-cik-0000320193, CIK 0000320193 | 없음 |
| document/file entity 연결 | pass | AAPL SEC records가 같은 entity_id 사용 | 없음 |

## 5단계: document 원장 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| Tier 1 SEC 범위 | pass | 10-K 10, 10-Q 12, DEF 14A 5, 8-K Item 2.02 12 후보 기록 | 없음 |
| source_url | pass | primary document URL 사용 | 없음 |
| test_collection 범위 | pass | IR/transcript/derived 누락은 실패 아님 | 없음 |

## 6단계: file 원장과 raw 파일 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| 신규 파일 hash/size | pass | 다운로드 성공 46 건은 SHA-256과 size 기록 | 없음 |
| existing fast path | pass | 기존 문서 4 건은 local_path 존재와 size > 0 기준으로 재사용 | 없음 |
| repair_required | pass | repair_required=0 | 없음 |

## 7단계: download-log와 승격 관계 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| download-log 기록 | pass | 실제 I/O 시도 46 건 기록, skipped_existing은 기록하지 않음 | 없음 |
| 성공 attempt 승격 | pass | files_collected_new=46 | 없음 |
| 실패 attempt | pass | failed attempts=0 | 없음 |

## 8단계: SEC 실적 발표 자료 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| 8-K Item 2.02 | pass | 12 건 items에 2.02 포함 | 없음 |
| EX-99.1 exhibit | pass | existing/new/not_found: 1/11/0 | 없음 |

## 9단계: IR 자료 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| IR 자료 | skipped_out_of_scope | 이번 run_scope는 SEC 확장 테스트 | 별도 IR 테스트에서 수행 |

## 10단계: Transcript optional 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| Transcript | skipped_out_of_scope | 이번 run_scope에서 제외 | 별도 transcript 테스트에서 수행 |

## 11단계: derived text 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| derived/text | skipped_out_of_scope | 이번 run_scope에서 제외 | 별도 text extraction 테스트에서 수행 |

## 12단계: 회사별 index.md 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| index 경로와 필수 섹션 | pass | artifacts/companies/AAPL/index.md 갱신 | 없음 |
| 다음 하네스 전달 요약 | pass | catalog/documents/files/raw 읽기 순서 명시 | 없음 |

## 13단계: 금지 내용 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| 분석/투자 판단 금지 | pass | 원자료 내용 요약, thesis, valuation 없음 | 없음 |
| 우회 금지 | pass | SEC 공식 API와 EDGAR raw만 사용 | 없음 |

## 14단계: QA 결과 작성
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| overall_status | pass | status=success, failed=0, repair_required=0 | 다음 단계 진행 가능 |

## 다음 하네스 사용 가능 여부
| 하네스 | 사용 가능 여부 | 읽을 자료 | 주의 |
|---|---|---|---|
| SEC 기반 후속 하네스 | 가능 | documents.jsonl, files.jsonl, raw/sec-edgar | 이번 run은 SEC test_collection이며 IR/transcript 제외 |
| IR/Transcript 하네스 | 제한적 | SEC raw만 참고 가능 | IR/transcript 원문은 아직 별도 수집 필요 |

## 다음 조치
- 같은 전체 SEC 범위 재실행으로 skipped_existing과 download-log 0건을 확인한다.
- IR 자료와 transcript optional 수집은 별도 테스트로 진행한다.

