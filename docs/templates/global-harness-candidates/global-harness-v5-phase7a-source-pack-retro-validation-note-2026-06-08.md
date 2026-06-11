# Global Harness v5 Phase 7-A Source Pack Retrospective Validation Note

- 작성일: 2026-06-08
- 상태: Phase 7-A validation note. Claude Code 교차검증 PASS, minor fix 반영.
- 범위: Source Pack 실제 구조와 대표 사건을 v5 core/module 후보에 read-only 방식으로 소급 대조
- 기준 문서:
  - `global-harness-v5-work-map.md`
  - `global-harness-v5-phase7-planning-consensus-note-2026-06-07.md`
  - `global-harness-core-structure-template-v1.md`
  - `global-harness-signal-routing-template-v0.md`

## 1. 검증 목적

이 문서는 이미 구축된 P2-S03 Source Pack 하네스를 Global Harness v5 core/module 후보에 소급 대조하기 위한 Phase 7-A 결과 note다.

목적은 다음과 같다.

- Source Pack 구축 과정에서 실제로 발생한 구조 문제, 보안 사건, QA 상태, docs 성장, candidate 관리가 v5 module로 설명되는지 확인한다.
- v5 core와 module registry가 Source Pack의 실제 `harness/`, adapter, docs, artifacts 구조와 충돌하지 않는지 확인한다.
- Phase 7-0에서 만든 `signal-routing` v0가 실제 Source Pack 경험에서 필요한 알림, 경고, escalation 신호를 표현할 수 있는지 점검한다.
- Phase 7-B `Industry Primer` 실전 pilot 전에 명백한 구조 오류를 줄인다.

이 검증은 새 하네스 실전 검증이 아니다.
Phase 7-B가 독립 실전 검증이며, Phase 7-A는 내부 일관성 filter다.

## 2. 확증 편향 한계

Source Pack 경험을 바탕으로 v5 core와 module 후보를 만들었기 때문에, 다시 Source Pack에 대조하면 잘 맞는다는 결론이 나오기 쉽다.

따라서 이 note의 결론은 아래 한계를 가진다.

```text
이 검증은 Source Pack 기반 내부 일관성 확인이다.
Source Pack 경험 자체가 잘못된 방향이었다면 이 검증은 그 오류를 잡지 못할 수 있다.
새 하네스와 다른 유형의 하네스에 대한 독립 검증은 Phase 7-B에서 수행한다.
```

Phase 7-A가 확인할 수 있는 것:

- Source Pack 현실과 v5 template 후보가 충돌하지 않는가?
- 실제 발생했던 보안, QA, docs, candidate 관리 이슈를 module이 설명할 수 있는가?
- `signal-routing`이 필요한 사용자 노출 신호를 식별할 수 있는가?
- Phase 7-B 전에 막아야 할 명백한 구조 오류가 있는가?

Phase 7-A가 확인할 수 없는 것:

- 새 하네스를 처음 만들 때 v5 template가 실제로 사용하기 쉬운가?
- 수집형이 아닌 분석형, 판단형, 모니터링형 하네스에서도 같은 구조가 자연스러운가?
- Source Pack에서 잘못 처리한 방식이 template로 굳어진 것은 아닌가?
- 전역 배포 후 Codex 전역과 Claude Code 전역 사이 drift를 실제로 막을 수 있는가?

## 3. Read-Only 검증 원칙

이 검증은 read-only retrospective validation이다.

허용된 쓰기 작업:

- 이 validation note 작성
- `global-harness-v5-work-map.md` 갱신
- `README.md` 갱신

금지된 작업:

- 기존 Source Pack `harness/` 파일 수정
- `.agents/`, `.claude/`, `.codex/` adapter 수정
- `artifacts/catalog/`, `artifacts/raw/`, `artifacts/runs/`, `artifacts/companies/` 수정
- 기존 docs 구조 이동, 삭제, rename
- catalog, index, raw, run-summary, QA 결과의 직접 보정
- 발견된 문제를 즉시 해결하는 자동 수정

발견된 문제는 직접 고치지 않고 Section 10의 B 진입 전 수정 후보 또는 관찰 후보 목록에만 기록한다.

