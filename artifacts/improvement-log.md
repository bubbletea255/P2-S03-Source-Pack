# 하네스 개선 기록

Source Pack 하네스를 사용하면서 발견한 문제, 개선 사항, 변경 내용을 기록합니다.

---

| 날짜 | 변경 내용 | 대상 파일 | 사유 |
|---|---|---|---|
| 2026-05-29 | Source Pack 하네스 최초 구성 | 전체 | 가치투자 리서치 파이프라인 Phase 2 Step 3 구축 |
| 2026-06-02 | 프로젝트 폴더 이동 및 경로 상대화 | `저장.md`, `settings.local.json` | 폴더명·위치 변경 시 에러 방지 목적. 절대경로 3곳 → 런타임 프로젝트 루트 기준으로 교체 |
| 2026-06-02 | SEC 권한 일반화 | `settings.local.json` | 특정 CIK 하드코딩 제거 → `Bash(curl * sec.gov *)` 패턴으로 교체. WebFetch에 `www.sec.gov`, `efts.sec.gov` 추가 |
| 2026-06-02 | AGENTS.md 파일 포인터 수정 | `AGENTS.md` | Codex용 파일 경로가 실제 위치와 불일치 → `.agents/skills/...`, `.codex/agents/...toml`로 교정 |
