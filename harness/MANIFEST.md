# Harness Manifest

이 문서는 Source Pack v2 하네스의 파일 역할과 입력/출력 의존 관계를 정리한다.

## 파일 역할 지도

| 영역 | 파일 | 역할 |
|---|---|---|
| 공통 흐름 | `harness/ORCHESTRATOR.md` | 전체 실행 모드, 순서, 중단 조건 |
| 계약 | `harness/contracts/source-pack.contract.md` | 목표, 입력, 출력, 완료 기준, 금지사항 |
| 절차 | `harness/procedures/source-pack-orchestrator.md` | 티커 선택, 설정 확인, 실행 조율 |
| 절차 | `harness/procedures/source-pack-collector.md` | CIK 조회, SEC/IR/Transcript 수집 |
| 절차 | `harness/procedures/source-pack-qa.md` | 산출물 검증 |
| Schema | `harness/schemas/source-pack-index.schema.md` | 회사별 index.md 출력 형식 |
| Schema | `harness/schemas/source-pack-run-summary.schema.md` | 실행 요약 출력 형식 |
| Rubric | `harness/rubrics/source-pack-qa.rubric.md` | 품질 평가 기준 |
| Codex adapter | `.agents/skills/source-pack-orchestrator/SKILL.md` | Codex 진입점 |
| Codex adapter | `.agents/skills/source-pack-collector/SKILL.md` | Codex 수집 adapter |
| Claude adapter | `.claude/skills/source-pack-orchestrator/SKILL.md` | Claude 진입점 |
| Claude adapter | `.claude/agents/source-pack-collector.md` | Claude 수집 adapter |
| 설정 | `config.md` | 수집 범위, 속도 제한, SEC User-Agent |
| 입력 | `watchlist.md` | 관심 종목 목록 |
| 산출물 | `artifacts/{TICKER}/phase2/step3-source-pack/index.md` | 회사별 원자료 인덱스 |

## 산출물 의존 관계

```text
watchlist.md + config.md + 사용자 요청
    ↓
티커 확정 및 실행 모드 판단
    ↓
CIK / 회사 메타데이터 확인
    ↓
SEC 공시 / IR / Transcript / 산업 자료 링크 수집
    ↓
artifacts/{TICKER}/phase2/step3-source-pack/index.md
    ↓
QA 검증
    ↓
다음 하네스: Industry Primer, Value Chain, Business Model, Market, Competition
```

## 다음 Phase 전달 계약

다음 하네스는 `index.md`에서 아래를 먼저 읽는다.

- 회사 메타데이터
- 수집 현황 요약
- 최신 10-K, 10-Q, Proxy, 실적 발표 자료 링크
- Transcript와 IR 자료의 수집 여부
- 수동 확인 필요 목록
- `다음 하네스 전달 요약`

## 수정 경계

공통 업무 의미는 `harness/`에 둔다.
adapter는 실행 환경별 진입과 도구 사용 방식만 담는다.
