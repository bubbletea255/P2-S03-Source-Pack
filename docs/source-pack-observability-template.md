# Source Pack Observability Template

이 문서는 Source Pack 하네스에 가벼운 관측 가능성을 붙이기 위한 템플릿이다.

목표는 새로운 시스템을 만드는 것이 아니다.
다음 IR 자료 수집 실행부터 `run-summary.md` 끝에 짧은 선택 섹션을 추가해, 하네스가 어디서 무거워지는지 나중에 판단할 근거를 남기는 것이다.

## 1. 원칙

### 1.1 선택 섹션이다

`하네스 운영 관찰`은 필수 산출물이 아니다.
비어 있거나 누락되어도 QA 실패, run 실패, catalog 실패로 보지 않는다.

이 섹션은 수집 품질 보증이 아니라 하네스 감량과 운영 개선을 위한 학습용 메모다.

### 1.2 정확한 토큰 계측이 아니다

토큰 수를 AI가 추정해 기록하지 않는다.
정확히 알 수 없는 숫자를 그럴듯하게 쓰면 나중에 판단을 흐린다.

대신 아래처럼 비교적 세기 쉬운 값을 기록한다.

- 참고한 지침 파일 수
- 참고한 지침 라인 수의 대략값
- 오래 걸렸다고 느낀 구간
- 다음에 줄일 후보

### 1.3 새 파일을 만들지 않는다

초기 적용에서는 별도 `run-metrics.md`를 만들지 않는다.
기존 `artifacts/runs/{run-id}/run-summary.md` 끝에 선택 섹션으로 붙인다.

새 파일이 필요하다고 판단하는 시점은 같은 관찰 섹션이 여러 run에서 실제로 유용하다고 확인된 뒤다.

## 2. 적용 위치

다음 실행부터 각 run의 요약 파일 마지막에 추가한다.

```text
artifacts/runs/{run-id}/run-summary.md
```

권장 위치:

```text
... 기존 run-summary 내용 ...

## 하네스 운영 관찰
```

## 3. 템플릿

아래 블록을 `run-summary.md` 마지막에 붙인다.

```md
## 하네스 운영 관찰

이 섹션은 하네스 감량과 운영 개선을 위한 선택 메모다.
비어 있거나 누락되어도 QA 실패 또는 run 실패로 보지 않는다.
정확한 토큰 계측이 아니라, 다음 개선 판단을 위한 근사 관찰값으로 기록한다.

- instructions_files_consulted:
- instructions_lines_consulted_estimate:
- bottleneck_note:
- trim_candidate:
```

## 4. 필드 설명

| 필드 | 의미 | 기록 방법 |
|---|---|---|
| `instructions_files_consulted` | 이번 실행에서 주요하게 참고한 지침 파일 수 | 전체를 읽었거나 판단에 사용한 주요 파일만 센다 |
| `instructions_lines_consulted_estimate` | 참고한 지침 라인 수의 대략값 | 정확한 계측이 아니라 감량 판단용 근사치로 적는다 |
| `bottleneck_note` | 오래 걸렸거나 무겁게 느껴진 구간 | 한 문장으로 적는다 |
| `trim_candidate` | 나중에 줄이거나 자동화할 후보 | 다음 개선 행동으로 이어질 수 있게 적는다 |

## 5. 작성 예시

### 5.1 IR 자료 수집 run 예시

```md
## 하네스 운영 관찰

이 섹션은 하네스 감량과 운영 개선을 위한 선택 메모다.
비어 있거나 누락되어도 QA 실패 또는 run 실패로 보지 않는다.
정확한 토큰 계측이 아니라, 다음 개선 판단을 위한 근사 관찰값으로 기록한다.

- instructions_files_consulted: 7
- instructions_lines_consulted_estimate: 1800
- bottleneck_note: IR 처리 규칙을 확인하느라 collector procedure와 catalog schema를 많이 참조했다.
- trim_candidate: IR 전용 빠른 체크리스트를 별도 1페이지로 만들 후보.
```

### 5.2 SEC incremental update run 예시

```md
## 하네스 운영 관찰

이 섹션은 하네스 감량과 운영 개선을 위한 선택 메모다.
비어 있거나 누락되어도 QA 실패 또는 run 실패로 보지 않는다.
정확한 토큰 계측이 아니라, 다음 개선 판단을 위한 근사 관찰값으로 기록한다.

- instructions_files_consulted: 5
- instructions_lines_consulted_estimate: 1200
- bottleneck_note: skipped_existing과 repair_required 판단 기준을 다시 확인했다.
- trim_candidate: incremental update 판단 규칙을 짧은 decision table로 분리할 후보.
```

