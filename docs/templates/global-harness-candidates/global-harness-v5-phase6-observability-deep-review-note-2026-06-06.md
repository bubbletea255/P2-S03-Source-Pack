# Phase 6 Observability Deep Review Note

- 작성일: 2026-06-06
- 대상 module: `observability`
- 비교 원본: `docs/templates/source-pack-observability-template.md`
- 전역 후보: `docs/templates/global-harness-candidates/global-harness-observability-template-v0.md`
- 상태: global observability v0와 Source Pack 원본 템플릿의 차이 검토

## 1. 목적

Source Pack observability template에서 전역 module로 보존할 구조와 제거해야 할 Source Pack 특수 맥락을 분리한다.

이 note는 `global-harness-observability-template-v1.md`를 바로 만드는 문서가 아니다.
먼저 무엇을 살리고, 무엇을 일반화하고, 무엇을 reference-only로 둘지 판정한다.

## 2. 핵심 판정

현재 global observability v0는 방향은 맞지만 너무 압축되어 있다.

보존된 것:

- 선택 메모라는 원칙
- QA 실패 조건이 아니라는 원칙
- 정확한 토큰 계측이 아니라는 원칙
- 기본 4개 필드
- 자동화 도입 조건

빠진 것:

- 새 파일을 만들지 않는 초기 적용 원칙
- 실행 요약 파일 끝에 붙이는 위치 기준
- 필드별 기록 방법
- 숫자를 거칠게 적는 기준
- 병목과 감량 후보를 어떻게 써야 하는지
- QA와 분리되는 구체 사례
- 전역화 승격 기준
- 좋은 구조와 Source Pack 예시를 구분하는 판정

결론:

```text
global observability v0는 v1 후보화가 필요하다.
단, Source Pack 원본을 그대로 복사하지 않고 전역 구조만 보존한다.
```

## 3. 판정 기준

### 3.1 Source Pack 특수 용어 제거 또는 일반화

| 원본 요소 | 처리 |
|---|---|
| Source Pack | 전역 본문에서는 `하네스`로 일반화 |
| IR 자료 수집 | 전역 본문에서는 `자료 수집 run` 또는 `해당 하네스 실행`으로 일반화 |
| SEC, 8-K, transcript, ticker | 전역 본문에서는 제거. 필요하면 reference/example로만 유지 |
| catalog, raw 파일, collector procedure | 전역 본문에서는 특정 하네스 산출물 예시로만 취급 |
| skipped_existing, repair_required 같은 Source Pack 상태 맥락 | 전역 본문에는 직접 넣지 않음. QA/status module reference로만 연결 가능 |
| `{전역 하네스 템플릿 폴더}/observability-lite-template.md` | 현재 후보 폴더 기준에 맞춰 `global-harness-observability-template-v1.md` 후보로 일반화 |

### 3.2 구조 보존

| 원본 구조 | 처리 |
|---|---|
| 선택 섹션 | 보존 |
| QA 실패/run 실패 조건이 아님 | 보존 |
| 정확한 토큰 계측 금지 | 보존 |
| 새 파일을 만들지 않는 초기 적용 원칙 | 보존 |
| 실행 요약 파일 끝에 붙이는 방식 | `run-summary.md` 또는 이에 준하는 실행 요약으로 일반화 |
| 4개 기본 필드 | 보존 |
| 필드별 기록 방법 | 보존하되 Source Pack 예시는 제거 |
| 숫자는 거칠게 적는 기준 | 보존 |
| 병목은 한 문장으로 적는 기준 | 보존 |
| 감량 후보는 행동으로 이어지게 적는 기준 | 보존 |
| QA와 분리되는 구체 조건 | 보존 |
| 전역화 승격 기준 | 보존하되 Source Pack 3회 사용 조건은 "pilot 3회"로 일반화 |

## 4. Section별 비교

