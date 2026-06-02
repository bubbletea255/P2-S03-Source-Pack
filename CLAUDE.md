# 가치투자 리서치 하네스

이 프로젝트는 가치투자 리서치 21단계 파이프라인을 자동화하는 하네스 모음입니다.

## 자연어 라우팅

아래와 같은 요청이 오면 `source-pack-orchestrator`를 먼저 실행합니다.

- "source pack 만들어줘"
- "source pack 실행해줘"
- "{티커} 공시 자료 수집해줘"
- "{티커} 자료 정리해줘"
- "관심 종목 source pack 업데이트해줘"
- "source pack 업데이트해줘"
- "공시 링크 수집해줘"

## 주요 파일 위치

| 파일 | 역할 |
|---|---|
| `watchlist.md` | 관심 종목 티커 목록 — 직접 편집 |
| `config.md` | 수집 범위·속도 제한·필터 설정 — 직접 편집 |
| `artifacts/{TICKER}/phase2/step3-source-pack/index.md` | 회사별 공시 링크 모음 (최종 산출물) |
| `artifacts/README.md` | 산출물 전체 지도 |
| `artifacts/improvement-log.md` | 하네스 개선 기록 |

## 하네스 구성

| 구성 요소 | 위치 | 역할 |
|---|---|---|
| Orchestrator | `.claude/skills/source-pack-orchestrator/SKILL.md` | 전체 흐름 관리 (사용자 선택 → 순차 실행 → 결과 요약) |
| Collector Agent | `.claude/agents/source-pack-collector.md` | 회사 한 곳의 공시 수집 + index.md 생성 |

## 하네스 실행 방법

자연어로 요청하거나, `/source-pack` 명령으로 직접 실행할 수 있습니다.

## 변경 이력

| 날짜 | 변경 내용 |
|---|---|
| 2026-05-29 | Source Pack 하네스 최초 구성 (Phase 2 Step 3) |
