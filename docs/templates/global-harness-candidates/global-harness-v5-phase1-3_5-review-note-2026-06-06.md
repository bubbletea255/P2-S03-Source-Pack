# Global Harness v5 Phase 1-3.5 Review Note

- 작성일: 2026-06-06
- 상태: checkpoint review note. v5 core 또는 module template 자체가 아니다.
- 검토 범위:
  - `global-harness-v5-core-scope-consensus-2026-06-06.md`
  - `global-harness-v5-core-gap-analysis-2026-06-06.md`
  - `global-harness-core-structure-template-v1.md`
  - `global-harness-v5-design-preflight-extraction-note-2026-06-06.md`
  - `global-harness-design-preflight-template-v0.md`
  - `global-harness-v5-work-map.md`

## 1. 한 줄 결론

Phase 1-3.5의 큰 방향은 일관적이다.

v5 core를 모델 중립 실행 구조로 유지하고, v4의 설계 판단 장치를 `design-preflight` module로 보존하며, 전역 `harness-lab`을 수정하지 않는다는 핵심 원칙은 문서 전반에서 유지되고 있다.

다만 Phase 4로 넘어가기 전에 작은 문서 drift와 module 후보 추적 누락을 정리하는 편이 좋다.

## 2. 확인된 일관성

| 항목 | 판정 | 근거 |
|---|---|---|
| 전역 `harness-lab` 비수정 | OK | consensus, v1 core, work map 모두 원본 유지와 포인터만 둔다는 원칙을 유지한다. |
| v5 core 정체성 | OK | v5 core는 철학 core가 아니라 Codex/Claude Code가 같은 `harness/` 원장을 읽는 모델 중립 실행 구조로 정의되어 있다. |
| `Completion Contract` 용어 미사용 | OK | consensus, gap analysis, v1 core, work map 모두 별도 용어를 만들지 않는 방향으로 정리되어 있다. |
| core + modules 구조 | OK | v1 core는 공통 원장, thin adapter, 산출물 계약, 수정 경계를 직접 다루고, 상세 운영 장치는 module registry로 보낸다. |
| v4 설계 판단 장치 보존 | OK | extraction note와 `design-preflight` v0를 통해 v4의 하네스 유형, 품질 축, 수준 선언, 7요소, 도메인 맥락이 보존됐다. |
| `design-preflight` 범위 | OK | 새 module은 실제 파일 생성 template가 아니라 파일 생성 전 설계 판단 template로 경계를 명시했다. |
| `comparison mode` 발견 | OK | extraction note에서 새 후보로 발견됐고, work map Phase 5/6과 열린 질문에 추적 항목이 생겼다. |

## 3. 수정 후보

### R1. v1 core의 schema/rubric 위치 문구가 `design-preflight` 범위와 약간 충돌한다

- 위치: `global-harness-core-structure-template-v1.md` Section 13
- 현재 의미: 하네스 유형별 상세 schema/rubric을 v4 reference 또는 `design-preflight`/별도 module에서 다룬다고 읽힌다.
- 문제: `global-harness-design-preflight-template-v0.md` Section 17은 contract/procedure/schema/rubric 상세 template를 `file-template` 또는 `type-schema` module로 넘긴다고 명시한다.
- 판단: `design-preflight`는 상세 schema/rubric을 직접 다루는 문서가 아니라, 어떤 schema/rubric이 필요한지 결정하는 preflight 문서다.
- 권장 수정: v1 core의 해당 문구를 아래처럼 좁힌다.

```md
| 하네스 유형별 상세 schema/rubric | v4 reference 또는 별도 `type-schema`/`qa-scaffold` module에서 다룬다. `design-preflight`는 schema/rubric 선택에 필요한 유형과 품질 축만 정한다. |
```

### R2. `file-template`, `type-schema`, `adapter-template`, `meta-orchestrator` 후보가 work map에 아직 추적되지 않는다