| Source Pack 원본 section | global v0 현황 | 판정 | v1 반영 방향 |
|---|---|---|---|
| 목적 | 하네스가 무거워지는 지점을 기록한다는 목적은 보존 | aligned but thin | 목적은 유지하되 "새 시스템이 아니라 선택 메모"라는 문장을 보강 |
| 1.1 선택 섹션 | QA 실패 조건 아님, run 실패 아님은 있음 | partial | "필수 산출물이 아님", "품질 보증이 아니라 운영 개선 메모"를 보강 |
| 1.2 정확한 토큰 계측 아님 | 원칙만 있음 | partial | AI가 토큰 수를 추정하지 않는다는 금지와 대체 기록값을 보강 |
| 1.3 새 파일을 만들지 않는다 | 없음 | missing | 초기 적용은 별도 metrics 파일을 만들지 않는다는 원칙 추가 |
| 2. 적용 위치 | 없음 | missing | `run-summary.md` 또는 실행 요약 마지막에 붙인다고 일반화 |
| 3. 템플릿 | 필드만 있음 | partial | 설명 문구 포함 template block으로 보강 |
| 4. 필드 설명 | 뜻만 있음 | partial | "기록 방법" 열 추가 |
| 5. 작성 예시 | 없음 | reference-only | Source Pack 예시는 직접 복사하지 않음. 필요하면 일반 예시 1개만 v1에 추가 |
| 6. 작성 기준 | 없음 | missing | 숫자, 병목, trim 후보 작성 기준 보존 |
| 7. QA와 분리 | 원칙만 있음 | partial | QA 실패로 보지 않는 구체 조건을 보존 |
| 8. 다음 IR 실행에서 사용하는 방법 | 없음 | Source Pack-specific | 전역 적용 순서로 일반화 가능 |
| 9. 전역 템플릿 승격 기준 | 없음 | missing | pilot 3회, 감량 후보 발견, 작성 부담, QA 혼동 없음, 다른 하네스 유용성 기준으로 일반화 |
| 10. 한 줄 요약 | 없음 | useful | "무겁게 만드는 장치가 아니라 줄일 후보를 찾는 메모" 취지를 보존 |

## 5. v1에 직접 넣을 후보

### 5.1 원칙

v1에는 아래 원칙을 직접 넣는 것이 좋다.

- observability는 선택 섹션이다.
- 비어 있거나 누락되어도 QA 실패 또는 run 실패로 보지 않는다.
- 정확한 토큰 계측을 요구하지 않는다.
- AI가 모르는 토큰 수를 추정해 쓰지 않는다.
- 초기 적용에서는 별도 metrics 파일을 만들지 않는다.
- 실행 요약 파일 마지막에 가볍게 붙인다.
- 반복적으로 유용하다고 확인될 때만 자동화나 별도 파일을 검토한다.

### 5.2 기본 template

v1에는 global v0의 단순 필드 목록보다 아래처럼 설명 문구가 포함된 블록이 더 적절하다.

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

### 5.3 필드 설명

| 필드 | 의미 | 기록 방법 |
|---|---|---|
| `instructions_files_consulted` | 이번 실행에서 주요하게 참고한 지침 파일 수 | 전체를 읽었거나 판단에 사용한 주요 파일만 센다 |
| `instructions_lines_consulted_estimate` | 참고한 지침 라인 수의 대략값 | 정확한 계측이 아니라 감량 판단용 근사치로 적는다 |
| `bottleneck_note` | 오래 걸렸거나 무겁게 느껴진 구간 | 한 문장으로 적는다 |
| `trim_candidate` | 나중에 줄이거나 자동화할 후보 | 다음 개선 행동으로 이어질 수 있게 적는다 |

### 5.4 작성 기준

v1에는 최소한 아래 작성 기준을 넣는 것이 좋다.

- 숫자는 정확히 계산하려고 시간을 쓰지 않는다.
- 전체 파일을 읽은 경우와 일부 섹션만 읽은 경우를 구분한다.
- 여러 번 다시 확인한 파일은 중복 계산하지 않는다.
- 정확히 모르겠으면 `unknown` 또는 `대략 {숫자}`로 적는다.
- 병목은 한 문장으로 적는다.
- `복잡했다`, `오래 걸렸다`, `토큰이 많이 든 것 같다` 같은 흐린 표현은 피한다.
- `trim_candidate`는 다음 행동으로 이어지게 쓴다.

### 5.5 QA와 분리

v1에는 아래 경우에도 QA 실패로 보지 않는다고 명시하는 것이 좋다.

- observability 섹션이 없음
- 필드가 비어 있음
- 숫자가 대략값임
- `bottleneck_note`가 주관적임
- `trim_candidate`가 아직 실행되지 않음

## 6. v1에서 일반화할 항목

