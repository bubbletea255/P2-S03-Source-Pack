# Source Pack Handoff - 2026-06-03

이 문서는 새 Codex/Claude 채팅방에서 `P2-S03-Source-Pack` 작업을 이어가기 위한 인수인계 문서다.

새 채팅방에서는 이 파일을 먼저 읽고, 현재 git 상태와 실제 파일 내용을 확인한 뒤 이어서 진행한다.

## 1. 프로젝트 위치

작업 루트:

```text
C:\Users\frisa\Documents\Investment-Research-OS\P2-S03-Source-Pack
```

이 프로젝트는 가치투자 리서치 21단계 중 `P2-S03 Source Pack` 하네스다.

## 2. 최상위 목표

Source Pack 하네스의 목표는 미국 상장기업의 주요 원자료를 수집해, 이후 가치투자 21단계의 다른 하네스들이 안정적으로 입력으로 사용할 수 있는 원자료 저장소를 만드는 것이다.

핵심 목표:

- 링크 모음이 아니라 raw 원자료 파일을 가능한 범위에서 로컬에 저장한다.
- 기계가 읽을 수 있는 `catalog/*.jsonl` 원장을 만든다.
- 사람이 빠르게 확인할 수 있는 회사별 `index.md`를 만든다.
- 다음 하네스가 `index -> catalog -> files -> raw/derived` 순서로 읽을 수 있게 한다.
- Claude Code와 Codex가 같은 `harness/` 공통 원장을 읽는 모델 중립 구조를 유지한다.

Source Pack은 분석 하네스가 아니다.
Source Pack에서는 요약, 번역, thesis, valuation, 매수/매도 의견, 투자 판단을 작성하지 않는다.

## 3. 핵심 설계 결정

### 3.1 Source Pack은 수집형 하네스다

이 하네스는 투자 판단을 하는 곳이 아니라 원자료를 모으는 곳이다.

따라서 품질 기준은 다음과 같다.

- 출처 정확성
- raw 파일 재현성
- catalog 일관성
- 다운로드 실패 기록의 투명성
- 다음 하네스가 안전하게 읽을 수 있는 입력 패키지

분석형 리포트의 "산출물 깊이" 기준을 그대로 적용하지 않는다.

### 3.2 artifacts 구조를 새로 잡았다

권장 구조:

```text
artifacts/
├── README.md
├── improvement-log.md
│
├── companies/
│   └── {TICKER}/
│       └── index.md
│
├── catalog/
│   ├── entities.jsonl
│   ├── documents.jsonl
│   ├── files.jsonl
│   └── runs.jsonl
│
├── raw/
│   ├── sec-edgar/
│   ├── company-ir/
│   └── transcripts/
│
├── derived/
│   └── text/
│       ├── sec-edgar/
│       ├── company-ir/
│       └── transcripts/
│
└── runs/
    └── {run-id}/
        ├── download-log.jsonl
        ├── run-summary.md
        └── qa.md
```

`artifacts/raw/`와 `artifacts/derived/`는 대용량 원자료이므로 `.gitignore`에 추가했다.

### 3.3 catalog가 source of truth다

`companies/{TICKER}/sources.jsonl`은 만들지 않는다.

기계용 원본 장부는 아래 파일이다.

- `artifacts/catalog/entities.jsonl`
- `artifacts/catalog/documents.jsonl`
- `artifacts/catalog/files.jsonl`
- `artifacts/catalog/runs.jsonl`

회사별 `index.md`는 사람이 읽기 위한 지도다.
`index.md`와 catalog가 충돌하면 catalog를 우선한다.

### 3.4 link-only 모드는 운영 구조에서 제외했다

기존 실습 결과였던 link-only artifacts는 운영 입력으로 쓰지 않는다.
해당 로컬 폴더는 삭제하고, 예전 경로는 금지/legacy 문맥에서만 짧게 언급한다.

