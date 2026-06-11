# Phase 7-0 Signal Routing Scope Note

- 작성일: 2026-06-08
- 대상: `global-harness-signal-routing-template-v0.md` 후보 작성 전 범위 합의
- 기준 문서:
  - `global-harness-v5-phase7-planning-consensus-note-2026-06-07.md`
  - `global-harness-v5-work-map.md`
  - `global-harness-security-baseline-template-v1.md`
  - `global-harness-qa-scaffold-template-v1.md`
  - `global-harness-docs-organization-template-v1.md`
  - `global-harness-observability-template-v1.md`
  - `global-harness-candidate-ledger-template-v1.md`
- 상태: Claude Code 교차검증 PASS. v0 후보 작성 기준으로 사용.

## 1. 목적

이 note는 Phase 7-0에서 새로 만들 `signal-routing` module의 범위를 정한다.

`signal-routing`의 목적은 보안, QA, docs 정리, observability, candidate-ledger가 각자 다른 방식으로 알림, 경고, 에스컬레이션을 표현하지 않도록 공통 신호 언어를 제공하는 것이다.

핵심 목표:

- 각 module이 발생시키는 signal을 공통 필드로 표현한다.
- signal의 긴급도, 처리 위치, 기록 위치, 사용자 노출 기준을 분리한다.
- 사용자에게 즉시 보여야 하는 위험 신호가 silent log에만 묻히지 않게 한다.
- secret, credential, 비공개 원문 같은 민감값을 알림에 노출하지 않는다.
- Phase 7-A Source Pack 소급 검증 전에 최소 routing 계약을 확보한다.

## 2. signal-routing의 역할

`signal-routing`은 알림 기능 전체를 구현하는 module이 아니다.

이 module은 하네스 전체에서 signal을 표현하는 문법을 제공한다.

| 역할 | 설명 |
|---|---|
| 공통 어휘 제공 | `severity`, `user_visibility` 같은 공통 값을 정의한다. |
| routing 계약 제공 | signal을 어떤 module이 처리하고 어디에 기록할지 구분한다. |
| 안전한 알림 원칙 제공 | 민감값을 사용자 알림, 로그, QA finding, run summary에 직접 쓰지 않는 기준을 제공한다. |
| adapter 경계 제공 | 실제 UI, 대화 메시지, CLI 출력, CI failure, app toast는 adapter나 하네스별 UI가 정하게 한다. |
| Phase 7 검증 기준 제공 | Source Pack 소급 검증에서 "이 신호가 사용자에게 보여야 했는가"를 검토할 기준을 제공한다. |

짧게 말하면:

```text
각 module = 실제 signal의 의미와 trigger를 소유
signal-routing = severity, route, record, user visibility 문법을 소유
adapter = 실제 표시 방식과 알림 채널을 소유
```

## 3. 비범위

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

`signal-routing`이 모든 알림을 중앙에서 소유하면 각 module이 독립적으로 발전하기 어렵다.
따라서 이 module은 얇은 계약으로 유지한다.

## 4. signal model

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

### route_to와 record_in의 경계

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

## 5. severity 어휘

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

## 6. user_visibility 3단계

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

## 7. safe notification 원칙

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

## 8. signal envelope 최소 필드

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

## 9. signal_key와 signal_id 고유성 기준

`signal_key`는 `source_module` 범위 안에서 고유하다.
전역 식별자는 `{source_module}.{signal_key}` 조합으로 만든다.

예시:

| source_module | signal_key | signal_id |
|---|---|---|
| `security-baseline` | `secret_possible_detected` | `security-baseline.secret_possible_detected` |
| `docs-organization` | `docs_structure_complexity` | `docs-organization.docs_structure_complexity` |
| `observability` | `repeated_bottleneck_detected` | `observability.repeated_bottleneck_detected` |
| `candidate-ledger` | `review_trigger_met` | `candidate-ledger.review_trigger_met` |
| `qa-scaffold` | `qa_escalation_required` | `qa-scaffold.qa_escalation_required` |

`signal_key`는 전역에서 단독으로 고유할 필요는 없다.
다만 같은 `source_module` 안에서는 같은 이름이 다른 의미로 재사용되면 안 된다.

## 10. module별 signal 소유 경계

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

## 11. adapter 책임

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

## 12. v0 한계와 v1 승격 조건

