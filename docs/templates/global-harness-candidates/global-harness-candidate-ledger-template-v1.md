# Global Harness Candidate Ledger Template v1

- 작성일: 2026-06-07
- 기반: `global-harness-candidate-ledger-template-v0.md`와 Phase 6 candidate-ledger scope note
- 기준 문서: `global-harness-v5-phase6-candidate-ledger-scope-note-2026-06-07.md`
- 상태: 전역 하네스 후보 module. 아직 전역 `harness-lab` 반영 아님.

## 1. 목적

이 template는 새 분류, 새 상태값, 새 source, 새 schema 값, 새 예외 규칙을 발견했을 때 바로 정식 schema나 절차로 넣지 않고 후보로 기록하기 위한 전역 후보 module이다.

candidate-ledger는 정식 schema가 되기 전의 대기실이다.
새 후보를 발견하면 먼저 기록하고, 증거와 반복성이 쌓이면 검토, pilot, 승인, 정식 반영 여부를 결정한다.

핵심 문장:

```text
candidate-ledger는 새 후보를 바로 정식화하지 않고,
후보와 증거를 분리해 기록한 뒤 검토/검증/승인으로 넘기는 얇은 governance module이다.
```

## 2. 범위와 비범위

이 module이 다루는 것:

- 후보를 바로 정식 schema/rule로 넣지 않는 원칙
- 후보로 기록할 수 있는 대상 범주
- candidate record와 evidence event 구분
- 최소 record skeleton
- lifecycle/status 정의
- review trigger
- 승격, 폐기, 보류 원칙
- `approval-gate`, `qa-scaffold`, `type-schema`, `pilot-first / testing` 등 다른 module과의 연결

이 module이 다루지 않는 것:

- 실제 pilot 실행 계획 전체
- schema migration 절차
- QA rubric 상세 변경
- 승인 문구와 위험도별 승인 방식
- 자동 승격 스크립트
- 도메인별 후보 예시 전체
- 모든 하네스에 동일한 ledger 파일명과 위치 강제

candidate-ledger는 후보를 영원히 쌓아두는 보관함이 아니다.
후보가 검토, pilot, 정식 반영, 폐기, 보류 중 어디로 가야 하는지 판단할 수 있게 만드는 장치다.

## 3. candidate-ledger의 역할

candidate-ledger의 역할:

| 역할 | 설명 |
|---|---|
| 후보 기록 | 새 분류, 상태값, source, schema 값, 예외, QA ambiguity, workflow branch를 바로 정식화하지 않고 후보로 남긴다. |
| 증거 누적 | 후보를 뒷받침하는 관찰 기록과 반복성을 추적한다. |
| 검토 트리거 | 언제 사람 검토나 pilot 검증이 필요한지 알려준다. |
| 승격/폐기/보류 경계 | 후보를 정식 반영, 폐기, 보류하기 전 필요한 조건을 정한다. |
| module 연결 | `approval-gate`, `qa-scaffold`, `type-schema`, `pilot-first / testing`과의 경계를 분명히 한다. |

candidate-ledger는 "후보가 있다"를 말한다.
pilot 실행, 승인 절차, schema 변경, QA 변경은 각 module과 하네스별 절차가 맡는다.

## 4. 후보를 바로 정식화하지 않는 원칙

핵심 원칙:

```text
새 유형을 발견했다고 바로 정식 규칙으로 승격하지 않는다.
후보로 기록하고, 증거와 반복성이 쌓이면 검토와 승인 후 정식화한다.
```

이 원칙이 필요한 이유:

- 한 번 나온 특수 사례를 schema에 바로 넣으면 schema가 빠르게 복잡해진다.
- 상태값과 예외가 늘어나면 QA와 downstream 소비자가 흔들릴 수 있다.
- 새로운 source나 자동화는 작은 검증 없이 운영 절차에 넣으면 drift를 만든다.
- 후보를 기록하지 않으면 같은 애매함이 반복돼도 학습되지 않는다.

candidate-ledger는 판단을 미루는 장치가 아니라, 정식화 전 필요한 근거를 모으는 장치다.

## 5. 후보로 기록할 대상 범주

아래 항목은 candidate-ledger에 후보로 기록할 수 있다.