예전 경로:

```text
artifacts/{TICKER}/phase2/step3-source-pack/index.md
```

이 경로는 이제 금지/legacy 문맥에서만 언급된다.
운영 자료가 필요하면 새 raw/catalog 구조로 다시 수집한다.

### 3.5 transcript는 optional source다

Transcript 원문은 Source Pack에서 가능한 경우 수집할 수 있다.
하지만 transcript 번역, 요약, Q&A, 투자 관점 분석은 별도 Transcript 하네스로 분리한다.

Transcript 수집은 다음 문제 때문에 optional로 다룬다.

- 많은 사이트가 Cloudflare 등으로 봇 접근을 막는다.
- 무료 열람 횟수 제한이나 유료벽이 있다.
- 로그인 우회, 차단 우회, 유료벽 우회는 하지 않는다.

실패 시:

- 실패를 `download-log.jsonl`에 기록한다.
- 동일 ticker/quarter/source 실패는 90일 cooldown을 적용한다.
- 무한 재시도하지 않는다.

### 3.6 Orchestrator와 Runbook 역할을 분리했다

혼동을 줄이기 위해 아래처럼 정리했다.

- `source-pack-orchestrator`: 사용자 자연어 요청을 받는 adapter/skill 이름이다. 유지한다.
- `harness/ORCHESTRATOR.md`: 상위 실행 원장이다. 실행 모드, 승인 조건, 중단 조건, 수정 경계를 정의한다.
- `harness/procedures/source-pack-runbook.md`: 실제 1회 실행 절차서다. 티커 선택, 설정 확인, Collector/QA 조율을 담당한다.

기존 `harness/procedures/source-pack-orchestrator.md`는 삭제하고 `source-pack-runbook.md`로 대체했다.

## 4. 주요 완료 작업

### 4.1 템플릿/개념 정리

완료한 논의:

- 수집형 하네스와 분석형 하네스를 같은 품질 언어로 평가하면 안 된다는 점을 확인했다.
- `산출물 깊이`라는 표현은 수집형 Source Pack에 부적절할 수 있다고 판단했다.
- 공용 템플릿 v4에서 하네스 유형별 품질 축을 분리했다.
- 오케스트레이션은 작업 유형이 아니라 실행 구조라는 점을 정리했다.

관련 파일:

- `docs/공용_하네스_템플릿_체크리스트_v4.md`

### 4.2 raw/catalog 구조 청사진 작성

완료한 파일:

- `docs/source-pack-artifacts-raw-structure-v1.md`

이 파일에는 다음 내용이 들어 있다.

- 최종 artifacts 폴더 구조
- catalog JSONL 스키마 방향
- raw 경로 규칙
- derived/text 경로 규칙
- `.gitignore` 정책
- `download-log.jsonl -> catalog/files.jsonl` 승격 규칙
- transcript optional source 정책
- 기존 AAPL/U link-only 결과 처리 방향
- 다음 하네스가 Source Pack을 읽는 방식

### 4.3 harness 공통 원장 수정

수정/작성된 핵심 파일:

- `harness/ORCHESTRATOR.md`
- `harness/MANIFEST.md`
- `harness/contracts/source-pack.contract.md`
- `harness/procedures/source-pack-runbook.md`
- `harness/procedures/source-pack-collector.md`
- `harness/procedures/source-pack-qa.md`
- `harness/rubrics/source-pack-qa.rubric.md`
- `harness/schemas/source-pack-catalog.schema.md`
- `harness/schemas/source-pack-index.schema.md`
- `harness/schemas/source-pack-run-summary.schema.md`

중요한 상태:

- `source-pack-catalog.schema.md`는 신규 파일이다.
- `source-pack-runbook.md`는 신규 파일이다.
- `source-pack-orchestrator.md`는 삭제되었다.

### 4.4 Codex/Claude adapter 동기화

수정된 adapter:

- `.agents/skills/source-pack-orchestrator/SKILL.md`
- `.agents/skills/source-pack-collector/SKILL.md`
- `.claude/skills/source-pack-orchestrator/SKILL.md`
- `.claude/agents/source-pack-collector.md`

원칙:

- adapter에는 공통 업무 규칙을 길게 복사하지 않는다.
- adapter는 `harness/` 공통 원장을 읽도록 연결만 한다.
- `source-pack-orchestrator`라는 skill 이름은 유지한다.

### 4.5 안내 파일 갱신

수정된 안내 파일:

- `AGENTS.md`
- `CLAUDE.md`
- `artifacts/README.md`
- `artifacts/improvement-log.md`
- `.gitignore`

`AGENTS.md`와 `CLAUDE.md`는 같은 구조 안내를 담도록 동기화했다.

`.gitignore`에는 다음이 추가되었다.

```text
artifacts/raw/
artifacts/derived/
```

## 5. 현재 git 상태 요약

마지막 확인 기준으로 변경된 파일은 대략 아래와 같다.

수정됨:

- `.agents/skills/source-pack-collector/SKILL.md`
- `.agents/skills/source-pack-orchestrator/SKILL.md`
- `.claude/agents/source-pack-collector.md`
- `.claude/skills/source-pack-orchestrator/SKILL.md`
- `.gitignore`
- `AGENTS.md`
- `CLAUDE.md`
- `artifacts/README.md`
- `artifacts/improvement-log.md`
- `docs/대화 원본/_checkpoint.md`
- `harness/MANIFEST.md`
- `harness/ORCHESTRATOR.md`
- `harness/contracts/source-pack.contract.md`
- `harness/procedures/source-pack-collector.md`
- `harness/procedures/source-pack-qa.md`
- `harness/rubrics/source-pack-qa.rubric.md`
- `harness/schemas/source-pack-index.schema.md`
- `harness/schemas/source-pack-run-summary.schema.md`

삭제됨:

- `harness/procedures/source-pack-orchestrator.md`

새 파일:

- `docs/source-pack-artifacts-raw-structure-v1.md`
- `harness/procedures/source-pack-runbook.md`
- `harness/schemas/source-pack-catalog.schema.md`
- `docs/source-pack-handoff-2026-06-03.md`

주의:

- `docs/대화 원본/_checkpoint.md`는 대화 저장 자동 산출물로 보인다. 커밋에 포함할지 여부를 사용자가 결정해야 한다.
- raw 원자료 다운로드는 아직 실제로 실행하지 않았다.
- catalog JSONL 파일들도 아직 운영 데이터로 초기화하지 않았다.

## 6. 검증된 사항

마지막 검증에서 확인한 것:

- `source-pack-orchestrator.md`를 가리키는 남은 참조는 없다.
- 예전 경로 `artifacts/{TICKER}/phase2/step3-source-pack/index.md`는 금지/legacy 문맥에만 남아 있다.
- `AGENTS.md`와 `CLAUDE.md`는 같은 구조로 동기화되어 있다.
- `git diff --check`는 줄 끝 공백 오류 없이 통과했다.

다음 채팅방에서는 이 검증을 다시 한 번 실제 파일 기준으로 확인하는 것이 좋다.

권장 확인 명령:

```powershell
git status --short
git diff --check
rg -n "source-pack-orchestrator\.md" harness .agents .claude AGENTS.md CLAUDE.md artifacts\README.md
Compare-Object (Get-Content -Encoding UTF8 AGENTS.md) (Get-Content -Encoding UTF8 CLAUDE.md) -IncludeEqual:$false
```

## 7. 아직 하지 않은 일

아직 하지 않은 핵심 작업:

