# Global Harness v5 Phase 5 Pilot-First Decision Note

- 작성일: 2026-06-06
- 상태: Phase 5 부분 결정 note. v5 core 또는 module template 자체가 아니다.
- 검토 범위:
  - `global-harness-core-structure-template-v1.md` Section 12 `Available Modules Registry`
  - `global-harness-candidate-ledger-template-v0.md`
  - `global-harness-v5-core-scope-consensus-2026-06-06.md`
  - `global-harness-template-v5-discussion-handoff-2026-06-06.md`
  - `global-harness-v5-work-map.md`

## 1. 결정

`pilot-first / testing`은 지금 별도 module v0 파일로 만들지 않는다.

현재 v5 후보 단계에서는 아래 두 곳에 묶어 처리한다.

| 위치 | 역할 |
|---|---|
| `candidate-ledger` | 새 source, 새 schema, 새 분류, 새 상태값, 새 예외를 바로 정식화하지 않고 후보로 기록한다. |
| Phase 7 pilot validation | 후보 구조를 실제 하네스에 작게 적용해보고 운영 반영 여부를 판단한다. |

따라서 v1 registry에는 `pilot-first / testing`을 발견 가능한 항목으로 남기되, 현재 후보 파일은 "별도 파일 없음"으로 표시한다.

## 2. 이유

`pilot-first`는 단독 템플릿보다 운영 원칙에 가깝다.

핵심 질문은 아래다.

- 새 source를 바로 운영 수집 범위에 넣어도 되는가?
- 새 자동화를 바로 기본 절차로 넣어도 되는가?
- 새 schema 변경을 곧장 production schema에 반영해도 되는가?
- 작은 검증 없이 큰 구조 변경을 하면 어떤 drift가 생기는가?

이 질문들은 별도 파일 하나로만 해결되기보다, 후보 원장과 pilot 검증 흐름이 함께 있어야 작동한다.

## 3. 별도 module로 만들지 않는 이유

| 이유 | 설명 |
|---|---|
| 현재 v0 범위가 작다 | 아직 반복 실행을 통해 독립 testing template가 필요한지 검증되지 않았다. |
| candidate-ledger와 겹친다 | 새 분류, 상태값, source, schema 후보를 기록한다는 점에서 candidate-ledger와 강하게 연결된다. |
| Phase 7과 겹친다 | 실제 작게 검증하고 전역화 여부를 판단하는 일은 Phase 7 pilot validation이 담당한다. |
| core registry를 과하게 늘리지 않는다 | 아직 파일이 없는 module을 확정 파일처럼 보이게 만들 필요가 없다. |

## 4. 나중에 별도 module로 분리할 조건

아래 조건이 반복되면 `pilot-testing` 또는 `testing-evolution` module을 별도 후보로 만들 수 있다.

- 여러 하네스에서 pilot plan 형식이 반복된다.
- pilot 결과를 기록하는 공통 schema가 필요해진다.
- 새 자동화나 새 source를 검증하는 절차가 candidate-ledger만으로 부족하다.
- pilot과 production의 산출물 분리가 자주 문제가 된다.
- 사용자 승인 전/후 운영 반영 절차가 여러 하네스에서 반복된다.

## 5. v1 registry 반영 내용

`pilot-first / testing` row는 아래 의미로 조정한다.

```md
| pilot-first / testing | 별도 파일 없음. `candidate-ledger`와 Phase 7 pilot validation에서 처리 | 새 source, 새 자동화, 새 schema 변경을 운영 반영 전에 후보로 기록하고 작게 검증할 때 |
```

## 6. 다음 단계

이 note를 반영한 뒤 Phase 5의 "`pilot-first`를 별도 module로 둘지 `candidate-ledger/testing`에 묶을지 결정" 작업은 완료로 본다.

다음 작업은 `comparison mode`를 별도 module로 둘지 `qa-scaffold`에 포함할지 결정하는 것이다.
