# Global Harness Core Structure Template v1

- 작성일: 2026-06-06
- 상태: v5 core 후보 템플릿. 아직 전역 `harness-lab`, 공용 템플릿 v5, module template에 반영한 확정 문서가 아니다.
- 기반 문서:
  - `docs/templates/공용_하네스_템플릿_체크리스트_v4.md`
  - `docs/templates/global-harness-candidates/global-harness-core-structure-template-v0.md`
  - `docs/templates/global-harness-candidates/global-harness-v5-core-scope-consensus-2026-06-06.md`
  - `docs/templates/global-harness-candidates/global-harness-v5-core-gap-analysis-2026-06-06.md`

## 1. 목적: 모델 중립 실행 구조

이 템플릿은 새 하네스를 만들 때 Codex와 Claude Code가 같은 업무 원장을 읽고 실행하도록 돕는 모델 중립 실행 구조 후보이다.

v1 core의 목표:

- 업무 의미를 `harness/` 공통 원장에 둔다.
- Codex와 Claude Code 실행 방식은 얇은 adapter로 둔다.
- 중간 산출물, 최종 산출물, 실행 기록을 파일로 남긴다.
- 완료 기준을 adapter가 아니라 공통 원장에 둔다.
- 수정 경계를 명확히 해 drift를 줄인다.
- 승인, QA, 보안은 core에 최소 hook만 두고 상세는 module로 분리한다.

핵심 문장:

```text
v5 core는 harness/ 공통 원장을 중심으로 업무 의미를 모델 중립적으로 고정하고,
Codex/Claude Code 같은 실행 환경은 얇은 adapter로 연결하며,
산출물 계약과 수정 경계를 통해 같은 하네스를 반복 실행 가능하게 만드는 최소 구조다.
```

설계 전 확인:

이 템플릿은 하네스의 실행 구조를 정의한다.
적용 전에 먼저 하네스 유형, 산출물 역할, 품질 축, 유형별 수준 선언, 하네스 7요소, 도메인 프로그램 맥락을 확인한다.
상세한 유형 분류와 설계 판단 장치는 `design-preflight` module을 1차로 따른다.
v4는 설계 배경 reference로 참조 가능하다.

도메인 프로그램이 있으면, 예를 들어 가치투자 21단계 같은 상위 구조가 있으면, 그 프로그램 구조를 먼저 참조한다.
여러 전문 하네스를 하나의 거대한 Phase 목록으로 합치지 않는다.

## 2. 전역 `harness-lab`과 v5 core의 관계

전역 `harness-lab`은 하네스 설계 철학과 방법론의 기초이다.
이 v1 core 후보는 `harness-lab`을 대체하거나 수정하지 않는다.

v1 core에는 아래 포인터만 둔다.

```text
이 템플릿은 전역 harness-lab의 하네스 설계 방법론을 전제로 한다.
v5 core는 harness-lab을 대체하거나 수정하지 않고,
프로젝트별 모델 중립 실행 구조를 정의한다.
```

주의:

- 전역 `harness-lab` 내용을 이 문서에 길게 복사하지 않는다.
- 이 문서를 이유로 전역 `harness-lab`을 바로 수정하지 않는다.
- 먼저 후보 템플릿으로 사용하고, 실제 하네스에서 검증한 뒤 전역화 여부를 따로 판단한다.

## 3. core와 module의 구분

v5 구조는 하나의 거대한 문서가 아니라 core + modules 구조를 따른다.

| 구분 | 역할 | 예시 |
|---|---|---|
| core 본문 | 모든 모델 중립 하네스에 필요한 최소 실행 구조 | 공통 원장, 얇은 adapter, 산출물 계약, 수정 경계 |
| core 최소 hook | 중요하지만 상세는 하네스별로 달라지는 안전 장치 | 승인 게이트, QA, 보안 |
| module registry | 선택 운영 장치의 존재를 알려주는 목록 | 관측 가능성, 체크포인트, docs 정리, candidate ledger |
| core 밖 | v5 core가 다루지 않는 영역 | 전역 `harness-lab` 수정, Agent Team 런타임 상세, 특정 도메인 예시 |

core 본문은 공통 구조를 정의한다.
module은 하네스의 위험도, 길이, 운영 방식에 따라 선택 적용한다.

## 4. 공통 원장: `harness/`

`harness/`는 하네스의 업무 의미를 담는 source of truth이다.

권장 최소 구조:

```text
harness/
├── ORCHESTRATOR.md
├── MANIFEST.md
├── contracts/
├── procedures/
├── schemas/
└── rubrics/
```