- 위치: extraction note Section 3/4.3, design-preflight Section 17
- 현재 상태: `comparison`은 Phase 5/6에 추가됐지만, 다른 후보들은 work map의 module 후보나 열린 질문에 아직 없다.
- 문제: v4 Part 2~4에서 분리된 자산 중 일부가 다음 단계에서 사라질 수 있다.
- 판단: 당장 module 파일을 만들 필요는 없지만, Phase 5 또는 Phase 6에서 "후보로 둘지, reference-only로 둘지" 검토할 항목으로 남기는 편이 안전하다.
- 권장 추가 후보:
  - `file-template` 또는 `contract-procedure-template`
  - `type-schema`
  - `adapter-template`
  - `meta-orchestrator`

### R3. extraction note의 상태 문구가 최신 상태와 맞지 않는다

- 위치: `global-harness-v5-design-preflight-extraction-note-2026-06-06.md` 상단 상태와 Section 7
- 현재 의미: 아직 `global-harness-design-preflight-template-v0.md`를 만든 것은 아니며, 사용자가 검토한 뒤 생성 여부를 결정한다고 되어 있다.
- 현재 실제 상태: `global-harness-design-preflight-template-v0.md`가 생성됐고, work map에서도 결정됨으로 표시됐다.
- 권장 수정: extraction note를 "판정 문서이며, 이후 design-preflight v0가 생성됨"으로 업데이트한다.

### R4. v1 core의 "다음 검증" 목록이 최신 work map보다 좁다

- 위치: `global-harness-core-structure-template-v1.md` Section 15
- 현재 상태: README 링크 대상이 v1과 gap analysis 중심으로 적혀 있다.
- 최신 work map: README에 v1, gap analysis, extraction note, design-preflight 링크를 추가할지 결정하는 것으로 확장됐다.
- 권장 수정: v1 Section 15의 README 항목을 최신 work map과 맞춘다.

### R5. v1 core Section 1의 v4 reference 표현은 유지 가능하지만 우선순위를 조금 더 명확히 할 수 있다

- 위치: `global-harness-core-structure-template-v1.md` Section 1 설계 전 확인
- 현재 문구: 상세한 유형 분류와 설계 판단 장치는 v4 reference, 전역 `harness-lab`, 또는 `design-preflight` module을 따른다.
- 판단: 틀린 문구는 아니다. 다만 `design-preflight` v0가 생긴 지금은 v4가 primary path처럼 보이지 않는 편이 좋다.
- 권장 수정: `design-preflight`를 1차 참조로, v4를 historical baseline/reference로 표현한다.

## 4. Phase 4 진입 가능 여부

조건부 가능.

Phase 1-3.5의 구조적 결정은 충분히 정리됐고, Phase 4 approval/QA/security hook cross-check로 넘어갈 수 있다.

다만 위 R1-R4는 작고 명확한 문서 정합성 문제이므로, Phase 4 전에 처리하는 편이 좋다.
특히 R1과 R3은 문서끼리 서로 다르게 읽힐 수 있어 먼저 고치는 것을 권장한다.

## 5. 추천 다음 순서

1. R1, R3, R4를 먼저 문서 수정으로 정리한다.
2. R2는 work map Phase 5/6 또는 열린 질문에 후보 추적 항목으로 추가한다.
3. R5는 선택 수정으로 처리한다.
4. 그 뒤 README 링크 여부를 결정한다.
5. Phase 4 approval/QA/security hook cross-check로 이동한다.

## 6. 후속 처리 상태

아래 상태는 이 review note 작성 직후의 반영 결과이다.

| 항목 | 상태 | 반영 위치 |
|---|---|---|
| R1 schema/rubric 위치 문구 정리 | 완료 | `global-harness-core-structure-template-v1.md` Section 13 |
| R2 추가 module 후보 추적 | 완료 | `global-harness-v5-work-map.md` Phase 5, 열린 질문 |
| R3 extraction note 상태 갱신 | 완료 | `global-harness-v5-design-preflight-extraction-note-2026-06-06.md` Section 7 |
| R4 README 링크 대상 확장 | 완료 | `global-harness-core-structure-template-v1.md` Section 15 |
| R5 `design-preflight` 1차 참조 정리 | 완료 | `global-harness-core-structure-template-v1.md` Section 1, Section 14 |

남은 일은 수정 반영 결과를 확인한 뒤, 작업 지도에 따라 `global-harness-design-preflight-template-v0.md` 최종 검토와 Phase 4 hook cross-check로 이동하는 것이다.
