# Global Harness v5 Phase 7-B S04 Migration / Bootstrap Plan

- 작성일: 2026-06-11
- 상태: Claude Code 교차검증 PASS
- 단계: Phase 7-B Industry Primer first slice 하네스 생성 전 migration/bootstrap 계획
- 목적: `P2-S03-Source-Pack` 안에 Industry Primer 하네스를 섞지 않고, `P2-S04-Industry-Primer` 독립 하네스로 이전/초기화하기 위한 계획을 고정한다.
- 범위: migration/bootstrap plan. Step 2(Claude Code 교차검증)와 Step 3(S03 handoff + work-map freeze)는 완료. 아직 실행 전: `P2-S04-Industry-Primer` 폴더 생성, git init, S03 -> S04 파일 복사, S04 bootstrap, Industry Primer 하네스 파일 생성.

## 1. 결론

Industry Primer first slice 하네스는 `P2-S03-Source-Pack` 아래에 만들지 않는다.

대신 아래 독립 폴더를 새 프로젝트 루트로 사용한다.

```text
C:\Users\frisa\Documents\Investment-Research-OS\P2-S04-Industry-Primer
```

이 결정의 핵심은 파일명 충돌 방지가 아니라 하네스 경계 보존이다.

`P2-S03-Source-Pack`은 Source Pack 수집형 하네스다.
`P2-S04-Industry-Primer`는 Phase 2 Step 4 Industry Primer 판단형/분석형 하네스가 된다.

두 하네스는 같은 Investment Research OS 안에서 연결되지만, 같은 `harness/`, 같은 adapter, 같은 `artifacts/`를 공유하지 않는다.

## 2. 왜 P2-S04 별도 폴더가 필요한가

### 2.1 하네스 성격이 다르다

| 하네스 | 성격 | 주요 산출물 |
|---|---|---|
| P2-S03 Source Pack | 수집형 하네스 | SEC, IR, transcript raw source, catalog, company index |
| P2-S04 Industry Primer | 판단형/분석형 하네스 | Industry Primer slice, QA result, pilot observation note |

Source Pack은 원자료를 수집하고 정리한다.
Industry Primer는 Source Pack과 웹/외부자료를 읽어 산업 구조를 설명하고 다음 단계 질문을 남긴다.

둘은 입력/출력, QA 기준, 승인 조건, 실패 모드가 다르다.

### 2.2 기존 S03 `harness/`는 Source Pack 전용이다

현재 S03 구조는 Source Pack을 기준으로 구성되어 있다.

```text
P2-S03-Source-Pack/
  harness/
    ORCHESTRATOR.md
    MANIFEST.md
    contracts/source-pack.contract.md
    procedures/source-pack-*.md
    schemas/source-pack-*.schema.md
    rubrics/source-pack-qa.rubric.md
```

여기에 `industry-primer-*` 파일을 추가하면 파일명은 겹치지 않더라도, 하나의 `ORCHESTRATOR.md`, 하나의 `MANIFEST.md`, 하나의 `AGENTS.md`가 두 하네스의 의미를 동시에 떠안게 된다.

이는 장기적으로 자연어 라우팅, 승인 조건, artifact 위치, QA 기준을 혼합시킬 위험이 있다.

### 2.3 단계별 하네스 폴더 구조가 더 자연스럽다

권장 상위 구조는 아래와 같다.

```text
C:\Users\frisa\Documents\Investment-Research-OS\
  P2-S03-Source-Pack\
    ...Source Pack 하네스...

  P2-S04-Industry-Primer\
    ...Industry Primer 하네스...
```

P2-S04는 S03의 하위 기능이 아니라, 21단계 리서치 프로세스의 다음 독립 단계다.

## 3. Migration 전체 순서

| 순서 | 단계 | 실행 주체 | 실제 파일 변경 |
|---|---|---|---|
| 1 | migration plan 작성 | Codex | S03에 이 plan note만 추가 |
| 2 | migration plan Claude Code 교차검증 | Claude Code | 없음 |
| 3 | S03 handoff + work-map freeze | Codex | S03 work-map 최종 갱신, S03 handoff note 작성 |
| 4 | S04 폴더 생성 + git init | 사용자 직접 실행 Gate | S04 폴더와 `.git/` 생성 |
| 5 | S03 -> S04 파일 복사 | 사용자 또는 Codex | S04로 reference/template 사본 복사 |
| 6 | S04 bootstrap 작성 | Codex | S04 `README.md`, `AGENTS.md`, `CLAUDE.md`, 초기 폴더 안내 작성 |
| 6.5 | S04 bootstrap Claude Code 교차검증 | Claude Code | 필요 시 bootstrap 수정 |
| 7 | Industry Primer 하네스 파일 생성 | Codex | S04 `harness/`, `.agents/`, `.claude/`, `artifacts/` 생성 |

