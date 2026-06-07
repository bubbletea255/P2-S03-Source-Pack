# Global Harness v5 Work Map

- 작성일: 2026-06-06
- 상태: 살아있는 작업 지도. v5 core나 module template 자체가 아니라, 앞으로의 작업 순서와 진행 상태를 추적하는 운영판이다.
- 위치: `docs/templates/global-harness-candidates/`
- 원칙: 작업이 진행되면 이 문서를 갱신한다. 순서 변경, 항목 추가, 보류, 삭제를 허용한다.

## 1. 이 문서의 역할

이 문서는 Global Harness v5 논의를 이어가기 위한 작업 지도다.

목적:

- 앞으로 해야 할 일을 한곳에서 본다.
- 어떤 파일을 기준으로 작업할지 확인한다.
- v5 core, module template, 검증, 전역화 논의를 섞지 않는다.
- 완료한 일과 남은 일을 체크한다.
- 다음 채팅방이나 다음 작업자가 바로 이어갈 수 있게 한다.

주의:

```text
이 문서는 템플릿 본문이 아니다.
이 문서는 전역 harness-lab을 수정하지 않는다.
이 문서는 v5 core 초안을 쓰기 전의 작업 항법장치다.
```

## 2. 현재 큰 방향

현재 합의된 구조:

| 층위 | 의미 | 처리 |
|---|---|---|
| 전역 `harness-lab` | 하네스 설계 철학과 방법론의 기초 | 원본 유지. 수정하지 않음 |
| v5 core | Codex와 Claude Code가 같은 하네스를 실행하기 위한 모델 중립 실행 구조 | 후보 문서로 설계 |
| v5 modules | 승인, QA, 보안, 관측, 체크포인트, docs 정리, candidate 원장 등 선택 운영 장치 | 주제별 module template로 분리 |
| pilot / validation | v5 후보 구조가 실제 하네스에서 작동하는지 확인 | 다음 하네스에서 검증 |

현재 핵심 판단:

- v4는 폐기하지 않는다.
- v5 core는 v4를 기반으로 보강한다.
- v5 core는 거대한 단일 문서가 아니라 core + modules 구조로 간다.
- `Completion Contract`라는 새 용어는 만들지 않는다.
- 완료 기준은 산출물 계약, QA 기준, 승인/중단 조건의 조합으로 설명한다.
- 관측 가능성, 체크포인트, docs organization, candidate ledger는 core 본문이 아니라 module registry에서 발견 가능하게 둔다.

## 3. 기준 문서 지도

