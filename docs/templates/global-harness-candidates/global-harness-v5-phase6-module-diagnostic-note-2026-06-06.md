# Phase 6 Module Diagnostic Note

- 작성일: 2026-06-06
- 대상: approval-gate, qa-scaffold, security-baseline, observability, checkpoint, docs-organization, candidate-ledger
- 기준 문서: `global-harness-v5-work-map.md`, `global-harness-core-structure-template-v1.md`, 각 module v0 후보 파일
- 상태: Phase 6 개별 v1 후보화 전 전체 진단

## 1. 목적

Phase 6에서는 module v0를 바로 전역화하거나 v1로 고치지 않는다.
먼저 각 module v0가 현재 v5 구조에서 어떤 상태인지, v1 후보화가 필요한지, 어떤 순서로 다룰지를 판정한다.

이 note는 실행 파일이나 최종 module template가 아니다.
개별 module 수정에 들어가기 전의 작업 지도다.

## 2. 판단 기준

| 기준 | 질문 |
|---|---|
| v0 상태 | 현재 v0가 원칙 수준인지, 바로 쓸 수 있는 template 수준인지 |
| v1화 필요도 | v5 core와 함께 쓰려면 지금 다듬어야 하는지 |
| 수정 위험도 | 잘못 다듬으면 core/module 경계나 하네스 품질에 영향을 주는지 |
| 비교 대상 | v1화 전에 대조해야 할 문서나 실제 사례가 있는지 |
| 추천 처리 | 바로 v1 후보화, deep review, 보류, 또는 pilot 이후 재검토 중 무엇이 맞는지 |

## 3. 전체 진단표

| module | v0 상태 | v1화 필요도 | 수정 위험도 | 비교 대상 | 추천 처리 |
|---|---|---|---|---|---|
| approval-gate | 논의/실행 분리 원칙은 명확하지만, 승인 문구와 위험 작업 경계가 아직 얇다 | 높음 | 중간 | v1 core approval hook, Phase 4 cross-check, 현재 사용자 workflow | v1 후보화 우선 |
| qa-scaffold | 공통 QA 질문과 상태값 후보는 좋지만, QA 결과 파일 템플릿과 repair loop가 아직 없다 | 높음 | 중간~높음 | v1 core QA hook, Source Pack QA 절차, `comparison`/`type-schema` 연결 | v1 후보화 우선 |
| security-baseline | 최소 보안 기본선은 명확하지만, 체크리스트와 공개/비공개 권고가 아직 얇다 | 중간 | 높음 | v1 security hook, `.gitignore` 후보, 공개 범위 승인 규칙 | scope를 좁혀 v1 후보화 |
| observability | global v0가 원본 Source Pack observability template의 핵심을 많이 압축했다 | 높음 | 중간 | `docs/templates/source-pack-observability-template.md` | 별도 deep review 우선 |
| checkpoint | compact checkpoint 원칙과 기본 섹션은 좋지만, `/저장`과 adapter 경계가 미정이다 | 중간 | 중간 | `/저장` workflow, session checkpoint 실제 사용 사례 | v1 후보화 전 adapter 경계 정리 |
| docs-organization | 삭제 금지와 역할별 정리 원칙은 좋지만, 적용 기준과 README template가 없다 | 낮음~중간 | 낮음 | 현재 `docs/` 구조, README 후보 | 뒤쪽에서 가볍게 v1 후보화 |
| candidate-ledger | 후보 기록 철학과 필드는 좋지만, pilot-first 기록과 schema 승격 절차가 아직 얇다 | 중간~높음 | 중간 | `pilot-first / testing`, Source Pack candidate ledger 사례 | qa/security 이후 v1 후보화 |

## 4. Module별 진단

### 4.1 approval-gate

현재 강점:

- 논의 요청과 실행 요청을 분리한다.
- 파일 수정은 명시적 요청이 필요하다는 원칙이 분명하다.
- 삭제, 대량 이동, schema 변경, 외부 공개 같은 위험 작업을 따로 본다.

v1 후보화에서 보강할 것:

- read-only 검토, 일반 편집, 구조 변경, 위험 작업의 승인 수준을 나눈다.
- "평가해줘", "논의해보자"는 실행 승인이 아니라는 규칙을 더 짧은 체크리스트로 만든다.
- 사용자가 명시적으로 "수정해줘"라고 했을 때도 범위가 큰 작업이면 어떤 확인을 추가로 해야 하는지 정한다.