Step 4는 사용자 직접 실행 Gate다.
새 프로젝트 루트 생성과 git 초기화는 단순 파일 편집이 아니라 작업 경계 결정이므로 AI가 묵시적으로 수행하지 않는다.

## 4. S03에 남길 것

아래 항목은 `P2-S03-Source-Pack`에 남긴다.

| 항목 | 처리 | 이유 |
|---|---|---|
| S03 `harness/` | S03에 유지 | Source Pack 전용 공통 원장 |
| S03 `.agents/` | S03에 유지 | Source Pack Codex adapter |
| S03 `.claude/` | S03에 유지 | Source Pack Claude adapter |
| S03 `artifacts/` | S03에 유지 | Source Pack raw/catalog/company index 산출물 |
| S03 `AGENTS.md` | S03에 유지 | Source Pack 자연어 라우팅 |
| S03 `CLAUDE.md` | S03에 유지 | Source Pack Claude Code 프로젝트 안내 |
| S03 `docs/design`, `docs/pilots`, `docs/current`, `docs/handoff` 기존 내용 | S03에 유지 | Source Pack 설계/실행/인계 기록 |
| S03 `docs/session-checkpoints` | S03에 유지, S04로 복사하지 않음 | checkpoint anchor와 과거 세션 상태가 S04 저장 흐름을 오염시킬 수 있음 |

S03은 migration 이후에도 Source Pack 하네스이며, Source Pack 자료 수집과 catalog 관리의 canonical 실행 위치다.

## 5. S04로 복사할 것

아래 항목은 S04 bootstrap 전에 복사한다.

| S03 원본 | S04 대상 | 이유 |
|---|---|---|
| `docs/reference/` | `P2-S04-Industry-Primer/docs/reference/` | 21단계 구조, Step 4 원본 템플릿, Step 5-7 handoff calibration 자료 |
| `docs/templates/global-harness-candidates/` | `P2-S04-Industry-Primer/docs/templates/global-harness-candidates/` | Phase 7-B Industry Primer 설계, blueprint, risk review, migration plan의 active working copy |
| `docs/templates/공용_하네스_템플릿_체크리스트_v4.md` | `P2-S04-Industry-Primer/docs/templates/공용_하네스_템플릿_체크리스트_v4.md` | v5 후보 이전 baseline reference |

### 5.1 `global-harness-candidates` 소유권 표현

S04로 복사된 `global-harness-candidates`는 Phase 7-B Industry Primer build/pilot을 위한 active working copy다.

이 말은 아래를 뜻한다.

- S04에서 Industry Primer build/pilot을 진행하는 동안 S04 copy를 업데이트한다.
- S03 copy는 migration 시점의 설계 스냅샷으로 남긴다.
- Phase 7-C에서 전역 template canonical 위치는 별도로 결정한다.
- S04 copy가 영구 전역 canonical source라고 확정하지 않는다.

## 6. S04로 복사하지 않을 것

아래 항목은 S04로 복사하지 않는다.

| 항목 | 제외 이유 |
|---|---|
| `docs/session-checkpoints/` | 대화 checkpoint anchor와 과거 저장 흐름이 새 프로젝트를 오염시킬 수 있음 |
| S03 `harness/` | Source Pack 전용 원장. Industry Primer 하네스는 S04에서 새로 작성 |
| S03 `artifacts/` | Source Pack 산출물은 복사하지 않고 경로로 참조 |
| S03 `.agents/` | Source Pack adapter. Industry Primer adapter는 S04에서 새로 작성 |
| S03 `.claude/` | Source Pack adapter. Industry Primer adapter는 S04에서 새로 작성 |
| S03 `AGENTS.md` | Source Pack 프로젝트 안내. S04용으로 새로 작성 |
| S03 `CLAUDE.md` | Source Pack 프로젝트 안내. S04용으로 새로 작성 |
| S03 `docs/design/`, `docs/pilots/`, `docs/current/` | Source Pack 세부 설계/실행 기록. 필요하면 S04에서 상대 경로로 참조 |
| S03 raw/source files | Source Pack 원자료는 S03에 보관하고 S04에서 읽기 입력으로 참조 |

