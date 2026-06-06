# Source Pack Handoff - 2026-06-04

이 문서는 새 Codex/Claude 채팅방에서 `P2-S03-Source-Pack` 작업을 이어가기 위한 인수인계 문서다.

새 채팅방에서는 이 파일을 먼저 읽고, 현재 git 상태와 실제 파일 내용을 확인한 뒤 이어서 진행한다.

## 1. 프로젝트 위치

작업 루트:

```text
C:\Users\frisa\Documents\Investment-Research-OS\P2-S03-Source-Pack
```

이 프로젝트는 가치투자 리서치 21단계 중 `P2-S03 Source Pack` 하네스다.

## 2. Source Pack의 목적

Source Pack은 분석 리포트를 만드는 하네스가 아니다.

목표는 SEC 공시, 회사 IR 자료, 가능한 경우 transcript 원문을 raw 파일과 catalog 원장으로 정리해 다음 하네스들이 안정적으로 입력으로 사용할 수 있게 만드는 것이다.

Source Pack에서 하지 않는 일:

- 투자 thesis 작성
- valuation 작성
- 매수/매도 판단
- 요약/번역 중심의 분석 리포트 작성
- 원자료 없이 링크만 모아 운영 입력으로 사용하는 방식

Source Pack에서 하는 일:

- 원자료 후보 확인
- raw 파일 다운로드
- SHA-256, 파일 크기, local_path 기록
- `artifacts/catalog/*.jsonl` 원장 갱신
- `artifacts/companies/{TICKER}/index.md` 사람용 지도 갱신
- `artifacts/runs/{run-id}/` 실행 기록 작성
- QA로 다음 하네스 입력 가능 여부 확인

## 3. 현재 큰 결론

구조 설계와 SEC 수집 안정화 테스트는 사실상 완료된 상태다.

현재까지 검증된 것:

- AAPL SEC Tier 1 full-range 수집
- 동일 범위 재실행 시 `skipped_existing`으로 빠르게 건너뛰는 incremental sync
- 8-K Item 2.02 EX-99.1 exhibit 수집
- primary 문서 수와 file record 수가 달라지는 케이스
- raw 파일 누락 시 `repair_required` 감지
- 자동 재다운로드 금지
- 수동 복구 후 정상 fast path 복귀
- SEC raw 다운로드 transport preflight 규칙
- 실행 후 장애 진단 순서

다음 큰 작업은 SEC 구조를 더 붙잡는 것이 아니라, IR 자료 수집 쪽으로 넘어가서 작은 범위로 테스트하는 것이다.

## 4. 핵심 artifacts 구조

운영 구조:

```text
artifacts/
├── README.md
├── improvement-log.md
├── companies/
│   └── {TICKER}/
│       └── index.md
├── catalog/
│   ├── entities.jsonl
│   ├── documents.jsonl
│   ├── files.jsonl
│   └── runs.jsonl
├── raw/
│   ├── sec-edgar/
│   ├── company-ir/
│   └── transcripts/
├── derived/
│   └── text/
└── runs/
    └── {run-id}/
        ├── download-log.jsonl
        ├── run-summary.md
        └── qa.md
```

`artifacts/raw/`와 `artifacts/derived/`는 대용량 원자료 영역이며 git 제외 대상이다.

## 5. Source of Truth 원칙

다음 하네스가 실제 입력 자료를 판단할 때는 `runs.jsonl`만 보면 안 된다.

원칙:

- 실제 보유 문서의 source of truth: `artifacts/catalog/documents.jsonl`
- 실제 보유 파일의 source of truth: `artifacts/catalog/files.jsonl`
- `artifacts/catalog/runs.jsonl`: 실행 delta와 run pointer
- `artifacts/companies/{TICKER}/index.md`: 사람이 읽는 지도
- `artifacts/runs/{run-id}/download-log.jsonl`: 실제 I/O attempt 기록

`runs.jsonl`의 카운트는 "이번 run에서 새로 일어난 일"을 뜻한다.
이미 보유한 파일 전체 목록이 아니다.

현재 `runs.jsonl` 필드:

```json
{
  "collected_new": 0,
  "skipped_existing": 0,
  "repair_required": 0,
  "failed": 0,
  "files_collected_new": 0
}
```

문서 단위 delta:

- `collected_new`
- `skipped_existing`
- `repair_required`
- `failed`

파일 단위 delta:

- `files_collected_new`

## 6. Incremental Sync 원칙

새로운 실행은 기존 catalog와 raw 파일을 먼저 확인한다.

판정:

| 판정 | 의미 | 행동 |
|---|---|---|
| `new_document` | catalog에 없는 신규 문서 | 다운로드 시도 |
| `skipped_existing` | 이미 수집했고 fast path 조건 통과 | 다운로드하지 않음 |
| `repair_required` | 과거에 성공했으나 파일 또는 관계가 깨짐 | 자동 복구 금지, 사람 확인 |
| `retry_eligible` | 과거 실패/보류였고 현재 재시도 가능 | 다운로드 시도 |