## 4. 검토한 Source Pack 증거

이번 검증은 exhaustive audit이 아니라 대표 사건과 핵심 폴더 중심 검토다.

읽은 핵심 구조:

| 영역 | 검토한 증거 | 검토 목적 |
|---|---|---|
| 공통 원장 | `harness/ORCHESTRATOR.md`, `harness/MANIFEST.md`, `harness/contracts/source-pack.contract.md` | v5 core의 `harness/` 중심 구조와 충돌 여부 확인 |
| 실행 절차 | `harness/procedures/source-pack-runbook.md`, `harness/procedures/source-pack-ir-collector.md` 일부 내용과 상태 지도 | 승인, candidate, overlap, 보안 격리 처리 방식 확인 |
| schema/catalog | `harness/schemas/source-pack-catalog.schema.md`, `artifacts/catalog/ir-taxonomy-candidates.jsonl` | type-schema, candidate-ledger 연결 확인 |
| 현재 상태 지도 | `docs/current/source-pack-architecture-map-2026-06-06.md`, `docs/current/source-pack-ir-current-state-map-2026-06-05.md`, `docs/current/source-pack-ir-v1-closeout-2026-06-05.md` | 대표 사건과 현재 구현/보류 경계 확인 |
| IR taxonomy | `docs/current/ir-taxonomy-checkpoint-2026-06-05.md` | schema 후보, 보류, 보안 격리 근거 확인 |
| 실행 결과 | `artifacts/runs/run-20260604-ntra-ir-pilot/run-summary.md`, `artifacts/runs/run-20260605-app-sec-ir-overlap-recheck/run-summary.md`, `artifacts/runs/run-20260605-app-sec-ir-overlap-recheck/qa.md`, `artifacts/runs/run-20260605-tem-ir-pilot/run-summary.md` | 보안, QA, overlap, candidate, observability 사례 확인 |
| 후보 템플릿 | v5 core v1, signal-routing v0, Phase 7 planning consensus | 실제 Source Pack 사례와 module 후보 대조 |

검토하지 않은 것:

- raw 원자료 본문
- 전체 catalog의 모든 record
- 모든 run directory의 전체 파일
- 외부 웹사이트 또는 최신 데이터

## 5. Module별 소급 대조 결과

판정값:

- `applicable`: Source Pack 사례와 잘 맞고 Phase 7-B로 넘어갈 수 있음
- `applicable with watch`: 구조는 맞지만 Phase 7-B에서 사용성 또는 과잉 여부를 관찰해야 함
- `needs adjustment`: B 전에 작은 수정 후보가 있음
- `blocked`: B 전에 core/module 구조 수정이 필요함
- `not applicable`: Source Pack에는 적용할 근거가 약함