## 7. Source Pack 입력 참조 방식

S04는 Source Pack 산출물을 복사하지 않는다.

대신 아래 상대 경로 원칙을 사용한다.

```text
../P2-S03-Source-Pack/artifacts/companies/{TICKER}/index.md
../P2-S03-Source-Pack/artifacts/raw/...
../P2-S03-Source-Pack/artifacts/derived/...
../P2-S03-Source-Pack/artifacts/catalog/...
```

APP/adtech first slice의 기본 Source Pack 입력은 아래 후보에서 시작한다.

```text
../P2-S03-Source-Pack/artifacts/companies/APP/index.md
../P2-S03-Source-Pack/artifacts/raw/company-ir/APP/...
../P2-S03-Source-Pack/artifacts/raw/sec-edgar/...
```

정확한 입력 파일은 S04의 Industry Primer runbook preflight 단계에서 다시 확인한다.

Source Pack top-up은 S04에서 조용히 실행하지 않는다.
필요하면 approval-gate를 통해 S03 Source Pack 하네스에서 별도 작업으로 수행한다.

## 8. S03 work-map freeze와 handoff

Migration plan이 Claude Code 교차검증 PASS를 받은 뒤, 실제 migration 직전에 S03에 마지막 흔적을 남긴다.

### 8.1 S03 work-map 처리

S03 work-map은 migration 시점에 마지막으로 갱신한 뒤 freeze한다.

갱신 내용:

- Phase 7-B Industry Primer build/pilot은 `P2-S04-Industry-Primer`로 이전한다고 명시한다.
- S03에서 남은 Industry Primer todo를 계속 진행하지 않는다고 명시한다.
- 이후 Phase 7-B Industry Primer 하네스 생성, pilot 실행, observation, 후속 work-map 관리는 S04 work-map에서 추적한다고 명시한다.
- S03 work-map은 Source Pack 하네스와 Global Harness v5 설계 스냅샷으로 보존한다고 명시한다.

주의:

- S03 work-map을 아무 표시 없이 freeze하지 않는다.
- 현재 남아 있는 `실제 Industry Primer first slice 하네스 파일 생성 여부 사용자 승인 게이트` todo는 S04 이전 결정으로 처리되어야 한다.

### 8.2 S03 handoff note

S03에는 아래 handoff note를 남긴다.

```text
docs/handoff/s04-migration-handoff-2026-06-11.md
```

권장 내용:

```md
# S04 Migration Handoff

- 작성일: 2026-06-11
- 대상: Phase 7-B Industry Primer first slice build/pilot

Phase 7-B Industry Primer 작업은 `P2-S03-Source-Pack` 내부에서 계속 진행하지 않고,
`C:\Users\frisa\Documents\Investment-Research-OS\P2-S04-Industry-Primer`
독립 하네스로 이전한다.

S03은 Source Pack 하네스와 Global Harness v5 설계 스냅샷을 보존한다.
이후 Industry Primer 하네스 생성, first slice pilot, observation, Phase 7-B 남은 작업 추적은 S04 work-map에서 수행한다.
```

이 handoff note는 S03을 다시 열었을 때 왜 Industry Primer build가 S03에서 멈췄는지 설명하는 표지판이다.

## 9. S04에서 새로 만들 것

S04에는 아래 파일과 폴더를 새로 만든다.

### 9.1 Bootstrap 단계에서 만들 것

```text
P2-S04-Industry-Primer/
  README.md
  AGENTS.md
  CLAUDE.md
  docs/
    reference/
    templates/
      global-harness-candidates/
      공용_하네스_템플릿_체크리스트_v4.md
  artifacts/
    README.md
    runs/
```

### 9.2 Industry Primer 하네스 생성 단계에서 만들 것

