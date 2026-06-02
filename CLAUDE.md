# Source Pack v2

이 프로젝트는 가치투자 리서치 21단계 중 Phase 2 Step 3, 즉 원자료 인덱스 구축 하네스입니다.
Claude Code와 Codex 양쪽에서 동일한 `harness/` 공통 원장을 따릅니다.

## 자연어 라우팅

아래 요청이 오면 `source-pack-orchestrator`를 먼저 실행합니다.

| 요청 유형 | 예시 표현 |
|---|---|
| 새 실행 | "source pack 만들어줘", "{티커} 공시 자료 수집해줘" |
| 업데이트 | "source pack 업데이트해줘", "관심 종목 source pack 업데이트해줘" |
| 부분 재검토 | "{티커} IR 자료만 다시 확인해줘", "Transcript만 보완해줘" |
| 교차 검증 | "Claude Codex 비교해줘", "두 결과 비교해줘" |

## 하네스 구조

| 영역 | 위치 | 역할 |
|---|---|---|
| 공통 원장 | `harness/ORCHESTRATOR.md` | 전체 실행 흐름 |
| 파일 지도 | `harness/MANIFEST.md` | 파일 역할과 의존 관계 |
| 계약 | `harness/contracts/source-pack.contract.md` | 목표, 입력, 출력, 완료 기준 |
| 절차 | `harness/procedures/` | Orchestrator, Collector, QA 절차 |
| 출력 형식 | `harness/schemas/` | index.md와 실행 요약 schema |
| 평가 기준 | `harness/rubrics/` | Source Pack QA rubric |
| Codex 진입점 | `.agents/skills/source-pack-orchestrator/SKILL.md` | Codex adapter |
| Claude 진입점 | `.claude/skills/source-pack-orchestrator/SKILL.md` | Claude adapter |
| 산출물 | `artifacts/{TICKER}/phase2/step3-source-pack/index.md` | 회사별 원자료 인덱스 |

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