1. 현재 변경분 커밋 전 점검
2. 커밋에 포함할 파일과 제외할 파일 결정
3. GitHub에 현재 구조 전환 작업 업로드
4. 새 artifacts 디렉터리 초기화
5. `artifacts/catalog/*.jsonl` 초기 파일 생성
6. AAPL 하나로 작은 범위의 첫 raw 수집 테스트
7. SEC raw 다운로드, SHA-256, byte size, MIME type 기록 검증
8. `download-log.jsonl -> catalog/files.jsonl` 승격 흐름 검증
9. `artifacts/companies/AAPL/index.md` 생성 검증
10. QA 실제 실행
11. 첫 테스트에서 발견된 schema/procedure/rubric 문제 반영
12. 테스트 성공 후 수집 범위 확장

## 8. 다음 단계 추천 순서

새 채팅방에서는 아래 순서로 진행하는 것이 좋다.

### Step 1. 현재 상태 재확인

먼저 git 상태와 주요 파일 존재를 확인한다.

```powershell
git status --short
Get-ChildItem -Force harness\procedures
Get-ChildItem -Force harness\schemas
```

### Step 2. 커밋 전 포함/제외 결정

특히 아래 파일을 커밋할지 확인한다.

- `docs/대화 원본/_checkpoint.md`
- 삭제된 기존 link-only artifacts의 git 상태
- 새 handoff 문서

권장:

- 하네스 구조 파일과 handoff 문서는 커밋한다.
- 자동 대화 checkpoint는 사용자가 원하지 않으면 제외한다.

### Step 3. 저장 지점 만들기

커밋 메시지 후보:

```text
Refactor Source Pack harness to raw catalog runbook structure
```

또는 한국어:

```text
Source Pack raw/catalog runbook 구조 도입
```

### Step 4. 새 artifacts 운영 구조 초기화

필요한 폴더:

```text
artifacts/companies/
artifacts/catalog/
artifacts/runs/
```

raw/derived는 실제 다운로드 시 생성하거나 빈 폴더를 만들 수 있다.
다만 `artifacts/raw/`, `artifacts/derived/`는 gitignore 대상이다.

초기 catalog 파일:

```text
artifacts/catalog/entities.jsonl
artifacts/catalog/documents.jsonl
artifacts/catalog/files.jsonl
artifacts/catalog/runs.jsonl
```

빈 JSONL 파일로 시작할 수 있다.

### Step 5. 작은 범위의 AAPL 테스트

처음부터 전체 수집을 하지 않는다.

권장 테스트 범위:

- AAPL
- 최신 10-K 1개 또는 최신 10-Q 1개
- SEC EDGAR raw primary document 다운로드
- exhibit은 선택
- IR/transcript는 첫 테스트에서 제외 가능

목표:

- User-Agent 설정 확인
- CIK 확인
- SEC filing 후보 확인
- raw 파일 저장
- SHA-256 계산
- `download-log.jsonl` 기록
- `documents.jsonl`/`files.jsonl` 기록
- `companies/AAPL/index.md` 작성
- QA 실행

### Step 6. 테스트 후 하네스 보정

실행 중 발견될 수 있는 문제:

- SEC URL 규칙의 세부 차이
- accession 번호 하이픈 제거/유지 혼동
- primary document filename 획득 위치
- MIME type 기록 방식
- JSONL 필드 nullable 처리
- run status mapping
- index.md 사람이 읽기 좋은 정도

이 문제들은 실제 실행 후 작게 보정한다.

## 9. 새 채팅방에서 특히 조심할 점

### 9.1 설계 논의를 다시 처음부터 반복하지 말 것

이미 합의된 큰 결정:

- Source Pack은 수집형이다.
- raw/catalog 구조로 간다.
- link-only는 운영 구조에서 제외한다.
- transcript 분석은 별도 하네스로 뺀다.
- `source-pack-orchestrator` adapter 이름은 유지한다.
- 실제 실행 절차 파일은 `source-pack-runbook.md`다.

다음 채팅방은 이 결정을 전제로 진행한다.