| module / 항목 | Source Pack 소급 증거 | 판정 | 메모 |
|---|---|---|---|
| v5 core | `harness/`가 업무 의미를 담고, `.agents/`와 `.claude/`가 얇은 adapter로 연결된다. `AGENTS.md`와 `CLAUDE.md`는 같은 구조 안내를 공유한다. | applicable | 모델 중립 실행 core의 방향과 실제 Source Pack 구조가 일치한다. |
| design-preflight | `ORCHESTRATOR.md`와 contract에서 Source Pack을 수집형 하네스로 정의하고, 출처 추적성, 로컬 파일 존재성, catalog 일관성, 누락 관리, downstream readiness를 품질 축으로 둔다. | applicable | v4 설계 판단 장치를 별도 preflight로 분리한 결정은 Source Pack 구조를 설명한다. |
| type-schema | Source Pack은 `harness/schemas/`와 controlled/extensible IR `document_type` 구조를 가진다. `ir-taxonomy-candidates.jsonl`은 schema 후보를 바로 승격하지 않는 장치다. | applicable with watch | no-file registry 항목으로 충분해 보이나, Industry Primer에서 glossary/schema 후보를 다룰 때 다시 검증해야 한다. |
| security-baseline | NTRA pilot에서 Bitdefender 격리 파일이 발생했고, 복구/열람/운영 승격 금지 원칙이 필요했다. | applicable | 보안 격리, raw data, 외부 공개, 자동 해결 금지 원칙이 실제 사례와 맞다. |
| approval-gate | 운영 catalog/index 병합, schema 변경, comparison merge, 보안 격리 파일 재시도/복구는 사용자 승인 지점으로 남아 있다. | applicable | 논의/검토와 실제 운영 반영을 분리하는 구조가 Source Pack에서 실제로 필요했다. |
| qa-scaffold | APP recheck QA는 `pass`, NTRA는 `fail`, TEM은 `partial_pass`를 사용했다. `repair_required`, `unverified`, `security_quarantined`도 실제 맥락에 등장한다. | applicable | 공통 QA 범주와 하네스별 rubric 분리 방향이 맞다. |
| comparison | `ORCHESTRATOR.md`는 Codex/Claude 비교 실행과 operational merge 승인 조건을 가진다. | applicable | 별도 파일 없이 registry와 연결 module 기준으로 설명하는 현재 방식이 적절하다. |
| signal-routing | 보안 격리, schema 후보, docs 성장, QA escalation, transport bottleneck 같은 신호가 여러 module에 걸쳐 발생했다. | applicable with watch | v0 envelope가 소급 사례를 설명할 수 있다. 실제 live pilot에서 부담과 표현 명확성을 확인해야 한다. |
| observability | TEM run-summary에는 `instructions_files_consulted`, `bottleneck_note`, `trim_candidate`가 선택 섹션으로 기록됐다. | applicable | 선택 관측 섹션은 Source Pack에서 실제로 작동했다. 자동화는 아직 보류가 맞다. |
| candidate-ledger | TEM의 `ir-earnings-presentation` 후보가 `ir-taxonomy-candidates.jsonl`에 기록됐다. APP/NTRA 사례도 candidate 판단의 근거가 된다. | applicable | 새 분류와 상태값을 바로 정식화하지 않는 대기실 구조가 검증된다. |
| pilot-first / testing | AAPL, NTRA, APP, TEM pilot과 APP overlap recheck는 변경을 바로 운영화하지 않고 작게 검증한 사례다. | applicable | 별도 파일 없이 candidate-ledger와 Phase 7 validation에 묶는 결정이 맞다. |
| checkpoint | 긴 논의, `/저장` 기반 compact checkpoint, handoff note가 필요했다. | applicable | v5 checkpoint는 실행 skill을 대체하지 않고 출력 계약을 정의하는 것이 적절하다. |
| docs-organization | `docs/current`, `docs/design`, `docs/pilots`, `docs/templates`, `docs/handoff`, `docs/reference`가 분화되었다. | applicable with watch | 성장 후 정리 기준은 실제로 필요했다. 다만 알림/정리 신호는 signal-routing과 연결해 B에서 확인한다. |

결론:

```text
Source Pack 실제 구조와 v5 core/module 후보 사이에 blocking conflict는 발견되지 않았다.
다만 type-schema no-file 항목, signal-routing live 사용성, docs-organization 신호 노출은 Phase 7-B에서 관찰해야 한다.
```

## 6. Signal-Routing으로 포착했어야 할 신호

아래 표는 Source Pack 구축 과정에서 signal-routing v0가 있었다면 공통 envelope로 표현했을 신호 후보다.