`signal-routing`은 새 module이므로 v0로 시작한다.
v0는 공통 signal 문법의 최소 후보일 뿐, 완성된 전역 표준이 아니다.

v0 한계:

- 아직 Source Pack 소급 검증으로만 확인되지 않았다.
- Industry Primer 실전 pilot에서의 사용성은 검증되지 않았다.
- 모든 하네스 유형의 signal을 커버한다고 볼 수 없다.
- 별도 signal log가 필요한지 아직 결정하지 않는다.
- adapter별 실제 UI 구현 방식은 다루지 않는다.

v1 승격 조건 후보:

| 조건 | 확인 방법 |
|---|---|
| Phase 7-A에서 최소 3개 이상 module의 signal을 표현할 수 있었다. | Source Pack 소급 검증 note |
| security, QA, docs, candidate, observability 신호가 같은 envelope로 무리 없이 기록됐다. | module별 대조표 |
| `severity`와 `user_visibility` 해석이 모호하지 않았다. | 검증 중 ambiguity 기록 확인 |
| `route_to`와 `record_in` 구분이 실제 사례에서 모호함 없이 작동했다. | Source Pack 대표 사건 검토 |
| 별도 signal log가 필요한지 아닌지 판단할 근거가 생겼다. | Phase 7-A/B 평가 |
| Phase 7-B Industry Primer pilot에서도 같은 구조가 작동했다. | Industry Primer pilot evaluation |

v1 승격은 Phase 7-A만으로 결정하지 않는다.
Phase 7-B 실전 pilot 결과까지 본 뒤 판단한다.

## 13. Phase 7-A에서 검증할 질문

Phase 7-A Source Pack 소급 검증에서는 signal-routing v0가 아래 질문에 답할 수 있는지 확인한다.

| 질문 | 확인할 것 |
|---|---|
| 보안 위험은 즉시 사용자에게 보였어야 했는가? | security-baseline signal과 `user_visibility: immediate` 적용 |
| QA escalation은 어떤 route를 가져야 했는가? | qa-scaffold의 escalation 필드와 approval/security 연결 |
| docs 복잡화는 maintenance signal로 표현 가능한가? | docs-organization 성장 신호와 run-summary/work-map 기록 |
| 반복 병목과 trim 후보는 observability에서 candidate-ledger로 자연스럽게 이어지는가? | observability `trim_candidate`, candidate-ledger review trigger |
| candidate-ledger review trigger는 signal로 표현 가능한가? | `route_to: candidate-ledger`, evidence 기록 위치 |
| comparison은 no-file 항목으로도 signal을 남길 수 있는가? | QA/approval/observability 연결 기준 |
| IR taxonomy overlap 사건은 어떤 signal 조합으로 표현되는가? | `sec_equivalent_not_found_in_scoped_8k`, `security_quarantined` 사례 |
| `route_to`와 `record_in`을 구분하기 어려운 사례가 있는가? | 모호 사례 기록 |

대표 사건:

```text
IR taxonomy overlap 충돌 사건
- sec_equivalent_not_found_in_scoped_8k
- security_quarantined
```

이 사건은 candidate-ledger, security-baseline, signal-routing, type-schema, approval-gate가 실제 현장에서 어떻게 연결되어야 하는지 검증하는 데 유용하다.

## 14. v0 권장 목차

`global-harness-signal-routing-template-v0.md` 후보 파일은 아래 목차로 작성하는 것을 권장한다.

1. 목적
2. 비범위
3. signal model
4. severity 어휘
5. user_visibility
6. safe notification 원칙
7. signal envelope
8. `route_to`와 `record_in` 경계
9. module별 signal 소유 경계
10. adapter 책임
11. 다른 module과의 연결
12. 적용 전 체크리스트

v0 본문에는 이 scope note의 작업 진행 메타 문구를 넣지 않는다.
실행에 필요한 signal-routing 계약만 옮긴다.

## 15. 다음 단계

1. 이 scope note를 Claude Code에 교차검증한다.
2. 이견이 있으면 이 note를 먼저 수정한다.
3. 합의 후 `global-harness-signal-routing-template-v0.md` 후보 파일을 작성한다.
4. v0 작성 후 Claude Code 교차검증을 진행한다.
5. v1 core Section 12 registry에 signal-routing을 반영할지 결정한다.

현재 이 문서는 Claude Code 교차검증 PASS 상태이며, `global-harness-signal-routing-template-v0.md` 후보 작성 기준으로 사용한다.
