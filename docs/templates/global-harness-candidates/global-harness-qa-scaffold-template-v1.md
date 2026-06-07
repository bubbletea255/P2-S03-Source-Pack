# Global Harness QA Scaffold Template v1

- 작성일: 2026-06-07
- 기반: Source Pack QA procedure/rubric에서 전역 QA scaffold로 일반화
- 기준 문서: `global-harness-v5-phase6-qa-scaffold-scope-note-2026-06-07.md`
- 상태: 전역 하네스 후보 module. 아직 전역 `harness-lab` 반영 아님.

## 1. 목적

이 template는 새 하네스가 자기 산출물에 맞는 QA 절차와 결과 파일을 만들 수 있도록 돕는 전역 후보 module이다.

목표는 모든 하네스에 같은 QA 질문을 강제하는 것이 아니다.
하네스마다 산출물, schema, rubric, 금지 조건이 다르므로, 이 module은 공통 QA 범주와 기록 구조만 제공한다.

핵심 문장:

```text
qa-scaffold는 하네스별 QA를 대신 쓰는 문서가 아니라,
각 하네스가 자기 QA procedure, schema, rubric을 만들 때 빠뜨리면 안 되는 공통 뼈대다.
```

## 2. 범위와 비범위

이 module이 다루는 것:

- 공통 QA 항목 범주
- 공통 상태값 후보와 의미
- `partial_pass`와 `unverified`의 구분
- QA 결과 파일의 최소 skeleton
- repair/recheck 최소 흐름
- escalation 표시 방식
- `comparison`과 `type-schema` 연결 기준

이 module이 다루지 않는 것:

- 하네스별 상세 QA 절차
- 특정 schema field 검사
- rubric 점수 가중치
- 자동 실패 조건의 구체 목록
- 운영 원장 상태 매핑
- 승인 절차 수행
- 보안 판단 상세

하네스별 세부 검증은 각 하네스의 `harness/procedures/`, `harness/schemas/`, `harness/rubrics/`, contract, runbook에서 정한다.

## 3. QA 설계 흐름

새 하네스에 QA를 붙일 때는 아래 순서로 설계한다.

1. QA 대상과 범위를 정한다.
2. 필수 입력, 필수 출력, 중간 산출물, 최종 산출물을 확인한다.
3. 공통 QA 항목 범주 중 해당 하네스에 필요한 범주를 고른다.
4. 각 범주 안에서 하네스별 실제 QA 질문을 만든다.
5. 하네스별 schema와 rubric이 있으면 검증 기준으로 연결한다.
6. 전체 QA 상태와 finding-level 상태를 구분한다.
7. QA 결과를 `qa.md` 또는 이에 준하는 파일로 남긴다.
8. repair/recheck가 필요한 항목을 기록한다.
9. 승인, 중단, 보안 검토가 필요하면 Escalation에 표시한다.
10. 다음 하네스나 사람이 사용할 수 있는지 `Downstream Use`에 기록한다.

QA는 대화 보고로 끝내지 않는다.
후속 사용에 영향을 주는 결과는 파일로 남긴다.

## 4. 공통 QA 항목 범주

v1은 고정 QA 질문이 아니라 공통 범주를 제공한다.
각 하네스는 아래 범주 안에서 자기 산출물에 맞는 실제 질문을 만든다.

| 범주 | 확인할 것 | 하네스별로 정할 것 |
|---|---|---|
| completeness | 필수 입력, 출력, 섹션, 기록이 빠지지 않았는가 | 필수 산출물과 필수 섹션 |
| placement | 산출물이 약속된 위치에 저장됐는가 | 저장 경로와 파일명 규칙 |
| traceability | 입력, 근거, 중간 산출물, 최종 산출물이 연결되는가 | 연결해야 할 record, log, index |
| schema/rubric conformance | 하네스별 schema와 rubric을 따르는가 | schema field, 허용값, rubric 항목 |
| violation | 금지 조건, 도메인 금지사항, 보안/권한 위반이 없는가 | 금지 문구, 금지 행동, 자동 실패 조건 |
| downstream readiness | 다음 하네스나 사람이 읽고 사용할 수 있는가 | 후속 입력 계약과 주의사항 |

