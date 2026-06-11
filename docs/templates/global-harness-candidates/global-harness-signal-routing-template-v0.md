# Global Harness Signal Routing Template v0

- 작성일: 2026-06-08
- 기반: `global-harness-v5-phase7-signal-routing-scope-note-2026-06-08.md`
- 상태: v0 후보. 전역 적용 확정 아님.

## 1. 목적

이 template는 하네스 안에서 발생하는 알림, 경고, 에스컬레이션, 개선 후보 신호를 공통 언어로 표현하기 위한 전역 후보 module이다.

`signal-routing`의 목적은 보안, QA, docs 정리, observability, candidate-ledger가 각자 다른 방식으로 신호를 표현하지 않도록 최소한의 routing 계약을 제공하는 것이다.

핵심 목표:

- 각 module이 발생시키는 signal을 공통 필드로 표현한다.
- signal의 긴급도, 처리 위치, 기록 위치, 사용자 노출 기준을 분리한다.
- 사용자에게 즉시 보여야 하는 위험 신호가 silent log에만 묻히지 않게 한다.
- secret, credential, 비공개 원문 같은 민감값을 알림에 노출하지 않는다.
- 실제 알림 UI나 저장 파일 구조는 adapter와 하네스별 정책에 맡긴다.

짧게 말하면:

```text
각 module = 실제 signal의 의미와 trigger를 소유
signal-routing = severity, route, record, user visibility 문법을 소유
adapter = 실제 표시 방식과 알림 채널을 소유
```

## 2. 비범위

`signal-routing`은 아래를 하지 않는다.

| 비범위 | 이유 |
|---|---|
| 중앙 알림 시스템 구현 | Codex, Claude Code, CLI, CI, app UI마다 알림 방식이 다르다. |
| 모든 module의 signal 목록 중앙 관리 | 새 module이 생길 때마다 signal-routing을 수정하게 되면 결합도가 커진다. |
| 별도 `signals.jsonl` 강제 | 작은 하네스에는 새 artifacts 부담이 과하다. 기존 run-summary, QA 결과, candidate-ledger를 활용할 수 있어야 한다. |
| security, QA, docs, observability, candidate 판단 재정의 | 각 module이 자기 domain의 trigger와 의미를 소유한다. |
| 사용자 승인 절차 수행 | 승인 수준과 실제 승인 흐름은 `approval-gate`가 맡는다. |
| 보안 판단 수행 | secret, 격리 파일, 공개 범위 판단은 `security-baseline`이 맡는다. |
| QA pass/fail 판정 수행 | QA finding과 상태값은 `qa-scaffold`가 맡는다. |

이 module은 얇은 계약으로 유지한다.
signal-routing이 모든 알림을 중앙에서 소유하면 각 module이 독립적으로 발전하기 어렵다.

## 3. signal model

signal-routing은 signal을 네 축으로 나눈다.

| 축 | 질문 | 예시 |
|---|---|---|
| `severity` | 얼마나 급한가? | `critical`, `action_required`, `maintenance`, `info` |
| `route_to` | 누가 능동적으로 처리 판단을 맡는가? | `approval-gate`, `security-baseline`, `candidate-ledger` |
| `record_in` | 어디에 수동으로 기록할 것인가? | `run-summary`, `qa-result`, `work-map` |
| `user_visibility` | 사용자에게 언제 보일 것인가? | `immediate`, `run_summary`, `log_only` |

이 네 축을 섞지 않는다.

특히 `candidate`와 `improvement`는 severity가 아니다.
이들은 "얼마나 급한가"보다 "어디로 보내고 어떻게 기록할 것인가"에 가깝다.

예시:

```yaml
source_module: observability
signal_key: repeated_bottleneck_detected
severity: maintenance
route_to:
  - candidate-ledger
record_in:
  - run-summary
  - candidate-ledger
user_visibility: run_summary
```

## 4. severity 어휘

v0에서는 severity를 작게 시작한다.