각 영역의 역할:

| 위치 | 역할 |
|---|---|
| `harness/ORCHESTRATOR.md` | 하네스의 전체 목적, 실행 모드, 승인/중단 조건, 공통 운영 원칙 |
| `harness/MANIFEST.md` | 파일 역할, 입력/출력 의존 관계, adapter와 산출물 지도 |
| `harness/contracts/` | 단계별 목표, 입력, 출력, 완료 기준, 금지사항 |
| `harness/procedures/` | 단계별 수행 절차 |
| `harness/schemas/` | 산출물 형식과 필수 필드 |
| `harness/rubrics/` | 검증 기준과 품질 평가 기준 |

기본 규칙:

- 업무 의미 변경은 `harness/`에서 한다.
- adapter에는 공통 업무 규칙을 길게 복사하지 않는다.
- 다음 하네스가 읽어야 하는 입력과 출력은 `harness/MANIFEST.md`와 산출물 계약에 남긴다.

## 5. 얇은 adapter: Claude Code / Codex 실행 표면

adapter는 특정 실행 환경에서 공통 원장을 읽고 실행하게 하는 얇은 연결층이다.

권장 위치:

| 위치 | 역할 |
|---|---|
| `.claude/agents/` | Claude Code 실행 adapter |
| `.claude/skills/` | Claude Code skill 구조를 사용하는 경우의 adapter |
| `.agents/skills/` | Codex 실행 adapter |
| `.codex/` | Codex 실행 설정 또는 보조 adapter |

adapter에 둘 것:

- 자연어 트리거와 진입점 설명
- 실행 전 반드시 읽을 `harness/` 파일 목록
- 해당 환경에서 사용할 도구와 실행 방식
- 산출물 저장 위치 안내
- adapter 자체의 금지사항

adapter에 두지 않을 것:

- 공통 업무 판단 기준
- 하네스의 핵심 완료 기준
- schema나 rubric의 상세 내용
- 다른 adapter의 세부 실행 방식
- 전역 `harness-lab` 내용 복사본

Codex adapter 예시 원칙:

```text
이 Skill은 Codex 실행 진입점이다.
공통 업무 규칙은 harness/를 단일 원본으로 따른다.
이 Skill에는 공통 contract/procedure/schema/rubric을 길게 복사하지 않는다.
```

Claude Code adapter 예시 원칙:

```text
이 Agent 또는 Skill은 Claude Code 실행 진입점이다.
공통 업무 규칙은 harness/를 단일 원본으로 따른다.
이 adapter에는 공통 contract/procedure/schema/rubric을 길게 복사하지 않는다.
```

## 6. 권장 최소 파일 구조

새 하네스는 아래 구조를 기본 후보로 삼는다.
하네스가 작으면 일부 폴더를 줄일 수 있지만, 줄인 이유를 남긴다.

```text
{harness-project}/
├── AGENTS.md
├── CLAUDE.md
├── docs/
├── harness/
│   ├── ORCHESTRATOR.md
│   ├── MANIFEST.md
│   ├── contracts/
│   ├── procedures/
│   ├── schemas/
│   └── rubrics/
├── .claude/
│   ├── agents/
│   └── skills/
├── .agents/
│   └── skills/
├── .codex/
└── artifacts/
    ├── README.md
    ├── improvement-log.md
    └── runs/
```

최소 생성 기준:

| 파일 또는 폴더 | 최소 필요 여부 | 이유 |
|---|---:|---|
| `AGENTS.md` | 예 | Codex와 새 세션용 프로젝트 안내판 |
| `CLAUDE.md` | 예 | Claude Code와 새 세션용 프로젝트 안내판 |
| `harness/ORCHESTRATOR.md` | 예 | 공통 실행 원칙 |
| `harness/MANIFEST.md` | 예 | 파일 역할과 의존 관계 |
| `harness/contracts/` | 예 | 산출물 계약과 완료 기준 |
| `harness/procedures/` | 예 | 수행 절차 |
| `harness/schemas/` | 하네스에 따라 | 기계가 읽는 산출물이 있으면 필요 |
| `harness/rubrics/` | 하네스에 따라 | QA나 비교가 필요하면 필요 |
| `.agents/skills/` | Codex 실행 시 예 | Codex adapter |
| `.claude/agents/` 또는 `.claude/skills/` | Claude Code 실행 시 예 | Claude adapter |
| `artifacts/README.md` | 예 | 산출물 지도 |
| `artifacts/improvement-log.md` | 예 | 개선 기록 |