`approval/stop`은 QA 항목 범주가 아니다.
승인 필요와 중단 필요는 QA 결과에서 도출되는 escalation 조건이다.

## 5. 상태값 모델

### 5.1 전체 QA 상태

`overall_status`는 전체 QA 결과의 상태다.

| 상태 | 의미 |
|---|---|
| `pass` | 필수 QA가 통과했고 후속 사용 가능하다. |
| `partial_pass` | 검증은 수행됐고 핵심 사용은 가능하지만 일부 보완이나 주의가 필요하다. |
| `unverified` | 검증 자체가 충분히 수행되지 않아 후속 사용 전 확인이 필요하다. |
| `fail` | 후속 사용 위험이 크거나 필수 조건이 깨졌다. |
| `stopped` | 승인, 외부 조건, 도구, 전제 조건 문제로 QA 전체를 완료하지 못했다. |

### 5.2 finding-level 상태

개별 finding도 상태를 가질 수 있다.
전체 `overall_status`와 개별 `finding_status`를 섞지 않는다.

| 상태 | 의미 |
|---|---|
| `pass` | 해당 항목은 통과했다. |
| `partial_pass` | 해당 항목은 일부 보완이 필요하지만 핵심 사용을 막지는 않는다. |
| `unverified` | 해당 항목은 충분히 검증되지 않았다. |
| `fail` | 해당 항목은 필수 조건을 깨거나 후속 사용 위험이 있다. |
| `stopped` | 해당 항목은 전제 조건 미충족, 권한, 도구 문제 등으로 검증을 끝내지 못했다. |

`repair_required`는 전체 QA 상태가 아니다.
`repair_required`는 개별 finding 또는 repair/recheck action에서 사용하는 조치 상태다.

## 6. `partial_pass`와 `unverified` 구분

`partial_pass`와 `unverified`는 서로 다르다.

| 상태 | 핵심 차이 | repair/recheck 방향 |
|---|---|---|
| `partial_pass` | 검증은 됐고 핵심 산출물은 사용할 수 있지만 일부 문제가 있다. | 문제 항목을 수정한 뒤 해당 항목을 재검증한다. |
| `unverified` | 검증 조건이 부족해 충분히 판단하지 못했다. | 입력, 도구, 권한, 기준 등 검증 조건을 먼저 확보한 뒤 QA를 다시 실행한다. |

`partial_pass`는 "확인했고 일부 부족함"이다.
`unverified`는 "충분히 확인하지 못함"이다.

후속 하네스가 읽으면 위험한 경우에는 보수적으로 `unverified` 또는 `fail`을 우선한다.

## 7. Repair / Recheck 흐름

repair loop는 하네스별 실제 수정 절차를 전역에서 강제하지 않는다.
다만 최소 기록 흐름은 아래를 따른다.

```text
finding 발견
-> evidence 기록
-> required_action 지정
-> owner_or_location 지정
-> recheck_needed 여부 표시
-> 재검증 결과 기록
```

상태별 기본 처리:

| finding 상태 | 기본 처리 |
|---|---|
| `pass` | 별도 repair 없음 |
| `partial_pass` | 문제가 있는 항목만 수정하고 부분 재검증 |
| `unverified` | 검증 조건을 확보한 뒤 QA 재실행 또는 해당 범주 재검증 |
| `fail` | 필수 조건 파손이면 수정 전 후속 사용 금지 |
| `stopped` | 중단 원인을 해소한 뒤 같은 체크를 다시 수행 |
| `repair_required` | action 상태로 사용. 전체 QA 상태로 쓰지 않음 |

repair/recheck 결과도 파일에 남긴다.
고쳤다는 대화 보고만으로 완료 처리하지 않는다.