| signal_id | 근거 사건 | severity | route_to | record_in | user_visibility | 소급 판단 |
|---|---|---|---|---|---|---|
| `security-baseline.security_quarantined_file` | NTRA Q1 earnings presentation PDF가 Bitdefender에 의해 격리됨 | `critical` | `approval-gate` | `run-summary`, `qa-result` | `immediate` | silent log로 충분하지 않다. 사용자에게 즉시 알리고 복구/열람/승격 금지를 명시해야 한다. |
| `security-baseline.automatic_fix_for_security_issue_blocked` | 격리 파일을 자동 복구하거나 열면 위험함 | `critical` | `approval-gate` | `run-summary`, `qa-result` | `immediate` | security-baseline v1의 자동 해결 금지 원칙과 직접 연결된다. |
| `approval-gate.operational_merge_requires_approval` | NTRA/TEM run-local raw를 운영 catalog/index에 병합하지 않음 | `action_required` | `approval-gate` | `run-summary` | `immediate` | 운영 반영은 실행 중 승인 여부를 분명히 해야 하며, 승인 요청/결과는 `record_in` 기준으로 남긴다. |
| `qa-scaffold.qa_failure_or_partial_pass` | NTRA `fail`, TEM `partial_pass`, APP initial `partial_pass` | `action_required` | `qa-scaffold` | `qa-result`, `run-summary` | `run_summary` | QA 상태와 repair/recheck 후보가 다음 행동으로 연결되어야 한다. |
| `candidate-ledger.taxonomy_candidate_observed` | TEM `ir-earnings-presentation` 후보 원장 기록 | `maintenance` | `candidate-ledger` | `candidate-ledger`, `run-summary` | `run_summary` | 새 schema 후보는 즉시 schema 변경이 아니라 candidate evidence로 보낸다. |
| `candidate-ledger.review_trigger_near_or_met` | NTRA/APP/TEM 맥락에서 earnings presentation 후보가 반복 관찰됨 | `action_required` | `candidate-ledger`, `approval-gate` | `candidate-ledger`, `run-summary` | `run_summary` | 후보가 반복되면 사용자 review 또는 taxonomy checkpoint로 연결해야 한다. |
| `type-schema.schema_value_semantics_unclear` | `ir-financial-update`를 별도 type으로 볼지 source label로 볼지 논의 | `maintenance` | `candidate-ledger` | `candidate-ledger`, `run-summary` | `run_summary` | type-schema는 no-file 항목이므로 실제 처리는 candidate-ledger와 하네스별 schema가 맡는다. |
| `type-schema.overlap_status_boundary_needed` | APP `different_hash_from_sec_candidate`와 `sec_equivalent_not_found_in_scoped_8k` 구분 | `maintenance` | `candidate-ledger`, `qa-scaffold` | `qa-result`, `run-summary` | `run_summary` | 상태값 의미가 downstream 해석에 영향을 주므로 QA와 candidate 경계가 필요하다. |
| `observability.transport_bottleneck_observed` | TEM에서 shell external HTTPS transport 실패 후 fallback 사용 | `maintenance` | `observability` | `run-summary` | `run_summary` | 반복되면 downloader/checker 자동화 후보가 된다. |
| `docs-organization.docs_structure_growth_detected` | docs가 current/design/pilots/templates/handoff/reference로 분화됨 | `maintenance` | `docs-organization` | `docs/README`, `work-map` | `run_summary` | 사용자가 매번 docs를 직접 확인하지 않아도 정리 필요성을 볼 수 있어야 한다. |
| `comparison.operational_merge_after_comparison_requires_approval` | comparison mode 결과를 운영 산출물로 합치려면 승인 필요 | `action_required` | `approval-gate` | `comparison-result`, `run-summary` | `immediate` | comparison은 no-file 항목이지만 approval/QA/observability와 연결된다. |
| `checkpoint.long_session_checkpoint_needed` | 긴 템플릿 논의와 교차검증이 여러 세션에 걸쳐 이어짐 | `maintenance` | `checkpoint` | `session-checkpoint` | `run_summary` | 원문 저장 없이 compact checkpoint가 필요하다. adapter는 가벼운 세션에서 조용히 처리할 수 있다. |

소급 결론:

- `signal-routing` v0의 4축 구조(`severity`, `route_to`, `record_in`, `user_visibility`)는 Source Pack 사례를 표현할 수 있다.
- `candidate`나 `improvement`를 severity로 두지 않고 route/record 성격으로 처리한 결정은 Source Pack 후보 원장 사례와 잘 맞는다.
- 별도 signal log를 강제하지 않고 기존 run-summary, QA, candidate-ledger에 기록하게 한 결정도 Source Pack 규모에 적합하다.

## 7. 대표 사건 중심 검토

### 7.1 Source Pack이 예상보다 무거워진 사건

Source Pack은 처음에는 원자료 수집 하네스였지만, IR 확장, taxonomy, overlap, security, observability, docs 재정리까지 포함되면서 구조가 무거워졌다.

