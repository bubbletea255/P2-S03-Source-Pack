---
name: source-pack-orchestrator
description: Source Pack v2 하네스의 Claude Code 전체 실행 진입점. "source pack 만들어줘", "source pack 실행해줘", "공시 자료 수집해줘", "관심 종목 업데이트", "{티커} 자료 정리해줘", "Claude Codex 비교해줘" 같은 요청에서 사용한다. 공통 원장인 harness/ORCHESTRATOR.md, MANIFEST, source-pack-runbook을 따른다.
---

# Source Pack Orchestrator Adapter

Claude Code 환경에서 Source Pack v2 하네스를 시작할 때 사용하는 진입점이다.
이 Skill의 이름은 사용자 라우팅용으로 유지한다. 실제 실행 절차는 `harness/procedures/source-pack-runbook.md`를 따른다.

## 시작 전 필독

아래 파일을 순서대로 읽는다.

1. `harness/ORCHESTRATOR.md`
2. `harness/MANIFEST.md`
3. `harness/contracts/source-pack.contract.md`
4. `harness/procedures/source-pack-runbook.md`
5. `harness/schemas/source-pack-catalog.schema.md`
6. `harness/schemas/source-pack-index.schema.md`
7. `harness/schemas/source-pack-run-summary.schema.md`
8. `artifacts/README.md`

## 실행 절차

1. 사용자의 요청이 새 실행, 증분 업데이트, 부분 재검토, 테스트 실행, 비교 모드 중 무엇인지 판단한다.
2. `harness/ORCHESTRATOR.md`와 `harness/procedures/source-pack-runbook.md`의 실행 흐름을 따른다.
3. 티커별 수집은 `source-pack-collector` Agent를 사용한다.
4. 각 티커 완료 후 `harness/procedures/source-pack-qa.md` 기준으로 산출물을 점검한다.
5. 최종 요약은 `harness/schemas/source-pack-run-summary.schema.md` 형식으로 보고한다.

## Claude adapter 규칙

- 공통 업무 규칙은 `harness/`를 단일 원본으로 따른다.
- 이 Skill에는 세부 수집 규칙을 길게 복사하지 않는다.
- 이 Skill에서 `.agents/` 또는 `.codex/`를 수정하지 않는다.
- 공통 원장 수정이 필요하면 먼저 사용자에게 변경 범위를 요약한다.

## 출력

- 회사별 지도: `artifacts/companies/{TICKER}/index.md`
- 기계용 원장: `artifacts/catalog/*.jsonl`
- 실행 기록: `artifacts/runs/{run-id}/`
- 조건부 원자료: `artifacts/raw/...`, `artifacts/derived/text/...`
- 필요한 경우 `artifacts/README.md`, `artifacts/improvement-log.md`
