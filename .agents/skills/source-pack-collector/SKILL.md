---
name: source-pack-collector
description: Source Pack v2의 Codex collector adapter. 미국 상장 주식 한 회사의 SEC 공시, IR 자료, 실적 발표 자료, transcript 후보 링크를 수집해 회사별 index.md를 만들거나 업데이트할 때 사용한다. 공통 원장인 harness/ 계약, 절차, schema, rubric을 따른다.
---

# Source Pack Collector Adapter

Codex 환경에서 Source Pack Collector를 실행하는 adapter다.

## 시작 전 필독

아래 파일을 순서대로 읽는다.

1. `harness/contracts/source-pack.contract.md`
2. `harness/procedures/source-pack-collector.md`
3. `harness/schemas/source-pack-index.schema.md`
4. `harness/procedures/source-pack-qa.md`
5. `harness/rubrics/source-pack-qa.rubric.md`

## 실행 원칙

- 공통 업무 규칙은 위 `harness/` 파일을 단일 원본으로 따른다.
- 이 Skill에는 공통 업무 규칙을 길게 복사하지 않는다.
- 이 Skill에서 `.claude/`를 수정하지 않는다.
- SEC/IR 요청은 속도 제한을 지키고 병렬화하지 않는다.

## 출력

- `artifacts/{TICKER}/phase2/step3-source-pack/index.md`
- 실행 요약은 `harness/schemas/source-pack-run-summary.schema.md`를 따른다.
