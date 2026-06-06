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
| `docs/templates/global-harness-candidates/global-harness-approval-gate-template-v0.md` | approval gate v0 후보 | core hook과 module v1 후보 작성에 사용 |
| `docs/templates/global-harness-candidates/global-harness-qa-scaffold-template-v0.md` | QA scaffold v0 후보 | QA hook과 module v1 후보 작성에 사용 |
| `docs/templates/global-harness-candidates/global-harness-security-baseline-template-v0.md` | security baseline v0 후보 | 보안 hook과 module v1 후보 작성에 사용 |
| `docs/templates/global-harness-candidates/global-harness-observability-template-v0.md` | observability v0 후보 | module registry와 module v1 후보 작성에 사용 |
| `docs/templates/global-harness-candidates/global-harness-checkpoint-template-v0.md` | checkpoint v0 후보 | module registry와 module v1 후보 작성에 사용 |
| `docs/templates/global-harness-candidates/global-harness-docs-organization-template-v0.md` | docs organization v0 후보 | module registry와 module v1 후보 작성에 사용 |
| `docs/templates/global-harness-candidates/global-harness-candidate-ledger-template-v0.md` | candidate ledger v0 후보 | module registry와 module v1 후보 작성에 사용 |
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
| todo | 후보 폴더 README에 작업 지도 존재를 반영할지 결정 | README 갱신 여부 결정 |

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
| todo | v1 approval hook과 approval-gate v0 비교 | approval hook 검증 |
| todo | v1 QA hook과 qa-scaffold v0 비교 | QA hook 검증 |
| todo | v1 security hook과 security-baseline v0 비교 | security hook 검증 |
| todo | v1 hook 문구가 module 세부 규칙을 과도하게 복사하거나 충돌하지 않는지 점검 | hook review note |

### Phase 5. Available Modules Registry 검증

목표: `global-harness-core-structure-template-v1.md`에 작성된 module registry의 module 목록, 경로 표기 방식, 미정 항목을 검토하고 실제 후보 파일들과 연결되는지 확인한다.

| 상태 | 작업 | 산출물 |
|---|---|---|
| todo | v1 module registry와 후보 파일 목록 비교 | module registry 검증 |
| todo | 각 module의 사용 조건 한 줄이 실제 v0 내용과 맞는지 확인 | module usage note |
| todo | 실제 파일 경로 표기 방식 결정 | module registry draft |
| todo | pilot-first를 별도 module로 둘지 candidate-ledger/testing에 묶을지 결정 | decision note |
| todo | comparison mode를 별도 module로 둘지 `qa-scaffold`에 포함할지 검토 | comparison module decision |
| todo | extraction note에서 발견된 추가 후보(`file-template`, `type-schema`, `adapter-template`, `meta-orchestrator`)를 registry/backlog에 둘지 검토 | additional module candidate decision |

### Phase 6. module v0 검토와 v1 후보화

목표: 각 module v0를 바로 전역화하지 않고, v5 구조에 맞는 v1 후보로 다듬을지 결정한다.

| 상태 | module | 작업 |
|---|---|---|
| todo | approval-gate | core hook과 module 상세의 경계를 정리 |
| todo | qa-scaffold | 공통 QA 뼈대와 하네스별 QA의 경계를 정리 |
| todo | security-baseline | 최소 보안 hook과 상세 보안 절차의 경계를 정리 |
| todo | observability | Source Pack 이름을 제거하고 전역 관측 module로 일반화 |
| todo | checkpoint | compact checkpoint와 원문 저장 금지 원칙 정리 |
| todo | docs-organization | 처음부터 강제할 구조와 문서가 많아졌을 때 적용할 구조 구분 |
| todo | candidate-ledger | candidate 패턴과 schema 승격 절차 구분 |
| todo | comparison | v4 comparison mode 운영 규칙을 별도 module 후보로 둘지 QA scaffold에 통합할지 정리 |

### Phase 7. pilot 검증과 전역화 판단

목표: v5 core와 module 후보를 실제 다음 하네스에서 검증한 뒤 전역 반영 여부를 결정한다.

| 상태 | 작업 | 산출물 |
|---|---|---|
| todo | pilot 대상 하네스 선정 | pilot plan |
| todo | v5 core 후보를 pilot 청사진에 적용 | pilot notes |
| todo | module 중 필요한 것만 선택 적용 | pilot notes |
| todo | 적용 중 drift, 누락, 과잉 규칙 기록 | evaluation note |
| todo | 전역 `harness-lab` 수정 여부는 별도 논의로 보류 | deferred decision |

## 6. 현재 작업 보드

### In Progress

- 현재 없음

### Next

1. Phase 1~3.5 review note 수정 반영 결과 확인
2. `global-harness-design-preflight-template-v0.md` 최종 검토
3. README에 v1, gap analysis, extraction note, design-preflight 링크를 추가할지 결정
4. approval, QA, security hook이 각 module v0와 충돌하지 않는지 검토
5. Phase 4 최소 hook cross-check로 이동

### Backlog

- approval gate module v1 후보 정리
- QA scaffold module v1 후보 정리
- security baseline module v1 후보 정리
- observability module 전역화 문구 정리
- checkpoint module 전역화 문구 정리
- docs organization module 적용 기준 정리
- candidate ledger module 승격/폐기 기준 정리
- comparison module 후보 검토
- README에 work map 링크 추가 여부 결정

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

## 8. 열려 있는 질문

| 질문 | 현재 상태 | 다음 행동 |
|---|---|---|
| v5 core 초안을 별도 파일로 만들까, 기존 core v0를 갱신할까? | 결정됨 | `global-harness-core-structure-template-v1.md` 새 파일 생성 |
| module registry의 경로 표기 방식을 어떻게 고정할까? | 미정 | registry 작성 시 결정 |
| `AGENTS.md` / `CLAUDE.md` drift 검사는 수동 체크리스트로 충분한가? | 미정 | v5 core 초안 후 재검토 |
| pilot-first는 candidate-ledger에 묶을까, 별도 testing module로 둘까? | 미정 | module registry 작성 시 결정 |
| `design-preflight`를 별도 module v0로 분리할까? | 결정됨 | `global-harness-design-preflight-template-v0.md` 생성 |
| `comparison mode`를 별도 module로 둘까, `qa-scaffold`에 포함할까? | 미정 | Phase 5 module registry 검증 시 결정 |
| extraction note의 추가 후보(`file-template`, `type-schema`, `adapter-template`, `meta-orchestrator`)를 registry/backlog에 둘까? | 미정 | Phase 5 module registry 검증 시 결정 |
| Source Pack 사례를 v5 core 본문에 넣을까, reference로만 둘까? | 결정됨 | v1 본문에는 길게 넣지 않고 기반 문서와 reference/example로만 처리 |
| 후보 폴더 README를 언제 갱신할까? | 미정 | work map 안정화 후 결정 |

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

1. Phase 1~3.5 review note 수정 반영 결과를 확인한다.
2. `global-harness-design-preflight-template-v0.md`를 최종 검토한다.
3. README에 v1, gap analysis, extraction note, design-preflight 링크를 추가할지 결정한다.
4. approval, QA, security hook이 각 module v0와 충돌하지 않는지 검토한다.
5. Phase 4 최소 hook cross-check로 이동한다.