```text
P2-S04-Industry-Primer/
  harness/
    ORCHESTRATOR.md
    MANIFEST.md
    contracts/
      industry-primer.contract.md
    procedures/
      industry-primer-runbook.md
      industry-primer-slice.md
      industry-primer-qa.md
    schemas/
      industry-primer-slice.schema.md
      industry-primer-qa.schema.md
    rubrics/
      industry-primer-qa-rubric.md
  .agents/
    skills/
      industry-primer-orchestrator/
        SKILL.md
  .claude/
    skills/
      industry-primer-orchestrator/
        SKILL.md
```

`industry-primer-orchestrator`는 migration plan 작성 시점에는 아직 존재하지 않는다.
따라서 S04 `AGENTS.md`와 `CLAUDE.md` 초안에서는 이 adapter가 생성 예정임을 명시한다.

## 10. S04 git repo 구조 추천

추천: S04를 독립 git repo로 초기화한다.

```text
C:\Users\frisa\Documents\Investment-Research-OS\P2-S04-Industry-Primer\.git
```

이유:

- S03과 S04는 서로 다른 하네스다.
- Source Pack 수집 산출물과 Industry Primer 분석 산출물의 변경 이력을 분리할 수 있다.
- 나중에 S05/S06이 생겨도 각 단계 하네스의 변경 이력을 독립적으로 관리할 수 있다.
- 상위 mono-repo 결정은 전체 Investment Research OS 구조가 안정된 뒤 별도로 판단해도 늦지 않다.

대안:

| 옵션 | 판단 |
|---|---|
| S04 독립 git repo | 권장 |
| Investment-Research-OS 상위 mono-repo | 지금은 보류. 전체 OS 관리 정책이 아직 없음 |
| git 없이 시작 | 비추천. 하네스 파일 생성과 pilot 변경 추적이 중요함 |

## 11. S04 AGENTS.md 초안

아래는 S04 `AGENTS.md` 초안이다.

주의:

- 이 초안은 S04 bootstrap 단계에서 실제 파일로 작성한다.
- `industry-primer-orchestrator`는 아직 생성 전인 예정 adapter다.
- Source Pack adapter 이름이나 라우팅 규칙을 복사하지 않는다.

```md
# Industry Primer Harness

이 프로젝트는 가치투자 리서치 21단계 중 P2-S04 Industry Primer 하네스입니다.

Industry Primer는 Source Pack이 만든 원자료 지도와 외부 자료를 입력으로 읽어,
산업 정의, 산업 참여자 구조, 핵심 용어, 다음 단계 질문을 작성하는 판단형/분석형 하네스입니다.

Industry Primer는 투자 결론, valuation, moat, competition 결론을 미리 내리지 않습니다.
후속 Value Chain, Business Model, Market Share, Competition, Moat 단계가 사용할 수 있는 안전한 산업 맥락과 handoff 질문을 남깁니다.

## 자연어 라우팅

아래 요청이 오면 `industry-primer-orchestrator`를 먼저 사용합니다.

주의: `industry-primer-orchestrator`는 S04 하네스 생성 단계에서 만들어질 예정 adapter입니다.
adapter가 아직 생성되지 않은 bootstrap 단계에서는 `harness/procedures/industry-primer-runbook.md`와 blueprint 문서를 기준으로 수동 진행합니다.

| 요청 유형 | 예시 표현 |
|---|---|
| 새 실행 | "Industry Primer 작성해줘", "{티커} 산업 primer 만들어줘" |
| first slice 실행 | "APP/adtech first slice pilot 실행해줘" |
| QA | "Industry Primer QA 해줘", "source gap과 handoff 질문 검토해줘" |
| 보완 | "Industry Primer 보완해줘", "Section 3 참여자 구조만 다시 봐줘" |
| 관찰 | "pilot observation note 작성해줘", "이번 실행에서 하네스 문제를 기록해줘" |

## 주요 입력

Source Pack 입력은 복사하지 않고 아래 S03 경로에서 참조합니다.

```text
../P2-S03-Source-Pack/artifacts/companies/{TICKER}/index.md
../P2-S03-Source-Pack/artifacts/raw/...
../P2-S03-Source-Pack/artifacts/catalog/...
```

APP first slice의 기본 입력은 `../P2-S03-Source-Pack/artifacts/companies/APP/index.md`에서 시작합니다.

## 기준 문서

S04의 Industry Primer build/pilot은 아래 문서를 우선 확인합니다.

| 문서 | 역할 |
|---|---|
| `docs/templates/global-harness-candidates/global-harness-v5-work-map.md` | S04 active work-map |
| `docs/templates/global-harness-candidates/global-harness-v5-phase7b-industry-primer-blueprint-v0-2026-06-09.md` | Industry Primer 하네스 설계도 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase7b-industry-primer-pilot-plan-note-v1-2026-06-08.md` | APP/adtech first slice 실행 계획 |
| `docs/templates/global-harness-candidates/global-harness-v5-phase7b-industry-primer-pre-build-risk-review-note-2026-06-09.md` | challenge review와 risk-control 기준 |
| `docs/reference/Phase 2 - Step 4 Industry Primer 템플릿.md` | 원본 Step 4 템플릿 |