| 범주 | 예시 |
|---|---|
| 자료/source 후보 | 새 입력 source, 새 provider, 새 문서 묶음 |
| schema 값 후보 | 새 enum 값, 새 subtype, 새 output field 후보 |
| 상태값 후보 | 새 status, failure reason, processing state |
| 예외 규칙 후보 | 기존 절차로 처리하기 애매한 반복 예외 |
| QA ambiguity | 같은 검증 질문에서 반복되는 애매함 |
| workflow branch | 새 분기, 새 처리 모드, 새 재시도 방식 |
| automation 후보 | 운영 절차에 넣기 전 작은 검증이 필요한 자동화 |

전역 v1은 하네스별 후보 유형을 exhaustive하게 열거하지 않는다.
각 하네스는 자기 domain, schema, procedure에 맞는 `candidate_type` 값을 정할 수 있다.

## 6. candidate record와 evidence event 구분

candidate-ledger는 `candidate record`와 `evidence event`를 개념적으로 구분한다.

| 구분 | 의미 |
|---|---|
| candidate record | 후보의 현재 상태를 나타내는 기록 |
| evidence event | 후보를 뒷받침하는 개별 관찰 기록 |

개념 분리는 전역 v1에서 요구한다.
물리 분리는 하네스별로 정한다.

가능한 저장 방식:

| 방식 | 설명 |
|---|---|
| single-file | 같은 JSONL 파일에 candidate record와 evidence event를 함께 저장한다. |
| split-file | candidate records와 evidence events를 별도 JSONL 파일로 나눈다. |
| summary-only | 작은 하네스에서 candidate record 안에 요약 증거만 둔다. |

전역 원칙:

- candidate record는 후보의 현재 상태를 보여준다.
- evidence event는 후보가 왜 필요한지 보여주는 관찰 증거다.
- 같은 파일에 저장할 수도 있고, 별도 파일로 나눌 수도 있다.
- 물리 저장 구조는 하네스별로 정한다.
- append-first를 기본 원칙으로 보되, 작은 하네스에서는 단일 record 갱신을 허용할 수 있다.

## 7. 최소 record skeleton

candidate record의 최소 필드 후보:

| 필드 | 의미 | 비고 |
|---|---|---|
| `candidate_id` | 후보 고유 ID | 하네스 안에서 안정적으로 참조 가능해야 한다. |
| `candidate_type` | 후보 종류 | source, schema_value, status, exception, qa_ambiguity, workflow_branch 등 |
| `title` | 사람이 읽는 짧은 이름 | 선택 가능하지만 권장 |
| `summary` | 후보 설명 | 왜 후보인지 한두 문장으로 적는다. |
| `first_observed_at` | 처음 관찰된 시점 | 날짜 또는 run-id 기반 시점 |
| `last_observed_at` | 마지막 관찰된 시점 | 증거가 추가될 때 갱신 가능 |
| `status_changed_at` | 현재 status로 바뀐 시점 | 선택 필드. 상태 변경 시 권장 |
| `observed_in` | 관찰 위치 | run, file, artifact, issue, QA note 등 |
| `handled_as` | 현재 임시 처리 방식 | 기존 분류로 임시 처리했는지, 별도 note로 남겼는지 |
| `reason` | 후보로 남기는 이유 | 기존 규칙으로 충분하지 않은 이유 |
| `evidence_count` | 증거 수 요약 | 집계값 또는 최신 요약값으로 본다. |
| `status` | 현재 lifecycle 상태 | Section 8 상태값을 따른다. |
| `recommended_action` | 다음 행동 | observe, review, pilot, promote, reject, defer 등 |
| `owner_or_reviewer` | 검토 책임자 또는 확인 주체 | 하네스별로 선택 |
| `notes` | 추가 설명 | 선택 |

evidence event를 별도로 두는 경우의 최소 필드 후보:

| 필드 | 의미 |
|---|---|
| `candidate_id` | 연결되는 candidate record ID |
| `observed_at` | 관찰 시점 |
| `observed_in` | 관찰 위치 |
| `evidence_summary` | 증거 요약 |
| `handled_as` | 해당 관찰을 임시로 처리한 방식 |
| `impact` | schema, QA, downstream, workflow에 미치는 영향 |
| `source_ref` | 안전한 파일 경로, run-id, note 링크 등 |

`evidence_count` 처리 원칙:

- `evidence_count`는 후보의 원천 증거 자체가 아니다.
- append-only가 필요한 하네스에서는 evidence event를 별도 행으로 누적하고 `evidence_count`는 집계값으로 본다.
- 단일 record를 갱신하는 하네스에서는 `evidence_count`, `last_observed_at`, `status_changed_at` 갱신을 허용할 수 있다.
- 어떤 방식을 쓰는지 하네스별 ledger policy에 명시한다.

