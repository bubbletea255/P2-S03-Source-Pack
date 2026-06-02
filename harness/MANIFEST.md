# Harness Manifest

이 문서는 Source Pack v2 하네스의 파일 역할과 입력/출력 의존 관계를 정리한다.

## 파일 역할 지도

| 영역 | 파일 | 역할 |
|---|---|---|
| 공통 흐름 | `harness/ORCHESTRATOR.md` | 전체 실행 모드, 승인 조건, 중단 조건 |
| 계약 | `harness/contracts/source-pack.contract.md` | 목표, 입력, 출력, 완료 기준, 금지사항 |
| 절차 | `harness/procedures/source-pack-runbook.md` | 티커 선택, 설정 확인, Collector/QA 실행 조율 |
| 절차 | `harness/procedures/source-pack-collector.md` | CIK 조회, SEC/IR/transcript 원자료 수집, catalog 갱신 |
| 절차 | `harness/procedures/source-pack-qa.md` | catalog, raw 파일, index, run 기록 검증 |
| Schema | `harness/schemas/source-pack-catalog.schema.md` | `catalog/*.jsonl`과 `download-log.jsonl` 형식 |
| Schema | `harness/schemas/source-pack-index.schema.md` | 회사별 `index.md` 출력 형식 |
| Schema | `harness/schemas/source-pack-run-summary.schema.md` | 실행 요약 출력 형식 |
| Rubric | `harness/rubrics/source-pack-qa.rubric.md` | 품질 평가 기준 |
| Codex adapter | `.agents/skills/source-pack-orchestrator/SKILL.md` | Codex 진입점 |
| Codex adapter | `.agents/skills/source-pack-collector/SKILL.md` | Codex 수집 adapter |
| Claude adapter | `.claude/skills/source-pack-orchestrator/SKILL.md` | Claude 진입점 |
| Claude adapter | `.claude/agents/source-pack-collector.md` | Claude 수집 adapter |
| 설정 | `config.md` | 수집 범위, 속도 제한, SEC User-Agent |
| 입력 | `watchlist.md` | 관심 종목 목록 |
| 산출물 | `artifacts/companies/{TICKER}/index.md` | 회사별 사람용 원자료 지도 |
| 산출물 | `artifacts/catalog/*.jsonl` | 기계용 원장 |
| 산출물 | `artifacts/runs/{run-id}/` | 실행 기록, 다운로드 로그, QA |
| 산출물 | `artifacts/raw/`, `artifacts/derived/` | 원자료 파일과 추출 텍스트, git 제외 |

## 산출물 의존 관계

```text
watchlist.md + config.md + 사용자 요청
    ↓
harness/ORCHESTRATOR.md
    ↓
harness/procedures/source-pack-runbook.md
    ↓
티커 확정 및 실행 모드 판단
    ↓
CIK / 회사 메타데이터 확인
    ↓
SEC 공시 / IR / 가능한 경우 transcript 원문 수집
    ↓
artifacts/raw/... + artifacts/catalog/*.jsonl
    ↓
artifacts/companies/{TICKER}/index.md
    ↓
artifacts/runs/{run-id}/qa.md
    ↓
다음 하네스: Industry Primer, Value Chain, Business Model, Market, Competition
```

## 다음 Phase 전달 계약

다음 하네스는 아래 순서로 Source Pack 산출물을 읽는다.

1. `artifacts/companies/{TICKER}/index.md`
2. `artifacts/catalog/documents.jsonl`
3. `artifacts/catalog/files.jsonl`
4. 필요한 경우 `artifacts/raw/...`
5. 필요한 경우 `artifacts/derived/text/...`
6. `artifacts/runs/{run-id}/qa.md`

다음 하네스는 `catalog_status`가 `fail`, `unverified`, `[확인 필요:]`인 자료를 확정 입력으로 사용하지 않는다.

## 수정 경계

공통 업무 의미는 `harness/`에 둔다.
adapter는 실행 환경별 진입과 도구 사용 방식만 담는다.

`source-pack-orchestrator` adapter 이름은 유지한다.
실제 절차 파일은 `harness/procedures/source-pack-runbook.md`를 사용한다.
