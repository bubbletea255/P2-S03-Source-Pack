# 하네스 개선 기록

Source Pack 하네스를 사용하면서 발견한 문제, 개선 사항, 변경 내용을 기록한다.

## 변경 이력

| 날짜 | 변경 내용 | 대상 파일 | 사유 |
|---|---|---|---|
| 2026-05-29 | Source Pack v1 최초 구성 | 전체 | 가치투자 리서치 Phase 2 Step 3 원자료 인덱스 구축 |
| 2026-06-02 | 프로젝트 폴더 이동과 권한 drift 점검 | v1 구조 | 절대경로, 포인터 불일치, SEC 권한 범위 문제 확인 |
| 2026-06-02 | Source Pack v2 공통 원장 구조 도입 | `harness/`, `.agents/`, `.claude/`, `AGENTS.md`, `CLAUDE.md` | Claude/Codex 모델 중립 구조와 adapter drift 방지 |
| 2026-06-02 | 기존 AAPL/U 산출물 v2 schema 정규화 | `artifacts/AAPL/.../index.md`, `artifacts/U/.../index.md` | Markdown 표 깨짐, 음수 누락, EX-99.1/Item 2.02 혼선 보정 |
| 2026-06-02 | raw/catalog 구조와 runbook 절차 도입 | `harness/`, `artifacts/README.md`, `.agents/`, `.claude/` | link-only 구조를 원자료 저장소와 기계용 원장 구조로 전환 |

## 다음 실행에서 확인할 점

- 실제 수집 실행 시 `source-pack-runbook.md`가 Collector와 QA를 순차 조율하는지 확인한다.
- `catalog/*.jsonl`의 필드명과 허용값이 Claude/Codex 실행에서 동일하게 유지되는지 확인한다.
- raw 다운로드 후 SHA-256, byte size, MIME type 기록이 누락되지 않는지 확인한다.
- Transcript optional source의 90일 cooldown이 이전 `download-log.jsonl` 기준으로 지켜지는지 확인한다.
- 비교 모드에서 운영 catalog/index를 사용자 승인 없이 덮어쓰지 않는지 확인한다.