## 8. QA 결과 파일 최소 skeleton

QA 결과는 `qa.md` 또는 이에 준하는 파일에 남긴다.
파일명과 위치는 하네스별 runbook, MANIFEST, 산출물 계약을 따른다.

최소 skeleton:

```md
# QA Result - {harness/run}

overall_status: pass | partial_pass | unverified | fail | stopped
qa_date: YYYY-MM-DD
target:
qa_scope:
inputs_checked:
outputs_checked:

## Summary

finding_counts:
- pass:
- partial_pass:
- unverified:
- fail:
- stopped:

## Findings

| category | finding_status | evidence | action |
|---|---|---|---|

## Repair / Recheck

| item | finding_status | required_action | owner_or_location | recheck_needed |
|---|---|---|---|---|

## Escalation

human_approval_needed:
stop_required:
security_review_needed:
approval_gate_reference:

## Downstream Use

| downstream | usable | caution |
|---|---|---|
```

`Summary`는 상태 집계만 둔다.
`human_approval_needed`, `stop_required`, `security_review_needed`는 `Escalation`에 둔다.

하네스별 QA procedure는 이 skeleton을 확장할 수 있다.
다만 QA 결과가 대화에만 남으면 안 된다.

## 9. Escalation 표시 방식

`qa-scaffold`는 승인 절차를 수행하지 않는다.
QA 결과에서 승인, 중단, 보안 검토가 필요하다는 사실을 표시하고, 실제 판단은 해당 module이나 하네스별 runbook으로 넘긴다.

| escalation 필드 | 의미 | 넘길 위치 |
|---|---|---|
| `human_approval_needed` | 사람이 승인해야 할 조치가 있다. | `approval-gate` |
| `stop_required` | 전체 실행, 운영 반영, 후속 사용을 멈춰야 한다. | `approval-gate`, runbook |
| `security_review_needed` | secret, credential, `.env`, 공개 범위, 격리 파일 이슈가 있다. | `security-baseline` |
| `approval_gate_reference` | 어떤 승인 판단이 필요한지 한 줄로 적거나 관련 결정 파일, run note, work map 항목 경로를 적는다. | `approval-gate` |

`stopped`는 두 층에서 구분한다.

| 위치 | 의미 |
|---|---|
| finding-level `stopped` | 특정 QA 항목을 끝까지 검증하지 못했다. |
| Escalation `stop_required` | 전체 실행, 운영 반영, 후속 사용을 멈춰야 한다. |

finding-level `stopped`는 전체 `stop_required`를 자동 trigger하지 않는다.
`stop_required`는 영향 범위, 필수 산출물 여부, 보안/승인 위험을 보고 별도로 판단한다.

`approval_gate_reference`는 엄격한 schema가 아니라 loose reference다.

예시:

```text
운영 registry 반영 전 사용자 승인 필요
docs/templates/global-harness-candidates/global-harness-approval-gate-template-v1.md Section 4 Level 3
artifacts/runs/{run-id}/run-summary.md 의 운영 반영 후보
```

## 10. comparison 연결

`comparison`은 별도 파일 없이 v1 registry에 등재된 연결 항목이다.
`qa-scaffold`는 comparison의 전체 실행 구조를 맡지 않고, 같은 기준으로 평가하는 부분만 맡는다.

`qa-scaffold`가 맡을 것:

- 두 실행 결과를 같은 QA 범주로 평가한다.
- 같은 rubric 또는 같은 finding table 구조를 사용하게 한다.
- 차이를 `Findings`, `Repair / Recheck`, `Downstream Use`에 기록할 수 있게 한다.

`qa-scaffold`가 맡지 않을 것:

- comparison 실행 구조 결정
- Codex/Claude Code 병렬 실행 절차
- 어떤 결과를 운영 반영할지 승인
- 비교 결과 기록 저장 위치 전체

연결 기준:

| 역할 | 위치 |
|---|---|
| comparison 필요 여부 | `design-preflight` |
| 공통 평가 기준 | `qa-scaffold` |
| 운영 반영 승인 | `approval-gate` |
| 비교 결과 기록 | `observability` 또는 하네스별 run 기록 |

## 11. type-schema 연결

`type-schema`는 별도 파일 없이 v1 registry에 등재된 연결 항목이다.
`qa-scaffold`는 품질 축을 새로 정하지 않고, 정해진 품질 축과 schema/rubric을 검증 기준으로 구체화한다.

경계:

| 역할 | 위치 |
|---|---|
| 하네스 유형과 품질 축 판단 | `design-preflight` |
| 실제 schema 파일 | 각 하네스의 `harness/schemas/` |
| 실제 rubric 파일 | 각 하네스의 `harness/rubrics/` |
| 검증 기준 구조화 | `qa-scaffold` |

주의:

```text
qa-scaffold는 품질 축을 정하지 않는다.
품질 축은 design-preflight에서 결정하고,
qa-scaffold는 그것을 QA 검증 기준과 결과 기록 구조로 번역한다.
```

## 12. 전역화 기준과 제외 항목

이 section은 새 하네스 적용자가 무엇을 전역 scaffold에서 가져오고, 무엇을 하네스별로 직접 정해야 하는지 확인하기 위한 경계표다.

| 항목 | 전역 scaffold가 제공하는 것 | 하네스별로 정할 것 |
|---|---|---|
| QA 결과 파일 | 최소 skeleton | 실제 파일명, 위치, 섹션, 세부 단계 |
| 상태값 | 공통 후보와 의미 | 운영 원장, run 상태, index 상태와의 매핑 |
| 검증 범주 | 6개 공통 범주 | 실제 QA 질문과 자동 실패 조건 |
| schema/rubric | 연결 기준 | 실제 schema field, 허용값, rubric 가중치 |
| repair/recheck | 최소 기록 흐름 | 실제 수정 절차와 재검증 조건 |
| downstream use | 사용 가능 여부 기록 방식 | 다음 하네스별 입력 계약과 주의사항 |

전역 scaffold는 특정 도메인의 상세 QA 절차를 복사하지 않는다.
상세 절차는 하네스별 `harness/procedures/`, `harness/schemas/`, `harness/rubrics/`, contract, runbook에서 정한다.

## 13. 적용 전 체크리스트

새 하네스에 이 module을 적용하기 전에 확인한다.

- [ ] QA 대상과 범위를 정했다.
- [ ] QA 결과를 저장할 파일 위치를 정했다.
- [ ] 필수 입력, 필수 출력, 중간 산출물, 최종 산출물을 확인했다.
- [ ] 공통 QA 범주 중 해당 하네스에 필요한 범주를 골랐다.
- [ ] 각 범주 안의 실제 QA 질문은 하네스별 procedure에서 정했다.
- [ ] schema와 rubric이 필요하면 각 하네스의 `harness/schemas/`와 `harness/rubrics/`에 둔다.
- [ ] `partial_pass`와 `unverified`를 구분하기로 했다.
- [ ] `repair_required`를 전체 QA 상태가 아니라 finding/action 상태로 쓰기로 했다.
- [ ] finding-level `stopped`와 전체 `stop_required`를 구분했다.
- [ ] 승인 필요, 중단 필요, 보안 검토 필요는 `Escalation`에 표시한다.
- [ ] 실제 승인 절차는 `approval-gate`를 따른다.
- [ ] 보안 이슈는 `security-baseline`을 따른다.
- [ ] comparison이 필요하면 같은 rubric 또는 같은 finding table 구조를 사용한다.
- [ ] type-schema 연결 시 품질 축은 `design-preflight`에서 정하고, qa-scaffold는 검증 기준으로 번역한다.
- [ ] Source Pack 같은 특정 도메인 QA 절차를 전역 template에 그대로 복사하지 않는다.
