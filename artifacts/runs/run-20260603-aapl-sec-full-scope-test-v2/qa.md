# Source Pack QA - AAPL

run_id: run-20260603-aapl-sec-full-scope-test-v2
qa_date: 2026-06-03
overall_status: pass

## 요약
- 통과: full SEC scope rerun produced skipped_existing=39, download-log=0
- 실패: failed=0
- 미검증: IR, transcript, derived text는 run_scope 밖
- 사람 승인 필요: repair_required=0

## 1단계: run 범위 확인
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| run_id와 run folder | pass | run-20260603-aapl-sec-full-scope-test-v2 생성 | 없음 |
| run_mode/run_scope | pass | test_collection run_scope 기록 | 없음 |

## 2단계: catalog JSONL 구조 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| JSONL 구조 | pass | catalog JSONL 파싱 가능 | 없음 |
| runs delta | pass | collected_new=0, skipped_existing=39, files_collected_new=0 | 없음 |

## 3단계: schema 허용값 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| run status/mode | pass | status=success, run_mode=test_collection | 없음 |
| file/document status | pass | 기존 AAPL SEC 문서와 파일 허용값 유지 | 없음 |

## 4단계: entity 관계 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| entity_id | pass | sec-cik-0000320193 | 없음 |
| 연결 관계 | pass | documents/files가 AAPL entity_id와 연결 | 없음 |

## 5단계: document 원장 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| Tier 1 SEC 범위 | pass | 10-K 10, 10-Q 12, DEF 14A 5, 8-K Item 2.02 12 모두 기존 보유 | 없음 |
| skipped_existing | pass | 새 document 후보 없음 | 없음 |

## 6단계: file 원장과 raw 파일 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| primary fast path | pass | 39개 primary local_path 존재 및 size > 0 | 없음 |
| exhibit fast path | pass | 12개 EX-99.1 exhibit local_path 존재 및 size > 0 | 없음 |

## 7단계: download-log와 승격 관계 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| download-log | pass | 0건, 실제 I/O 시도 없음 | 없음 |
| preflight | pass | 새 raw 다운로드 후보 없음, preflight not_applicable | 없음 |

## 8단계: SEC 실적 발표 자료 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| 8-K Item 2.02 | pass | 12건 기존 보유 | 없음 |
| EX-99.1 exhibit | pass | existing=12, new=0, not_found=0 | 없음 |

## 9단계: IR 자료 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| IR 자료 | skipped_out_of_scope | 이번 run_scope는 SEC idempotency 테스트 | 별도 IR 테스트에서 수행 |

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
| index 상태 | pass | index.md last_run_id 갱신 예정 | 없음 |
| 다음 하네스 전달 요약 | pass | 기존 index 구조 유지 | 없음 |

## 13단계: 금지 내용 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| 분석/투자 판단 금지 | pass | 원자료 내용 요약, thesis, valuation 없음 | 없음 |
| 우회 금지 | pass | SEC submissions API만 조회, raw archive 다운로드 없음 | 없음 |

## 14단계: QA 결과 작성
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| overall_status | pass | status=success, failed=0, repair_required=0 | 다음 단계 진행 가능 |

## 다음 하네스 사용 가능 여부
| 하네스 | 사용 가능 여부 | 읽을 자료 | 주의 |
|---|---|---|---|
| SEC 기반 후속 하네스 | 가능 | documents.jsonl, files.jsonl, raw/sec-edgar | 이번 run은 idempotency 확인이며 신규 raw 없음 |
| IR/Transcript 하네스 | 제한적 | SEC raw만 참고 가능 | IR/transcript 원문은 별도 수집 필요 |

## 다음 조치
- SEC full-scope idempotency는 통과했다.
- IR 자료와 transcript optional 수집은 별도 테스트로 진행한다.