`download-log.jsonl`은 실제 네트워크/디스크 I/O 시도만 기록한다.
따라서 `skipped_existing`만 있는 재실행에서는 download-log가 비어 있을 수 있다.

`repair_required`는 자동 재다운로드가 아니다.
원자료가 사라졌는지, catalog가 깨졌는지, SEC 원문이 바뀐 것인지 사람이 먼저 확인해야 한다.

## 7. files.jsonl Upsert 원칙

`files.jsonl`의 upsert key는 아래 복합 키다.

```text
file_id + document_id + file_role
```

이유:

- `file_id`는 SHA-256 기반 콘텐츠 식별자다.
- `document_id`는 어떤 공시/문서와 연결되는지 나타낸다.
- `file_role`은 primary, exhibit 등 역할을 나타낸다.

같은 hash가 다른 문서나 다른 역할에서 나타날 수 있으므로 `file_id` 단독 upsert는 부정확하다.

같은 file_id가 다른 `document_id` 또는 `file_role`에 연결되면 별도 record를 허용한다.
SEC accession 단위 raw 폴더의 자기완결성이 필요하면 같은 콘텐츠라도 accession별 local_path를 둘 수 있다.

## 8. Exhibit 처리 원칙

primary와 exhibit은 같은 문서 안에서도 다른 file record가 될 수 있다.

원칙:

- primary 문서는 document fast path의 핵심이다.
- run_scope 또는 config에 포함된 exhibit은 검증 대상이다.
- 수집된 exhibit의 local_path가 사라지면 `repair_required`다.
- 과거에 수집한 적 없는 optional exhibit은 `repair_required`가 아니라 신규 후보 또는 out-of-scope다.
- out-of-scope exhibit 누락은 실패가 아니다.

AAPL 8-K Item 2.02 EX-99.1 테스트로 문서 수와 파일 수가 달라지는 케이스를 검증했다.

## 9. SEC Download Preflight 원칙

SEC raw 다운로드는 대량 루프 전에 preflight를 통과해야 한다.

적용 조건:

- 후보 분류 후 `new_document` 또는 `retry_eligible` SEC raw 다운로드 후보가 1건 이상 있으면 필수
- 모든 후보가 `skipped_existing` 또는 `repair_required`이고 새 raw 다운로드가 없으면 preflight 불필요

preflight 통과 조건:

- HTTP 200
- 로컬 파일 존재
- `size_bytes > 0`
- SHA-256 계산 성공

preflight 실패 시:

- 본 다운로드 루프를 시작하지 않는다.
- run status는 `stopped`다.
- 아직 시도하지 않은 후보 문서를 대량으로 `failed` 처리하지 않는다.
- 실제 preflight attempt만 `download-log.jsonl`에 남긴다.
- `run-summary.md`와 `qa.md`에 `[확인 필요: SEC raw download transport failure]`를 기록한다.

PowerShell 환경에서는 SEC archive raw 다운로드에 아래 방식을 기본으로 사용한다.

```powershell
Invoke-WebRequest -Headers @{ "User-Agent" = $UserAgent } -UseBasicParsing -OutFile $TargetPath
```

저수준 `.NET HttpClient` 또는 `WebRequest` 방식은 같은 User-Agent를 넣어도 SEC archive에서 403을 받을 수 있었기 때문에 기본 구현으로 쓰지 않는다.

## 10. QA 원칙

모든 run의 QA는 표준 14단계 제목을 유지한다.

`test_collection`이나 exhibit 특화 검증도 단계 제목을 바꾸지 않는다.
특화 검증은 해당 단계 내부 행으로 추가한다.

표준 출력 형식:

```md
## 1단계: run 범위 확인
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
```

QA status와 run status는 같은 단어를 쓰지 않을 수 있다.
예를 들어 QA의 `partial_pass`는 run의 `partial_success`와 대응될 수 있다.

## 11. 장애 진단 구조

상용 Datadog 같은 대시보드나 실시간 알림은 만들지 않았다.
대신 개인 리서치 도구에 맞는 가벼운 관측 구조를 유지한다.

현재 확인할 수 있는 파일:

- `artifacts/catalog/runs.jsonl`: 실행 상태와 delta count
- `artifacts/runs/{run-id}/run-summary.md`: 사람이 읽는 실행 요약
- `artifacts/runs/{run-id}/download-log.jsonl`: 실제 다운로드 attempt
- `artifacts/runs/{run-id}/qa.md`: 표준 14단계 검증 결과
- `artifacts/catalog/documents.jsonl`: 문서 단위 상태
- `artifacts/catalog/files.jsonl`: 파일 단위 상태와 local_path
- `artifacts/companies/{TICKER}/index.md`: 다음 하네스 입력 가능 여부