v5 해석:

- 거대한 단일 template보다 core + module 분리가 필요했다.
- 모든 module을 강제하지 않고 필요한 시점에 선택 적용해야 한다.
- docs-organization과 observability는 초기부터 강제할 기능이 아니라 성장 후 적용 기준으로 두는 편이 맞다.

### 7.2 Earnings Call 분리 판단

실적발표 자료는 원래 Source Pack 안에 넣으려 했지만, 번역, 요약, 투자구조 분석까지 포함하면 Source Pack 범위를 넘어선다.

v5 해석:

- design-preflight가 하네스 유형과 산출물 역할을 먼저 확인해야 한다.
- Source Pack은 계속 수집형으로 유지하고, `Earnings Call`은 Step 3-2 후보 하네스로 분화하는 것이 자연스럽다.
- 다만 `Industry Primer`의 blocking dependency는 아니므로 Phase 7-B를 막지 않는다.

### 7.3 v4 단일 template에서 v5 core + modules로 분리한 사건

v4는 설계 판단, 실행 구조, 보안, QA, docs, 관측 가능성을 한 문서에 많이 담았다.
v5는 모델 중립 core와 topic module로 나누었다.

v5 해석:

- v5 core는 공통 원장과 얇은 adapter에 집중한다.
- 설계 판단은 design-preflight가 맡는다.
- 세부 운영 장치는 registry에서 발견 가능하게 둔다.

### 7.4 NTRA 보안 격리 사건

NTRA pilot에서 Q1 earnings presentation PDF가 보안 제품에 의해 격리되었다.
run-summary는 `security_quarantined`와 복구/열람 금지를 명시했다.

v5 해석:

- security-baseline의 최소 안전선이 실제로 필요했다.
- approval-gate는 보안 격리 파일 재시도, 복구, 열람, 운영 승격을 승인 대상으로 다뤄야 한다.
- signal-routing은 이 사건을 `critical` + `immediate`로 사용자에게 표시해야 한다.
- qa-scaffold는 이 상태를 단순 실패가 아니라 security review와 repair/recheck 경계로 기록해야 한다.

### 7.5 TEM candidate-ledger 사건

TEM pilot에서 `ir-earnings-presentation` 후보가 `ir-taxonomy-candidates.jsonl`에 기록되었다.

v5 해석:

- candidate-ledger는 실제로 필요한 module이다.
- 새 type을 즉시 schema에 넣지 않고 evidence로 누적하는 방향이 맞다.
- type-schema가 별도 파일 없이 registry 항목으로 남아 있어도, 후보 검토는 candidate-ledger와 하네스별 `harness/schemas/`에서 처리할 수 있다.

### 7.6 docs 성장과 정리 필요성

Source Pack docs는 `current`, `design`, `pilots`, `templates`, `handoff`, `reference`로 나뉘었다.

v5 해석:

- docs-organization v1의 "성장 후 정리 기준"은 실제 Source Pack에 맞다.
- 정리 필요 신호는 사용자가 폴더를 직접 열어보지 않아도 run-summary 또는 work map에서 볼 수 있어야 한다.
- signal-routing과 docs-organization 연결은 Phase 7-B에서 live로 검증해야 한다.

### 7.7 Codex / Claude Code 교차검증 workflow

v5 템플릿 작업은 Codex가 초안을 만들고 Claude Code가 평가하며, 사용자가 승인 후 수정하는 방식으로 진행되었다.

v5 해석:

- approval-gate의 "논의와 실행 분리"가 실제 협업에서 중요했다.
- comparison은 단순 QA가 아니라 실행 구조와 승인 흐름을 포함한다.
- checkpoint는 긴 논의의 연속성을 유지하는 데 필요했다.

## 8. 핵심 폴더 중심 검토

