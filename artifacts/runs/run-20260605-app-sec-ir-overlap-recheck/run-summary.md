# Source Pack 실행 요약

## 실행 정보
- 실행 모드: partial_recheck
- 대상 티커: APP
- run-id: run-20260605-app-sec-ir-overlap-recheck
- 설정 파일: config.md
- 회사별 index: artifacts/companies/APP/index.md 갱신
- catalog 원장: artifacts/catalog/documents.jsonl, artifacts/catalog/files.jsonl, artifacts/catalog/runs.jsonl 갱신
- raw 저장: artifacts/raw/sec-edgar/cik-0001751008/accession-0001751008-26-000042
- derived/text 생성: 없음
- run_scope: limited recheck: APP FY2026 Q1 earnings-related SEC 8-K Item 2.02 / EX-99.1 candidate only; compare against existing APP IR pilot raw 2 files; no APP full SEC collection, no IR recrawl, no transcript/webcast/audio/video

## 결과
| 티커 | 상태 | 문서 원장 | 파일 원장 | raw 파일 | derived/text | Transcript | IR 자료 | QA |
|---|---|---:|---:|---:|---:|---|---|---|
| APP | 성공 | 1 SEC 신규 + 2 IR notes 갱신 | 2 SEC 신규 + 2 IR notes 갱신 | 2 SEC raw + 기존 IR raw 2 확인 | 0 | out_of_scope | 재수집 없음 | pass |

## Incremental Sync 카운트
| 티커 | collected_new | skipped_existing | repair_required | failed | files_collected_new |
|---|---:|---:|---:|---:|---:|
| APP | 1 | 2 | 0 | 0 | 2 |

## SEC 후보 확인
| 후보 | accession | filing_date | items | primary | exhibit | 범위 판정 |
|---|---|---|---|---|---|---|
| FY2026 Q1 실적 8-K | 0001751008-26-000042 | 2026-05-06 | 2.02,9.01 | app-20260506.htm | exhibit991-1q26earningspre.htm | 이번 run_scope에 포함 |
| 2026-04-07 8-K | 0001751008-26-000014 | 2026-04-07 | 5.02,7.01,9.01 | app-20260402.htm | not_checked | 이번 Q1 실적 overlap 범위 밖 |

## Hash 비교 결과
| IR document_id | IR file hash | SEC 후보 | SEC file hash | exact hash | 처리 |
|---|---|---|---|---|---|
| ir-app-earnings-release-fy2026-q1 | sha256:700b8775dac1e211ff10b27feaf4d5511df87badecffefcac330ebf2421f4e9e | EX-99.1 earnings press release | sha256:498f7a0986a47990501cbc57e5b107f9da82940116b8ccb5da18d526eb7c4441 | false | overlap_status=different_hash_from_sec_candidate; future_recheck_required=false |
| ir-app-financial-supplement-fy2026-q1 | sha256:bf35ed7b995a09bc999f251dcb2b885d8338435c2dc7e8ec5302379ece4f0ce8 | 해당 유형의 SEC exhibit 없음 | - | false | overlap_status=sec_equivalent_not_found_in_scoped_8k; canonical_source=company-ir; sec_separate_financial_update_exhibit=not_found_in_scoped_8k; future_recheck_required=false |

## 주요 산출물
| 티커 | 파일 | 경로 |
|---|---|---|
| APP | SEC primary 8-K | artifacts/raw/sec-edgar/cik-0001751008/accession-0001751008-26-000042/primary.html |
| APP | SEC EX-99.1 | artifacts/raw/sec-edgar/cik-0001751008/accession-0001751008-26-000042/exhibits/exhibit991-1q26earningspre.htm |
| APP | raw metadata | artifacts/raw/sec-edgar/cik-0001751008/accession-0001751008-26-000042/metadata.json |
| APP | download log | artifacts/runs/run-20260605-app-sec-ir-overlap-recheck/download-log.jsonl |
| APP | QA | artifacts/runs/run-20260605-app-sec-ir-overlap-recheck/qa.md |

## 실패와 확인 필요
| 티커 | 항목 | 상태 | 이유 | 권장 조치 |
|---|---|---|---|---|
| APP | exact hash match | 확인 완료: false | IR HTML/PDF와 SEC EX-99.1 hash가 모두 다름 | 같은 파일로 dedup하지 않음 |
| APP | SEC full collection | out_of_scope | 이번 run은 Q1 2026 8-K Item 2.02 / EX-99.1 제한 재확인 | 필요 시 별도 승인 후 full collection |
| APP | transcript/webcast/audio/video | out_of_scope | 금지 범위 | 없음 |

## 다음 하네스 입력
| 티커 | 입력 파일 | 사용 조건 |
|---|---|---|
| APP | artifacts/companies/APP/index.md | 이번 run은 full Source Pack이 아니라 overlap recheck 결과임을 확인 |
| APP | sec:0001751008:0001751008-26-000042:8-k | FY2026 Q1 실적 8-K/EX-99.1 canonical SEC 자료 |
| APP | ir-app-earnings-release-fy2026-q1, ir-app-financial-supplement-fy2026-q1 | earnings release는 SEC EX-99.1과 hash가 다르고, financial supplement는 scoped 8-K 안에 SEC 대응 exhibit가 없어 IR raw 별도 원자료로 유지 |

## 다음 단계
- APP 전체 SEC collection은 아직 수행하지 않았다.
- 이번 recheck로 APP IR 2건의 `future_recheck_required`는 false로 해소했다.
- 다음 IR 구조 작업은 다른 회사 pilot 또는 Source Pack integrity check 설계 중 하나를 선택해 진행한다.