## 수정 원칙

- 업무 의미 변경은 `harness/`를 수정합니다.
- Codex 실행 방식 변경은 `.agents/`를 수정합니다.
- Claude Code 실행 방식 변경은 `.claude/`를 수정합니다.
- `AGENTS.md`를 바꾸면 `CLAUDE.md`에도 같은 구조 안내를 반영합니다.
- Source Pack raw 자료는 S04로 복사하지 않습니다.
- Source Pack top-up은 S04에서 조용히 실행하지 않고, 사용자 승인 후 S03 Source Pack 하네스에서 별도 수행합니다.

## 산출물 위치

Industry Primer 실행 산출물은 S04 안에 남깁니다.

```text
artifacts/runs/{run-id}/industry-primer-slice.md
artifacts/runs/{run-id}/qa.md
artifacts/runs/{run-id}/pilot-observation-note.md
```
```

## 12. S04 CLAUDE.md 초안

아래는 S04 `CLAUDE.md` 초안이다.

주의:

- 이 초안은 S04 bootstrap 단계에서 실제 파일로 작성한다.
- `industry-primer-orchestrator`는 아직 생성 전인 예정 adapter다.
- Source Pack adapter 이름이나 라우팅 규칙을 복사하지 않는다.

```md
# Industry Primer Harness

이 프로젝트는 가치투자 리서치 21단계 중 P2-S04 Industry Primer 하네스입니다.

Claude Code는 이 폴더를 Source Pack 수집 하네스로 취급하지 않습니다.
이 폴더의 목적은 Phase 2 Step 4 Industry Primer를 독립 하네스로 실행하고 검증하는 것입니다.

## 역할

Industry Primer는 Source Pack 원자료와 외부 자료를 읽어 아래 first slice를 작성합니다.

| 섹션 | 역할 |
|---|---|
| Section 1. 산업 한 줄 정의 | 산업 경계와 해결 문제를 짧게 정의 |
| Section 3. 산업 참여자 구조 | 참여자 유형과 역할 지도 작성 |
| Section 5. 핵심 용어 정리 | 다음 단계가 이해해야 할 핵심 용어 정리 |
| Section 13. 다음 단계 질문 | 후속 분석이 바로 사용할 수 있는 질문 작성 |

Industry Primer는 Value Chain, Business Model, Market Share, Competition, Moat, Valuation, 투자 판단 결론을 미리 내리지 않습니다.

## 자연어 라우팅

Industry Primer 실행, QA, 보완, observation 요청은 `industry-primer-orchestrator`를 우선 사용합니다.

주의: `industry-primer-orchestrator`는 S04 하네스 생성 단계에서 만들어질 예정 adapter입니다.
adapter 생성 전에는 `docs/templates/global-harness-candidates/`의 blueprint와 `harness/` 후보 구조를 기준으로 수동 진행합니다.

## Source Pack 입력 참조

Source Pack 산출물은 S04로 복사하지 않습니다.
필요한 Source Pack 입력은 상대 경로로 읽습니다.

```text
../P2-S03-Source-Pack/artifacts/companies/{TICKER}/index.md
../P2-S03-Source-Pack/artifacts/raw/...
../P2-S03-Source-Pack/artifacts/catalog/...
```

APP/adtech first slice에서는 `../P2-S03-Source-Pack/artifacts/companies/APP/index.md`를 preflight 시작점으로 둡니다.

## 기준 문서

작업 전 아래 문서를 확인합니다.

