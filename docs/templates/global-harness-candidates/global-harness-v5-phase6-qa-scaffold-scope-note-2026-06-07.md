# Global Harness v5 Phase 6 QA Scaffold Scope Note

- 작성일: 2026-06-07
- 상태: Phase 6 qa-scaffold v1 후보 작성 전 scope note. module template 본문이 아니다.
- 대상: `global-harness-qa-scaffold-template-v0.md`
- 기준 문서:
  - `global-harness-core-structure-template-v1.md`
  - `global-harness-v5-phase4-hook-cross-check-2026-06-06.md`
  - `global-harness-v5-phase5-remaining-module-location-decision-note-2026-06-06.md`
  - `global-harness-v5-phase6-module-diagnostic-note-2026-06-06.md`
  - `harness/procedures/source-pack-qa.md`
  - `harness/rubrics/source-pack-qa.rubric.md`

## 1. 목적

이 note는 `qa-scaffold` v1 후보에 무엇을 직접 담고, 무엇을 하네스별 QA procedure, schema, rubric으로 남길지 정리한다.

핵심 질문:

```text
qa-scaffold는 모든 하네스가 QA를 설계할 수 있게 하는 공통 뼈대인가,
아니면 Source Pack 같은 특정 하네스의 상세 QA 절차를 담는 문서인가?
```

현재 판단은 전자다.

`qa-scaffold`는 공통 QA 설계 뼈대다.
특정 도메인의 세부 검증 단계, schema field, rubric 가중치, 자동 실패 조건을 전역에서 강제하지 않는다.

## 2. 한 줄 결론

`qa-scaffold` v1 후보 파일을 만드는 것이 좋다.

다만 v1은 구체 QA 질문 목록이 아니라 아래를 제공해야 한다.

- 공통 QA 항목 범주
- 공통 상태값과 그 의미
- QA 결과 파일의 최소 skeleton
- failure, 확인 필요, repair, 재검증 기록 방식
- `comparison`과 `type-schema` 연결 기준
- `approval-gate`, `security-baseline`으로 넘길 escalation 표시 방식

## 3. qa-scaffold의 역할

`qa-scaffold`가 맡을 것:

| 역할 | 설명 |
|---|---|
| QA 설계 뼈대 | 하네스별 QA procedure를 만들 때 빠뜨리면 안 되는 범주와 결과 구조를 제공한다. |
| 상태값 후보 | `pass`, `partial_pass`, `unverified`, `fail`, `stopped` 같은 공통 판정 언어를 제공한다. |
| 결과 파일 skeleton | QA 결과가 대화에만 남지 않도록 최소 `qa.md` 형태를 제공한다. |
| repair/recheck 흐름 | 문제 발견 후 수정, 재검증, 기록의 최소 흐름을 제공한다. |
| comparison 연결 | 두 모델 또는 두 실행 결과를 같은 기준으로 평가할 수 있게 공통 rubric 연결을 둔다. |
| type-schema 연결 | 하네스 유형과 품질 축을 실제 검증 기준으로 구체화하는 위치를 안내한다. |

`qa-scaffold`가 맡지 않을 것:

| 비범위 | 이유 |
|---|---|
| 하네스별 상세 QA 절차 | 산출물 유형과 도메인마다 달라진다. |
| 특정 schema field 검사 | 각 하네스의 `harness/schemas/`와 QA procedure가 맡는다. |
| rubric 점수 가중치 | 하네스별 `harness/rubrics/`가 맡는다. |
| 운영 원장 상태 매핑 | 각 하네스의 schema, runbook, ledger 규칙이 맡는다. |
| 승인 절차 수행 | `qa-scaffold`는 승인 필요를 표시하고, 실제 승인 기준은 `approval-gate`가 맡는다. |
| 보안 판단 상세 | 보안 이슈 발견 시 `security-baseline`으로 넘긴다. |

## 4. v1에 직접 담을 항목

`qa-scaffold` v1에 직접 담을 항목은 아래로 제한한다.