| severity | 의미 | 일반 처리 |
|---|---|---|
| `critical` | 즉시 멈춤 또는 즉시 사용자 확인이 필요한 위험 | `user_visibility: immediate`, 필요 시 `stop_required: true` |
| `action_required` | 사람이 승인, 선택, 조치를 해야 하는 상태 | `approval-gate`, QA escalation, 사용자 확인으로 연결 |
| `maintenance` | 지금 당장 중단은 아니지만 정리, 관리, 후속 점검이 필요한 상태 | run-summary, work map, candidate-ledger, docs 정리 후보로 연결 |
| `info` | 참고용 정보 | 기록만 하거나 실행 요약에 남김 |

제외한 severity:

| 제외 후보 | 제외 이유 | 대체 표현 |
|---|---|---|
| `candidate` | candidate-ledger module 이름과 충돌하고, 긴급도가 아니라 처리 방향이다. | `severity: maintenance` 또는 `info`, `route_to: candidate-ledger` |
| `improvement` | 긴급도가 아니라 개선 성격이다. | `severity: maintenance`, `route_to: observability` 또는 `candidate-ledger` |
| `security` | domain/kind이지 긴급도가 아니다. | `route_to: security-baseline`, 필요 시 `severity: critical` |
| `qa` | domain/kind이지 긴급도가 아니다. | `route_to: qa-scaffold` |

## 5. user_visibility

`user_visibility`는 signal이 사용자에게 언제 보여야 하는지 정한다.

| user_visibility | 의미 | 사용 예 |
|---|---|---|
| `immediate` | 실행 중 즉시 사용자에게 표시한다. | secret 의심, 승인되지 않은 공개 위험, 중단 필요 |
| `run_summary` | 실행 종료 시 run-summary나 명시적 요약에 포함한다. | docs 정리 필요, 반복 병목, candidate review 후보 |
| `log_only` | 사용자가 능동적으로 보지 않아도 되는 기록으로 남긴다. | 참고 정보, 낮은 중요도의 내부 관찰 |

`critical` signal은 원칙적으로 `immediate`를 사용한다.
다만 실제 표시 채널은 adapter가 정한다.

`maintenance` signal은 보통 `run_summary`를 사용한다.
단, 정리하지 않으면 다음 하네스가 잘못 읽을 위험이 있으면 severity를 `action_required`로 올리고 `user_visibility: immediate`로 함께 조정할 수 있다.

## 6. safe notification 원칙

signal이 사용자에게 보여질 때도 실제 민감값을 노출하지 않는다.

공통 원칙:

1. secret, credential, token, password, session cookie 값을 signal에 포함하지 않는다.
2. 비공개 대화 원문이나 긴 사용자 붙여넣기 전문을 signal에 포함하지 않는다.
3. 값 대신 유형, 안전한 위치, 안전한 참조를 쓴다.
4. 자동 삭제, 이동, 복구, 덮어쓰기, 공개 전환을 하지 않았음을 필요 시 알린다.
5. 필요한 다음 조치와 사용자 확인 지점을 함께 알린다.
6. 구체적인 문구, UI, 경고창 형식은 adapter나 하네스별 UI가 정한다.

권장 표현:

```text
safe_summary: 민감 정보로 보이는 값이 발견됨
safe_reference: docs/example.md Section 3
```

피해야 할 표현:

```text
safe_summary: API key abc123...가 발견됨
```

## 7. signal envelope

v0는 signal log 파일을 강제하지 않는다.
다만 signal을 표현할 때 아래 필드 구조를 기준으로 삼는다.

```yaml
source_module:
signal_key:
signal_id:
severity:
route_to:
record_in:
user_visibility:
safe_summary:
safe_reference:
evidence_ref:
recommended_action:
stop_required:
approval_required:
created_at:
```

필드 의미:

| 필드 | 의미 |
|---|---|
| `source_module` | signal을 발생시킨 module 이름 |
| `signal_key` | source module 안에서 고유한 signal 이름 |
| `signal_id` | 전역 식별자. `{source_module}.{signal_key}` 형식 권장 |
| `severity` | 처리 강도 |
| `route_to` | 능동적으로 판단하거나 처리할 module 목록 |
| `record_in` | signal을 남길 기록 위치 목록 |
| `user_visibility` | 사용자 노출 수준 |
| `safe_summary` | 민감값 없는 한 줄 요약 |
| `safe_reference` | 안전한 파일 경로, section, run-id, note 위치 |
| `evidence_ref` | 더 자세한 근거를 볼 수 있는 안전한 참조 |
| `recommended_action` | 다음 조치 후보 |
| `stop_required` | 전체 실행, 운영 반영, 후속 사용 중단 필요 여부 |
| `approval_required` | 사용자 승인 필요 여부 |
| `created_at` | signal 생성 시점 |

