# Source Pack QA - APP SEC-IR Overlap Recheck

## Overall
- overall_status: pass
- run_id: run-20260605-app-sec-ir-overlap-recheck
- 대상: APP FY2026 Q1 earnings-related IR 2건의 SEC overlap 제한 재확인
- 결론: 승인된 좁은 범위 안에서 SEC 8-K Item 2.02 / EX-99.1 후보를 확인했고, APP IR raw 2건과 exact hash match가 없음을 기록했다.

## Scope QA
| 항목 | 상태 | 확인 내용 | 비고 |
|---|---|---|---|
| run_scope 준수 | pass | APP FY2026 Q1 8-K Item 2.02 / EX-99.1 후보만 확인 | 전체 SEC collection 아님 |
| SEC filings page harvesting | pass | 수행하지 않음 | SEC submissions API와 해당 accession 파일만 사용 |
| IR 재수집 | pass | 수행하지 않음 | 기존 APP IR raw 2건만 hash 재계산 |
| transcript/webcast/audio/video | pass | 수행하지 않음 | 금지 범위 준수 |
| NTRA 격리 파일 | pass | 접근/복구/열람하지 않음 | 금지 범위 준수 |
| source-pack-collector.md | pass | 수정하지 않음 | 절차 파일 변경 없음 |

## SEC 후보 QA
| 항목 | 상태 | 값 | 비고 |
|---|---|---|---|
| CIK | pass | 0001751008 | APP / AppLovin Corp |
| accession | pass | 0001751008-26-000042 | filing_date 2026-05-06 |
| form | pass | 8-K | Item 2.02, 9.01 |
| primary | pass | app-20260506.htm | raw primary.html 저장 |
| EX-99.1 | pass | exhibit991-1q26earningspre.htm | raw exhibits/ 저장 |
| 범위 밖 8-K 제외 | pass | 0001751008-26-000014 | items 5.02,7.01,9.01라 Q1 earnings overlap 범위 밖 |

## File QA
| file | 상태 | local_path | size | sha256 |
|---|---|---|---:|---|
| SEC primary 8-K | pass | artifacts/raw/sec-edgar/cik-0001751008/accession-0001751008-26-000042/primary.html | 25926 | 78221e42b3a1daf80e6fa6e7a41d669fda06206c4d2c9bbc58162dc325066c9a |
| SEC EX-99.1 | pass | artifacts/raw/sec-edgar/cik-0001751008/accession-0001751008-26-000042/exhibits/exhibit991-1q26earningspre.htm | 176622 | 498f7a0986a47990501cbc57e5b107f9da82940116b8ccb5da18d526eb7c4441 |
| IR earnings release | pass | artifacts/raw/company-ir/APP/2026-05-06_fy2026-q1-earnings-release/document.html | 256924 | 700b8775dac1e211ff10b27feaf4d5511df87badecffefcac330ebf2421f4e9e |
| IR financial update PDF | pass | artifacts/raw/company-ir/APP/2026-05-06_fy2026-q1-financial-update/document.pdf | 807036 | bf35ed7b995a09bc999f251dcb2b885d8338435c2dc7e8ec5302379ece4f0ce8 |

## Overlap QA
| IR document_id | SEC 후보 | same hash | 상태 | future_recheck_required |
|---|---|---|---|---|
| ir-app-earnings-release-fy2026-q1 | sec:0001751008:0001751008-26-000042:8-k / EX-99.1 | false | different_hash_from_sec_candidate | false |
| ir-app-financial-supplement-fy2026-q1 | scoped 8-K 안에 대응 SEC financial update/supplement exhibit 없음 | false | sec_equivalent_not_found_in_scoped_8k | false |

## Catalog QA
| 항목 | 상태 | 확인 내용 |
|---|---|---|
| documents.jsonl | pass | APP SEC 8-K 1건 추가, APP IR 2건 notes/last_checked_run_id 갱신 |
| files.jsonl | pass | APP SEC primary/exhibit 2건 추가, APP IR 2건 overlap notes 갱신 |
| runs.jsonl | pass | run-20260605-app-sec-ir-overlap-recheck 추가 |
| APP index.md | pass | SEC 8-K row와 IR overlap 결과 반영 |
| raw metadata | pass | accession metadata.json 생성 |

## Rubric
| 축 | 배점 | 점수 | 근거 |
|---|---:|---:|---|
| 출처 추적성 | 30 | 30 | SEC accession, primary, EX-99.1 source_url과 local_path 기록 |
| 로컬 파일 존재성 | 25 | 25 | raw 2건, 기존 IR 2건 모두 local_path/size/hash 확인 |
| catalog 일관성 | 25 | 25 | document-file-run-index 관계 갱신 |
| 다음 하네스 전달성 | 20 | 20 | exact hash 없음과 SEC/IR 관계를 notes 및 index에 명시 |
| 총점 | 100 | 100 | pass |

## 결론
- APP FY2026 Q1 earnings-related SEC overlap recheck는 승인 범위 안에서 완료됐다.
- same hash: 없음.
- different hash: APP IR earnings release는 SEC EX-99.1 후보와 exact hash가 다름.
- not found: APP IR financial supplement에 대응하는 별도 SEC financial update/supplement exhibit는 scoped 8-K 폴더 안에서 발견되지 않음.
- 기존 `future_recheck_required: true`는 이번 제한 재확인으로 false로 해소했다.