| 항목 | v1 포함 여부 | 이유 |
|---|---:|---|
| 공통 QA 항목 범주 | 포함 | 모든 하네스 QA가 최소한 어떤 축을 봐야 하는지 알려준다. |
| 공통 상태값 정의 | 포함 | 모델과 adapter가 같은 판정 언어를 쓰게 한다. |
| QA 결과 파일 최소 skeleton | 포함 | QA 결과가 대화에만 남지 않게 한다. |
| repair/recheck 최소 흐름 | 포함 | 실패를 숨기지 않고 수정과 재검증으로 연결한다. |
| `partial_pass`와 `unverified` 구분 | 포함 | 두 상태의 운영 의미와 repair 방식이 다르다. |
| `comparison` 연결 | 포함 | 같은 rubric으로 두 결과를 평가하게 한다. |
| `type-schema` 연결 | 포함 | 품질 축을 검증 기준으로 구체화하는 위치를 잡아준다. |
| escalation 표시 방식 | 포함 | 승인, 중단, 보안 검토 필요를 다른 module로 넘길 수 있게 한다. |

## 5. 하네스별로 둘 항목

아래 항목은 `qa-scaffold` v1에 직접 넣지 않는다.

| 항목 | 둘 위치 |
|---|---|
| Source Pack 14단계 QA 같은 도메인별 절차 | `harness/procedures/{harness}-qa.md` |
| SEC catalog, raw 파일, transcript 같은 특정 검증 규칙 | 하네스별 QA procedure |
| 특정 JSONL field, schema 허용값 검사 | 각 하네스의 `harness/schemas/`와 QA procedure |
| 점수 가중치와 1-5점 rubric | 각 하네스의 `harness/rubrics/` |
| 자동 실패 조건의 구체 목록 | 하네스별 QA procedure/rubric |
| 운영 catalog, registry, index 상태 매핑 | 하네스별 schema, runbook, ledger |
| 도메인 금지사항 상세 | 하네스별 contract, procedure, rubric |

## 6. 공통 QA 항목 범주

v1은 "공통 QA 질문"을 고정된 문장으로 강제하지 않는다.
대신 모든 하네스가 자기 질문을 만들 때 참조할 공통 범주를 둔다.

| 범주 | 확인할 것 | 하네스별로 정할 것 |
|---|---|---|
| completeness | 필수 입력, 출력, 섹션, 기록이 빠지지 않았는가 | 필수 산출물과 필수 섹션 |
| placement | 산출물이 약속된 위치에 저장됐는가 | 저장 경로와 파일명 규칙 |
| traceability | 입력, 근거, 중간 산출물, 최종 산출물이 연결되는가 | 연결해야 할 record, log, index |
| schema/rubric conformance | 하네스별 schema와 rubric을 따르는가 | schema field, 허용값, rubric 항목 |
| violation | 금지 조건, 도메인 금지사항, 보안/권한 위반이 없는가 | 금지 문구, 금지 행동, 자동 실패 조건 |
| downstream readiness | 다음 하네스나 사람이 읽고 사용할 수 있는가 | 후속 입력 계약과 주의사항 |

`approval/stop`은 위 범주에 넣지 않는다.
승인 필요와 중단 필요는 산출물 품질 평가 범주가 아니라 QA 결과에서 도출되는 escalation 조건이다.

## 7. 상태값 모델

### 7.1 전체 QA 상태

`overall_status` 후보는 아래 다섯 개를 기본으로 둔다.

| 상태 | 의미 |
|---|---|
| `pass` | 필수 QA가 통과했고 후속 사용 가능하다. |
| `partial_pass` | 검증은 수행됐고 핵심 사용은 가능하지만 일부 보완이나 주의가 필요하다. |
| `unverified` | 검증 자체가 충분히 수행되지 않아 후속 사용 전 확인이 필요하다. |
| `fail` | 후속 사용 위험이 크거나 필수 조건이 깨졌다. |
| `stopped` | 승인, 외부 조건, 도구, 전제 조건 문제로 QA 전체를 완료하지 못했다. |

`repair_required`는 전체 `overall_status`가 아니다.
`repair_required`는 개별 finding 또는 repair/recheck action에서 사용하는 조치 상태다.

### 7.2 `partial_pass`와 `unverified` 구분

| 상태 | 핵심 차이 | repair/recheck 방향 |
|---|---|---|
| `partial_pass` | 검증은 됐고 핵심 산출물은 사용할 수 있지만 일부 문제가 있다. | 문제 항목을 수정한 뒤 해당 항목을 재검증한다. |
| `unverified` | 검증 조건이 부족해 충분히 판단하지 못했다. | 입력, 도구, 권한, 기준 등 검증 조건을 먼저 확보한 뒤 QA를 다시 실행한다. |

`partial_pass`는 "확인했고 일부 부족함"이다.
`unverified`는 "충분히 확인하지 못함"이다.

