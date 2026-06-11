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
| `docs/templates/global-harness-candidates/global-harness-v5-phase7-planning-consensus-note-2026-06-07.md` | Phase 7 planning consensus note | Phase 7-0/A/B/C 구조, signal-routing, Source Pack 소급 검증, Industry Primer pilot, 전역 배포 판단의 기준 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase7-signal-routing-scope-note-2026-06-08.md` | Phase 7-0 signal-routing scope note | signal-routing v0 후보 작성 전 severity, route, record, user visibility 경계 정리 |
| `docs/templates/global-harness-candidates/global-harness-signal-routing-template-v0.md` | signal-routing v0 후보 | 알림/경고/에스컬레이션을 공통 signal envelope와 routing 계약으로 표현하는 현재 후보 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase7a-source-pack-retro-validation-note-2026-06-08.md` | Phase 7-A Source Pack 소급 검증 note | Source Pack 실제 구조와 대표 사건을 v5 core/module 후보에 read-only 방식으로 대조한 결과 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase7b-industry-primer-design-principles-note-2026-06-08.md` | Phase 7-B Industry Primer design principles note | pilot plan 작성 전 실행 주체, 산업/기업 단위, source tracking, QA, module 선택 원칙을 고정 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase7b-industry-primer-pilot-plan-note-v0-2026-06-08.md` | Phase 7-B Industry Primer pilot plan note v0 | pilot plan v1 작성 전 질문, 답변, 합의를 누적하는 살아있는 논의판 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase7b-industry-primer-pilot-plan-note-v1-2026-06-08.md` | Phase 7-B Industry Primer pilot plan note v1 | APP/adtech Industry Primer first slice pilot 실행 계획서 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase7b-industry-primer-blueprint-prep-note-2026-06-08.md` | Phase 7-B Industry Primer blueprint prep note | blueprint 작성 전 rubric, QA output, output schema, Section 13 handoff schema 합의 기준 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase7b-industry-primer-blueprint-prep-section11-consensus-note-2026-06-09.md` | Phase 7-B Industry Primer blueprint prep Section 11 consensus note | blueprint-prep note Section 11의 6개 질문에 대한 합의와 Claude Code 교차검증 PASS 기준 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase7b-industry-primer-first-slice-rubric-calibration-note-2026-06-09.md` | Phase 7-B Industry Primer first slice rubric calibration note | Section 1/3/5/13의 좋은 답, 보완 가능 답, blocked 기준과 full rubric/QA expansion 추적 기준 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase7b-industry-primer-blueprint-v0-2026-06-09.md` | Phase 7-B Industry Primer blueprint v0 | Industry Primer first slice 하네스의 contract, procedure, schema, rubric, output, adapter 경계와 module 연결 설계 초안 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase7b-industry-primer-pre-build-risk-review-note-2026-06-09.md` | Phase 7-B Industry Primer pre-build risk review note | 실제 하네스 파일 생성 전 첫 공식 challenge review와 blueprint v0 최소 수정 항목 확정 기준 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase7b-s04-migration-plan-2026-06-11.md` | Phase 7-B S04 migration/bootstrap plan | Industry Primer 하네스를 S03 안에 만들지 않고 P2-S04 독립 하네스로 이전하기 위한 계획과 Claude Code PASS 기준 |
| `docs/handoff/s04-migration-handoff-2026-06-11.md` | S04 migration handoff note | Phase 7-B Industry Primer build/pilot 추적을 S03에서 S04로 넘긴다는 최종 handoff 기록 |
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

기준 문서: `global-harness-v5-phase7-planning-consensus-note-2026-06-07.md`

Phase 7은 단일 pilot이 아니라 아래 네 하위 단계로 진행한다.

```text
Phase 7-0. signal-routing scope note + v0
Phase 7-A. Source Pack 소급 검증
Phase 7-B. Industry Primer 실전 검증
Phase 7-C. 전역 배포 판단
```

#### Phase 7-0. signal-routing scope note + v0

목표: 보안, QA, docs 정리, observability, candidate-ledger가 각자 다른 방식으로 알림/경고를 만들지 않도록 공통 signal-routing 계약을 먼저 정의한다.

| 상태 | 작업 | 산출물 |
|---|---|---|
| done | signal-routing scope note 작성 | `global-harness-v5-phase7-signal-routing-scope-note-2026-06-08.md` |
| done | signal-routing scope note Claude Code 교차검증 PASS 확인 | validation note |
| done | signal-routing v0 후보 파일 작성 | `global-harness-signal-routing-template-v0.md` |
| done | signal-routing v0 Claude Code 교차검증 PASS 확인 | validation note |
| done | signal-routing을 v1 core registry에 반영할지 결정 | `global-harness-core-structure-template-v1.md` Section 12 |

#### Phase 7-A. Source Pack 소급 검증

목표: Source Pack 경험과 v5 template 후보의 내부 일관성을 확인한다. 이 단계는 독립 실전 검증이 아니라 확증 편향 가능성이 있는 retrospective filter다.

| 상태 | 작업 | 산출물 |
|---|---|---|
| done | Source Pack 소급 검증 note 작성 | `global-harness-v5-phase7a-source-pack-retro-validation-note-2026-06-08.md` |
| done | module별 소급 대조 결과 작성 | validation note |
| done | signal-routing으로 포착했어야 할 신호 목록 작성 | validation note |
| done | 대표 사건과 핵심 폴더 중심으로 Source Pack 검토 | validation note |
| done | IR taxonomy overlap 충돌 사건 검토: `sec_equivalent_not_found_in_scoped_8k`, `security_quarantined` | validation note |
| done | B 진입 전 수정 후보 목록 작성 | validation note |
| done | A 완료 판정 기록 | `pass` |
| done | Source Pack 소급 검증 note Claude Code 교차검증 PASS 확인 | validation review |

#### Phase 7-B. Industry Primer 실전 검증

목표: v5 template 후보를 다음 하네스에 실제로 적용해 사용성, 누락, 과잉 규칙, drift를 검증한다.

| 상태 | 작업 | 산출물 |
|---|---|---|
| done | 다음 하네스 후보와 유형 결정 | user decision: Phase 2 - Step 4 `Industry Primer`, 산업 이해 / 구조화 / 분석 준비형 |
| done | 첫 pilot 대상 후보 결정 | APP / adtech / mobile advertising |
| done | Industry Primer design principles note 작성 | `global-harness-v5-phase7b-industry-primer-design-principles-note-2026-06-08.md` |
| done | Industry Primer design principles note Claude Code 교차검증 PASS 확인 | validation review |
| done | Industry Primer pilot plan note v0 작성 | `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v0-2026-06-08.md` |
| done | Industry Primer pilot plan 질문별 답변/합의 누적 | `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v0-2026-06-08.md` |
| done | Industry Primer pilot plan note v1 작성 | `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v1-2026-06-08.md` |
| done | Industry Primer pilot plan note v1 Claude Code 교차검증 PASS 확인 | validation review |
| done | Industry Primer blueprint prep note 작성 | `global-harness-v5-phase7b-industry-primer-blueprint-prep-note-2026-06-08.md` |
| done | Industry Primer blueprint prep note Claude Code 교차검증 PASS 확인 | validation review |
| done | blueprint 논의: blueprint-prep note Section 11 기준 6개 질문 커버 | `global-harness-v5-phase7b-industry-primer-blueprint-prep-section11-consensus-note-2026-06-09.md` |
| done | Industry Primer blueprint prep Section 11 consensus note Claude Code 교차검증 PASS 확인 | validation review |
| done | Industry Primer first slice rubric calibration note 작성 | `global-harness-v5-phase7b-industry-primer-first-slice-rubric-calibration-note-2026-06-09.md` |
| done | Industry Primer first slice rubric calibration note Claude Code 교차검증 PASS 확인 | validation review |
| done | Industry Primer blueprint 초안 작성 | `global-harness-v5-phase7b-industry-primer-blueprint-v0-2026-06-09.md` |
| done | Industry Primer blueprint Claude Code 교차검증 PASS 확인 | validation review |
| done | Phase 7-B challenge review 수행 (conformance vs challenge 구분 확립) | `global-harness-v5-phase7b-industry-primer-pre-build-risk-review-note-2026-06-09.md` |
| done | pre-build risk review note Claude Code 교차검증 PASS 확인 | validation review |
| done | pre-build risk review note 기준 blueprint v0 최소 수정 6개 반영 | `global-harness-v5-phase7b-industry-primer-blueprint-v0-2026-06-09.md` |
| done | 수정된 blueprint v0 Claude Code 교차검증 PASS 확인 | validation review |
| done | S04 migration/bootstrap plan 작성 | `global-harness-v5-phase7b-s04-migration-plan-2026-06-11.md` |
| done | S04 migration/bootstrap plan Claude Code 교차검증 PASS 확인 | validation review |
| done | 실제 Industry Primer first slice 하네스 파일 생성 여부 사용자 승인 게이트를 S04 이전 결정으로 처리 | `docs/handoff/s04-migration-handoff-2026-06-11.md` |
| done | Phase 7-B Industry Primer build/pilot 남은 작업을 P2-S04 work-map으로 이전 | `docs/handoff/s04-migration-handoff-2026-06-11.md` |
| done | S04로 넘길 TODO 기록: Step 7 하네스 파일 생성 후 S04 `CLAUDE.md`에 `harness/` 구조 섹션 추가 | `docs/handoff/s04-migration-handoff-2026-06-11.md` |
| done | S03 work-map handoff/freeze 상태 표시 | S03은 Source Pack 하네스와 Global Harness v5 설계 스냅샷으로 보존 |

참고:

- `Earnings Call`은 Step 3 Source Pack의 하위 분화 후보로 둔다.
- `Earnings Call`은 `Industry Primer`의 blocking dependency가 아니다.
- `Earnings Call` 산출물 계약은 Step 6 `Business Model` 또는 이후 Financial Quality/Monitoring 계열 하네스 전까지 정리한다.
- B 계획 수립은 A와 병렬로 진행할 수 있지만, B 실제 실행은 A 완료 후 진행한다.
- APP Source Pack은 full collection이 아니라 partial input이다. pilot plan에는 `Input Readiness Preflight`를 포함하고, 현재 APP 입력은 `Conditional Proceed`로 다룬다.
- blueprint 논의는 blueprint-prep note의 6개 질문(하네스 구조, rubric, QA output format, output schema, handoff, module 연결)을 중심으로 진행한다. 이 질문들은 순차적으로 분리 처리하지 않고, 하나의 blueprint 문서로 수렴하도록 함께 설계한다.

#### Phase 7-C. 전역 배포 판단

목표: Phase 7-A와 7-B 결과를 바탕으로 v5 template 후보를 전역 bundle로 배포할지 판단한다.

| 상태 | 작업 | 산출물 |
|---|---|---|
| todo | 전역 배포 여부 결정 | deployment decision note |
| todo | Codex/Claude 전역 배포 구조 결정 | packaging note |
| todo | canonical source 위치 결정 | packaging note |
| todo | registry 경로 문구 portable화 여부 결정 | packaging note |
| todo | version/changelog 정책 결정 | packaging note |
| todo | Source Pack 사례를 전역 bundle에 어느 정도 reference로 남길지 결정 | packaging note |
| todo | 전역 `harness-lab` 수정 여부는 계속 보류 또는 별도 논의 | deferred decision |

##### Phase 7-B에서 발견한 전역 template 반영 입력

이미 확정 — 반드시 반영:

- conformance review / challenge review 구분
- pre-build risk review gate

first slice 후 관찰 — 검증 후 반영 여부 결정:

- source interpretation risk rubric 언어
- User Review Required Claims 형식
- company bias / anti-cheerleading rubric 언어
- source quality tier 구체 기준
- system complexity / 운영 부담 수준

참고:

- 21단계 가설 검증 기준, JSONL migration trigger, Source Pack expansion trigger는 중요하지만 전역 template 반영 후보가 아니라 project-level 또는 Source Pack backlog에서 추적한다.
- User Review Required Claims는 AI가 최종 검증하는 장치가 아니라, 사용자가 승인 전 원문/source와 도메인 지식으로 검토할 고위험 claim을 드러내는 장치다.

## 6. 현재 작업 보드

### In Progress

- 현재 없음

### Next

1. S03에서 Phase 7-B Industry Primer build/pilot 작업을 더 진행하지 않는다.
2. 사용자가 `C:\Users\frisa\Documents\Investment-Research-OS\P2-S04-Industry-Primer` 폴더 생성과 독립 `git init` Gate를 직접 수행한다.
3. 이후 `global-harness-v5-phase7b-s04-migration-plan-2026-06-11.md`에 따라 S04 파일 복사, S04 bootstrap, S04 Industry Primer 하네스 파일 생성을 S04 work-map에서 추적한다.
4. S03의 `harness/`, `.agents/`, `.claude/`, `artifacts/`는 Source Pack 전용으로 유지한다.

### Backlog

- file-template 후보는 반복 scaffold 필요성이 확인되면 별도 module 여부 재검토
- adapter-template 후보는 추가 adapter 반복 패턴이 확인되면 별도 module 여부 재검토
- meta-orchestrator 후보는 multi-harness pilot 이후 별도 module 여부 재검토
- Phase 7-C에서 전역 배포 구조, canonical source, portable path, version/changelog 정책 결정

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
- Phase 7 planning consensus note 작성 및 Claude Code 교차검증 PASS 확인
- Phase 7 사용자 결정 Q1~Q5 완료: Industry Primer pilot, signal-routing v0, Source Pack 소급 검증 범위 확정
- Phase 7-0 signal-routing scope note 작성 완료
- Phase 7-0 signal-routing scope note Claude Code 교차검증 PASS 확인
- Phase 7-0 signal-routing v0 후보 파일 작성 완료
- Phase 7-0 signal-routing v0 Claude Code 교차검증 PASS 확인
- Phase 7-0 signal-routing을 v1 core Section 12 registry에 반영 완료
- Phase 7-A Source Pack 소급 검증 note 작성 완료
- Phase 7-A Source Pack 소급 검증 note Claude Code 교차검증 PASS 확인
- Phase 7-B Industry Primer design principles note 작성 완료
- Phase 7-B Industry Primer design principles note Claude Code 교차검증 PASS 확인
- Phase 7-B Industry Primer pilot plan note v0 작성 완료
- Phase 7-B Industry Primer pilot plan note v1 작성 완료
- Phase 7-B Industry Primer pilot plan note v1 Claude Code 교차검증 PASS 확인
- Phase 7-B Industry Primer blueprint prep note 작성 완료
- Phase 7-B Industry Primer blueprint prep note Claude Code 교차검증 PASS 확인
- Phase 7-B Industry Primer blueprint prep Section 11 consensus note 작성 완료
- Phase 7-B Industry Primer blueprint prep Section 11 consensus note Claude Code 교차검증 PASS 확인
- Phase 7-B Industry Primer first slice rubric calibration note 작성 완료
- Phase 7-B Industry Primer first slice rubric calibration note Claude Code 교차검증 PASS 확인
- Phase 7-B Industry Primer blueprint v0 작성 완료
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
| 2026-06-08 | `global-harness-v5-phase7-planning-consensus-note-2026-06-07.md`를 Phase 7 작업 지도 재편의 기준 문서로 삼는다 | Phase 7 구조, 사용자 결정 Q1~Q5, signal-routing, Source Pack 소급 검증, Industry Primer pilot, 전역 배포 판단이 합의됐기 때문 |
| 2026-06-08 | Phase 7은 7-0, 7-A, 7-B, 7-C로 나누어 진행한다 | signal-routing을 먼저 정의하고, Source Pack 소급 검증과 Industry Primer 실전 검증, 전역 배포 판단을 분리해야 검증 목적과 한계가 흐려지지 않기 때문 |
| 2026-06-08 | Phase 7-B 첫 실전 pilot 대상은 Phase 2 - Step 4 `Industry Primer`로 한다 | Source Pack 다음 순서로 자연스럽고, Source Pack과 충분히 다른 유형이며, `Moat`처럼 앞단 입력 의존성이 과하지 않기 때문 |
| 2026-06-08 | `Earnings Call`은 Step 3 Source Pack의 하위 분화 후보로 두되 `Industry Primer`의 blocking dependency로 보지 않는다 | Industry Primer는 산업 수준 이해가 핵심이며, Earnings Call 산출물 계약은 Step 6 `Business Model` 또는 이후 하네스 전까지 정리하면 되기 때문 |
| 2026-06-08 | signal-routing은 Phase 7-0에서 scope note와 v0까지 작성한다 | 보안, QA, docs, observability, candidate-ledger의 알림/경고 방식이 drift되지 않게 공통 severity/routing/user-visible 계약이 필요하기 때문 |
| 2026-06-08 | Phase 7-A는 Source Pack 전체를 대상으로 하되 대표 사건과 핵심 폴더 중심으로 소급 검증한다 | 전체 구조를 놓치지 않으면서도 exhaustive audit으로 작업 범위가 폭발하는 것을 막기 위함 |
| 2026-06-08 | signal-routing scope note에서는 `severity`, `route_to`, `record_in`, `user_visibility`를 분리한다 | 긴급도, 처리 module, 기록 위치, 사용자 노출 기준을 섞으면 알림/경고 drift가 생기기 때문 |
| 2026-06-08 | `candidate`와 `improvement`는 severity가 아니라 route/record 성격으로 다룬다 | `candidate-ledger`와 이름 충돌을 피하고 severity를 처리 강도 기준으로 유지하기 위함 |
| 2026-06-08 | signal-routing은 별도 signal log를 강제하지 않고 공통 envelope와 routing 원칙만 정의한다 | 작은 하네스에 artifacts 부담을 늘리지 않고 기존 run-summary, QA 결과, candidate-ledger를 활용하기 위함 |
| 2026-06-08 | signal-routing v0는 scope note의 진행 메타 문구를 제외하고 실행에 필요한 template 계약만 담는다 | v0가 작업 기록이 아니라 새 하네스에 재사용 가능한 module template이어야 하기 때문 |
| 2026-06-08 | signal-routing은 v1 core Section 12 registry에 등재한다 | 파일이 존재하고 Claude Code 교차검증 PASS를 받았으며, 여러 module의 알림/경고/escalation을 연결하는 cross-module 계약이라 발견 가능성이 필요하기 때문 |
| 2026-06-08 | signal-routing registry 위치는 `comparison` 뒤, `observability` 앞에 둔다 | QA/comparison/approval 쪽에서 나온 신호를 공통 envelope로 정리한 뒤 observability/candidate-ledger로 이어지는 흐름이 자연스럽기 때문 |
| 2026-06-08 | Phase 7-A Source Pack 소급 검증은 read-only retrospective validation으로 수행한다 | 기존 Source Pack 하네스, adapter, artifacts, catalog, docs 구조를 임의로 수정하지 않고 문제는 B 진입 전 수정 후보 목록에 기록하기 위함 |
| 2026-06-08 | Phase 7-A validation note의 초기 완료 판정은 `pass`로 둔다 | Source Pack 실제 구조와 v5 core/module 후보 사이 blocking conflict가 발견되지 않았고 B 전 직접 수정이 필요한 항목이 없다고 판단했기 때문 |
| 2026-06-08 | Phase 7-A validation note Claude Code 교차검증 결과 PASS로 본다 | 필수 요건을 모두 충족했고, signal table의 minor observation은 `user_visibility` 단일값과 security self-routing 제거로 정리했기 때문 |
| 2026-06-08 | Industry Primer 하네스는 도구 중립 구조로 설계한다 | 기존 GPT 프롬프트를 하네스 본문으로 복사하지 않고 reference로 흡수하며, Codex/Claude/ChatGPT 계열 실행자는 얇은 adapter로 두기 위함 |
| 2026-06-08 | Industry Primer v0 산출물은 산업 중심 + target company context로 두되 run 단위 standalone / immutable로 둔다 | 산업별 master primer, update, company appendix 모델을 v0부터 넣으면 version/reuse/reconcile 문제가 커지기 때문 |
| 2026-06-08 | Industry Primer source tracking은 v0에서 `source_id`, `source_type`, `title`, `url_or_path`를 최소 필드로 두고 웹 source에는 `accessed_at`을 요구한다 | Source Pack 파일 출처와 웹 외부자료를 함께 쓰면서도 `reliability` 같은 주관 필드는 pilot 이후로 미루기 위함 |
| 2026-06-08 | Industry Primer QA는 구조, 출처, 범위, 판단, handoff 5층 구조로 설계한다 | Industry Primer는 Source Pack처럼 파일 존재 중심 QA가 아니라 판단 기반 산출물이므로 범위 초과와 후속 질문 품질까지 검증해야 하기 때문 |
| 2026-06-08 | Industry Primer에는 security-baseline을 기본 적용하되 최소 안전선으로 좁혀 둔다 | 웹자료, 외부 파일, Source Pack 자료, 비공개 대화/checkpoint를 다룰 수 있으므로 보안 기본선은 필요하지만 보안 운영 매뉴얼로 확장하지 않기 위함 |
| 2026-06-08 | Industry Primer 첫 pilot에서 candidate-ledger는 optional/lightweight로 둔다 | 반복 schema/source/status 후보가 검증되기 전부터 후보 원장을 무겁게 쓰면 하네스보다 운영 체계가 먼저 커질 수 있기 때문 |
| 2026-06-08 | Phase 7-B 첫 pilot 대상은 APP / adtech / mobile advertising으로 둔다 | 사용자가 Unity와 AppLovin을 잘 알고 있고, APP은 Source Pack 일부 자료가 있어 Industry Primer의 실제 입력 부족 대응까지 검증할 수 있기 때문 |
| 2026-06-08 | APP Source Pack은 `Conditional Proceed` 입력으로 다룬다 | 현재 APP 자료는 partial collection이므로 full Source Pack 보강을 선행하지 않고, pilot plan의 Input Readiness Preflight에서 부족분을 웹검색, 외부자료, 확인 필요로 처리하기 위함 |
| 2026-06-08 | Industry Primer pilot plan은 v0 논의판으로 시작하고 최종 합의 후 v1을 새로 만든다 | 질문과 꼬리 질문이 많아 한 번에 최종 note를 작성하면 누락 위험이 크므로 질문별 답변과 합의를 누적하기 위함 |
| 2026-06-08 | Industry Primer 첫 slice 범위는 Section 1, 3, 5, 13으로 한다 | 첫 slice는 완성형 산업 분석이 아니라 산업 경계, 참여자 지도, glossary/type-schema, handoff QA가 작동하는지 확인하는 메커니즘 검증이기 때문. Section 4 하위 시장 구분은 Section 3에서 간략한 컨텍스트로만 다루고 full pilot에서 검증 |
| 2026-06-08 | Industry Primer 첫 slice는 APP partial Source Pack을 `Conditional Proceed` 입력으로 사용한다 | 필수 control input은 APP index, 필수 content input은 APP Q1 IR 2건과 FY2026 Q1 8-K / EX-99.1로 둔다. QA/run-summary는 Input Readiness Preflight 참조로 격하하고, 누락 자료의 정확한 status/signal 어휘는 Q5에서 확정한다 |
| 2026-06-08 | Industry Primer 첫 slice의 웹검색/외부자료는 산업 구조, 용어, 참여자, 플랫폼 정책 이해로 제한한다 | ATT/SKAN/IDFA/Privacy Sandbox는 adtech 구조 이해에 필수이므로 허용하되, 정밀 TAM/CAGR/점유율, 경쟁우위, moat, valuation, 투자 판단은 Market Share/full pilot 또는 이후 단계로 넘긴다 |
| 2026-06-08 | Industry Primer 첫 slice의 source_register는 slice output 내부 섹션으로 둔다 | 첫 slice는 작고 사람이 읽는 흐름이 중요하므로 별도 artifact를 만들지 않는다. full pilot에서 source 수가 많아지거나 기계 재사용 필요성이 생기면 별도 artifact 후보로 기록한다 |
| 2026-06-08 | Industry Primer 첫 slice의 필수 산출물은 slice output, QA result, pilot observation note 3개로 둔다 | 본문, 검증, 템플릿 평가를 분리하기 위함. source_register는 slice output 내부 필수 섹션, signal list는 QA 또는 observation note 내부 선택 섹션으로 둔다 |
| 2026-06-08 | Industry Primer 첫 slice module 적용은 tier 방식으로 둔다 | 필수 적용은 design-preflight, approval-gate, qa-scaffold, signal-routing, pilot-first/testing. security-baseline은 기본, type-schema는 좁게, observability/docs-organization은 가볍게, comparison/checkpoint는 조건부, candidate-ledger는 관찰만 적용. 이 결정으로 Q8/Q9/Q10도 함께 닫는다 |
| 2026-06-08 | Industry Primer 첫 slice QA 완료 판정은 `pass`, `pass with adjustments`, `blocked` 3단계와 5층 QA 집계 규칙으로 둔다 | 완벽한 산출물 여부가 아니라 후속 단계로 안전하게 넘길 수 있는지를 기준으로 판단한다. 구조, 출처, 범위, 판단, handoff 중 하나라도 `blocked`이면 전체 `blocked`, blocked 없이 하나라도 adjustments이면 전체 `pass with adjustments`로 둔다 |
| 2026-06-08 | Industry Primer 첫 slice Source Pack top-up은 기본값이 아니라 예외로 둔다 | 현재 APP partial Source Pack으로 계속 진행, top-up 후보 기록, 실제 top-up 승인 요청 3단계로 구분한다. scope 확장은 top-up trigger가 아니라 approval-gate 사안이며, 범위 확장 승인 후 source 충분성 재평가에서 blocking gap이 발견될 때만 top-up 요청으로 이어진다 |
| 2026-06-08 | Industry Primer pilot plan note v1 전환 기준은 실행자가 v1만 읽고 APP/adtech first slice pilot을 수행할 수 있는지로 둔다 | v1은 논의 기록이 아니라 실행 계획서로 작성한다. Q1~Q12 합의 누락 여부와 v0 합의에 없는 새 조건, 기준, 예외, 필수 산출물 추가 여부를 Claude Code 교차검증 기준에 포함한다 |
| 2026-06-08 | `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v1-2026-06-08.md`를 새 실행 계획서로 작성한다 | v0 질문 보드와 논의 흔적을 제거하고 APP/adtech first slice pilot 실행에 필요한 목적, 전제, 범위, 입력, 산출물, module 적용, QA, top-up, 실행 순서만 남기기 위함 |
| 2026-06-08 | Industry Primer blueprint prep note를 blueprint 작성 전 기준 문서로 둔다 | blueprint 착수 전 논의한 기준 문서 세트, 판단형 QA/rubric, QA output format, output schema, Section 13 handoff schema 합의를 보존하기 위함 |
| 2026-06-08 | Section 13 handoff schema v0는 `question`, `source_ref`, `status`를 필수로 두고 `target_step`, `why_it_matters`는 제외한다 | 다음 하네스와의 과결합을 막으면서도 handoff 질문의 구체성과 근거, source gap 상태를 추적하기 위함 |
| 2026-06-08 | Industry Primer blueprint는 rubric, QA output format, output schema를 함께 설계해야 한다 | Industry Primer QA는 판단형 QA이므로 rubric만 있고 QA 결과 형식이나 부분 구조화 기준이 없으면 실행자마다 산출물이 drift될 수 있기 때문 |
| 2026-06-09 | Industry Primer blueprint-prep Section 11의 6개 질문 합의를 별도 consensus note로 고정한다 | blueprint 작성 전에 하네스 구조, 판단형 rubric, QA output format, 부분 schema, Section 13 handoff schema, module tier 연결 합의를 기억 의존이 아니라 문서 기준으로 보존하기 위함 |
| 2026-06-09 | `industry-primer-qa.schema.md`는 별도 후보 파일로 둔다 | Industry Primer QA는 판단형 QA라서 procedure(실행 순서), rubric(판단 기준), schema(`qa.md` 기록 형식)를 분리해야 drift를 줄일 수 있기 때문 |
| 2026-06-09 | `industry-primer-slice.md`는 runbook에서 분리된 slice 작성 절차 후보로 둔다 | runbook은 전체 실행 흐름을 관리하고, slice procedure는 Section 1/3/5/13 작성 규칙과 Source Pack/웹 source 사용 방식을 담당하기 때문 |
| 2026-06-09 | Industry Primer first slice rubric calibration note를 blueprint 작성 전 기준 문서로 둔다 | Section 1/3/5/13의 실제 내용 기준을 먼저 고정해야 판단형 rubric이 구조만 있고 내용이 비는 문제를 막을 수 있기 때문 |
| 2026-06-09 | full Industry Primer rubric/QA expansion은 first slice pilot 완료 후, full pilot 진입 전에 수행한다 | first slice rubric은 메커니즘 검증용 기준이므로 14개 섹션 전체의 깊은 QA 기준을 대체하지 않기 때문 |
| 2026-06-09 | Industry Primer blueprint v0를 실제 하네스 파일 생성 전 설계도로 작성한다 | 사용자 승인 전 `harness/`, `.agents/`, `.claude/`, `artifacts/` 파일을 만들지 않고 contract, procedure, schema, rubric, output, adapter 경계와 module 연결만 먼저 검증하기 위함 |
| 2026-06-10 | Industry Primer blueprint v0의 기준 문서 우선순위는 숫자 표를 유지하되 주제별 override를 둔다 | 기본 실행 범위/입력/산출물은 pilot plan note v1, build 직전 risk control은 pre-build risk review note, 작업 상태는 work-map을 따르게 해 우선순위 역전과 자기참조를 피하기 위함 |
| 2026-06-10 | Industry Primer Section 3 rubric에 허용/회색지대/금지 구분과 first slice pilot용 gray-zone threshold를 추가한다 | adtech 구조 설명은 경쟁우위 결론으로 기울기 쉬우므로 descriptive 구조 설명, 회색 지대, 금지 영역 결론을 분리해 QA drift를 줄이기 위함 |
| 2026-06-10 | User Review Required Claims와 Additional Gray-Zone Claims를 구분한다 | 사용자가 직접 검토할 상위 5개 claim과 5개에는 못 들었지만 회색 지대로 남길 claim을 분리하고, 두 목록 모두 exhaustive guarantee가 아님을 명시하기 위함 |
| 2026-06-10 | source quality tier는 v0에서 source_register 필드로 추가하지 않고 추적 위치를 분리한다 | Industry Primer 내부 필드화 여부는 pilot observation에서, 판단형 하네스 전역 적용 여부는 Phase 7-C에서 판단하기 위함 |
| 2026-06-11 | Industry Primer first slice 하네스는 S03 내부가 아니라 `P2-S04-Industry-Primer` 독립 하네스로 이전한다 | Source Pack 수집형 하네스와 Industry Primer 판단형/분석형 하네스를 같은 `harness/`, adapter, `artifacts/` 아래 섞으면 orchestration과 라우팅 경계가 흐려지기 때문 |
| 2026-06-11 | S03 work-map은 S04 migration 시점의 handoff/freeze 상태로 남기고, Phase 7-B 남은 build/pilot 작업은 S04 work-map에서 추적한다 | S03은 Source Pack 하네스와 Global Harness v5 설계 스냅샷으로 보존하고, 실제 Industry Primer 실행 하네스의 active 작업장은 S04로 분리하기 위함 |

## 8. 열려 있는 질문

| 질문 | 현재 상태 | 다음 행동 |
|---|---|---|
| v5 core 초안을 별도 파일로 만들까, 기존 core v0를 갱신할까? | 결정됨 | `global-harness-core-structure-template-v1.md` 새 파일 생성 |
| module registry의 경로 표기 방식을 어떻게 고정할까? | 결정됨 | v1 registry는 파일명만 쓰고 기준 폴더를 `docs/templates/global-harness-candidates/`로 명시 |
| `AGENTS.md` / `CLAUDE.md` drift 검사는 수동 체크리스트로 충분한가? | 미정 | Phase 7-C packaging/drift policy에서 재검토 |
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
| v1 core Section 12의 후보 폴더 기준 경로 문구를 전역 배포 시 어떻게 바꿀까? | 미정 | Phase 7-C packaging note에서 template bundle 기준 문구로 변경할지 결정 |
| docs 정리, 보안, QA, observability의 알림/경고를 공통 signal-routing module로 분리할까? | 결정됨 | Phase 7-0에서 signal-routing scope note와 v0 후보 파일 작성 |
| signal-routing을 v1 core registry에 언제 올릴까? | 결정됨 | v0 Claude Code 교차검증 PASS 후 `comparison` 뒤, `observability` 앞에 등재 |
| signal-routing severity 어휘는 무엇으로 확정할까? | 결정됨 | v0는 `critical`, `action_required`, `maintenance`, `info` 4단계로 시작하고 `candidate`/`improvement`는 route/record 성격으로 처리 |
| 전역 template bundle의 최종 canonical source는 어디인가? | 미정 | Phase 7-C 또는 21단계 상위 하네스 설계 시 결정 |
| Phase 7-A 결과가 `pass with adjustments`이면 B 전에 어디까지 수정할까? | 결정됨 | Phase 7-A 최종 판정은 `pass`. B 전 blocking 수정 없음 |
| Source Pack 사례를 전역 bundle에 어느 정도 reference로 남길까? | 미정 | Phase 7-C packaging note에서 reference/example 범위 결정 |
| Source Pack 사례를 v5 core 본문에 넣을까, reference로만 둘까? | 결정됨 | v1 본문에는 길게 넣지 않고 기반 문서와 reference/example로만 처리 |
| Industry Primer pilot 대상 회사/산업은 APP/adtech로 확정할까? | 결정됨 | APP / adtech / mobile advertising을 첫 pilot 대상으로 둔다 |
| APP Source Pack이 partial인데 먼저 보강해야 할까? | 결정됨 | full 보강을 선행하지 않고 `Conditional Proceed`로 pilot plan에 반영 |
| Industry Primer 첫 slice 범위는 어디까지로 할까? | 결정됨 | 첫 slice는 Section 1, 3, 5, 13으로 제한한다. Section 2/4/6~12/14는 full pilot에서 검증하되, Section 4 하위 시장 맥락은 Section 3에서 간략히 다룬다 |
| Industry Primer 첫 slice에서 Source Pack 입력 범위는 어디까지인가? | 결정됨 | APP index를 control input으로, APP Q1 IR 2건과 FY2026 Q1 8-K / EX-99.1을 content input으로 사용한다. QA/run-summary는 preflight 참조로만 사용 |
| Industry Primer 첫 slice에서 웹검색/외부자료 범위는 어디까지인가? | 결정됨 | 구조/용어/참여자/platform policy는 허용, 시장 규모는 맥락적 규모만 허용, 정밀 수치·경쟁우위·valuation 판단은 제외 |
| Industry Primer 첫 slice에서 source_register는 어디에 둘까? | 결정됨 | slice output 내부 필수 섹션으로 둔다. 별도 artifact는 full pilot 후보로만 기록 |
| Industry Primer 첫 slice의 필수 산출물은 무엇인가? | 결정됨 | Industry Primer slice output, Slice QA result, Pilot observation note 3개로 둔다 |
| Industry Primer 첫 slice에서 어떤 module을 실제로 적용할까? | 결정됨 | tier 방식으로 적용한다. comparison/checkpoint는 조건부, candidate-ledger는 별도 파일 없이 observation note에 후보만 기록 |
| Industry Primer 첫 slice QA pass / pass with adjustments / blocked 기준은 무엇인가? | 결정됨 | 5층 QA 기준으로 판정한다. 금지 영역은 Value Chain, Business Model, Market Share, Competition, Moat, Valuation, 투자 판단으로 정의하고, comparison은 판정이 애매할 때 조건부로 발동한다 |
| Industry Primer 첫 slice에서 comparison mode를 사용할까? | 결정됨 | 첫 slice 기본 실행에서는 비활성화하고 결과가 애매하거나 교차검증이 필요할 때만 조건부 사용 |
| Industry Primer 첫 slice에서 candidate-ledger를 켤까? | 결정됨 | 별도 ledger 파일 없이 Pilot observation note Section B에 후보만 기록 |
| Industry Primer 첫 slice에서 checkpoint를 언제 사용할까? | 결정됨 | 세션 전환, 컨텍스트 압축 위험, 사용자 저장 요청 시에만 사용 |
| Industry Primer APP Source Pack top-up trigger는 충분한가? | 결정됨 | Source Pack top-up은 기본값이 아니라 예외로 둔다. 현재 APP partial Source Pack으로 진행 / top-up 후보 기록 / 실제 top-up 승인 요청 3단계로 구분 |
| Industry Primer pilot plan v1 전환 조건은 무엇인가? | 결정됨 | 실행자가 v1만 읽고 APP/adtech first slice pilot을 수행할 수 있으면 v1로 전환한다. v1에는 실행 계획만 담고, v0 질문 보드/논의 흔적/진행 메타 문구는 옮기지 않는다 |
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

1. S03에서 Phase 7-B Industry Primer build/pilot 작업을 더 진행하지 않는다.
2. 사용자가 `C:\Users\frisa\Documents\Investment-Research-OS\P2-S04-Industry-Primer` 폴더 생성과 독립 `git init` Gate를 직접 수행한다.
3. 이후 `global-harness-v5-phase7b-s04-migration-plan-2026-06-11.md`에 따라 S04 파일 복사, S04 bootstrap, S04 Industry Primer 하네스 파일 생성을 S04 work-map에서 추적한다.
4. S03의 `harness/`, `.agents/`, `.claude/`, `artifacts/`는 Source Pack 전용으로 유지한다.
