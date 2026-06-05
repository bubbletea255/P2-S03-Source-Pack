# Source Pack Run Summary Schema

실행 완료 후 사용자에게 아래 형식으로 요약한다.

```md
# Source Pack 실행 요약

## 실행 정보
- 실행 모드:
- 대상 티커:
- run-id:
- 설정 파일:
- 회사별 index:
- catalog 원장:
- raw 저장:
- derived/text 생성:

## 결과
| 티커 | 상태 | 문서 원장 | 파일 원장 | raw 파일 | derived/text | Transcript | IR 자료 | QA |
|---|---|---:|---:|---:|---:|---|---|---|

## Incremental Sync 카운트
| 티커 | collected_new | skipped_existing | repair_required | failed | files_collected_new |
|---|---:|---:|---:|---:|---:|

## 주요 산출물
| 티커 | 파일 | 경로 |
|---|---|---|

## 실패와 확인 필요
| 티커 | 항목 | 상태 | 이유 | 권장 조치 |
|---|---|---|---|---|

## 다음 하네스 입력
| 티커 | 입력 파일 | 사용 조건 |
|---|---|---|

## 다음 단계
- 

## 선택 섹션: 하네스 운영 관찰

이 섹션은 하네스 감량과 운영 개선을 위한 선택 메모다.
비어 있거나 누락되어도 QA 실패 또는 run 실패로 보지 않는다.
정확한 토큰 계측이 아니라, 다음 개선 판단을 위한 근사 관찰값으로 기록한다.

- instructions_files_consulted:
- instructions_lines_consulted_estimate:
- bottleneck_note:
- trim_candidate:
```

## Incremental Sync 카운트 정의

`incremental_update`와 `test_collection`을 포함한 모든 run은 가능한 경우 아래 카운트를 남긴다.

| 필드 | 의미 |
|---|---|
| `collected_new` | 이번 run에서 새로 다운로드해 catalog에 `collected`로 승격한 문서 수 |
| `skipped_existing` | 기존 catalog와 local file fast path 조건을 통과해 다시 다운로드하지 않은 문서 수 |
| `repair_required` | 과거에 수집 성공했으나 현재 catalog/file 관계가 깨져 사람 확인이 필요한 문서 수 |
| `failed` | 이번 run에서 실제 시도했지만 실패했거나 후속 사용이 위험한 문서 수 |
| `files_collected_new` | 이번 run에서 `files.jsonl`에 새로 승격한 파일 record 수. exhibit 수집 시 문서 수와 다를 수 있음 |

`collected_new`, `skipped_existing`, `repair_required`, `failed`는 문서 단위 카운트다.
`files_collected_new`는 파일 원장 record 단위 카운트다.

`repair_required`는 자동 복구가 아니다.
사유는 `qa.md`와 `run-summary.md`의 실패와 확인 필요 표에 남긴다.

권장 repair 사유:

| 사유 | 의미 |
|---|---|
| `missing_primary_file_id` | collected 문서에 대표 파일 ID가 없음 |
| `missing_file_record` | 대표 파일 ID에 대응하는 files.jsonl record가 없음 |
| `missing_local_file` | catalog가 가리키는 local_path 파일이 없음 |
| `zero_size` | local_path 파일 크기가 0 |
| `sha256_mismatch` | 정밀 검증에서 실제 hash와 catalog hash가 다름 |

`retry_eligible`은 별도 카운트로 두지 않는다.
재시도 대상은 실제 시도 결과에 따라 `collected_new` 또는 `failed`로 집계한다.

## 상태 표기

사용자 보고의 상태는 아래 한국어 표기를 쓴다.

| 내부 QA 상태 | 사용자 보고 상태 |
|---|---|
| `pass` | 성공 |
| `partial_pass` | 부분 성공 |
| `unverified` | 미검증 |
| `fail` | 실패 |
| `stopped` | 중단 |

`catalog_status`가 `fail`, `unverified`, `[확인 필요:]`인 자료는 다음 하네스의 확정 입력으로 쓰지 않는다고 명시한다.

## 선택 섹션: 하네스 운영 관찰

`하네스 운영 관찰`은 Source Pack 하네스 자체를 나중에 감량하고 개선하기 위한 선택 섹션이다.
수집 품질 보증 항목이 아니며 QA 점수나 run 성공/실패 판단에 포함하지 않는다.

권장 필드:

| 필드 | 의미 |
|---|---|
| `instructions_files_consulted` | 이번 실행에서 주요하게 참고한 지침 파일 수 |
| `instructions_lines_consulted_estimate` | 참고한 지침 라인 수의 대략값 |
| `bottleneck_note` | 오래 걸렸거나 무겁게 느껴진 구간 |
| `trim_candidate` | 나중에 줄이거나 자동화할 후보 |

작성 원칙:

- 가능한 경우 run-summary 마지막에 붙인다.
- 정확한 토큰 수를 추정해 쓰지 않는다.
- 숫자는 대략값 또는 `unknown`으로 둘 수 있다.
- 비어 있거나 누락되어도 QA 실패, run 실패, catalog 실패로 보지 않는다.
- 자동 측정 스크립트는 이 schema의 요구사항이 아니다.