## 7. `ORCHESTRATOR` / runbook / procedure 구분

v5 core에서는 전체 운영 원칙, 1회 실행 절차, 세부 작업 절차를 구분한다.

| 문서 | 역할 | 예시 내용 |
|---|---|---|
| `harness/ORCHESTRATOR.md` | 하네스 전체 운영 원칙 | 목적, 실행 모드, 승인 조건, 중단 조건, 공통 파일 지도 |
| `harness/procedures/{harness-name}-runbook.md` | 1회 실행 절차 | run-id 결정, 입력 확인, 단계 실행 순서, QA 호출, 결과 요약 |
| `harness/procedures/{phase-or-task}.md` | 세부 작업 절차 | 자료 수집, 정규화, 분석, 검증 같은 단계별 작업법 |

구분 기준:

- 여러 실행에 항상 적용되는 운영 원칙은 `ORCHESTRATOR.md`에 둔다.
- 특정 실행을 시작해서 끝내는 순서는 runbook에 둔다.
- 개별 단계의 손작업 절차는 procedure에 둔다.

주의:

- `ORCHESTRATOR.md`에 모든 세부 절차를 밀어 넣지 않는다.
- runbook이 없으면 실제 1회 실행 순서가 adapter에 흩어지기 쉽다.
- procedure가 없으면 단계별 작업법이 대화나 adapter에 남아 drift가 생긴다.

## 8. 산출물 계약

하네스 결과는 대화에만 남기지 않는다.
다음 사람이 읽거나 다음 하네스가 입력으로 사용할 수 있게 파일로 남긴다.

산출물 계약에 포함할 것:

| 항목 | 설명 |
|---|---|
| 필수 산출물 | 반드시 만들어야 하는 파일 목록 |
| 선택 산출물 | 가능하면 만들지만 실패해도 전체 실패는 아닌 파일 |
| 사람용 산출물 | 사람이 읽는 index, summary, report, checklist |
| 기계용 산출물 | catalog, jsonl, csv, schema 기반 출력 |
| 중간 산출물 | 다음 단계가 읽어야 하는 파일 |
| 최종 산출물 | 이번 실행의 완료 결과 |
| 저장 위치 | `artifacts/`, `artifacts/runs/{run-id}/`, 회사별 index 등 |
| 후속 입력 | 다음 하네스가 무엇을 읽어야 하는지 |

기본 규칙:

- 필수 산출물 경로는 contract 또는 MANIFEST에 명시한다.
- 산출물이 없으면 완료로 보지 않는다.
- 기존 운영 산출물을 덮어쓰는 작업은 사용자 승인 없이 하지 않는다.
- 비교 모드 결과는 운영 산출물과 분리해 저장한다.

## 9. 완료 기준의 위치

`Completion Contract`라는 별도 용어는 만들지 않는다.

완료 여부는 아래 세 요소의 조합으로 정의한다.

| 요소 | 위치 | 설명 |
|---|---|---|
| 무엇을 만들고 어디에 둘 것인가 | 산출물 계약, `harness/contracts/`, `harness/MANIFEST.md` | 필수 산출물과 경로 |
| 그것이 충분한지 어떻게 볼 것인가 | QA 기준, `harness/rubrics/`, QA procedure | 검증 기준 |
| 언제 멈추고 사람에게 물을 것인가 | `harness/ORCHESTRATOR.md`, runbook, contract | 승인/중단 조건 |

권장 문구:

```text
완료 기준은 별도 adapter에 두지 않는다.
각 하네스는 harness/ 안의 산출물 계약, QA 기준, 승인/중단 조건을 통해 완료 여부를 정의한다.
Codex와 Claude Code adapter는 이 기준을 복사하지 않고 같은 원장을 참조한다.
```

## 10. 수정 경계와 `AGENTS.md` / `CLAUDE.md` 동기화

수정 경계:

| 바꾸고 싶은 것 | 수정 위치 |
|---|---|
| 업무 의미, 목표, 허용/금지 판단 | `harness/`, 특히 `contracts/` |
| 전체 실행 모드와 승인/중단 조건 | `harness/ORCHESTRATOR.md` |
| 1회 실행 순서 | `harness/procedures/{harness-name}-runbook.md` |
| 세부 수행 절차 | `harness/procedures/` |
| 출력 형식 | `harness/schemas/` |
| 평가 기준 | `harness/rubrics/` |
| Claude Code 실행 방식 | `.claude/agents/`, `.claude/skills/` |
| Codex 실행 방식 | `.agents/skills/`, `.codex/` |
| 실행 산출물 | `artifacts/` |
| 논의, 설계, handoff, reference | `docs/` |