`route_to`와 `record_in`은 리스트를 허용한다.
하나의 signal이 여러 module로 처리되고 여러 artifact에 기록될 수 있기 때문이다.

예시:

```yaml
source_module: security-baseline
signal_key: secret_possible_detected
signal_id: security-baseline.secret_possible_detected
severity: critical
route_to:
  - approval-gate
record_in:
  - run-summary
user_visibility: immediate
safe_summary: 민감 정보로 보이는 값이 발견됨
safe_reference: docs/example.md Section 4
evidence_ref: security review note
recommended_action: 사용자 확인 전 자동 삭제, 이동, 복구, 공개를 하지 않는다.
stop_required: true
approval_required: true
created_at: 2026-06-08
```

## 8. `route_to`와 `record_in` 경계

`route_to`에는 판단, 추적, 승인처럼 능동적 처리를 수행하는 module을 기입한다.
`record_in`에는 run-summary, QA 결과 파일, work map처럼 수동으로 기록되는 artifact를 기입한다.

| 항목 | 분류 | 이유 |
|---|---|---|
| `approval-gate` | `route_to` | 승인 수준과 실행/중단 판단을 수행한다. |
| `security-baseline` | `route_to` | 보안 위험 여부와 do-not-proceed 조건을 판단한다. |
| `qa-scaffold` | `route_to` | QA escalation 구조와 finding 상태를 판단한다. |
| `candidate-ledger` | `route_to` | 후보 lifecycle, review trigger, 승격/폐기/보류를 능동적으로 관리한다. |
| `docs-organization` | `route_to` | 문서 정리 필요성과 이동 전후 점검을 판단한다. |
| `observability` | `route_to` | 병목, trim 후보, 운영 관찰을 판단한다. |
| `run-summary` | `record_in` | 실행 종료 시 기록되는 artifact다. |
| `qa-result` | `record_in` | QA 판단 결과가 저장되는 위치다. |
| `work-map` | `record_in` | 작업 추적 위치다. |

`candidate-ledger`는 특수하다.
`route_to: candidate-ledger`는 후보 lifecycle 판단을 맡긴다는 뜻이다.
`record_in: candidate-ledger`는 후보 record/evidence 저장 위치로 남긴다는 뜻이다.

## 9. module별 signal 소유 경계

각 module은 자기 domain의 실제 signal 이름, 의미, evidence, trigger 조건을 소유한다.
signal-routing은 그 signal을 표현하는 문법만 제공한다.

| module | module이 소유할 것 | signal-routing이 제공할 것 |
|---|---|---|
| `security-baseline` | secret 의심, 격리 파일, 외부 공개 위험, 자동 해결 금지 조건 | `critical`, `immediate`, safe notification, approval route |
| `qa-scaffold` | finding 상태, QA escalation, `human_approval_needed`, `stop_required`, `security_review_needed` | escalation을 signal envelope로 표현하는 방법 |
| `approval-gate` | 승인 수준, 실행/논의 경계, 위험 작업 승인 조건 | `approval_required` signal의 route 기준 |
| `docs-organization` | docs 복잡화, current/historical 혼재, 이동 전후 참조 점검 필요 | `maintenance`, `run_summary`, docs route 기준 |
| `observability` | bottleneck, trim candidate, 반복 실행 부담, 자동화 후보 | maintenance signal과 candidate-ledger 연결 기준 |
| `candidate-ledger` | 후보 record/evidence, review trigger, lifecycle 전환 | candidate review signal의 route와 record 기준 |
| `checkpoint` | 저장 실패, 보안 관련 checkpoint 예외, anchor 복구 실패 | 필요 시 `action_required` 또는 `maintenance` 표현 |
| `comparison` | 비교 결과 차이, 운영 반영 승인 필요, 공통 rubric 적용 필요 | no-file 항목으로 QA/approval/observability route 기준 |