1. `docs/templates/global-harness-candidates/global-harness-v5-work-map.md`
2. `docs/templates/global-harness-candidates/global-harness-v5-phase7b-industry-primer-blueprint-v0-2026-06-09.md`
3. `docs/templates/global-harness-candidates/global-harness-v5-phase7b-industry-primer-pilot-plan-note-v1-2026-06-08.md`
4. `docs/templates/global-harness-candidates/global-harness-v5-phase7b-industry-primer-pre-build-risk-review-note-2026-06-09.md`
5. `docs/reference/Phase 2 - Step 4 Industry Primer 템플릿.md`

## 승인 원칙

- Source Pack top-up은 사용자 승인 없이 수행하지 않습니다.
- first slice 범위 확장은 사용자 승인 없이 수행하지 않습니다.
- 실제 하네스 구조 변경은 사용자 승인 없이 수행하지 않습니다.
- User Review Required Claims는 QA 상태값이 아니라 사용자 승인 게이트의 입력 정보입니다.

## 산출물

Industry Primer 실행 산출물은 S04 `artifacts/runs/{run-id}/` 아래에 작성합니다.

필수 산출물:

- `industry-primer-slice.md`
- `qa.md`
- `pilot-observation-note.md`
```

## 13. S04 생성 후 Industry Primer 하네스 파일 생성 순서

S04 bootstrap과 Claude Code 교차검증이 끝난 뒤, 아래 순서로 실제 하네스 파일을 생성한다.

| 순서 | 생성/수정 | 설명 |
|---|---|---|
| 1 | `harness/ORCHESTRATOR.md` | S04 Industry Primer 전체 실행 모드와 승인 조건 |
| 2 | `harness/MANIFEST.md` | Industry Primer 하네스 파일 지도 |
| 3 | `harness/contracts/industry-primer.contract.md` | 목표, 입력, 출력, 금지 영역, 완료 기준 |
| 4 | `harness/procedures/industry-primer-runbook.md` | preflight -> slice -> QA -> challenge review -> observation |
| 5 | `harness/procedures/industry-primer-slice.md` | Section 1/3/5/13 작성 규칙 |
| 6 | `harness/procedures/industry-primer-qa.md` | 5층 QA, URRC, Additional Gray-Zone Claims, comparison trigger |
| 7 | `harness/schemas/industry-primer-slice.schema.md` | slice output, `source_register`, Section 13 schema |
| 8 | `harness/schemas/industry-primer-qa.schema.md` | `qa.md` 8섹션 format |
| 9 | `harness/rubrics/industry-primer-qa-rubric.md` | 판단형 rubric |
| 10 | `.agents/skills/industry-primer-orchestrator/SKILL.md` | Codex adapter |
| 11 | `.claude/skills/industry-primer-orchestrator/SKILL.md` | Claude Code adapter |
| 12 | `artifacts/README.md` | Industry Primer 산출물 위치 안내 |

실제 APP/adtech first slice 산출물은 이 파일 생성과 검증이 끝난 뒤 별도 실행 단계에서 작성한다.

## 14. 사용자 실행용 PowerShell 명령어

아래 명령어는 plan에 기록하기 위한 사용자 실행용 예시다.
이 plan 작성 단계에서는 실행하지 않는다.

### 14.1 Step 4: S04 폴더 생성과 git init

사용자가 직접 실행하는 Gate다.

```powershell
$Root = "C:\Users\frisa\Documents\Investment-Research-OS"
$S04 = Join-Path $Root "P2-S04-Industry-Primer"

if (Test-Path -LiteralPath $S04) {
  throw "Target already exists: $S04. Stop and inspect before continuing."
}

New-Item -ItemType Directory -Path $S04 | Out-Null
Set-Location -LiteralPath $S04
git init
```

### 14.2 Step 5: S03 -> S04 파일 복사

아래 명령어는 S04 폴더가 이미 존재하고 git init이 끝난 뒤 실행한다.

덮어쓰기 주의:

- 기본 명령어는 대상 폴더/파일이 이미 있으면 중단한다.
- 재실행이 필요하면 기존 대상 폴더를 사용자가 직접 확인한 뒤 삭제/백업하고 다시 실행한다.
- `docs/session-checkpoints`, S03 `harness/`, S03 `artifacts/`, S03 `.agents/`, S03 `.claude/`는 복사하지 않는다.

```powershell
$Root = "C:\Users\frisa\Documents\Investment-Research-OS"
$S03 = Join-Path $Root "P2-S03-Source-Pack"
$S04 = Join-Path $Root "P2-S04-Industry-Primer"

