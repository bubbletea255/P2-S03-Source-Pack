# Source Pack v2

이 프로젝트는 가치투자 리서치 21단계 중 P2-S03 Source Pack 하네스입니다.
Claude Code와 Codex 양쪽에서 동일한 `harness/` 공통 원장을 따릅니다.

Source Pack은 분석 리포트를 만들지 않습니다.
SEC 공시, IR 자료, 가능한 경우 transcript 원문을 수집해 raw 파일과 catalog 원장으로 정리하고, 다음 하네스가 입력으로 읽을 수 있게 만드는 수집형 하네스입니다.

## 자연어 라우팅

아래 요청이 오면 `source-pack-orchestrator`를 먼저 실행합니다.

| 요청 유형 | 예시 표현 |
|---|---|
| 새 실행 | "source pack 만들어줘", "{티커} 공시 자료 수집해줘" |
| 업데이트 | "source pack 업데이트해줘", "관심 종목 source pack 업데이트해줘" |
| 부분 재검토 | "{티커} IR 자료만 다시 확인해줘", "Transcript만 보완해줘" |
| 테스트 실행 | "{티커} 1건으로 테스트해줘", "AAPL 최신 10-K 하나로 수집 테스트해줘" |
| 교차 검증 | "Claude Codex 비교해줘", "두 결과 비교해줘" |

`source-pack-orchestrator`는 사용자 요청을 받는 adapter 진입점 이름이다.
실제 1회 실행 절차는 `harness/procedures/source-pack-runbook.md`를 따른다.

## 하네스 구조

| 영역 | 위치 | 역할 |
|---|---|---|
| 공통 원장 | `harness/ORCHESTRATOR.md` | 전체 실행 모드, 승인 조건, 중단 조건 |
| 파일 지도 | `harness/MANIFEST.md` | 파일 역할과 의존 관계 |
| 계약 | `harness/contracts/source-pack.contract.md` | 목표, 입력, 출력, 완료 기준 |
| 실행 절차 | `harness/procedures/source-pack-runbook.md` | 티커 선택, 설정 확인, Collector/QA 조율 |
| 수집 절차 | `harness/procedures/source-pack-collector.md` | 원자료 다운로드와 catalog 갱신 |
| QA 절차 | `harness/procedures/source-pack-qa.md` | catalog, raw 파일, index 검증 |
| 출력 형식 | `harness/schemas/` | catalog, index.md, 실행 요약 schema |
| 평가 기준 | `harness/rubrics/` | Source Pack QA rubric |
| Codex 진입점 | `.agents/skills/source-pack-orchestrator/SKILL.md` | Codex adapter |
| Claude 진입점 | `.claude/skills/source-pack-orchestrator/SKILL.md` | Claude adapter |
| 회사별 지도 | `artifacts/companies/{TICKER}/index.md` | 사람용 원자료 지도 |
| 기계용 원장 | `artifacts/catalog/*.jsonl` | 다음 하네스가 읽는 catalog |
| 실행 기록 | `artifacts/runs/{run-id}/` | download-log, run-summary, qa |
| 원자료 | `artifacts/raw/`, `artifacts/derived/` | git 제외 대용량 원자료 |

## 수정 원칙

- 업무 의미 변경은 `harness/`를 수정합니다.
- Claude 실행 방식 변경은 `.claude/`를 수정합니다.
- Codex 실행 방식 변경은 `.agents/` 또는 `.codex/`를 수정합니다.
- `AGENTS.md`를 바꾸면 `CLAUDE.md`에도 같은 구조 안내를 반영합니다.
- 공통 업무 규칙을 adapter에 길게 복사하지 않습니다.

## 주요 입력 파일

| 파일 | 역할 |
|---|---|
| `watchlist.md` | 관심 종목 티커 목록 |
| `config.md` | 수집 범위, 속도 제한, SEC User-Agent |
| `artifacts/README.md` | 산출물 지도 |
| `artifacts/improvement-log.md` | 개선 기록 |

## 변경 이력

| 날짜 | 변경 내용 |
|---|---|
| 2026-05-29 | Source Pack v1 최초 구성 |
| 2026-06-02 | Source Pack v2 공통 원장 구조로 전환 |
| 2026-06-02 | Source Pack v3 하네스 유형 분류 체계 도입, 수집형 품질 축 명세, 비교 모드 원칙 추가, 공용 템플릿 v4 생성 |
| 2026-06-02 | raw/catalog 구조와 `source-pack-runbook.md` 실행 절차 도입 |
| 2026-06-03 | Source Pack v4 runbook/adapter 분리 완성, collector/qa/rubric 전면 재작성, catalog schema 신규 |
| 2026-06-03 | Source Pack v5 legacy link-only 폴더(AAPL/U) 삭제, 문서 금지 문구 정리 — 구조 변경 마무리 |
| 2026-06-03 | Source Pack v6 test_collection 공식 실행 모드 추가, run_scope 필드 도입 — AAPL 파이프라인 테스트 준비 |
| 2026-06-03 | Source Pack v7 AAPL test_collection 실행 산출물, files.jsonl composite key, upsert/append 구분, QA 14단계 완성 |
| 2026-06-03 | Source Pack v8 incremental sync 설계 완성, runs.jsonl delta 필드 교체(collected_new/skipped_existing/repair_required/failed/files_collected_new), AAPL SEC 소규모 확장 테스트 산출물(10-Q/DEF 14A/8-K primary 3건 추가, skipped_existing 검증) |
| 2026-06-03 | Source Pack v9 SEC raw preflight 필수화(환경 무관, new_document/retry_eligible 1건 이상 시), qa.md 출력 템플릿 14단계 4열 형식으로 교체, exhibit/repair 판단 원칙 6개 추가, AAPL SEC Tier 1 전체 범위 수집 완료(39문서/51파일), repair_required 전체 사이클 검증(감지→복구→정상복귀) |
| 2026-06-03 | Source Pack v10 AAPL SEC Tier 1 수집 완료 — 전체 범위 idempotency 통과(skipped_existing=39, download-log 0건), 9단계 테스트 시퀀스 완성 |
| 2026-06-03 | Source Pack v11 장애 진단 순서 보강 — source-pack-runbook.md에 실행 후 사후 진단 섹션 신규 추가(보는 파일 순서 5단계, 상태값 의미·위치·행동 테이블) |
| 2026-06-04 | Source Pack v12 SEC 수집 안정화 완료 롤백 지점 — .gitignore 보안 강화(credentials/대화기록 제외), 대화 파일 git 추적 해제, docs 보강(observability template/handoff/IR 수집 기준 PDF) |