이 둘을 섞으면 후속 하네스가 "써도 되는 자료인지" 판단하기 어렵다.

### 7.3 finding-level `stopped`와 escalation `stop_required`

`stopped`는 두 층에서 다르게 쓰인다.

| 위치 | 의미 |
|---|---|
| finding-level `stopped` | 특정 QA 항목을 끝까지 검증하지 못했다. |
| Escalation `stop_required` | 전체 실행, 운영 반영, 후속 사용을 멈춰야 한다. |

원칙:

- finding-level `stopped`는 전체 `stop_required`를 자동 trigger하지 않는다.
- `stop_required`는 영향 범위, 필수 산출물 여부, 보안/승인 위험을 보고 별도로 판단한다.
- optional 항목의 finding-level `stopped`는 전체 중단이 아니라 `partial_pass` 또는 `unverified`로 이어질 수 있다.
- 필수 산출물, 보안 이슈, 승인 없는 운영 반영이 걸리면 finding 상태와 별개로 `stop_required`가 필요할 수 있다.

## 8. Repair / Recheck 흐름

repair loop는 문제를 고치는 절차 전체를 전역에서 강제하지 않는다.
다만 아래 최소 흐름은 v1에 둔다.

```text
finding 발견
-> evidence 기록
-> required_action 지정
-> owner_or_location 지정
-> recheck_needed 여부 표시
-> 재검증 결과 기록
```

상태별 처리 방향:

| finding 상태 | 기본 처리 |
|---|---|
| `pass` | 별도 repair 없음 |
| `partial_pass` | 문제가 있는 항목만 수정하고 부분 재검증 |
| `unverified` | 검증 조건을 확보한 뒤 QA 재실행 또는 해당 범주 재검증 |
| `fail` | 필수 조건 파손이면 수정 전 후속 사용 금지 |
| `stopped` | 중단 원인을 해소한 뒤 같은 체크를 다시 수행 |
| `repair_required` | action 상태로 사용. 전체 QA 상태로 쓰지 않음 |

## 9. QA 결과 파일 최소 skeleton

v1에는 아래 최소 구조를 포함하는 것이 좋다.
각 하네스는 이 skeleton을 확장할 수 있지만, QA 결과를 대화 보고로만 끝내지 않는다.

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

## 10. Escalation 기준

`qa-scaffold`는 승인 절차를 수행하지 않는다.
QA 결과에서 승인, 중단, 보안 검토가 필요하다는 사실을 표시하고, 실제 판단은 해당 module로 넘긴다.

| escalation 필드 | 의미 | 넘길 위치 |
|---|---|---|
| `human_approval_needed` | 사람이 승인해야 할 조치가 있다. | `approval-gate` |
| `stop_required` | 전체 실행, 운영 반영, 후속 사용을 멈춰야 한다. | `approval-gate`, runbook |
| `security_review_needed` | secret, credential, `.env`, 공개 범위, 격리 파일 이슈가 있다. | `security-baseline` |
| `approval_gate_reference` | 어떤 승인 판단이 필요한지 한 줄로 적거나 관련 결정 파일, run note, work map 항목 경로를 적는다. | `approval-gate` |

`approval_gate_reference`는 엄격한 schema가 아니라 loose reference다.

예시:

```text
운영 registry 반영 전 사용자 승인 필요
docs/templates/global-harness-candidates/global-harness-approval-gate-template-v1.md Section 4 Level 3
artifacts/runs/{run-id}/run-summary.md 의 운영 반영 후보
```

## 11. comparison 연결

Phase 5 결정에 따라 `comparison`은 별도 파일 없이 registry에 등재되어 있고, `qa-scaffold`와 연결된다.

`qa-scaffold`가 맡을 것:

- 두 실행 결과를 같은 QA 범주로 평가한다.
- 같은 rubric 또는 같은 finding table 구조를 사용하게 한다.
- 차이를 `Findings`, `Repair / Recheck`, `Downstream Use`에 기록할 수 있게 한다.

`qa-scaffold`가 맡지 않을 것:

- comparison 실행 구조를 결정하지 않는다.
- Codex/Claude Code 병렬 실행 절차를 정의하지 않는다.
- 어떤 결과를 운영 반영할지 승인하지 않는다.
- 비교 결과 기록 저장 위치 전체를 독점하지 않는다.

연결 기준:

| 역할 | 위치 |
|---|---|
| comparison 필요 여부 | `design-preflight` |
| 공통 평가 기준 | `qa-scaffold` |
| 운영 반영 승인 | `approval-gate` |
| 비교 결과 기록 | `observability` 또는 하네스별 run 기록 |

## 12. type-schema 연결

Phase 5 결정에 따라 `type-schema`는 별도 파일 없이 registry에 등재되어 있고, `qa-scaffold`와 각 하네스의 `harness/schemas/`에 연결된다.

경계:

- `design-preflight`는 하네스 유형과 품질 축을 정한다.
- 각 하네스의 `harness/schemas/`는 실제 schema 파일을 둔다.
- 각 하네스의 `harness/rubrics/`는 실제 rubric을 둔다.
- `qa-scaffold`는 정해진 품질 축과 schema/rubric을 QA 검증 기준으로 구체화하는 뼈대를 제공한다.

주의:

`qa-scaffold`가 품질 축을 새로 정한다고 쓰지 않는다.
품질 축은 `design-preflight`에서 결정하고, `qa-scaffold`는 그것을 검증 기준으로 번역한다.

## 13. Source Pack에서 일반화할 것과 제외할 것

Source Pack QA는 참고 사례로 유용하지만, 전역 `qa-scaffold`가 Source Pack QA를 복사하면 안 된다.

일반화할 것:

| Source Pack 패턴 | 전역화 방식 |
|---|---|
| QA 결과를 `qa.md` 파일로 남김 | 모든 하네스가 QA 결과를 파일로 남기는 skeleton |
| `overall_status` 사용 | 공통 상태값 후보 |
| 기준/판정/근거/조치 테이블 | `Findings` table |
| 다음 하네스 사용 가능 여부 | `Downstream Use` section |
| 자동 실패 조건 | 하네스별 violation 기준을 둘 수 있다는 원칙 |
| 더 보수적인 판정 우선 | `fail`, `unverified`, `partial_pass` 판정 원칙 |

제외할 것:

| Source Pack 요소 | 제외 이유 |
|---|---|
| 14단계 QA 제목과 순서 | Source Pack 실행 절차에 특화됨 |
| SEC catalog JSONL field 검사 | Source Pack schema에 특화됨 |
| raw/file hash, transcript, IR 세부 규칙 | 수집형 하네스 도메인 규칙 |
| 100점 rubric 가중치 | Source Pack 품질 축에 특화됨 |
| `runs.jsonl.status`, `index.md.catalog_status` 매핑 | Source Pack 운영 원장 schema에 특화됨 |

## 14. v1 권장 목차

`global-harness-qa-scaffold-template-v1.md`를 만든다면 아래 목차를 권장한다.

1. 목적
2. 범위와 비범위
3. QA 설계 흐름
4. 공통 QA 항목 범주
5. 상태값 모델
6. `partial_pass`와 `unverified` 구분
7. Repair / Recheck 흐름
8. QA 결과 파일 최소 skeleton
9. Escalation 표시 방식
10. comparison 연결
11. type-schema 연결
12. 전역화 기준과 제외 항목
13. 적용 전 체크리스트

v1 파일의 메타 정보에는 아래 한 줄을 둔다.

```md
- 기반: Source Pack QA procedure/rubric에서 전역 QA scaffold로 일반화
```

v1 Section 12는 작업 이력 설명이 아니라 새 하네스 적용자가 읽는 전향적 경계표로 쓴다.

권장 형식:

| 항목 | 전역 scaffold가 제공하는 것 | 하네스별로 정할 것 |
|---|---|---|
| QA 결과 파일 | 최소 skeleton | 실제 섹션과 세부 단계 |
| 상태값 | 공통 후보와 의미 | 하네스별 상태 매핑 |
| 검증 범주 | 6개 범주 | 실제 질문과 자동 실패 조건 |
| schema/rubric | 연결 기준 | 실제 schema field와 rubric 가중치 |
| repair/recheck | 최소 흐름 | 실제 수정 절차와 재검증 조건 |
| downstream use | 사용 가능 여부 기록 방식 | 다음 하네스별 입력 계약 |

## 15. 결정 결과

Claude Code 교차검증 후 아래처럼 결정했다.

```text
global-harness-qa-scaffold-template-v1.md 후보 파일을 만든다.
```

작성 시에는 Section 14 권장 목차의 13개 섹션을 따르고, `다음 검증` 같은 작업 진행 상태 섹션은 v1 본문에 넣지 않는다.