| 폴더 / 파일 | 현재 역할 | v5 대조 |
|---|---|---|
| `harness/` | Source Pack 업무 의미의 source of truth | v5 core의 공통 원장 원칙과 일치 |
| `harness/ORCHESTRATOR.md` | 목적, 실행 모드, 승인/중단 조건 | approval-gate, comparison, design-preflight와 연결 |
| `harness/MANIFEST.md` | 파일 역할과 의존 관계 | docs-organization의 MANIFEST 경계와 일치 |
| `harness/contracts/` | 산출물 계약과 완료 기준 | v5 core의 Completion Contract 미사용 결정과 일치 |
| `harness/procedures/` | runbook, collector, QA 절차 | v5 core의 runbook/procedure 구분과 일치 |
| `harness/schemas/` | catalog, run-summary schema | type-schema no-file 항목과 하네스별 schema 경계 확인 |
| `harness/rubrics/` | Source Pack QA 기준 | qa-scaffold가 공통 범주만 제공하고 상세 rubric은 하네스별로 둔 결정과 일치 |
| `.agents/skills/` | Codex adapter | 얇은 adapter 원칙과 일치 |
| `.claude/skills/` | Claude Code adapter | 얇은 adapter 원칙과 일치 |
| `artifacts/catalog/` | 기계용 catalog, candidate ledger | candidate-ledger와 type-schema 연결 확인 |
| `artifacts/runs/` | run-summary, QA, download-log | observability, qa-scaffold, signal-routing 기록 위치 확인 |
| `artifacts/raw/` | 대용량 원자료 | security-baseline과 하네스별 raw 공개/제외 판단 필요 |
| `docs/current/` | 현재 상태 지도 | docs-organization 적용 사례 |
| `docs/templates/global-harness-candidates/` | v5 후보 canonical workspace | Phase 7-C 전까지 임시 canonical source |

## 9. IR Taxonomy Overlap 사건 검토

### 9.1 `sec_equivalent_not_found_in_scoped_8k`

APP SEC-IR overlap recheck에서 `ir-app-financial-supplement-fy2026-q1`은 scoped 8-K 안에 대응 SEC financial update/supplement exhibit가 없다고 기록되었다.

중요한 구분:

- `different_hash_from_sec_candidate`: SEC 후보는 있으나 hash가 다름
- `sec_equivalent_not_found_in_scoped_8k`: 제한 확인 범위 안에서 대응 SEC exhibit가 없음

v5 module 연결:

| module | 해석 |
|---|---|
| type-schema | 상태값 의미가 downstream 해석에 영향을 주므로 하네스별 schema/rubric에서 정의해야 한다. |
| qa-scaffold | "중복 없음 확정"이 아니라 "제한 확인 범위 안에서 없음"을 QA가 확인해야 한다. |
| candidate-ledger | 반복되는 overlap 상태값이나 새 상태값 후보는 바로 schema에 넣지 않고 evidence로 누적할 수 있다. |
| approval-gate | 운영 catalog/index notes 갱신은 승인 범위 안에서 수행해야 한다. |
| signal-routing | 상태 경계가 모호하거나 downstream 위험이 있으면 maintenance 또는 action_required signal로 노출할 수 있다. |

소급 판정:

```text
v5 후보는 이 사건을 설명할 수 있다.
특히 type-schema를 no-file registry 항목으로 두고, 실제 상태값은 하네스별 schema/QA와 candidate-ledger가 관리하게 한 결정은 적절하다.
```

### 9.2 `security_quarantined`

NTRA pilot에서 Q1 earnings presentation PDF가 보안 제품에 의해 격리되었다.

Source Pack의 실제 처리:

- `collection_status: failed`
- `repair_required: true`
- `repair_reason: security_quarantined`
- 복구/열람/운영 승격 금지
- 사용자 승인과 별도 보안 검토 전 재시도 금지

v5 module 연결:

| module | 해석 |
|---|---|
| security-baseline | 보안 격리 파일은 절대 자동 복구/열람/승격하지 않는다. |
| approval-gate | 재시도, 복구, 열람, 운영 반영은 별도 명시 승인 대상이다. |
| qa-scaffold | 단순 fail이 아니라 security review와 repair/recheck 경계가 필요하다. |
| signal-routing | `critical` + `immediate` 신호로 사용자에게 표시해야 한다. |
| candidate-ledger | 보안 상태값 자체가 반복되거나 절차 후보가 생기면 candidate로 기록할 수 있다. |