`harness/procedures/source-pack-runbook.md`에는 `## 실패 처리` 뒤에 `## 장애 진단 순서`가 추가되어 있다.

진단 순서:

1. `artifacts/catalog/runs.jsonl`
2. 해당 run의 `run-summary.md`
3. 해당 run의 `download-log.jsonl`
4. 해당 run의 `qa.md`
5. `documents.jsonl`과 `files.jsonl`

중요 상태:

| 상태 | 주로 확인할 곳 | 의미 |
|---|---|---|
| `failed` | `runs.jsonl`, `qa.md` | 수집 또는 QA 실패 |
| `stopped` | `runs.jsonl`, `qa.md` | 시작 전 조건, 승인, preflight, transport 문제로 중단 |
| `partial_success` | `runs.jsonl`, `run-summary.md` | 일부 성공, 일부 실패 또는 확인 필요 |
| `repair_required` | `runs.jsonl` 집계, `files.jsonl`/`qa.md` 상세 | 기존 파일 또는 관계 깨짐 |
| `unverified` | `qa.md`, `index.md` | 다음 하네스 입력으로 쓰기 전 보류 |

## 12. 현재 AAPL SEC 수집 상태

마지막 확인 기준:

- ticker: AAPL
- entity_id: `sec-cik-0000320193`
- documents catalog: 39건
- files catalog: 51건
- 회사 index: `artifacts/companies/AAPL/index.md`
- catalog_status: `valid`
- collection_mode: `test_collection`
- last_run_id: `run-20260603-aapl-sec-full-scope-test-v2`

수집 범위:

- 10-K primary: 10년치 10건
- 10-Q primary: 최근 12개 분기 12건
- DEF 14A primary: 5년치 5건
- 8-K Item 2.02 primary: 12개 분기 12건
- 8-K Item 2.02 EX-99.1 exhibit: 12건
- Company IR: 아직 제외
- Transcript: 아직 제외
- derived text: 아직 생성하지 않음

## 13. 실행 기록

`artifacts/catalog/runs.jsonl` 기준 실행 기록:

| run_id | 검증한 것 | 결과 |
|---|---|---|
| `run-20260603-aapl-test` | 최신 10-K 1건 최초 수집 | success |
| `run-20260603-aapl-sec-smoke` | 10-Q, DEF 14A, 8-K primary 소규모 확장 | success |
| `run-20260603-aapl-sec-smoke-v2` | 같은 소규모 범위 재실행, skip 검증 | success |
| `run-20260603-aapl-8k-exhibit-test` | 최신 8-K Item 2.02 EX-99.1 exhibit 신규 수집 | success |
| `run-20260603-aapl-8k-exhibit-test-v2` | 같은 exhibit 범위 재실행, skip 검증 | success |
| `run-20260603-aapl-8k-exhibit-repair-test` | raw 파일 숨김 후 `repair_required` 감지 | partial_success |
| `run-20260603-aapl-8k-exhibit-repair-restore-test` | 파일 복구 후 fast path 복귀 | success |
| `run-20260603-aapl-sec-full-scope-test` | AAPL SEC Tier 1 full-range 확장 | success |
| `run-20260603-aapl-sec-full-scope-test-v2` | full-range 재실행, 39건 skip 검증 | success |

full-range 수집 성과:

- 신규 문서: 35건
- 기존 문서 재사용: 4건
- 신규 파일 record/raw 파일: 46건
- 실패: 0건

full-range 재실행 성과:

- 신규 문서: 0건
- 기존 문서 재사용: 39건
- 신규 파일: 0건
- raw 다운로드: 0건

## 14. 중간에 있었던 중요 문제와 해결

full-range 수집 중 처음에 SEC archive 다운로드가 403으로 대량 실패하는 문제가 있었다.

원인:

- SEC submissions API는 작동했지만 raw archive 다운로드 구현이 막혔다.
- 저수준 `.NET WebRequest`/`HttpClient` 방식은 User-Agent를 넣어도 SEC archive에서 403을 반환할 수 있었다.

해결:

- PowerShell `Invoke-WebRequest -UseBasicParsing -Headers` 방식으로 바꿔 정상 수집했다.
- 잘못된 bulk-fail 기록을 남기지 않도록 preflight와 조기 중단 규칙을 문서화했다.
- 아직 시도하지 않은 후보를 대량 `failed` 처리하지 않는 원칙을 추가했다.

이 문제는 다시 반복될 수 있으므로, 다음 raw 다운로드 구현도 반드시 preflight를 통과해야 한다.

## 15. 현재 미완료 영역

아직 본격적으로 하지 않은 것:

1. Company IR 자료 수집 테스트
2. Transcript optional source 수집 테스트
3. `derived/text` 추출 여부 결정과 테스트
4. AAPL 외 다른 회사로 SEC 수집 재현성 확인
5. 실제 운영 모드 `new_collection` 또는 `incremental_update`로 여러 티커 실행
6. Source Pack 결과물을 다음 IR/분석 하네스가 읽는 방식 검증

## 16. 다음 추천 작업

새 채팅방에서는 아래 순서가 자연스럽다.

### Step 1. 현재 상태 재확인

```powershell
git status --short
git diff --check
Get-Content -Encoding UTF8 artifacts\catalog\runs.jsonl
```

### Step 2. 미확정 git 상태 정리

사용자가 "현재 상태를 GitHub에 올렸다"고 했지만, 마지막 확인한 로컬 상태에는 아래 변경이 보였다.
새 채팅방에서 반드시 다시 확인한다.

```text
M  .gitignore
D  docs/대화 요약/20260529_1901_대화요약.md
D  docs/대화 원본/20260529_1851_대화원본.md
D  docs/대화 원본/20260529_1901_대화원본.md
D  docs/대화 원본/_checkpoint.md
?? docs/source-pack-observability-template.md
```

주의:

- 이 변경들은 사용자 또는 다른 agent가 만든 것일 수 있다.
- 임의로 되돌리지 않는다.
- 특히 `docs/source-pack-observability-template.md`는 가벼운 관측 템플릿으로 보인다. 채택할지, 보관만 할지, 삭제할지는 사용자가 결정해야 한다.

### Step 3. AAPL IR 소규모 테스트

권장 요청:

```text
AAPL Company IR 자료 소규모 수집 테스트를 진행해줘.
최신 investor relations 자료 1~2건만 raw 파일로 저장하고 catalog/index/QA까지 확인해줘.
```

처음부터 IR 전체 archive를 대량 수집하지 않는다.
IR 사이트 구조, 다운로드 가능한 PDF/HTML, robots/접근 제한, catalog field 처리부터 작게 확인한다.

### Step 4. IR 성공 후 범위 확장

IR 소규모 테스트가 성공하면:

- 최근 earnings release
- 최근 presentation
- 최근 annual meeting/shareholder letter 계열 자료
- SEC 8-K exhibit과 IR 자료의 중복 여부

순서로 확장한다.

### Step 5. Transcript는 optional로 별도 테스트

Transcript는 차단, 로그인, 유료벽 가능성이 높다.
따라서 IR 이후 별도 optional source로 테스트한다.
로그인 우회, 유료벽 우회, 차단 우회는 하지 않는다.

## 17. 새 채팅방에서 반복하지 말아야 할 논쟁

이미 합의된 것:

- Source Pack은 분석 하네스가 아니라 수집 하네스다.
- link-only artifacts는 운영 입력으로 쓰지 않는다.
- `documents.jsonl`과 `files.jsonl`이 source of truth다.
- `runs.jsonl`은 실행 delta이지 전체 inventory가 아니다.
- `download-log.jsonl`은 실제 I/O attempt만 기록한다.
- `skipped_existing`은 다운로드하지 않는다.
- `repair_required`는 자동 재다운로드하지 않는다.
- `files.jsonl` upsert key는 `file_id + document_id + file_role`이다.
- 모든 QA는 표준 14단계 제목을 유지한다.
- SEC raw 다운로드는 후보가 있으면 preflight 필수다.
- Datadog 수준 대시보드/알림은 지금 만들지 않는다.
- 실행 후 진단은 runbook의 `장애 진단 순서`를 따른다.

## 18. 새 채팅방 시작용 프롬프트

새 채팅방에서는 아래 문장을 붙여넣으면 된다.

```text
C:\Users\frisa\Documents\Investment-Research-OS\P2-S03-Source-Pack\docs\source-pack-handoff-2026-06-04.md 를 먼저 읽고, 그 내용을 기준으로 이어서 진행해줘.

우선 현재 git 상태와 artifacts/catalog/runs.jsonl을 확인하고, 미확정 변경이 있는지 알려줘.
그 다음 AAPL Company IR 소규모 수집 테스트로 넘어가자.
```

## 19. 현재 결론

P2-S03 Source Pack은 SEC 수집 하네스로서 중요한 안정화 테스트를 통과했다.

이제 남은 핵심은 "SEC 구조를 더 다듬기"가 아니라, IR 자료 수집으로 넘어가서 Source Pack이 SEC 외 원자료까지 안정적으로 처리할 수 있는지 검증하는 것이다.

새 채팅방의 첫 목표:

1. 현재 git 상태 확인
2. 미확정 파일 처리 판단
3. AAPL IR 소규모 수집 테스트
4. IR catalog/index/QA 보정
5. 성공하면 IR 범위 확장