module이 새 signal을 추가할 때마다 signal-routing을 수정할 필요는 없다.
다만 새 signal이 기존 severity나 user_visibility로 표현되지 않으면 signal-routing v1 승격 후보로 기록한다.

## 10. adapter 책임

adapter는 실제 사용자 노출 방식을 정한다.

가능한 채널:

- Codex 대화 메시지
- Claude Code 대화 메시지
- CLI console 출력
- CI failure message
- app modal, toast, banner
- run-summary의 명시적 blocked 또는 warning section

signal-routing은 아래를 강제하지 않는다.

- 정확한 문장 format
- UI component 종류
- modal, toast, banner 사용 여부
- CLI 색상
- CI exit code
- 별도 signal log 파일 위치

adapter는 signal-routing의 원칙을 지켜야 한다.

- `user_visibility: immediate`는 사용자가 즉시 볼 수 있는 채널로 보여준다.
- 민감값은 표시하지 않는다.
- `stop_required: true`인 signal은 계속 진행하지 않는다.
- `approval_required: true`인 signal은 approval-gate 기준을 따른다.

## 11. 다른 module과의 연결

`signal-routing`은 다른 module을 대체하지 않는다.
각 module 사이에 signal이 어떻게 이동하고 기록되는지를 정리한다.

| 연결 대상 | signal-routing에서 다루는 것 | 상대 module이 다루는 것 |
|---|---|---|
| `security-baseline` | 보안 위험 signal을 `critical`, `immediate`, safe notification으로 표현 | secret, credential, 격리 파일, 공개 범위 판단 |
| `approval-gate` | `approval_required` 또는 `stop_required` signal을 approval로 route | 승인 수준과 실행/중단 판단 |
| `qa-scaffold` | QA escalation을 signal envelope로 표현 | finding 상태, QA 결과 skeleton, repair/recheck |
| `observability` | 반복 병목과 trim 후보를 maintenance signal로 표현 | run-summary의 운영 관찰 필드 |
| `candidate-ledger` | review trigger와 개선 후보를 route/record로 표현 | 후보 record/evidence와 lifecycle |
| `docs-organization` | docs 복잡화와 정리 필요를 maintenance signal로 표현 | docs 위치, 색인, 이동 전후 참조 점검 |
| `checkpoint` | 저장 실패나 보안 관련 checkpoint 예외를 signal로 표현 | compact checkpoint, anchor, session handoff |
| `comparison` | 모델 간 비교 결과의 차이와 운영 반영 승인 필요를 signal로 표현 | no-file 항목. design/QA/approval/observability 연결 |

## 12. 적용 전 체크리스트

새 하네스에 이 module을 적용하기 전에 확인한다.

- [ ] signal-routing을 중앙 알림 시스템이 아니라 얇은 signal 문법으로 적용한다.
- [ ] 각 module이 자기 signal의 실제 trigger와 의미를 소유한다고 정했다.
- [ ] `severity`, `route_to`, `record_in`, `user_visibility`를 섞지 않는다.
- [ ] severity는 `critical`, `action_required`, `maintenance`, `info`로 작게 시작한다.
- [ ] `candidate`와 `improvement`를 severity로 쓰지 않는다.
- [ ] `route_to`에는 능동 handler module을 둔다.
- [ ] `record_in`에는 기록 artifact 또는 기록 위치를 둔다.
- [ ] `candidate-ledger`가 `route_to`와 `record_in`에 모두 쓰일 수 있음을 구분했다.
- [ ] `user_visibility: immediate` signal은 사용자가 즉시 볼 수 있는 채널로 노출한다.
- [ ] secret, credential, token, 비공개 원문을 signal에 직접 쓰지 않는다.
- [ ] `signal_key`는 `source_module` 안에서 고유하게 둔다.
- [ ] `signal_id`는 `{source_module}.{signal_key}` 형식을 기본으로 둔다.
- [ ] 별도 signal log 파일을 처음부터 강제하지 않는다.
- [ ] 실제 UI, CLI, CI, 대화 메시지 형식은 adapter가 정한다.
- [ ] `stop_required: true`나 `approval_required: true`는 approval-gate 기준을 따른다.