소급 판정:

```text
v5 후보는 이 사건을 설명할 수 있다.
다만 당시에는 signal-routing이 없었으므로, 앞으로는 security event를 silent log에 묻지 않고 즉시 사용자 노출 신호로 표현해야 한다.
```

## 10. B 진입 전 수정 후보 목록

### 10.1 B 전에 직접 수정이 필요한 blocking 항목

현재 발견 없음.

Source Pack 소급 검증에서 v5 core/module 후보의 blocking conflict는 발견되지 않았다.

### 10.2 B 전에 note 또는 계획에 반영할 관찰 항목

| 항목 | 처리 |
|---|---|
| type-schema no-file 항목의 충분성 | Industry Primer pilot plan에서 glossary/schema/rubric 후보가 생길 때 no-file registry 방식이 충분한지 관찰한다. |
| signal-routing live 사용성 | Industry Primer pilot 중 실제 signal을 최소 1회 envelope로 표현해보고, 과하거나 헷갈리는지 기록한다. |
| docs-organization signal 노출 | docs가 복잡해지는 경우 사용자가 어떻게 알게 되는지 run-summary, work map, signal-routing 연결로 확인한다. |
| Source Pack example 오염 방지 | Phase 7-B 문서에는 SEC/IR/ticker 예시를 전역 규칙처럼 넣지 않는다. |
| module 선택 부담 | Industry Primer 청사진에서 모든 module을 강제하지 않고 필요한 module만 선택했는지 확인한다. |
| registry path portable화 | Phase 7-C packaging note에서 처리한다. B 전 blocking 항목은 아니다. |

### 10.3 보류 또는 Phase 7-C로 넘길 항목

| 항목 | 보류 이유 |
|---|---|
| Codex/Claude 전역 배포 | Phase 7-B 실전 검증 전에는 배포하지 않는다. |
| canonical source 결정 | 21단계 상위 하네스 또는 별도 bundle 구조와 함께 판단한다. |
| `docs/templates/global-harness-candidates/` 경로 문구 portable화 | 전역 bundle packaging에서 결정한다. |
| Source Pack 사례 reference 범위 | Phase 7-C에서 example/reference 정책으로 결정한다. |
| Earnings Call 산출물 계약 | Step 6 Business Model 또는 이후 하네스 전에 정리한다. Industry Primer의 blocking dependency가 아니다. |

## 11. A에서 확인하지 못한 것

이 note는 아래 사항을 검증하지 못한다.

| 미검증 항목 | 이유 | 검증 위치 |
|---|---|---|
| Industry Primer에서 v5 core가 실제로 사용하기 쉬운가 | 새 하네스 실전 적용이 필요하다 | Phase 7-B |
| 분석 준비형 하네스에서 QA/rubric이 과하지 않은가 | Source Pack은 수집형이다 | Phase 7-B |
| signal-routing envelope가 live 작업에서 부담이 되지 않는가 | 소급 대조만으로는 사용 감각을 알 수 없다 | Phase 7-B |
| type-schema no-file registry가 충분한가 | Industry Primer에서 glossary/schema 후보가 실제로 생겨야 한다 | Phase 7-B |
| 전역 배포 후 drift 관리 | 전역 배포를 아직 하지 않는다 | Phase 7-C |

## 12. A 완료 판정

판정:

```text
pass
```

이유:

- Source Pack 실제 구조와 v5 core/module 후보 사이에 blocking conflict가 발견되지 않았다.
- Source Pack 대표 사건인 보안 격리, IR taxonomy candidate, SEC-IR overlap, docs 성장, observability, QA 상태를 각 module이 설명할 수 있다.
- `signal-routing` v0는 소급 사례의 신호를 표현할 수 있다.
- B 전에 기존 Source Pack 하네스 파일이나 v5 core/module 후보를 즉시 수정해야 하는 blocking 항목은 발견되지 않았다.

단, 이 `pass`는 전역 배포 승인이 아니다.

다음 행동:

1. 작업 지도에서 Phase 7-A 교차검증 PASS를 완료 처리한다.
2. Phase 7-B `Industry Primer` pilot plan 논의로 넘어간다.
