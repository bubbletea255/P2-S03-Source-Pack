# Global Harness v5 Phase 5 Module Registry Comparison

- 작성일: 2026-06-06
- 상태: Phase 5 부분 검토 note. v5 core 또는 module template 자체가 아니다.
- 검토 범위:
  - `global-harness-core-structure-template-v1.md` Section 12 `Available Modules Registry`
  - `docs/templates/global-harness-candidates/` 실제 후보 파일 목록
  - `global-harness-v5-design-preflight-extraction-note-2026-06-06.md`의 추가 module 후보

## 1. 한 줄 결론

v1 registry에 적힌 파일 기반 module 8개는 모두 실제 후보 파일이 있다.

`pilot-first / testing`은 검토 당시 v1 registry에서 파일이 `미정`으로 표시되어 있었으므로 누락이 아니라 의도된 미결정 항목이었다.
이후 `global-harness-v5-phase5-pilot-first-decision-note-2026-06-06.md`에서 별도 파일 없이 `candidate-ledger`와 Phase 7 pilot validation에 묶어 처리하기로 결정했다.

다만 extraction note에서 발견된 `comparison`, `file-template`, `type-schema`, `adapter-template`, `meta-orchestrator`는 아직 v1 registry의 정식 module 파일로 존재하지 않는다. 이들은 Phase 5의 후속 결정 대상으로 유지한다.

## 2. v1 Registry와 실제 후보 파일 대조

| v1 registry module | v1 후보 파일 | 실제 파일 존재 | 판정 |
|---|---|---|---|
| `design-preflight` | `global-harness-design-preflight-template-v0.md` | yes | OK |
| `approval-gate` | `global-harness-approval-gate-template-v0.md` | yes | OK |
| `qa-scaffold` | `global-harness-qa-scaffold-template-v0.md` | yes | OK |
| `security-baseline` | `global-harness-security-baseline-template-v0.md` | yes | OK |
| `observability` | `global-harness-observability-template-v0.md` | yes | OK |
| `checkpoint` | `global-harness-checkpoint-template-v0.md` | yes | OK |
| `docs-organization` | `global-harness-docs-organization-template-v0.md` | yes | OK |
| `candidate-ledger` | `global-harness-candidate-ledger-template-v0.md` | yes | OK |
| `pilot-first / testing` | 검토 당시 `미정`; 이후 별도 파일 없음으로 결정 | no dedicated file | `candidate-ledger` + Phase 7 pilot validation |

## 3. 후보 폴더에 있지만 registry module은 아닌 파일

아래 파일들은 후보 작업의 근거, 작업 지도, 검토 note, 이전 후보이므로 v1 module registry에 들어가지 않는 것이 자연스럽다.

| 파일 | 성격 | 판정 |
|---|---|---|
| `README.md` | 후보 폴더 안내 | registry module 아님 |
| `global-harness-v5-work-map.md` | 작업 지도 | registry module 아님 |
| `global-harness-template-v5-discussion-handoff-2026-06-06.md` | 논의 인계 | registry module 아님 |
| `global-harness-v5-core-scope-consensus-2026-06-06.md` | core 범위 합의 | registry module 아님 |
| `global-harness-v5-core-gap-analysis-2026-06-06.md` | gap analysis | registry module 아님 |
| `global-harness-v5-design-preflight-extraction-note-2026-06-06.md` | v4 추출 note | registry module 아님 |
| `global-harness-v5-phase1-3_5-review-note-2026-06-06.md` | checkpoint review | registry module 아님 |
| `global-harness-v5-phase4-hook-cross-check-2026-06-06.md` | hook cross-check | registry module 아님 |
| `global-harness-core-structure-template-v0.md` | 초기 core 후보 | registry module 아님 |
| `global-harness-core-structure-template-v1.md` | v5 core 후보 본문 | registry module 아님 |

## 4. extraction note에서 발견된 추가 후보

아래 항목은 v4 extraction 과정에서 발견됐지만 아직 정식 registry module로 확정되지 않았다.

| 추가 후보 | 현재 파일 | 현재 위치 | 다음 판단 |
|---|---|---|---|
| `comparison` | 없음 | work map Phase 5/6 추적 중 | 별도 module vs `qa-scaffold` 포함 결정 |
| `file-template` | 없음 | extraction note 추적 중 | registry 후보 vs backlog/reference 결정 |
| `type-schema` | 없음 | extraction note 추적 중 | registry 후보 vs `qa-scaffold` 경계 결정 |
| `adapter-template` | 없음 | extraction note 추적 중 | registry 후보 vs core/reference 결정 |
| `meta-orchestrator` | 없음 | extraction note 추적 중 | registry 후보 vs backlog/reference 결정 |

## 5. 현재 판정

Phase 5 첫 작업인 "v1 module registry와 후보 파일 목록 비교"는 완료로 본다.

blocking issue는 없다.

다음 Phase 5 작업은 각 module의 "사용할 때" 한 줄이 실제 v0 내용과 맞는지 확인하는 것이다. 이 작업에서 registry 설명이 과하거나 부족하면 v1 core Section 12를 수정할 수 있다.