### 5.3 Transcript 보완 run 예시

```md
## 하네스 운영 관찰

이 섹션은 하네스 감량과 운영 개선을 위한 선택 메모다.
비어 있거나 누락되어도 QA 실패 또는 run 실패로 보지 않는다.
정확한 토큰 계측이 아니라, 다음 개선 판단을 위한 근사 관찰값으로 기록한다.

- instructions_files_consulted: 6
- instructions_lines_consulted_estimate: 1600
- bottleneck_note: transcript optional source와 유료벽/로그인 우회 금지 규칙을 여러 문서에서 재확인했다.
- trim_candidate: transcript 수집 금지/허용 규칙을 runbook 앞쪽의 짧은 요약으로 이동할 후보.
```

## 6. 작성 기준

### 6.1 숫자는 거칠게 적는다

정확한 라인 수를 계산하느라 시간을 쓰지 않는다.
대략 아래 기준으로 충분하다.

| 상황 | 기록 방식 |
|---|---|
| 파일 전체를 읽음 | 해당 파일 전체 라인 수를 대략 포함 |
| 일부 섹션만 읽음 | 읽은 구간의 대략 라인 수만 포함 |
| 여러 번 다시 확인함 | 중복 계산하지 않고 주요 참조량만 기록 |
| 정확히 모르겠음 | `unknown` 또는 `대략 {숫자}`로 기록 |

### 6.2 병목은 한 문장으로 적는다

좋은 예:

- `catalog schema 허용값 확인에 시간이 걸렸다.`
- `QA 14단계 중 raw/file 관계 검증을 오래 확인했다.`
- `IR 자료와 SEC 8-K exhibit의 역할 구분을 다시 읽었다.`

피할 예:

- `복잡했다.`
- `오래 걸렸다.`
- `토큰이 많이 든 것 같다.`

### 6.3 감량 후보는 행동으로 이어지게 적는다

좋은 예:

- `catalog 허용값 표를 빠른 참조용 1페이지로 분리.`
- `IR 수집 전용 decision table 후보.`
- `QA 중 기계 검증 가능한 항목을 validate script 후보로 표시.`

피할 예:

- `나중에 줄이기.`
- `문서 개선.`
- `자동화 필요.`

## 7. QA와 분리

`하네스 운영 관찰`은 QA 항목이 아니다.

아래 경우에도 QA 실패로 처리하지 않는다.

- 섹션이 없음
- 필드가 비어 있음
- 숫자가 대략값임
- `bottleneck_note`가 주관적임
- `trim_candidate`가 아직 실행되지 않음

QA는 수집 산출물의 정확성, catalog 관계, raw 파일 존재, 금지 내용 위반 여부를 판단한다.
운영 관찰은 하네스 자체를 나중에 줄이기 위한 메모다.

## 8. 다음 IR 실행에서 사용하는 방법

다음 IR 자료 수집 run에서는 아래 순서로 적용한다.

1. 기존 Source Pack runbook과 collector 절차를 그대로 따른다.
2. IR 수집 결과를 `run-summary.md`에 정리한다.
3. 마지막에 `## 하네스 운영 관찰` 선택 섹션을 붙인다.
4. 이번 실행에서 실제로 주요하게 참고한 지침 파일 수와 대략 라인 수를 적는다.
5. 오래 걸린 구간이 있으면 `bottleneck_note`에 한 문장으로 적는다.
6. 나중에 줄일 수 있어 보이는 후보를 `trim_candidate`에 적는다.
7. 이 섹션이 비어 있더라도 QA 결과에는 반영하지 않는다.

## 9. 나중에 전역 템플릿으로 승격하는 기준

아래 조건이 충족되면 이 템플릿을 전역 하네스 템플릿 후보로 볼 수 있다.

- Source Pack에서 3회 이상 사용했다.
- 적어도 2회 이상 실제 감량 후보가 발견됐다.
- 작성 부담이 작다고 느껴졌다.
- QA와 혼동되지 않았다.
- 다른 하네스에도 같은 4개 필드가 유용해 보였다.

전역화 후보 위치:

```text
{전역 하네스 템플릿 폴더}/observability-lite-template.md
```

전역화할 때도 원칙은 같다.

- 선택 섹션으로 유지한다.
- QA 실패 조건으로 쓰지 않는다.
- 정확한 토큰 수를 요구하지 않는다.
- 새 하네스 생성 시 `run-summary.md` 또는 이에 준하는 실행 요약에만 가볍게 붙인다.

## 10. 한 줄 요약

`하네스 운영 관찰`은 하네스를 더 무겁게 만들기 위한 장치가 아니라, 나중에 무엇을 줄일지 알기 위한 작은 메모장이다.