| 원본 표현 | v1 일반화 |
|---|---|
| 다음 IR 자료 수집 run | 다음 하네스 실행 또는 pilot run |
| `artifacts/runs/{run-id}/run-summary.md` | 해당 하네스의 `run-summary.md` 또는 이에 준하는 실행 요약 |
| Source Pack runbook과 collector 절차 | 해당 하네스의 runbook/procedure |
| IR 수집 결과 | 해당 하네스 실행 결과 |
| catalog schema 허용값 | 하네스별 schema 또는 판단 기준 |
| IR 전용 빠른 체크리스트 | 하네스별 빠른 체크리스트 |
| Source Pack에서 3회 이상 사용 | pilot 또는 실제 실행에서 3회 이상 사용 |

## 7. reference-only로 둘 항목

아래 항목은 전역 v1 본문에 직접 넣지 않는다.

| 항목 | 이유 |
|---|---|
| IR 자료 수집 run 예시 | Source Pack 도메인 예시이므로 전역 본문에는 과하다 |
| SEC incremental update run 예시 | SEC와 Source Pack 상태값에 묶여 있다 |
| Transcript 보완 run 예시 | transcript 수집 금지/허용 규칙은 Source Pack 도메인 맥락이다 |
| catalog/raw 파일 QA 설명 | QA scaffold 또는 Source Pack reference 영역이다 |
| skipped_existing / repair_required 세부 판단 | Source Pack 상태값과 연결된 예시다 |

다만 이 예시들은 `docs/templates/source-pack-observability-template.md`에 reference로 보존한다.

## 8. 제외할 항목

아래 항목은 global observability v1에 넣지 않는다.

- 특정 ticker, SEC, IR, transcript 운영 규칙
- Source Pack catalog field나 raw 파일 구조
- 정확한 토큰 계측 요구
- 별도 `run-metrics.md`를 초기부터 만드는 규칙
- observability 섹션을 QA 필수 조건으로 만드는 규칙
- line-count 자동화 도구를 처음부터 강제하는 규칙

## 9. 자동화 도입 기준

global v0의 자동화 도입 조건은 방향이 좋다.
v1에서는 원본 템플릿의 "새 파일은 반복 유용성이 확인된 뒤" 원칙과 합쳐 아래처럼 정리하는 것이 좋다.

자동화나 별도 metrics 파일은 아래 조건이 반복될 때만 검토한다.

- 같은 bottleneck이 3회 이상 반복된다.
- 읽는 지침 파일 수가 계속 증가한다.
- 실행 시간이 실제 업무보다 지침 해석에 더 많이 쓰인다.
- 사용자가 감량 또는 성능 개선을 논의하기 시작한다.
- observability 섹션이 최소 3회 이상 작성됐고, 실제 trim candidate가 2회 이상 발견됐다.
- QA와 혼동되지 않고 작성 부담이 작다고 확인됐다.

## 10. v1 후보화 권장안

권장 결정:

```text
global-harness-observability-template-v1.md를 새 후보 파일로 만든다.
v0는 초기 후보로 보존한다.
v1은 Source Pack 원본 구조를 일반화하되, Source Pack 예시는 reference-only로 둔다.
```

v1 권장 목차:

1. 목적
2. 범위와 비범위
3. 선택 섹션 원칙
4. 적용 위치
5. 기본 template
6. 필드 설명과 기록 방법
7. 작성 기준
8. QA와 분리
9. 자동화/별도 metrics 파일 도입 조건
10. Source Pack reference에서 일반화한 것
11. 적용 전 체크리스트

## 11. Work Map 반영 제안

이 note 기준으로 work map에는 아래처럼 반영하는 것이 좋다.

| 위치 | 반영 |
|---|---|
| Phase 6 observability row | deep review note 작성 완료, v1 후보 파일 작성 여부는 Claude Code 교차검증 후 결정 |
| Next | 이 note를 Claude Code로 교차검증한 뒤 `global-harness-observability-template-v1.md` 작성 여부 결정 |
| Done | Phase 6 observability deep review note 작성 완료 |

## 12. 다음 결정

다음 결정은 하나다.

```text
이 review note를 기준으로 global-harness-observability-template-v1.md를 새 후보 파일로 만들 것인가?
```

내 권장안은 "만든다"이다.

이유:

- global v0는 방향은 맞지만 실무 작성 기준이 너무 얇다.
- Source Pack 원본에는 전역으로 보존할 좋은 구조가 충분히 있다.
- v0를 직접 덮어쓰기보다 v1 후보를 새 파일로 만들면 변경 이력이 안전하다.