추천:

- Phase 6의 첫 번째 또는 두 번째 개별 v1 후보화 대상으로 적절하다.
- 현재 사용자 workflow가 논의와 실행을 분리하는 방향으로 정리됐으므로, 실제 필요성이 이미 검증됐다.

### 4.2 qa-scaffold

현재 강점:

- 모든 하네스가 확인해야 할 공통 QA 질문을 잘 잡고 있다.
- `pass`, `partial_pass`, `unverified`, `fail`, `stopped`, `repair_required` 상태값 후보가 있다.
- Source Pack의 세부 QA를 전역으로 복사하지 않는 경계가 있다.

v1 후보화에서 보강할 것:

- QA 결과 파일의 최소 템플릿이 필요하다.
- failure, 확인 필요, repair loop, 재검증의 최소 기록 방식을 정해야 한다.
- `comparison`에서 두 모델 결과를 같은 기준으로 평가하는 "공통 rubric" 연결을 명시해야 한다.
- `type-schema`와 연결될 때, qa-scaffold가 품질 축을 정하는 것이 아니라 검증 기준으로 구체화한다는 경계를 유지해야 한다.

추천:

- approval-gate와 함께 Phase 6 초반에 다룬다.
- 다만 Source Pack QA 전체를 전역으로 가져오면 과해지므로, 공통 뼈대만 남겨야 한다.

### 4.3 security-baseline

현재 강점:

- secret, credential, `.env`, 민감 파일, 보안 격리 파일, 외부 공개 승인 경계가 명확하다.
- 보안 제품이 격리한 파일을 열거나 복구하지 않는 원칙이 들어 있다.
- 전역에서 모든 docs/raw 공개 여부를 단정하지 않는 경계가 있다.

v1 후보화에서 보강할 것:

- 하네스 생성 시 최소 보안 체크리스트가 필요하다.
- `.gitignore` 후보와 프로젝트별 예외 판단을 분리해야 한다.
- 공개 저장소와 비공개 저장소에서 점검할 항목을 나눌지 검토한다.

추천:

- v1 후보화는 필요하지만 scope를 작게 유지한다.
- 보안 module이 지나치게 커지면 core보다 무거워질 수 있으므로, "초보자가 실수하지 않게 하는 최소 안전선"으로 제한한다.

### 4.4 observability

현재 강점:

- 선택 메모이며 QA 실패 조건이 아니라는 원칙이 있다.
- 기본 4개 필드가 있다: `instructions_files_consulted`, `instructions_lines_consulted_estimate`, `bottleneck_note`, `trim_candidate`.
- 자동화 도입 조건이 있다.

현재 갭:

- Source Pack 원본 템플릿에는 적용 위치, 작성 예시, 숫자 기록 기준, QA와의 분리, 전역화 기준이 더 자세히 들어 있다.
- global v0는 원본의 핵심을 너무 압축했을 가능성이 있다.
- "Source Pack 이름 제거한 완성 템플릿"이 아직 미완료로 남아 있다.

추천:

- Phase 6에서 별도 deep review를 먼저 한다.
- 원본 Source Pack template를 그대로 복사하지 말고, 전역 module에 필요한 항목만 추려 v1 후보를 만든다.
- 특히 "선택 섹션", "QA 실패 조건 아님", "정확한 토큰 계측 아님", "새 파일을 만들지 않음" 네 가지는 보존 우선이다.

### 4.5 checkpoint

현재 강점:

- 원문 전체 저장 금지와 compact checkpoint 원칙이 명확하다.
- `docs/session-checkpoints/` 위치 후보와 기본 섹션 후보가 있다.
- `initial`, `incremental`, `recovery` 저장 모드 후보가 있다.

v1 후보화에서 보강할 것:

- `/저장` 명령과 일반 handoff 문서의 관계를 정해야 한다.
- Codex/Claude adapter가 각각 checkpoint를 어떻게 남길지 경계를 정해야 한다.
- checkpoint가 대화 원문 저장으로 오해되지 않게 금지 문구를 강화해야 한다.

추천:

- Phase 6 중반에 다룬다.
- 실제 `/저장` workflow가 이미 있으므로, v1 후보화 전 현재 사용 방식과 충돌하지 않는지 확인한다.