if (-not (Test-Path -LiteralPath $S04)) {
  throw "S04 folder does not exist. Create it first: $S04"
}

$Targets = @(
  (Join-Path $S04 "docs\reference"),
  (Join-Path $S04 "docs\templates\global-harness-candidates"),
  (Join-Path $S04 "docs\templates\공용_하네스_템플릿_체크리스트_v4.md")
)

foreach ($Target in $Targets) {
  if (Test-Path -LiteralPath $Target) {
    throw "Target already exists: $Target. Stop and inspect before copying."
  }
}

New-Item -ItemType Directory -Force -Path (Join-Path $S04 "docs") | Out-Null
New-Item -ItemType Directory -Force -Path (Join-Path $S04 "docs\templates") | Out-Null

Copy-Item -LiteralPath (Join-Path $S03 "docs\reference") `
  -Destination (Join-Path $S04 "docs\reference") `
  -Recurse

Copy-Item -LiteralPath (Join-Path $S03 "docs\templates\global-harness-candidates") `
  -Destination (Join-Path $S04 "docs\templates\global-harness-candidates") `
  -Recurse

Copy-Item -LiteralPath (Join-Path $S03 "docs\templates\공용_하네스_템플릿_체크리스트_v4.md") `
  -Destination (Join-Path $S04 "docs\templates\공용_하네스_템플릿_체크리스트_v4.md")

Write-Host "S04 reference/template copy completed."
Write-Host "Excluded: docs/session-checkpoints, harness, artifacts, .agents, .claude, AGENTS.md, CLAUDE.md"
```

## 15. Migration plan Claude Code 교차검증 요청 기준

Claude Code에는 아래 항목을 확인하게 한다.

| 검증 항목 | 확인 질문 |
|---|---|
| P2-S04 분리 필요성 | Source Pack 하네스 안에 Industry Primer를 만들지 않는 이유가 충분한가 |
| 전체 순서 | plan -> 검증 -> S03 handoff/freeze -> 사용자 Gate -> 복사 -> bootstrap -> 검증 -> 하네스 생성 순서가 명확한가 |
| S03/S04 경계 | S03에 남길 것, S04로 복사할 것, 복사하지 않을 것이 명확한가 |
| active working copy | S04 `global-harness-candidates`가 active working copy이며 전역 canonical이 아님을 명시했는가 |
| Source Pack 참조 | Source Pack 산출물은 복사하지 않고 상대 경로로 참조한다고 명시했는가 |
| work-map freeze | S03 work-map은 마지막 handoff/freeze 표시 후 S04 work-map으로 추적을 넘긴다고 명시했는가 |
| S03 handoff note | `docs/handoff/s04-migration-handoff-2026-06-11.md` 메모 필요성이 명시됐는가 |
| AGENTS 초안 | S04가 Industry Primer 하네스임을 선언하고, Source Pack 라우팅 흔적이 없는가 |
| CLAUDE 초안 | Claude Code가 S04를 Source Pack으로 오해하지 않도록 충분히 안내하는가 |
| adapter 예정 상태 | `industry-primer-orchestrator`가 아직 생성 전이며 예정 adapter라고 명시했는가 |
| PowerShell 명령어 | 대상 경로 존재 확인, 하위 폴더 생성, 덮어쓰기 중단, 제외 목록이 포함됐는가 |
| 제외 목록 | `docs/session-checkpoints`, S03 `harness/`, `artifacts/`, `.agents/`, `.claude/`가 복사 명령어에 포함되지 않았는가 |
| 과잉 실행 방지 | 이 plan이 실제 S04 생성, 파일 복사, handoff 작성, work-map freeze를 수행하지 않는다는 점이 명확한가 |

## 16. 현재 판정

```text
P2-S04 migration/bootstrap plan 작성 단계.
아직 실제 P2-S04 폴더 생성 전.
아직 S03 work-map freeze 전.
아직 S03 handoff note 작성 전.
아직 S03 -> S04 파일 복사 전.
아직 Industry Primer 하네스 파일 생성 전.
```

다음 단계:

1. 이 migration plan을 Claude Code에 교차검증한다.
2. PASS 후 S03 handoff + work-map freeze를 수행한다.
3. 사용자가 직접 P2-S04 폴더 생성 + git init Gate를 수행한다.
4. 이후 migration copy와 S04 bootstrap으로 넘어간다.