### 9.2 실제 raw 다운로드는 작게 시작할 것

첫 실행에서 전체 10년치 10-K, 12분기 10-Q, 모든 8-K, IR, transcript까지 시도하면 문제 범위가 너무 커진다.

첫 테스트는 최소 범위로 한다.

### 9.3 SEC User-Agent를 반드시 확인할 것

`config.md`의 SEC User-Agent가 비어 있으면 실행을 중단해야 한다.
이 규칙은 `ORCHESTRATOR.md`, `runbook`, `collector`에 반영되어 있다.

### 9.4 운영 catalog를 함부로 덮어쓰지 말 것

새 구조 첫 테스트 전에는 catalog가 없거나 비어 있을 수 있다.
기존 운영 데이터가 생긴 뒤에는 덮어쓰기 전 사용자 승인이 필요하다.

### 9.5 파일 경로 변경 시 절대경로 의존을 피할 것

이 하네스는 폴더명과 위치가 바뀌어도 작동하도록 상대 경로와 프로젝트 루트 기준 경로를 선호한다.
문서에는 repo 내부 상대 경로를 쓴다.

## 10. 주요 파일별 역할

| 파일 | 역할 |
|---|---|
| `AGENTS.md` | Codex/공통 프로젝트 안내판 |
| `CLAUDE.md` | Claude Code 프로젝트 안내판 |
| `harness/ORCHESTRATOR.md` | 상위 실행 원장 |
| `harness/MANIFEST.md` | 파일 역할과 의존 관계 지도 |
| `harness/contracts/source-pack.contract.md` | Source Pack의 목표, 입력, 출력, 완료 기준, 금지사항 |
| `harness/procedures/source-pack-runbook.md` | 1회 실행 절차서 |
| `harness/procedures/source-pack-collector.md` | 실제 수집 절차 |
| `harness/procedures/source-pack-qa.md` | QA 절차 |
| `harness/rubrics/source-pack-qa.rubric.md` | QA 채점 기준 |
| `harness/schemas/source-pack-catalog.schema.md` | catalog JSONL schema |
| `harness/schemas/source-pack-index.schema.md` | 회사별 index.md schema |
| `harness/schemas/source-pack-run-summary.schema.md` | 사용자 보고 schema |
| `artifacts/README.md` | 산출물 구조 안내 |
| `artifacts/improvement-log.md` | 하네스 개선 기록 |
| `.agents/skills/source-pack-orchestrator/SKILL.md` | Codex 진입점 adapter |
| `.agents/skills/source-pack-collector/SKILL.md` | Codex collector adapter |
| `.claude/skills/source-pack-orchestrator/SKILL.md` | Claude 진입점 adapter |
| `.claude/agents/source-pack-collector.md` | Claude collector adapter |

## 11. 새 채팅방 시작용 짧은 프롬프트

새 채팅방에서는 아래 문장을 붙여넣으면 된다.

```text
C:\Users\frisa\Documents\Investment-Research-OS\P2-S03-Source-Pack\docs\source-pack-handoff-2026-06-03.md 를 먼저 읽고, 그 내용을 기준으로 이어서 진행해줘.

우선 현재 git 상태를 확인하고, 커밋 전 점검부터 시작하자. 특히 어떤 파일을 커밋에 포함할지, docs/대화 원본/_checkpoint.md 같은 자동 저장 파일을 포함할지 여부를 먼저 판단해줘.
```

## 12. 현재 결론

구조 재설계 단계는 완료로 보아도 된다.

다음 단계는 설계 논의를 더 늘리는 것이 아니라, 현재 상태를 저장하고 아주 작은 실제 수집 테스트로 들어가는 것이다.

권장 다음 행동:

1. 커밋 전 점검
2. GitHub 저장
3. catalog 초기화
4. AAPL 최신 SEC filing 1개로 raw 다운로드 테스트
5. QA 실행
6. 발견된 문제만 작게 보정