### 4.6 docs-organization

현재 강점:

- 문서가 적을 때부터 과도하게 나누지 않는다는 원칙이 좋다.
- 많아졌을 때의 역할별 폴더 후보가 있다.
- 이동 전 참조 점검과 삭제 금지 원칙이 있다.
- handoff의 역사적 경로는 유지할 수 있다는 판단이 실용적이다.

v1 후보화에서 보강할 것:

- 문서 수나 혼잡도 기준이 필요하다.
- 이동 전 점검 명령 예시가 있으면 실무성이 높아진다.
- `docs/README.md` 템플릿과 링크 업데이트 우선순위를 정해야 한다.

추천:

- 위험도는 낮으므로 Phase 6 후반에 가볍게 다룬다.
- 전역 강제 구조가 아니라 "문서가 많아졌을 때 적용하는 정리 module"로 유지한다.

### 4.7 candidate-ledger

현재 강점:

- 새 유형, 상태값, 예외 규칙을 바로 schema에 넣지 않고 후보로 누적한다는 원칙이 좋다.
- candidate record 필드가 꽤 구체적이다.
- 사용자 승인 요청 기준 후보가 있다.

v1 후보화에서 보강할 것:

- `pilot-first / testing`과 어떻게 연결되는지 명시해야 한다.
- 후보를 정식 schema로 승격하는 절차가 필요하다.
- JSONL schema template를 둘지, table-based ledger로도 허용할지 결정해야 한다.

추천:

- qa-scaffold와 security-baseline 이후에 다룬다.
- schema 변경 승인과 연결되므로 approval-gate와도 맞물려야 한다.

## 5. 추천 작업 순서

내 추천 순서는 아래와 같다.

| 순서 | module | 이유 |
|---|---|---|
| 1 | observability | 사용자가 원본 Source Pack template와 global v0의 차이를 이미 지적했고, 별도 비교 대상이 명확하다. |
| 2 | approval-gate | 현재 협업 방식의 핵심 안전장치이며 논의/실행 분리 원칙과 직접 연결된다. |
| 3 | qa-scaffold | `comparison`, `type-schema`, 하네스별 완료 기준과 연결되므로 초반에 정리해야 한다. |
| 4 | security-baseline | 보안은 중요하지만 scope를 작게 유지해야 하므로 approval/QA 경계 이후에 다루는 편이 좋다. |
| 5 | candidate-ledger | pilot-first/testing과 schema 승격 절차를 이어서 정리한다. |
| 6 | checkpoint | `/저장` workflow와 충돌하지 않게 중반 이후 정리한다. |
| 7 | docs-organization | 위험도가 낮고 정리 module 성격이 강하므로 후반에 다룬다. |
| 8 | module registry | Phase 6 종료 후 Section 12 정렬 원칙과 전체 순서를 재검토한다. |

대안:

- core hook과 가까운 순서를 우선하면 approval-gate -> qa-scaffold -> security-baseline -> observability 순서도 가능하다.
- 그러나 현재 열린 질문과 사용자의 우려를 기준으로는 observability를 먼저 deep review하는 편이 더 낫다.

## 6. Phase 6 산출물 제안

Phase 6에서는 module마다 바로 v1 파일을 만들기보다, 각 module별로 아래 산출물 중 하나를 선택한다.

| 산출물 | 사용할 때 |
|---|---|
| `*-phase6-*-review-note-2026-06-06.md` | v0와 원본/reference를 비교하고 판단을 먼저 남길 때 |
| `global-harness-*-template-v1.md` | v1 후보화 방향이 명확하고 별도 파일로 남길 때 |
| work map decision only | v0 유지 또는 pilot 이후 재검토로 충분할 때 |

## 7. 다음 논의 질문

다음 단계에서 바로 결정할 질문은 하나다.

```text
Phase 6 첫 개별 작업을 observability deep review로 시작할까,
아니면 core hook과 가까운 approval-gate부터 v1 후보화할까?
```

내 추천은 observability deep review부터 시작하는 것이다.

이유:

- Source Pack 원본 템플릿이라는 명확한 비교 대상이 있다.
- 사용자가 이미 누락 가능성을 우려한 module이다.
- v1 후보 파일을 만들기 전에 무엇을 보존할지 판정하는 방식이 Phase 3.5 design-preflight와 같은 안정적인 흐름이다.
