# S04 Migration Handoff

- 작성일: 2026-06-11
- 대상: Phase 7-B Industry Primer first slice build/pilot
- 상태: S03 handoff/freeze 기록. 이후 Industry Primer build/pilot active tracking은 S04에서 수행.

## 1. Handoff 결론

Phase 7-B Industry Primer 작업은 `P2-S03-Source-Pack` 내부에서 실제 하네스 파일을 생성하지 않는다.

이후 Industry Primer first slice 하네스 생성, bootstrap, pilot 실행, observation, 후속 work-map 관리는 아래 독립 폴더에서 이어간다.

```text
C:\Users\frisa\Documents\Investment-Research-OS\P2-S04-Industry-Primer
```

S03은 Source Pack 하네스와 Global Harness v5 설계 스냅샷을 보존하는 위치로 남긴다.

## 2. 기준 문서

이번 이전의 직접 기준 문서는 아래 plan note다.

```text
docs/templates/global-harness-candidates/global-harness-v5-phase7b-s04-migration-plan-2026-06-11.md
```

이 plan은 Claude Code 교차검증 PASS를 받았다.

## 3. S03에서 완료된 것

S03에서 완료된 작업:

- Industry Primer design principles note 작성 및 Claude Code PASS
- Industry Primer pilot plan note v1 작성 및 Claude Code PASS
- blueprint-prep note와 Section 11 consensus note 작성 및 Claude Code PASS
- first slice rubric calibration note 작성 및 Claude Code PASS
- Industry Primer blueprint v0 작성, pre-build risk review 반영, Claude Code 재교차검증 PASS
- S04 migration/bootstrap plan 작성 및 Claude Code PASS
- S03 work-map에 S04 이전 결정과 handoff/freeze 상태 반영

## 4. S03에서 더 진행하지 않을 것

아래 작업은 S03에서 더 진행하지 않는다.

- Industry Primer actual harness file 생성
- `harness/contracts/industry-primer.contract.md` 생성
- `harness/procedures/industry-primer-*.md` 생성
- `harness/schemas/industry-primer-*.schema.md` 생성
- `harness/rubrics/industry-primer-qa-rubric.md` 생성
- `.agents/skills/industry-primer-orchestrator/` 생성
- `.claude/skills/industry-primer-orchestrator/` 생성
- Industry Primer `artifacts/runs/{run-id}/` 실행 산출물 생성
- APP/adtech first slice 실행

위 작업들은 S04 bootstrap 이후 S04 work-map에서 추적한다.

## 5. S04로 넘기는 다음 작업

S04에서 이어갈 순서:

1. 사용자가 `P2-S04-Industry-Primer` 폴더를 직접 생성한다.
2. 사용자가 S04에서 독립 `git init` Gate를 수행한다.
3. migration plan의 PowerShell 명령어 또는 동등한 절차로 S03에서 S04로 reference/template 파일을 복사한다.
4. S04 `README.md`, `AGENTS.md`, `CLAUDE.md`, `artifacts/README.md` bootstrap 파일을 작성한다.
5. S04 bootstrap 파일을 Claude Code에 교차검증한다.
6. Industry Primer 하네스 파일을 S04에 생성한다.
7. 생성된 하네스 파일을 Claude Code에 교차검증한다.
8. APP/adtech first slice pilot을 실행한다.

## 6. S04로 넘길 TODO

아래 TODO는 S04 work-map에 반영해야 한다.

| TODO | 이유 |
|---|---|
| Step 7 하네스 파일 생성 후 S04 `CLAUDE.md`에 `harness/` 구조 섹션 추가 | bootstrap 시점에는 `harness/`가 아직 없으므로, 실제 하네스 파일 생성 후 Claude Code가 읽을 구조 지도를 보강해야 함 |
| S04 `global-harness-candidates`는 active working copy로 취급 | Phase 7-C 전역 canonical 위치는 아직 결정되지 않았기 때문 |
| Source Pack 입력은 복사하지 않고 `../P2-S03-Source-Pack/artifacts/...`로 참조 | S03 Source Pack 산출물과 S04 Industry Primer 실행 산출물을 섞지 않기 위함 |
| Source Pack top-up은 S04에서 조용히 실행하지 않고 사용자 승인 후 S03 Source Pack 하네스에서 별도 수행 | 수집형 하네스와 판단형 하네스의 책임 경계를 지키기 위함 |

## 7. S03 보존 원칙

S03은 계속 Source Pack 하네스다.

S03에 남는 것:

- Source Pack `harness/`
- Source Pack `.agents/`, `.claude/`
- Source Pack `artifacts/`
- Source Pack `AGENTS.md`, `CLAUDE.md`
- Global Harness v5 설계 스냅샷

S03 `docs/templates/global-harness-candidates/`는 migration 시점의 설계 스냅샷으로 보존한다.
S04로 복사된 `global-harness-candidates`가 Phase 7-B Industry Primer build/pilot의 active working copy가 된다.

## 8. 현재 판정

```text
S03 Phase 7-B Industry Primer build/pilot handoff 완료.
S03에서 실제 Industry Primer 하네스 파일은 만들지 않음.
S04 폴더 생성은 아직 사용자 Gate 전.
S03 -> S04 파일 복사는 아직 실행 전.
Industry Primer first slice pilot은 아직 실행 전.
```