| 파일 | 역할 | 현재 사용법 |
|---|---|---|
| `docs/templates/공용_하네스_템플릿_체크리스트_v4.md` | 기존 공용 템플릿의 기준 뼈대 | v5 core와 비교할 baseline |
| `docs/templates/global-harness-candidates/README.md` | 후보 템플릿 폴더의 안내 문서 | 후보 파일 목록과 전역화 원칙 확인 |
| `docs/templates/global-harness-candidates/global-harness-template-v5-discussion-handoff-2026-06-06.md` | 이전 논의 인계 문서 | v5 논의의 배경과 11개 항목 확인 |
| `docs/templates/global-harness-candidates/global-harness-v5-core-scope-consensus-2026-06-06.md` | v5 core 범위 합의 메모 | core/module 경계의 현재 기준 |
| `docs/templates/global-harness-candidates/global-harness-core-structure-template-v0.md` | core structure v0 후보 | v5 core 초안의 직접 입력 |
| `docs/templates/global-harness-candidates/global-harness-v5-design-preflight-extraction-note-2026-06-06.md` | v4 설계 판단 장치 추출 note | `design-preflight` module 분리 여부 판단에 사용 |
| `docs/templates/global-harness-candidates/global-harness-design-preflight-template-v0.md` | design-preflight v0 후보 | 새 하네스 청사진 전에 유형, 산출물 역할, 품질 축, 수준 선언, 7요소를 결정할 때 사용 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase1-3_5-review-note-2026-06-06.md` | Phase 1~3.5 checkpoint review | Phase 4 진입 전 문서 drift와 누락 후보 확인에 사용 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase4-hook-cross-check-2026-06-06.md` | Phase 4 hook cross-check note | v1 approval/QA/security hook과 각 module v0의 충돌 여부 확인 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase5-module-registry-comparison-2026-06-06.md` | Phase 5 module registry file comparison | v1 registry와 실제 후보 파일 목록 대조 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase5-module-usage-note-2026-06-06.md` | Phase 5 module usage note | v1 registry의 "사용할 때" 문구와 각 module v0 범위 대조 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase5-path-convention-note-2026-06-06.md` | Phase 5 path convention note | v1 registry의 후보 파일 경로 표기 방식 결정 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase5-pilot-first-decision-note-2026-06-06.md` | Phase 5 pilot-first decision note | `pilot-first / testing`을 별도 module로 둘지 결정 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase5-remaining-module-location-decision-note-2026-06-06.md` | Phase 5 remaining module location decision note | `comparison`, `type-schema`, `file-template`, `adapter-template`, `meta-orchestrator` 위치 결정 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase6-module-diagnostic-note-2026-06-06.md` | Phase 6 module diagnostic note | 개별 module v1 후보화 전 전체 v0 상태와 작업 순서 진단 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase6-observability-deep-review-note-2026-06-06.md` | Phase 6 observability deep review note | Source Pack observability 원본과 global observability v0 비교 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase6-approval-gate-scope-note-2026-06-07.md` | Phase 6 approval-gate scope note | approval-gate v1 후보 작성 전 승인 수준과 경계 정리 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase6-qa-scaffold-scope-note-2026-06-07.md` | Phase 6 qa-scaffold scope note | qa-scaffold v1 후보 작성 전 공통 QA 뼈대와 하네스별 QA/rubric/schema 경계 정리 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase6-security-baseline-scope-note-2026-06-07.md` | Phase 6 security-baseline scope note | security-baseline v1 후보 작성 전 최소 안전선과 하네스별 보안 절차 경계 정리 |
| `docs/templates/global-harness-candidates/global-harness-approval-gate-template-v0.md` | approval gate v0 후보 | core hook과 module v1 후보 작성에 사용 |
| `docs/templates/global-harness-candidates/global-harness-approval-gate-template-v1.md` | approval gate v1 후보 | 승인 수준 4단계와 중단 조건을 담은 현재 approval-gate 후보 |
| `docs/templates/global-harness-candidates/global-harness-qa-scaffold-template-v0.md` | QA scaffold v0 후보 | QA hook과 module v1 후보 작성에 사용 |
| `docs/templates/global-harness-candidates/global-harness-qa-scaffold-template-v1.md` | QA scaffold v1 후보 | 공통 QA 범주, 상태값, QA 결과 skeleton, repair/recheck, escalation 기록 기준을 담은 현재 QA 후보 |
| `docs/templates/global-harness-candidates/global-harness-security-baseline-template-v0.md` | security baseline v0 후보 | 보안 hook과 module v1 후보 작성에 사용 |
| `docs/templates/global-harness-candidates/global-harness-security-baseline-template-v1.md` | security baseline v1 후보 | 최소 보안 안전선, do-not-proceed, 사용자 알림 원칙을 담은 현재 security-baseline 후보 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase6-checkpoint-scope-note-2026-06-07.md` | Phase 6 checkpoint scope note | checkpoint v1 후보 작성 전 전역 skill, adapter, v5 template 경계 정리 |
| `docs/templates/global-harness-candidates/global-harness-observability-template-v0.md` | observability v0 후보 | 보존된 초기 후보 |
| `docs/templates/global-harness-candidates/global-harness-observability-template-v1.md` | observability v1 후보 | Source Pack 원본 구조를 전역화한 현재 observability 후보 |
| `docs/templates/global-harness-candidates/global-harness-checkpoint-template-v0.md` | checkpoint v0 후보 | module registry와 module v1 후보 작성에 사용 |
| `docs/templates/global-harness-candidates/global-harness-checkpoint-template-v1.md` | checkpoint v1 후보 | compact checkpoint 출력 계약, 저장 모드, anchor matching, handoff 경계를 담은 현재 checkpoint 후보 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase6-docs-organization-scope-note-2026-06-07.md` | Phase 6 docs-organization scope note | docs-organization v1 후보 작성 전 docs 구조 강제 여부, README 색인, 이동 전후 참조 점검 경계 정리 |
| `docs/templates/global-harness-candidates/global-harness-docs-organization-template-v0.md` | docs organization v0 후보 | module registry와 module v1 후보 작성에 사용 |
| `docs/templates/global-harness-candidates/global-harness-docs-organization-template-v1.md` | docs organization v1 후보 | 성장 후 docs 정리, `docs/README.md` 색인, 이동 전후 참조 점검을 담은 현재 docs-organization 후보 |
| `docs/templates/global-harness-candidates/global-harness-candidate-ledger-template-v0.md` | candidate ledger v0 후보 | module registry와 module v1 후보 작성에 사용 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase6-candidate-ledger-scope-note-2026-06-07.md` | Phase 6 candidate-ledger scope note | candidate-ledger v1 후보 작성 전 record/evidence, append-first, 승격/폐기/보류 경계 정리 |
| `docs/templates/global-harness-candidates/global-harness-candidate-ledger-template-v1.md` | candidate ledger v1 후보 | 후보 record/evidence, lifecycle, review trigger, 승격/폐기/보류 기준을 담은 현재 candidate-ledger 후보 |
| `docs/current/source-pack-architecture-map-2026-06-06.md` | Source Pack 실제 사례 지도 | v5 후보가 현실 구조와 맞는지 확인 |
| `docs/templates/source-pack-observability-template.md` | Source Pack 관측 가능성 템플릿 | observability module 일반화 참고 |

## 4. 작업 원칙

작업 중 계속 지킬 원칙:

- 전역 `harness-lab`은 수정하지 않는다.
- v5 core는 모델 중립 실행 구조에 집중한다.
- 공통 업무 의미는 `harness/`에 둔다.
- Codex/Claude Code adapter는 얇게 둔다.
- adapter에 공통 업무 규칙을 길게 복사하지 않는다.
- 산출물 계약과 수정 경계는 v5 core 본문에 직접 둔다.
- 승인, QA, 보안은 core 본문에 최소 hook만 둔다.
- 관측 가능성, 체크포인트, docs organization, candidate ledger, pilot-first는 module registry에서 발견 가능하게 둔다.
- v5 후보를 전역화하기 전에 최소 한 번 이상 실제 하네스에서 검증한다.

## 5. 단계별 작업 계획

상태값:

- `todo`: 아직 시작하지 않음
- `in_progress`: 현재 작업 중
- `done`: 완료
- `deferred`: 의도적으로 뒤로 미룸
- `blocked`: 외부 결정이나 추가 정보가 필요함

### Phase 0. 기준 정리와 작업판 생성

목표: 현재 논의와 파일 지도를 고정하고, 다음 작업이 길을 잃지 않게 한다.

| 상태 | 작업 | 산출물 |
|---|---|---|
| done | v5 논의 handoff 읽기 | 논의 배경 파악 |
| done | v5 core 범위 합의 메모 작성 | `global-harness-v5-core-scope-consensus-2026-06-06.md` |
| done | 작업 지도 작성 | `global-harness-v5-work-map.md` |
| done | 후보 폴더 README에 작업 지도와 현재 후보 파일 지도를 반영 | `README.md` |

### Phase 1. v5 core 범위 확정

목표: v5 core에 직접 들어갈 것, 최소 hook만 둘 것, module registry로 보낼 것을 확정한다.

| 상태 | 작업 | 산출물 |
|---|---|---|
| done | `harness-lab`과 v5 core의 역할 분리 | consensus 문서 |
| done | `Completion Contract` 용어 미사용 결정 | consensus 문서 |
| done | A/B/C/D 분류 초안 정리 | consensus 문서 |
| done | A/B/C/D 분류를 v5 core 초안용 표현으로 압축 | `global-harness-v5-core-gap-analysis-2026-06-06.md` 섹션 8-9 |
| done | 보류 질문 중 core 작성 전에 필요한 것만 선별 | `global-harness-v5-core-gap-analysis-2026-06-06.md` 섹션 12 |

### Phase 2. v5 core structure v0 및 v4 비교

목표: 기존 `global-harness-core-structure-template-v0.md`와 `공용_하네스_템플릿_체크리스트_v4.md`를 합의 메모와 비교해 v1 초안 방향을 잡는다.

| 상태 | 작업 | 산출물 |
|---|---|---|
| done | core structure v0 읽기 | v0 요약 |
| done | v4 템플릿에서 이미 있는 core 요소 확인 | v4 baseline note |
| done | v0와 consensus 차이 표시 | `global-harness-v5-core-gap-analysis-2026-06-06.md` |
| done | v4와 consensus 차이 표시 | `global-harness-v5-core-gap-analysis-2026-06-06.md` |
| done | 유지/수정/신규 추가 항목 구분 | `global-harness-v5-core-gap-analysis-2026-06-06.md` |
| done | v5 core 본문 목차 권장안 작성 | `global-harness-v5-core-gap-analysis-2026-06-06.md` 섹션 9, 사용자 검토 후 확정 |
| done | v5 core v1 후보 파일명 결정 | `global-harness-core-structure-template-v1.md` |

### Phase 3. v5 core 본문 핵심 작성

목표: 모델 중립 실행 구조의 본문 핵심을 만든다.

| 상태 | 작업 | 산출물 |
|---|---|---|
| done | 목적과 `harness-lab` 포인터 작성 | `global-harness-core-structure-template-v1.md` |
| done | 공통 원장 `harness/` 구조 작성 | `global-harness-core-structure-template-v1.md` |
| done | 얇은 adapter 원칙 작성 | `global-harness-core-structure-template-v1.md` |
| done | runbook / procedure / orchestrator 구분 작성 | `global-harness-core-structure-template-v1.md` |
| done | 산출물 계약 작성 | `global-harness-core-structure-template-v1.md` |
| done | 수정 경계와 `AGENTS.md` / `CLAUDE.md` 동기화 작성 | `global-harness-core-structure-template-v1.md` |

### Phase 3.5. v4 설계 판단 장치 보존 보강

목표: v1 core가 v4의 설계 방법론을 길게 복사하지 않되, 하네스 유형, 산출물 역할, 품질 축, 수준 선언, 7요소, 도메인 맥락 같은 설계 판단 장치를 필수 진입 조건과 별도 reference/module로 보존하게 만든다.

원칙:

- v4 상세 방법론을 v1 core 본문에 길게 복사하지 않는다.
- v4의 설계 판단 장치는 `design-preflight` reference/module로 보존한다.
- v1 core에는 포인터, 체크 게이트, module registry 항목만 둔다.
- 설계 진입 조건을 확인한 뒤 실행 구조, adapter, hook을 검토한다.

| 상태 | 작업 | 산출물 |
|---|---|---|
| done | v1 core에 `design-preflight` 진입 조건 연결 | `global-harness-core-structure-template-v1.md` Section 1 설계 전 확인 |
| done | v1 Section 1에 설계 전 확인 포인터 추가 | `global-harness-core-structure-template-v1.md` |
| done | v1 Section 14 체크리스트를 설계 진입 조건 우선 순서로 조정 | `global-harness-core-structure-template-v1.md` |
| done | Module Registry에 `design-preflight` 초기 placeholder 추가 | `global-harness-core-structure-template-v1.md` Section 12 |
| done | v4를 초기 `design-preflight` reference로 명시 | `global-harness-core-structure-template-v1.md` Section 12 |
| done | v4에서 `design-preflight`로 보존할 의미 있는 항목 추출 | `global-harness-v5-design-preflight-extraction-note-2026-06-06.md` |
| done | 별도 `global-harness-design-preflight-template-v0.md` 후보 파일 작성 여부 결정 | decision: 작성 |
| done | `global-harness-design-preflight-template-v0.md` 후보 파일 작성 | `global-harness-design-preflight-template-v0.md` |
| done | 별도 파일을 만들면 v1 Module Registry의 후보 파일 경로 갱신 | `global-harness-core-structure-template-v1.md` Section 12 |

### Phase 4. 최소 hook 정리와 module v0 cross-check

목표: `global-harness-core-structure-template-v1.md`에 들어간 승인, QA, 보안 hook 문구가 각 module v0 파일과 충돌하지 않는지 확인하고, 필요하면 core hook 또는 module 후보 수정안을 정리한다.

| 상태 | 작업 | 산출물 |
|---|---|---|
| done | v1 approval hook과 approval-gate v0 비교 | `global-harness-v5-phase4-hook-cross-check-2026-06-06.md` |
| done | v1 QA hook과 qa-scaffold v0 비교 | `global-harness-v5-phase4-hook-cross-check-2026-06-06.md` |
| done | v1 security hook과 security-baseline v0 비교 | `global-harness-v5-phase4-hook-cross-check-2026-06-06.md` |
| done | v1 hook 문구가 module 세부 규칙을 과도하게 복사하거나 충돌하지 않는지 점검 | `global-harness-v5-phase4-hook-cross-check-2026-06-06.md` |

### Phase 5. Available Modules Registry 검증

목표: `global-harness-core-structure-template-v1.md`에 작성된 module registry의 module 목록, 경로 표기 방식, 미정 항목을 검토하고 실제 후보 파일들과 연결되는지 확인한다.

| 상태 | 작업 | 산출물 |
|---|---|---|
| done | v1 module registry와 후보 파일 목록 비교 | `global-harness-v5-phase5-module-registry-comparison-2026-06-06.md` |
| done | 각 module의 사용 조건 한 줄이 실제 v0 내용과 맞는지 확인 | `global-harness-v5-phase5-module-usage-note-2026-06-06.md` |
| done | 실제 파일 경로 표기 방식 결정 | `global-harness-v5-phase5-path-convention-note-2026-06-06.md` |
| done | pilot-first를 별도 module로 둘지 candidate-ledger/testing에 묶을지 결정 | `global-harness-v5-phase5-pilot-first-decision-note-2026-06-06.md` |
| done | remaining module location combined note 작성: `comparison`, `file-template`, `type-schema`, `adapter-template`, `meta-orchestrator`의 위치 결정 | `global-harness-v5-phase5-remaining-module-location-decision-note-2026-06-06.md` |

remaining module location 판정 기준:

- `registry 등재 여부`와 `별도 파일 생성 여부`를 분리해서 판단한다.
- 각 후보는 성격, registry 등재 여부, 별도 파일 생성 여부, 기존 module 연결, 최종 위치, 이유, 재검토 조건을 기록한다.
- `최종 위치`는 `registry yes/no, file yes/no, 연결: {module}` 형식의 한 줄 요약으로 쓴다.
- note 마지막에는 v1 registry 반영 결과 요약표와 work map 반영 결과 요약표를 둔다.
- `comparison mode`는 qa-scaffold 흡수로 단정하지 않는다. 실행 구조 판단을 중심으로 보고, qa-scaffold는 같은 rubric 평가 부분만 연결 후보로 본다.

### Phase 6. module v0 검토와 v1 후보화

목표: 각 module v0를 바로 전역화하지 않고, v5 구조에 맞는 v1 후보로 다듬을지 결정한다.

| 상태 | module | 작업 |
|---|---|---|
| done | all modules | 개별 v1 후보화 전 전체 module v0 상태, v1화 필요도, 추천 순서 진단 |
| done | approval-gate | core hook과 module 상세의 경계를 scope note로 정리 |
| done | approval-gate | scope note를 바탕으로 `global-harness-approval-gate-template-v1.md` 후보 파일 작성 |
| done | approval-gate | `global-harness-approval-gate-template-v1.md` Claude Code 교차검증 PASS 확인 |
| done | qa-scaffold | 공통 QA 뼈대와 하네스별 QA/rubric/schema의 경계를 scope note로 정리 |
| done | qa-scaffold | `global-harness-v5-phase6-qa-scaffold-scope-note-2026-06-07.md` Claude Code 교차검증 PASS 확인 |
| done | qa-scaffold | scope note 교차검증 후 `global-harness-qa-scaffold-template-v1.md` 후보 파일 작성 |
| done | qa-scaffold | `global-harness-qa-scaffold-template-v1.md` Claude Code 교차검증 PASS 확인 |
| done | security-baseline | 최소 보안 hook과 상세 보안 절차의 경계를 scope note로 정리 |
| done | security-baseline | `global-harness-v5-phase6-security-baseline-scope-note-2026-06-07.md` Claude Code 교차검증 PASS 확인 |
| done | security-baseline | scope note 교차검증 후 `global-harness-security-baseline-template-v1.md` 후보 파일 작성 |
| done | security-baseline | `global-harness-security-baseline-template-v1.md` Claude Code 교차검증 PASS 확인 |
| done | observability | `source-pack-observability-template.md`와 global observability v0를 비교하고, `global-harness-observability-template-v1.md` 후보 파일 작성 |
| done | checkpoint | compact checkpoint와 원문 저장 금지 원칙을 scope note로 정리 |
| done | checkpoint | `global-harness-v5-phase6-checkpoint-scope-note-2026-06-07.md` Claude Code 교차검증 PASS 확인 |
| done | checkpoint | scope note 교차검증 후 `global-harness-checkpoint-template-v1.md` 후보 파일 작성 |
| done | docs-organization | 처음부터 강제할 구조와 문서가 많아졌을 때 적용할 구조를 scope note로 구분 |
| done | docs-organization | `global-harness-v5-phase6-docs-organization-scope-note-2026-06-07.md` Claude Code 교차검증 PASS 확인 |
| done | docs-organization | scope note 교차검증 후 `global-harness-docs-organization-template-v1.md` 후보 파일 작성 |
| done | docs-organization | `global-harness-docs-organization-template-v1.md` Claude Code 교차검증 PASS 확인 |
| done | candidate-ledger | candidate 패턴, pilot-first 기록, schema 승격 절차를 scope note로 구분 |
| done | candidate-ledger | `global-harness-v5-phase6-candidate-ledger-scope-note-2026-06-07.md` Claude Code 교차검증 PASS 확인 |
| done | candidate-ledger | scope note 교차검증 후 `global-harness-candidate-ledger-template-v1.md` 후보 파일 작성 |
| done | candidate-ledger | `global-harness-candidate-ledger-template-v1.md` Claude Code 교차검증 PASS 확인 |
| done | comparison | Phase 5에서 별도 파일 없이 v1 registry에 등재하고 `design-preflight`, `qa-scaffold`, `approval-gate`, `observability`에 연결하기로 결정 |
| done | module registry | v1 Section 12 registry를 개념 흐름 기준으로 정렬하고 no-file 항목을 연결 module 바로 뒤에 배치 |
| done | module registry | 정렬 결과 Claude Code 교차검증 PASS 확인 |

### Phase 7. pilot 검증과 전역화 판단

목표: v5 core와 module 후보를 실제 다음 하네스에서 검증한 뒤 전역 반영 여부를 결정한다.

| 상태 | 작업 | 산출물 |
|---|---|---|
| todo | pilot 대상 하네스 선정 | pilot plan |
| todo | v5 core 후보를 pilot 청사진에 적용 | pilot notes |
| todo | module 중 필요한 것만 선택 적용 | pilot notes |
| todo | 적용 중 drift, 누락, 과잉 규칙 기록 | evaluation note |
| todo | 공통 signal/notification module 후보를 둘지 검토 | notification decision note |
| todo | 전역 배포 시 module registry 기준 경로 문구를 portable하게 바꿀지 검토 | packaging note |
| todo | 전역 `harness-lab` 수정 여부는 별도 논의로 보류 | deferred decision |

## 6. 현재 작업 보드

### In Progress

- 현재 없음

### Next

1. Phase 7 진입 전에 사용자 확인 질문을 먼저 논의한다.
2. 사용자 확인이 끝나면 Phase 7 pilot 검증과 전역화 판단 논의로 넘어간다.

### Backlog

- file-template 후보는 반복 scaffold 필요성이 확인되면 별도 module 여부 재검토
- adapter-template 후보는 추가 adapter 반복 패턴이 확인되면 별도 module 여부 재검토
- meta-orchestrator 후보는 multi-harness pilot 이후 별도 module 여부 재검토
- signal-routing 또는 notification/escalation 후보는 docs 정리, 보안, QA, observability 알림 신호가 반복되면 별도 module 여부 재검토
- 전역 배포 시 v1 core Section 12의 `docs/templates/global-harness-candidates/` 기준 경로 문구를 template bundle 기준 문구로 바꿀지 검토

### Done

- v5 discussion handoff 확인
- v5 core scope consensus 문서 생성
- v5 작업 지도 문서 생성
- v5 core gap analysis 문서 생성
- v5 core structure v1 후보 문서 생성
- v4 design-preflight extraction note 작성
- design-preflight module v0 후보 문서 생성
- Phase 1~3.5 checkpoint review note 작성
- Phase 1~3.5 review note 수정 후보 R1/R2/R3/R4/R5 반영
- 후보 폴더 README 갱신
- `global-harness-design-preflight-template-v0.md` 최종 검토
- Phase 4 approval/QA/security hook cross-check 완료
- v1 core Section 15 검증 상태 정합성 수정
- Phase 5 module registry와 실제 후보 파일 목록 비교 완료
- Phase 5 module 사용 조건 한 줄 검증 완료
- v1 registry `design-preflight` 사용 조건 항목 순서 정합성 수정
- Phase 5 module 경로 표기 방식 결정 완료
- Phase 5 `pilot-first / testing` 위치 결정 완료
- Phase 5 remaining module location combined note 작성 및 v1 registry 반영 완료
- Phase 6 module diagnostic note 작성 완료
- Phase 6 observability deep review note 작성 완료
- Phase 6 observability v1 후보 파일 작성 완료
- Phase 6 approval-gate scope note 작성 완료
- Phase 6 approval-gate v1 후보 파일 작성 완료
- Phase 6 approval-gate v1 Claude Code 교차검증 PASS 확인
- Phase 6 qa-scaffold scope note 작성 완료
- Phase 6 qa-scaffold scope note Claude Code 교차검증 PASS 확인
- Phase 6 qa-scaffold v1 후보 파일 작성 완료
- Phase 6 qa-scaffold v1 Claude Code 교차검증 PASS 확인
- Phase 6 security-baseline scope note 작성 완료
- Phase 6 security-baseline scope note Claude Code 교차검증 PASS 확인
- Phase 6 security-baseline v1 후보 파일 작성 완료
- Phase 6 security-baseline v1 Claude Code 교차검증 PASS 확인
- Phase 6 checkpoint scope note 작성 완료
- Phase 6 checkpoint scope note Claude Code 교차검증 PASS 확인
- Phase 6 checkpoint v1 후보 파일 작성 완료
- Phase 6 docs-organization scope note 작성 완료
- Phase 6 docs-organization scope note Claude Code 교차검증 PASS 확인
- Phase 6 docs-organization v1 후보 파일 작성 완료
- Phase 6 docs-organization v1 Claude Code 교차검증 PASS 확인
- Phase 6 candidate-ledger scope note 작성 완료
- Phase 6 candidate-ledger scope note Claude Code 교차검증 PASS 확인
- Phase 6 candidate-ledger v1 후보 파일 작성 완료
- Phase 6 candidate-ledger v1 Claude Code 교차검증 PASS 확인
- Phase 6 module registry 정렬 원칙 확정 및 v1 Section 12 순서 재정렬 완료
- Phase 6 module registry 정렬 결과 Claude Code 교차검증 PASS 확인
- `harness-lab`은 당장 수정하지 않는다는 원칙 확인
- `Completion Contract` 별도 용어를 만들지 않기로 결정

### Deferred

- 전역 `harness-lab` 수정
- 자동 drift 검사 스크립트 작성
- v5 후보의 전역 템플릿 승격
- Source Pack 외 다음 하네스 pilot 적용

### Blocked

- 현재 없음

## 7. 결정 기록

| 날짜 | 결정 | 이유 |
|---|---|---|
| 2026-06-06 | v5 core와 전역 `harness-lab`을 분리한다 | `harness-lab`은 모든 하네스의 기초이므로 충분한 검증 전 수정하지 않는다 |
| 2026-06-06 | v5 core는 모델 중립 실행 구조로 정의한다 | Codex와 Claude Code가 같은 `harness/` 원장을 읽고 실행하게 하기 위함 |
| 2026-06-06 | v5는 core + modules 구조로 간다 | 거대한 단일 문서를 피하고 선택 운영 장치를 분리하기 위함 |
| 2026-06-06 | `Completion Contract`라는 새 용어는 만들지 않는다 | 기존 산출물 계약, QA 기준, 승인/중단 조건과 중복되기 때문 |
| 2026-06-06 | module registry를 둔다 | core 본문을 가볍게 유지하면서 선택 module의 존재를 발견 가능하게 하기 위함 |
| 2026-06-06 | v5 core v1은 새 파일로 만드는 것을 권장한다 | v0는 후보 카드로, v4는 baseline으로 보존하는 편이 변경 이력이 분명하기 때문 |
| 2026-06-06 | `global-harness-core-structure-template-v1.md`를 새 후보 파일로 만든다 | v0와 v4 원본을 보존하면서 consensus와 gap analysis를 반영하기 위함 |
| 2026-06-06 | v4 설계 판단 장치는 v1 core 본문에 복사하지 않고 `design-preflight` 진입 조건과 reference/module로 보존한다 | v5 core를 작게 유지하면서 하네스 유형, 품질 축, 수준 선언, 7요소 같은 설계 판단 장치를 잃지 않기 위함 |
| 2026-06-06 | v4의 comparison mode 운영 규칙을 Phase 5 module registry 검토 대상으로 추가한다 | extraction note에서 comparison mode가 독립 module 후보 또는 QA scaffold 확장 후보로 발견됐기 때문 |
| 2026-06-06 | `design-preflight`는 별도 module v0 후보 파일로 분리한다 | v4 reference-only로 두면 새 하네스 작성자가 설계 판단을 건너뛸 위험이 있고, v1 core에 넣기에는 내용이 길기 때문 |
| 2026-06-06 | 후보 폴더 README를 현재 v5 후보 파일 지도 기준으로 갱신한다 | 작업 지도, v1 core, design-preflight, review note의 위치를 새 작업자가 바로 찾을 수 있게 하기 위함 |
| 2026-06-06 | Phase 4 hook cross-check 결과 v1 approval/QA/security hook과 각 module v0 사이에 blocking conflict는 없다 | v1 core는 최소 의무만 두고, 상세 문구와 상태값은 각 module v0가 맡는 구조가 유지되기 때문 |
| 2026-06-06 | Phase 5 registry 파일 대조 결과 v1의 파일 기반 module 8개는 모두 실제 후보 파일이 있다 | 검토 당시 `pilot-first / testing`과 `comparison` 등 추가 후보는 Phase 5 후속 결정 대상으로 남겼다 |
| 2026-06-06 | Phase 5 module 사용 조건 검토 결과 v1 registry의 `design-preflight`, `qa-scaffold`, `security-baseline` 설명을 현재 v0 범위에 맞게 조정한다 | registry가 module v0의 실제 범위를 과소/과대 설명하지 않게 하기 위함 |
| 2026-06-06 | v1 core Section 12 registry에서는 파일명만 유지하고, 경로 기준을 `docs/templates/global-harness-candidates/`로 명시한다 | core registry 표를 읽기 쉽게 유지하면서 후보 파일의 실제 기준 폴더를 분명히 하기 위함 |
| 2026-06-06 | `pilot-first / testing`은 별도 module v0 파일로 만들지 않고 `candidate-ledger`와 Phase 7 pilot validation에 묶어 처리한다 | pilot-first는 단독 템플릿보다 새 source/schema/automation 후보를 기록하고 작게 검증하는 운영 원칙에 가깝기 때문 |
| 2026-06-06 | `comparison`은 v1 registry에 등재하되 별도 파일은 만들지 않고 `design-preflight`, `qa-scaffold`, `approval-gate`, `observability`에 연결한다 | comparison은 QA만의 문제가 아니라 실행 구조와 운영 승인까지 걸친 후보이므로, 발견 가능성은 두되 파일 분리는 pilot 반복 후 판단하기 위함 |
| 2026-06-06 | `type-schema`는 v1 registry에 등재하되 별도 파일은 만들지 않고 `design-preflight`, `qa-scaffold`, 각 하네스의 `harness/schemas/`에 연결한다 | 유형별 schema/rubric 구조의 발견 가능성은 필요하지만 전역 type-schema module은 아직 반복 사례가 부족하기 때문 |
| 2026-06-06 | `file-template`, `adapter-template`, `meta-orchestrator`는 v1 registry에 등재하지 않고 backlog/reference로 추적한다 | 세 후보 모두 현재 v5 core registry에 노출하면 범위가 과해지거나 상세 구현을 선확정하는 인상을 줄 수 있기 때문 |
| 2026-06-06 | Phase 6는 개별 v1 후보화 전에 전체 module diagnostic note를 먼저 작성한다 | module별 v0 상태, v1화 필요도, 비교 대상, 추천 순서를 먼저 잡아야 개별 수정 중 범위가 흔들리지 않기 때문 |
| 2026-06-06 | observability deep review는 Source Pack 특수 용어 제거와 전역 구조 보존을 분리해서 판단한다 | Source Pack 도메인 예시가 전역 module로 역유입되는 것을 막으면서 원본 템플릿의 좋은 작성 구조를 잃지 않기 위함 |
| 2026-06-06 | `global-harness-observability-template-v1.md`를 새 후보 파일로 만든다 | global v0는 방향은 맞지만 실무 작성 기준이 얇고, Source Pack 원본의 좋은 구조를 전역화할 필요가 있기 때문 |
| 2026-06-07 | approval-gate v1은 파일 작성 전에 scope note로 승인 수준과 경계를 먼저 확정한다 | approval-gate는 v1, adapter, 하네스별 규칙의 경계가 핵심이므로 바로 v1 본문을 쓰면 scope가 흔들릴 수 있기 때문 |
| 2026-06-07 | approval-gate 승인 수준은 read-only, 일반 편집, 구조 변경, 위험 작업 4단계로 나눈다 | 모든 작업에 같은 승인 기준을 적용하면 과도하거나 위험하므로 작업 성격별 기준이 필요하기 때문 |
| 2026-06-07 | `global-harness-approval-gate-template-v1.md`를 새 후보 파일로 만든다 | approval-gate v0는 방향은 맞지만 승인 수준과 중단 조건이 얇고, scope note에서 v1 범위가 합의됐기 때문 |
| 2026-06-07 | `global-harness-approval-gate-template-v1.md` Claude Code 교차검증 결과 PASS로 본다 | scope note 권장 목차와 v1 실제 목차가 일치하고, 승인 수준, security-baseline 예외, 중단 조건, 교차검증 범위 제한이 합의대로 반영됐기 때문 |
| 2026-06-07 | qa-scaffold v1은 파일 작성 전에 scope note로 공통 QA 뼈대와 하네스별 QA/rubric/schema 경계를 먼저 확정한다 | qa-scaffold는 comparison, type-schema, 하네스별 schema/rubric과 연결되므로 바로 v1 본문을 쓰면 범위가 과해지거나 핵심 repair/recheck 흐름이 빠질 수 있기 때문 |
| 2026-06-07 | qa-scaffold의 공통 QA 항목은 고정 질문이 아니라 6개 범주로 둔다 | 하네스별 산출물과 schema가 다르므로 질문을 전역에서 고정하지 않고 completeness, placement, traceability, schema/rubric conformance, violation, downstream readiness 범주 안에서 각 하네스가 질문을 만들게 하기 위함 |
| 2026-06-07 | qa-scaffold에서 approval/stop은 QA 범주가 아니라 Escalation으로 분리한다 | 승인 필요와 중단 필요는 산출물 품질 평가 축이 아니라 QA 결과에서 도출되는 후속 행동 조건이기 때문 |
| 2026-06-07 | `repair_required`는 전체 QA 상태가 아니라 finding/action 상태로 둔다 | 전체 판정과 개별 조치 상태를 섞으면 `partial_pass`, `unverified`, repair loop의 의미가 흐려지기 때문 |
| 2026-06-07 | `global-harness-v5-phase6-qa-scaffold-scope-note-2026-06-07.md` Claude Code 교차검증 결과 PASS로 본다 | 합의된 6개 범주, Escalation 분리, stopped/stop_required 구분, approval_gate_reference 형식, v1 목차 수정안이 반영됐기 때문 |
| 2026-06-07 | `global-harness-qa-scaffold-template-v1.md`를 새 후보 파일로 만든다 | qa-scaffold scope note와 Claude Code 교차검증에서 공통 QA 범주, 결과 skeleton, repair/recheck, escalation 경계가 합의됐기 때문 |
| 2026-06-07 | `global-harness-qa-scaffold-template-v1.md` Claude Code 교차검증 결과 PASS로 본다 | 13개 섹션 목차, 6개 QA 범주, 상태값 분리, repair/recheck, escalation, comparison/type-schema 경계가 합의대로 반영됐기 때문 |
| 2026-06-07 | security-baseline v1은 파일 작성 전에 scope note로 최소 안전선과 하네스별 보안 절차 경계를 먼저 확정한다 | 보안 항목은 범위가 쉽게 커지므로 바로 v1 본문을 쓰면 보안 운영 매뉴얼이 될 위험이 있기 때문 |
| 2026-06-07 | security-baseline은 보안 운영 매뉴얼이 아니라 최소 안전선으로 유지한다 | v5 module은 secret, `.env`, 격리 파일, 외부 공개 같은 반복 실수를 막는 공통 기준에 집중해야 하기 때문 |
| 2026-06-07 | security-baseline 안전선은 절대 금지, 조건부 원칙, 인터페이스 원칙으로 나눈다 | secret 노출 금지처럼 전역 적용할 항목과 raw data 공개처럼 하네스별 판단할 항목의 강도를 구분하기 위함 |
| 2026-06-07 | 대화 원문과 checkpoint는 전역 프라이버시 원칙으로 다루고 raw data 공개/제외는 하네스별로 판단한다 | 대화 원문과 raw data는 위험 성격과 도메인 의존성이 다르기 때문 |
| 2026-06-07 | `.gitignore`는 전역에서 카테고리와 짧은 예시만 두고 최종 파일명 세트는 하네스별로 확정한다 | 전역 template가 모든 언어, 도구, secret 파일명을 exhaustive하게 관리하면 빠르게 낡기 때문 |
| 2026-06-07 | 보안 이슈 발견 시 자동 삭제, 이동, 복구, 덮어쓰기를 하지 않고 발견, 기록, 멈춤, 사용자 알림, 확인 순서를 따른다 | 좋은 의도의 자동 해결이 증거 훼손이나 추가 노출을 만들 수 있기 때문 |
| 2026-06-07 | `global-harness-security-baseline-template-v1.md`를 새 후보 파일로 만든다 | security-baseline scope note와 Claude Code 교차검증에서 최소 안전선, 3층 분류, do-not-proceed, 사용자 알림 원칙, approval/QA 연결 경계가 합의됐기 때문 |
| 2026-06-07 | `global-harness-security-baseline-template-v1.md` Claude Code 교차검증 결과 PASS로 본다 | scope note 합의의 13개 목차, 3층 분류, do-not-proceed, 자동 해결 금지, 사용자 알림 원칙, approval/QA 연결이 v1 본문에 반영됐기 때문 |
| 2026-06-07 | checkpoint v1은 실행 skill이 아니라 공통 출력 계약과 설계 원칙을 담는 module template로 정의한다 | 이미 전역 `session-checkpoint` skill과 프로젝트 adapter가 있으므로 v5 template가 실행 구현을 중복하면 역할이 꼬이기 때문 |
| 2026-06-07 | checkpoint scope note는 compact 정의, anchor matching 계약, session checkpoint와 handoff 구분을 포함한다 | 새 adapter가 같은 저장 모드와 인계 경계를 구현할 수 있게 하기 위함 |
| 2026-06-07 | `global-harness-v5-phase6-checkpoint-scope-note-2026-06-07.md` Claude Code 교차검증 결과 PASS로 본다 | 전역 skill, 프로젝트 adapter, v5 template의 3층 구조와 compact 정의, matching 계약, handoff 경계가 합의대로 정리됐기 때문 |
| 2026-06-07 | `global-harness-checkpoint-template-v1.md`를 새 후보 파일로 만든다 | checkpoint scope note와 Claude Code 교차검증에서 공통 출력 계약, 원문 저장 금지, 저장 모드, matching 계약, adapter 경계가 합의됐기 때문 |
| 2026-06-07 | docs-organization은 초기 폴더 구조 강제가 아니라 문서가 많아졌을 때 적용하는 성장 후 정리 기준으로 정의한다 | 작은 하네스가 과한 docs 구조로 무거워지는 것을 막고, 문서가 늘었을 때만 역할별 정리와 참조 점검을 적용하기 위함 |
| 2026-06-07 | `docs/README.md`는 docs 내부 색인이며 `harness/MANIFEST.md` 같은 전체 프로젝트 파일 지도를 대체하지 않는다 | 폴더 색인과 전체 파일 계약이 서로 다른 역할을 하므로 두 문서가 충돌하지 않게 하기 위함 |
| 2026-06-07 | docs-organization은 checkpoint, handoff, reference 개념을 재정의하지 않고 위치와 색인 원칙만 다룬다 | 이미 정의된 module 개념을 복사하면 문서 간 drift가 생길 수 있기 때문 |
| 2026-06-07 | `global-harness-docs-organization-template-v1.md`를 새 후보 파일로 만든다 | docs-organization scope note와 Claude Code 교차검증에서 성장 후 정리 기준, docs README 색인, 이동 전후 참조 점검 경계가 합의됐기 때문 |
| 2026-06-07 | candidate-ledger는 정식 schema가 되기 전의 대기실로 정의한다 | 새 분류, 상태값, source, schema 후보를 바로 정식화하지 않고 증거와 반복성을 확인하기 위함 |
| 2026-06-07 | candidate record와 evidence event는 개념적으로 구분하되 물리 저장 구조는 하네스별로 둔다 | 작은 하네스와 큰 하네스의 ledger 구현 방식이 다르며, 전역 v1이 파일 구조를 과하게 고정하지 않기 위함 |
| 2026-06-07 | `evidence_count`는 원천 증거가 아니라 집계값 또는 최신 요약값으로 본다 | append-only evidence 누적과 단일 record 갱신 방식 사이의 충돌을 줄이기 위함 |
| 2026-06-07 | `approved_for_change`와 `promoted`를 구분하고, `rejected`와 `deferred`도 근거와 승인 흐름을 둔다 | 변경 준비 승인과 실제 정식 반영, 폐기와 보류 판단을 섞지 않기 위함 |
| 2026-06-07 | `global-harness-candidate-ledger-template-v1.md`를 새 후보 파일로 만든다 | candidate-ledger scope note와 Claude Code 교차검증에서 record/evidence, append-first, 승격/폐기/보류 경계가 합의됐기 때문 |
| 2026-06-07 | v1 module registry는 파일 유무가 아니라 개념 흐름 기준으로 정렬하고, no-file 항목은 연결 module 바로 뒤에 둔다 | registry는 파일 목록이 아니라 module 발견과 적용 순서를 돕는 안내 표이므로 사용자가 하네스 설계 흐름대로 읽을 수 있어야 하기 때문 |
| 2026-06-07 | 알림/경고 공통 기능은 지금 즉시 별도 module로 만들지 않고 Phase 7에서 signal-routing 또는 notification/escalation 후보로 검토한다 | docs 정리, 보안, QA, observability가 각자 다른 경고 방식을 만들면 drift가 생길 수 있지만, 아직 반복 사용 검증 전이므로 후보로 추적하는 것이 안전하기 때문 |

## 8. 열려 있는 질문

| 질문 | 현재 상태 | 다음 행동 |
|---|---|---|
| v5 core 초안을 별도 파일로 만들까, 기존 core v0를 갱신할까? | 결정됨 | `global-harness-core-structure-template-v1.md` 새 파일 생성 |
| module registry의 경로 표기 방식을 어떻게 고정할까? | 결정됨 | v1 registry는 파일명만 쓰고 기준 폴더를 `docs/templates/global-harness-candidates/`로 명시 |
| `AGENTS.md` / `CLAUDE.md` drift 검사는 수동 체크리스트로 충분한가? | 미정 | v5 core 초안 후 재검토 |
| pilot-first는 candidate-ledger에 묶을까, 별도 testing module로 둘까? | 결정됨 | 별도 module v0 파일을 만들지 않고 `candidate-ledger`와 Phase 7 pilot validation에 묶어 처리 |
| `design-preflight`를 별도 module v0로 분리할까? | 결정됨 | `global-harness-design-preflight-template-v0.md` 생성 |
| `comparison mode`를 별도 module로 둘까, `qa-scaffold`에 포함할까? | 결정됨 | 별도 파일 없이 v1 registry에 등재하고 `design-preflight`, `qa-scaffold`, `approval-gate`, `observability`에 연결 |
| extraction note의 추가 후보(`file-template`, `type-schema`, `adapter-template`, `meta-orchestrator`)를 registry/backlog에 둘까? | 결정됨 | `type-schema`는 registry yes/file no, 나머지 세 후보는 backlog/reference로 추적 |
| global observability v0가 Source Pack 원본 템플릿의 핵심을 충분히 보존했는가? | 결정됨 | 방향은 맞지만 너무 압축됐으므로, `global-harness-observability-template-v1.md` 후보 파일 생성 |
| approval-gate v1 후보 파일을 만들까? | 결정됨 | `global-harness-approval-gate-template-v1.md` 후보 파일 생성 |
| qa-scaffold v1 후보 파일을 만들까? | 결정됨 | `global-harness-qa-scaffold-template-v1.md` 후보 파일 생성 |
| security-baseline v1 후보 파일을 만들까? | 결정됨 | `global-harness-security-baseline-template-v1.md` 후보 파일 생성 |
| checkpoint v1 후보 파일을 만들까? | 결정됨 | `global-harness-checkpoint-template-v1.md` 후보 파일 생성 |
| docs-organization v1 후보 파일을 만들까? | 결정됨 | `global-harness-docs-organization-template-v1.md` 후보 파일 생성 |
| candidate-ledger v1 후보 파일을 만들까? | 결정됨 | `global-harness-candidate-ledger-template-v1.md` 후보 파일 생성 |
| v1 module registry 정렬 원칙은 개념 흐름 기준으로 할까, 파일 유무 기준으로 할까? | 결정됨 | 개념 흐름 기준으로 정렬하고 no-file 항목은 연결 module 바로 뒤에 배치 |
| v1 core Section 12의 후보 폴더 기준 경로 문구를 전역 배포 시 어떻게 바꿀까? | 미정 | Phase 7 packaging/전역화 판단에서 template bundle 기준 문구로 변경할지 결정 |
| docs 정리, 보안, QA, observability의 알림/경고를 공통 signal-routing module로 분리할까? | 미정 | Phase 7 pilot/전역화 판단에서 반복 신호, severity, 사용자 알림 채널, adapter 경계를 검토 |
| Source Pack 사례를 v5 core 본문에 넣을까, reference로만 둘까? | 결정됨 | v1 본문에는 길게 넣지 않고 기반 문서와 reference/example로만 처리 |
| 후보 폴더 README를 언제 갱신할까? | 결정됨 | `README.md` 갱신 완료 |

## 9. 작업 시 업데이트 규칙

작업을 진행할 때 이 문서를 아래 방식으로 갱신한다.

- 새 작업을 시작하면 해당 항목을 `in_progress`로 바꾼다.
- 완료하면 `done`으로 바꾸고 산출물 경로를 적는다.
- 순서가 바뀌면 단계별 작업 계획과 현재 작업 보드를 함께 수정한다.
- 중요한 판단은 `결정 기록`에 남긴다.
- 아직 결론이 나지 않은 쟁점은 `열려 있는 질문`에 둔다.
- 실제 v5 core 또는 module 문서에 반영한 내용은 이 문서에서도 완료 처리한다.

## 10. 다음에 바로 할 일

다음 작업을 시작할 때는 아래 순서로 들어간다.

1. Phase 7 진입 전에 사용자 확인 질문을 먼저 논의한다.
2. 사용자 확인이 끝나면 Phase 7 pilot 검증과 전역화 판단 논의로 넘어간다.
