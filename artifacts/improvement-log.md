# 하네스 개선 기록

Source Pack 하네스를 사용하면서 발견한 문제, 개선 사항, 변경 내용을 기록한다.

## 변경 이력

| 날짜 | 변경 내용 | 대상 파일 | 사유 |
|---|---|---|---|
| 2026-05-29 | Source Pack v1 최초 구성 | 전체 | 가치투자 리서치 Phase 2 Step 3 원자료 인덱스 구축 |
| 2026-06-02 | 프로젝트 폴더 이동과 권한 drift 점검 | v1 구조 | 절대경로, 포인터 불일치, SEC 권한 범위 문제 확인 |
| 2026-06-02 | Source Pack v2 공통 원장 구조 도입 | `harness/`, `.agents/`, `.claude/`, `AGENTS.md`, `CLAUDE.md` | Claude/Codex 모델 중립 구조와 adapter drift 방지 |
| 2026-06-02 | 기존 AAPL/U 산출물 v2 schema 정규화 | `artifacts/AAPL/.../index.md`, `artifacts/U/.../index.md` | Markdown 표 깨짐, 음수 누락, EX-99.1/Item 2.02 혼선 보정 |

## 다음 실행에서 확인할 점

- 실제 수집 실행 시 `company_tickers_exchange.json` 우선 CIK 조회가 지켜지는지 확인한다.
- Item 2.02 filing에서 EX-99.1 첨부 여부를 실제 filing detail로 보강한다.
- Transcript 후보는 SEC/IR/Quartr/AlphaStreet 순서로 다시 확인한다.
- IR 자료 자동 탐색을 실제로 수행하고 미수집 사유를 갱신한다.
- 비교 모드에서 Claude/Codex adapter가 같은 `harness/` 원장을 읽는지 확인한다.