`AGENTS.md`와 `CLAUDE.md`:

- 두 파일은 같은 구조 안내를 가져야 한다.
- 한쪽에서 하네스 구조, 자연어 라우팅, 수정 원칙을 바꾸면 다른 쪽도 drift를 점검한다.
- 세부 업무 규칙은 두 파일에 길게 복사하지 않고 `harness/`를 가리킨다.

권장 포함 내용:

- 하네스 목적 한 문단
- 자연어 라우팅 예시
- `harness/` 공통 원장 포인터
- adapter 위치
- 산출물 위치
- 수정 원칙
- 변경 이력

## 11. 최소 hook: 승인 게이트, QA, 보안

아래 세 항목은 core 본문에 최소 hook만 둔다.
상세 체크리스트와 절차는 module template를 따른다.

### 11.1 승인 게이트 hook

```text
논의, 검토, 설계와 실제 파일 수정, 실행, 삭제, 운영 반영을 구분한다.
삭제, 이동, schema 변경, 운영 catalog 반영, 외부 제출은 명시 승인 없이는 수행하지 않는다.
상세 승인 문구와 위험도 분류는 approval-gate module을 따른다.
```

### 11.2 QA hook

```text
모든 하네스는 최소한 무엇을 검증할지, 어떤 파일을 확인할지, 실패 시 어디서 멈출지 정의해야 한다.
세부 상태값, rubric, 재검증 절차는 qa-scaffold module을 따른다.
```

### 11.3 보안 hook

```text
secret, credential, API key, .env, 대화 원문, 격리된 파일은 raw 산출물이나 docs에 무심코 저장하지 않는다.
상세 보안 점검과 공개 범위 판단은 security-baseline module을 따른다.
```

## 12. Available Modules Registry

module registry는 발견 가능성을 위한 목록이다.
module 세부 규칙을 v5 core 본문에 복사하지 않는다.
아래 `현재 후보 파일`의 경로 기준은 `docs/templates/global-harness-candidates/`이다.

| module | 현재 후보 파일 | 사용할 때 |
|---|---|---|
| design-preflight | `global-harness-design-preflight-template-v0.md` | 하네스 유형, 산출물 역할, 품질 축, 수준 선언, 허용/금지 판단, 7요소, 실행 구조, 도메인 맥락을 정해야 할 때 |
| type-schema | 별도 파일 없음. `design-preflight`(유형/품질 축 판단), `qa-scaffold`(검증 기준), 각 하네스의 `harness/schemas/` 연결 | 하네스 유형별 schema/rubric 구조를 정해야 할 때 |
| security-baseline | `global-harness-security-baseline-template-v1.md` | secret, credential, `.env`, 비공개 대화 원문, 격리 파일, 외부 공개 범위의 최소 안전선이 필요할 때 |
| approval-gate | `global-harness-approval-gate-template-v1.md` | 논의/검토와 실제 수정/실행 승인 경계를 상세화할 때 |
| qa-scaffold | `global-harness-qa-scaffold-template-v1.md` | QA 항목 범주, 상태값, 결과 파일 skeleton, repair/recheck, escalation 기록 기준이 필요할 때 |
| comparison | 별도 파일 없음. `design-preflight`(필요 여부), `qa-scaffold`(공통 rubric), `approval-gate`(운영 반영 승인), `observability`(비교 결과 기록) 연결 | 두 모델 실행 결과를 같은 기준으로 비교하고 운영 반영 여부를 판단할 때 |
| signal-routing | `global-harness-signal-routing-template-v0.md` | 여러 module의 알림, 경고, escalation을 공통 signal envelope와 routing 기준으로 표현해야 할 때 |
| observability | `global-harness-observability-template-v1.md` | run-summary에 운영 관찰 필드, 병목, trim 후보를 남길 때 |
| candidate-ledger | `global-harness-candidate-ledger-template-v1.md` | 새 분류, 상태값, source, schema 값, 예외를 바로 정식화하지 않고 후보 record/evidence로 추적할 때 |
| pilot-first / testing | 별도 파일 없음. `candidate-ledger`와 Phase 7 pilot validation에서 처리 | 새 source, 새 자동화, 새 schema 변경을 운영 반영 전에 후보로 기록하고 작게 검증할 때 |
| checkpoint | `global-harness-checkpoint-template-v1.md` | 긴 작업을 원문 저장 없이 compact checkpoint로 이어가야 할 때 |
| docs-organization | `global-harness-docs-organization-template-v1.md` | docs가 많아져 역할별 정리, `docs/README.md` 색인, 이동 전후 참조 점검이 필요할 때 |