## 8. lifecycle/status 정의

권장 status:

| status | 의미 |
|---|---|
| `observed` | 후보가 처음 관찰됐다. 아직 검토가 충분하지 않다. |
| `needs_review` | 반복성이나 영향이 있어 사람 검토가 필요하다. |
| `pilot_required` | 정식 반영 전에 작은 pilot 검증이 필요하다. |
| `approved_for_change` | 정식 변경을 준비해도 된다는 승인을 받았다. |
| `promoted` | schema, procedure, rubric, runbook 등 정식 규칙에 반영됐다. |
| `rejected` | 후보로 유지하지 않기로 결정했다. |
| `deferred` | 지금은 판단하지 않고 조건부로 재검토한다. |

주의:

- `approved_for_change`와 `promoted`를 구분한다.
- `approved_for_change`는 변경 준비 승인이다.
- `promoted`는 실제 정식 규칙에 반영된 상태다.
- `approved`처럼 의미가 넓은 상태값은 피하는 편이 좋다.

## 9. review trigger

아래 조건이 나타나면 candidate를 검토 대상으로 올린다.

| trigger | 의미 |
|---|---|
| 같은 후보가 3개 이상 distinct context에서 관찰된다 | 우연한 단발 사례가 아닐 수 있다. |
| 같은 후보가 5건 이상 누적된다 | 반복성이 충분히 보인다. |
| 기존 분류로 처리하면 의미 왜곡이 반복된다 | 기존 schema나 절차가 후보를 담지 못한다. |
| QA에서 같은 애매함이 2회 이상 기록된다 | 검증 기준이나 상태값 후보일 수 있다. |
| downstream 소비자가 후보를 구분해야 한다 | 다음 하네스 입력 안정성과 관련된다. |
| 새 source, 자동화, schema 변경이 운영 반영을 요구한다 | Phase 7 pilot validation 후보가 된다. |

trigger는 자동 승격 조건이 아니다.
trigger는 검토 또는 pilot으로 올릴 신호다.

## 10. 승격 원칙

후보를 정식 규칙으로 승격하려면 아래를 확인한다.

| 조건 | 설명 |
|---|---|
| 반복성 | distinct context 또는 충분한 evidence가 있다. |
| 의미 필요성 | 기존 분류, 상태값, schema로 처리하면 의미가 왜곡된다. |
| downstream 영향 | 다음 하네스나 사용자 판단이 이 후보 구분을 실제로 필요로 한다. |
| QA 가능성 | 새 후보가 QA 기준이나 schema 검증으로 확인 가능하다. |
| pilot 검증 | 운영 반영 전에 작은 검증이 필요하면 Phase 7 pilot을 거친다. |
| 사용자 승인 | schema, procedure, rubric, runbook 변경 전 approval-gate를 거친다. |
| 반영 위치 식별 | 어디에 반영할지 명확하다. 예: `harness/schemas/`, `harness/procedures/`, `harness/rubrics/` |

승격은 candidate-ledger 안에서 끝나지 않는다.
정식 반영은 각 하네스의 schema, procedure, rubric, runbook, MANIFEST 변경으로 완료된다.

## 11. 폐기/보류 원칙

후보는 승격되지 않을 수도 있다.
`rejected`와 `deferred`는 명확한 이유와 재검토 조건을 가져야 한다.

상태 변경 기준:

| 변경 | 기본 처리 |
|---|---|
| `observed` 생성 | agent/adapter가 가능하다. |
| `needs_review` 전환 | agent/adapter가 가능하되 근거를 남긴다. |
| `pilot_required` 전환 | 사용자 확인을 권장한다. |
| `approved_for_change` 전환 | 명시 승인 필요. |
| `promoted` 전환 | 실제 schema/procedure/rubric/runbook 변경 승인 필요. |
| `rejected` 전환 | 단순 중복이나 오류는 agent 판단 가능. 의미 있는 후보 폐기는 사용자 확인을 권장한다. |
| `deferred` 전환 | agent 판단 가능하되 재검토 조건을 남긴다. |

`rejected` 원칙:

- 단순 중복, 잘못된 관찰, 이미 기존 규칙으로 충분한 경우 rejected로 둘 수 있다.
- 후보가 의미 있는 구조 변경 가능성을 담고 있다면 사용자 확인 없이 폐기하지 않는다.
- rejected 사유를 남긴다.

`deferred` 원칙:

- deferred는 "언젠가"가 아니다.
- `review_after`, `review_condition`, `defer_reason` 중 최소 하나를 둔다.
- 조건 없는 deferred가 계속 쌓이면 ledger가 쓰레기통이 된다.

## 12. module 연결

candidate-ledger는 다른 module과 연결되지만, 그 module을 대체하지 않는다.

| 연결 대상 | candidate-ledger 역할 | 상대 module 역할 |
|---|---|---|
| `pilot-first / testing` | pilot으로 보낼 후보를 기록한다. | Phase 7에서 실제로 작게 검증한다. |
| `approval-gate` | 승격, 폐기, 운영 반영에 승인 필요 여부를 표시한다. | 승인 수준과 승인 절차를 정한다. |
| `qa-scaffold` | QA ambiguity, 반복 failure reason 후보를 기록한다. | QA 범주, 상태값, finding, escalation 구조를 제공한다. |
| `type-schema` | schema/rubric 후보를 바로 확정하지 않고 기록한다. | 하네스 유형별 schema/rubric 구조의 필요성을 연결한다. |
| `observability` | 반복 병목, trim 후보, 운영 관찰에서 나온 후보를 받는다. | run-summary와 실행 관찰 필드를 제공한다. |
| `docs-organization` | ledger 위치와 관련 note 색인을 docs에 남긴다. | docs 안에서 찾기 쉬운 위치와 색인을 정한다. |

candidate-ledger는 "후보가 있다"를 말한다.
pilot, 승인, schema 변경, QA 변경은 각 module과 하네스별 절차가 맡는다.

## 13. 하네스별로 정할 것

아래 항목은 전역 v1에서 고정하지 않는다.

| 항목 | 이유 |
|---|---|
| 실제 파일명과 위치 | 하네스마다 `harness/`, `artifacts/`, `docs/` 구조가 다르다. |
| JSONL schema의 정확한 필드 세트 | 후보 유형과 downstream 소비자가 다르다. |
| record/evidence 물리 분리 여부 | 작은 하네스와 큰 하네스의 운영 방식이 다르다. |
| candidate_id 형식 | 도메인, 날짜, run-id, type prefix 사용 여부가 다르다. |
| evidence_count 갱신 방식 | append-only 또는 in-place update 선택이 다르다. |
| status 값 추가 | 하네스별 lifecycle이 더 필요할 수 있다. |
| review trigger 숫자 | 3 contexts, 5 events 같은 기준은 조정될 수 있다. |
| schema/procedure/rubric 반영 절차 | 각 하네스의 공통 원장과 approval-gate를 따른다. |
| pilot 성공/실패 기준 | Phase 7 pilot validation과 하네스별 QA 기준을 따른다. |

전역 template는 후보 원장의 최소 계약과 경계를 제공한다.
실제 운영 정책은 각 하네스가 정한다.

## 14. 적용 전 체크리스트

새 하네스에 이 module을 적용하기 전에 확인한다.

- [ ] candidate-ledger를 정식 schema가 되기 전의 대기실로 적용한다.
- [ ] 새 후보를 바로 schema, procedure, rubric, runbook에 넣지 않기로 했다.
- [ ] 후보로 기록할 대상 범주를 하네스별로 정했다.
- [ ] candidate record와 evidence event를 개념적으로 구분했다.
- [ ] record/evidence를 같은 파일에 둘지, 별도 파일로 나눌지, summary-only로 둘지 정했다.
- [ ] append-first를 기본 원칙으로 보고, in-place update를 허용한다면 그 범위를 명시했다.
- [ ] `evidence_count`를 원천 증거가 아니라 집계값 또는 최신 요약값으로 본다.
- [ ] `first_observed_at`, `last_observed_at`, `status_changed_at` 사용 여부를 정했다.
- [ ] `approved_for_change`와 `promoted`를 구분했다.
- [ ] review trigger가 자동 승격 조건이 아니라 검토 또는 pilot 신호임을 확인했다.
- [ ] 승격 전 반복성, 의미 필요성, downstream 영향, QA 가능성, 반영 위치를 확인한다.
- [ ] schema, procedure, rubric, runbook 변경 전 approval-gate를 거친다.
- [ ] `rejected` 전환 시 의미 있는 후보 폐기는 사용자 확인을 권장한다.
- [ ] `deferred`에는 `review_after`, `review_condition`, `defer_reason` 중 최소 하나를 둔다.
- [ ] pilot 검증은 Phase 7 pilot validation으로 넘긴다.
- [ ] 실제 ledger 파일명, 위치, JSONL schema, candidate_id 형식은 하네스별로 정한다.