module 적용 원칙:

- 모든 module을 새 하네스에 강제하지 않는다.
- 하네스 위험도와 운영 길이에 따라 선택한다.
- module을 적용하면 어떤 파일과 절차가 추가되는지 `harness/MANIFEST.md` 또는 `docs/`에 남긴다.
- module 내용을 core 본문이나 adapter에 길게 복사하지 않는다.

## 13. v5 core 밖에 둘 항목

아래 항목은 이 core template가 직접 다루지 않는다.

| 항목 | 이유 |
|---|---|
| 전역 `harness-lab` 수정 | 모든 하네스에 영향을 주므로 충분한 검증 뒤 별도 논의가 필요하다. |
| Agent Team 상세 런타임 | `harness-lab` 또는 별도 agent-team 설계 문서의 영역이다. |
| 특정 도메인 예시 | Source Pack, SEC, IR, transcript 같은 사례는 reference/example로 둔다. |
| 자동화 스크립트 | 특정 도구나 런타임에 묶일 수 있으므로 core에 넣지 않는다. |
| HTML 리포트/대시보드 출력 템플릿 | 산출물 유형별 템플릿이지 모델 중립 실행 구조가 아니다. |
| 하네스 유형별 상세 schema/rubric | `type-schema` registry item, `qa-scaffold`, 각 하네스의 `harness/schemas/`, 또는 v4 reference에서 다룬다. `design-preflight`는 schema/rubric 선택에 필요한 유형과 품질 축만 정한다. |

## 14. 적용 전 체크리스트

새 하네스에 이 core template를 적용하기 전에 확인한다.

- [ ] 하네스 유형을 먼저 정했다.
- [ ] 산출물 역할과 품질 축을 확인했다.
- [ ] 유형별 수준 선언을 정했다.
- [ ] 목표, 컨텍스트, 도구, 중간 산출물, 검증, 승인, 기록/개선의 7요소를 확인했다.
- [ ] 도메인 프로그램이나 상위 단계 구조가 있으면 먼저 참조했다.
- [ ] 전역 `harness-lab`을 수정하지 않는다.
- [ ] 업무 의미가 `harness/`에 있다.
- [ ] adapter는 `harness/` 원장을 읽는 얇은 연결층이다.
- [ ] 공통 업무 규칙을 adapter에 길게 복사하지 않았다.
- [ ] `ORCHESTRATOR.md`, runbook, procedure의 역할이 구분되어 있다.
- [ ] 필수 산출물과 저장 위치가 명시되어 있다.
- [ ] 다음 하네스가 읽을 입력이 명시되어 있다.
- [ ] 완료 기준이 산출물 계약, QA 기준, 승인/중단 조건으로 표현되어 있다.
- [ ] `Completion Contract`라는 새 용어를 만들지 않았다.
- [ ] 수정 경계가 `harness/`, adapter, `artifacts/`, `docs/`로 나뉘어 있다.
- [ ] `AGENTS.md`와 `CLAUDE.md` 구조 안내가 서로 모순되지 않는다.
- [ ] 승인 게이트, QA, 보안은 최소 hook만 core에 있고 상세는 module로 분리된다.
- [ ] 필요한 module만 선택 적용한다.
- [ ] 설계 판단 상세가 필요하면 `design-preflight` module을 1차로 참조하고, v4는 설계 배경 reference로 확인한다.
- [ ] Source Pack 같은 특정 도메인 예시를 core 본문에 길게 넣지 않았다.
- [ ] v1을 전역 확정 문서로 취급하지 않고 후보 템플릿으로 기록했다.

## 15. 검증 상태와 남은 검증

이 v1 후보는 문서 작성으로 끝나지 않는다.

완료된 검증:

1. `global-harness-v5-work-map.md`에 v1 생성 사실을 기록했다.
2. README에 v1, gap analysis, extraction note, design-preflight, Phase 4 hook cross-check note를 반영했다.
3. `design-preflight` module v0가 extraction note와 충돌하지 않는지 검토했다.
4. approval, QA, security hook이 각 module v0와 충돌하지 않는지 검토했다.

남은 검증:

1. Phase 5에서 module registry와 실제 후보 파일 목록을 검증한다.
2. Phase 7에서 최소 한 개의 다음 하네스 청사진에 v1을 적용해본다.
3. 실제 적용 후 과하거나 부족한 규칙을 `docs/` 또는 improvement log에 남긴다.
