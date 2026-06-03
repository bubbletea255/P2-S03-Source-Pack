# Source Pack Runbook

이 파일은 Source Pack 하네스를 한 번 실행할 때 따르는 절차서다.

`harness/ORCHESTRATOR.md`는 상위 실행 원장이다. 실행 모드, 승인 조건, 비교 모드, 중단 조건, 수정 경계를 정의한다.
이 runbook은 승인된 요청을 실제 실행으로 옮기는 순서를 정의한다.

adapter 이름인 `source-pack-orchestrator`는 사용자 요청을 받는 진입점이므로 유지한다.
절차 파일 이름만 `source-pack-runbook.md`로 둔다.

## 목적

사용자 요청을 받아 대상 티커, 실행 모드, 설정, 실행 기록 위치를 확정하고 Source Pack Collector와 QA를 순차 조율한다.
Source Pack은 원자료 수집형 하네스이므로 이 runbook은 분석, 요약, 번역, 투자 판단을 지시하지 않는다.

## 실행 전 필독

아래 파일을 순서대로 읽는다.

1. `harness/ORCHESTRATOR.md`
2. `harness/MANIFEST.md`
3. `harness/contracts/source-pack.contract.md`
4. `harness/schemas/source-pack-catalog.schema.md`
5. `harness/schemas/source-pack-index.schema.md`
6. `harness/procedures/source-pack-collector.md`
7. `harness/procedures/source-pack-qa.md`
8. `harness/rubrics/source-pack-qa.rubric.md`
9. `config.md`
10. `watchlist.md`
11. `artifacts/README.md`

파일이 없으면 임의로 추정하지 말고 해당 단계의 실패 처리 규칙을 따른다.

## 0. 요청 분류

사용자 요청을 아래 실행 모드 중 하나로 분류한다.

| 모드 | 사용 상황 |
|---|---|
| `new_collection` | 해당 티커의 catalog/index가 없거나 새로 수집하라는 요청 |
| `incremental_update` | 기존 catalog/index를 기준으로 새 원자료를 추가하는 요청 |
| `partial_recheck` | SEC, IR, transcript, QA 등 특정 영역만 다시 확인하는 요청 |
| `test_collection` | 수집 파이프라인 검증을 위해 제한된 범위만 실행하는 요청 |
| `comparison` | Claude/Codex 또는 두 실행 결과를 비교하는 요청 |

모드가 불분명하면 현재 catalog와 회사별 index 존재 여부를 확인해 가장 보수적인 모드로 분류한다.
운영 원장을 덮어쓸 수 있는 작업은 사용자 승인 전 진행하지 않는다.
`test_collection`은 전체 Source Pack 완료가 아니라 선언된 테스트 범위의 실행 검증으로 분류한다.

## 1. 대상 확정

1. 사용자 메시지에 티커가 있으면 그 티커를 우선 사용한다.
2. 티커가 없으면 `watchlist.md`를 읽고 대상 후보를 표시한다.
3. 사용자가 `all`, 번호, 티커 직접 입력 중 하나로 대상을 선택하게 한다.
4. 티커는 대문자로 정규화한다.
5. 여러 티커는 순차 처리한다. SEC, IR, transcript 요청을 티커 간 병렬로 실행하지 않는다.

## 2. 설정 확인과 승인

`config.md`에서 아래 항목을 확인해 사용자에게 요약한다.

```text
대상 티커:
실행 모드:
수집 범위:
run_scope:
속도 제한:
SEC User-Agent:
8-K 주요 이벤트 필터:
raw 다운로드 여부:
derived/text 생성 여부:
transcript 원문 수집 여부:
운영 catalog 반영 여부:
```

다음 경우에는 자동 실행하지 않는다.

- SEC User-Agent가 비어 있거나 SEC 요구 형식에 맞지 않는다.
- 운영 catalog나 회사별 index를 덮어쓸 가능성이 있는데 사용자가 승인하지 않았다.
- transcript 유료벽, 봇 차단, 로그인 우회가 필요한 방식으로 접근해야 한다.
- 비교 모드 결과를 운영 catalog에 바로 반영하려는 요청이다.

사용자가 승인하면 다음 단계로 진행한다.

`test_collection`에서는 `config.md`의 전체 운영 범위를 그대로 적용하지 않는다.
사용자에게 승인받은 제한 범위를 `run_scope`에 한 줄로 기록하고, Collector와 QA는 그 선언 범위를 기준으로 실행한다.
예: `test only: latest AAPL 10-K primary SEC filing, no exhibits, no IR, no transcript`

## 3. 사전 점검

대상 티커별로 아래를 확인한다.

1. `artifacts/catalog/entities.jsonl`에 회사가 있는지 확인한다.
2. `artifacts/catalog/documents.jsonl`에서 기존 문서 원장을 확인한다.
3. `artifacts/catalog/files.jsonl`에서 raw 파일 승격 기록을 확인한다.
4. `artifacts/companies/{TICKER}/index.md`가 있는지 확인한다.
5. `artifacts/runs/`에서 최근 실행과 실패 기록을 확인한다.
6. transcript 요청이 있으면 최근 90일 내 동일 ticker/quarter/source 실패 기록을 확인한다.

기존 link-only 결과나 예전 경로는 운영 입력으로 사용하지 않는다.
운영 자료가 필요하면 새 raw/catalog 구조로 재수집한다.

## 4. 티커별 실행

각 티커마다 `harness/procedures/source-pack-collector.md`를 따른다.

Collector는 아래 산출물을 만들거나 갱신해야 한다.

- `artifacts/companies/{TICKER}/index.md`
- `artifacts/catalog/entities.jsonl`
- `artifacts/catalog/documents.jsonl`
- `artifacts/catalog/files.jsonl`
- `artifacts/runs/{run-id}/download-log.jsonl`
- `artifacts/runs/{run-id}/run-summary.md`
- 조건부: `artifacts/raw/...`
- 조건부: `artifacts/derived/text/...`

run-id는 `run-{YYYYMMDD}-{ticker-lower}`를 기본으로 한다.
이미 같은 run-id가 있으면 덮어쓰지 않고 `-v2`, `-v3`처럼 suffix를 붙인다.

## 5. QA 실행

티커별 Collector가 끝나면 즉시 `harness/procedures/source-pack-qa.md`를 실행한다.
평가는 `harness/rubrics/source-pack-qa.rubric.md`를 따른다.

QA 결과는 아래 파일에 남긴다.

- `artifacts/runs/{run-id}/qa.md`

QA 상태는 `pass`, `partial_pass`, `unverified`, `fail`, `stopped` 중 하나로 기록한다.
`fail` 또는 `unverified`가 나오면 운영 catalog와 회사별 index를 다음 하네스의 확정 입력으로 쓰지 않도록 표시한다.

## 6. 비교 모드

비교 모드는 기본적으로 입력 공유 방식으로 실행한다.

공유 입력:

- 같은 사용자 요청
- 같은 `watchlist.md`
- 같은 `config.md`
- 같은 기존 catalog
- 같은 기존 회사별 index

모델별 실행 결과는 분리된 run directory에 저장한다.
비교 모드에서는 운영 `catalog/*.jsonl`과 `artifacts/companies/{TICKER}/index.md`를 바로 덮어쓰지 않는다.
운영 반영은 비교 리포트 작성 후 사용자 승인으로만 진행한다.

## 7. 전체 산출물 정리

전체 실행이 끝나면 필요 시 아래 파일을 갱신한다.

- `artifacts/README.md`: 현재 catalog, 회사별 index, run 기록의 위치
- `artifacts/improvement-log.md`: 실행 중 발견한 하네스 개선 필요 사항

개선 기록에는 실제 규칙 변경을 섞지 않는다.
규칙 변경이 필요하면 사용자에게 변경 범위를 요약하고 별도 수정 단계로 넘긴다.

## 8. 사용자 보고

최종 보고는 `harness/schemas/source-pack-run-summary.schema.md` 형식을 따른다.

반드시 포함할 내용:

- 실행 모드
- 대상 티커
- 생성 또는 갱신된 run-id
- 회사별 상태
- 새로 수집한 SEC/IR/transcript 원자료 수
- raw 다운로드 실패와 사유
- QA 상태
- 다음 하네스가 읽어야 할 파일
- 사람 확인이 필요한 항목

대화에만 남기면 안 되는 내용은 반드시 파일에도 기록한다.

## 실패 처리

| 상황 | 처리 |
|---|---|
| `watchlist.md` 없음 | 사용자 직접 티커 입력으로 진행하거나 파일 생성 필요를 보고 |
| `config.md` 없음 | 기본값을 추정하지 말고 설정 파일 필요를 보고 |
| SEC User-Agent 없음 | 실행 중단 |
| CIK 조회 실패 | 해당 티커를 `stopped` 또는 `[확인 필요:]`로 기록 |
| raw 다운로드 실패 | `download-log.jsonl`에 실패 attempt를 기록하고 다음 후보로 이동 |
| SHA-256 검증 실패 | `files.jsonl` 승격 금지, `download-log.jsonl`에 사유 기록 |
| transcript 차단/유료벽 | optional 실패로 기록, 우회 시도 금지 |
| QA 실패 | 산출물 보존, 운영 입력 사용 위험 표시 |
| 사용자 승인 없음 | 운영 catalog/index 반영 중단 |

## 장애 진단 순서

이 섹션은 실행이 끝난 뒤 결과가 이상하거나 `failed`, `stopped`, `partial_success`, `repair_required`, `unverified` 같은 상태가 보일 때 사용한다.
실행 중 실패를 어떻게 처리할지는 위 `실패 처리`를 따르고, 실행 후 원인 파악은 아래 순서로 확인한다.

| 순서 | 볼 파일 | 확인할 것 |
|---:|---|---|
| 1 | `artifacts/catalog/runs.jsonl` | 최신 `run_id`, `status`, `collected_new`, `skipped_existing`, `repair_required`, `failed`, `files_collected_new` |
| 2 | `artifacts/runs/{run-id}/run-summary.md` | 실패와 확인 필요 요약, 영향받은 범위, 권장 다음 조치 |
| 3 | `artifacts/runs/{run-id}/download-log.jsonl` | 실제 attempt별 `attempt_status`, `http_status`, `error`, `source_url`, `target_path` |
| 4 | `artifacts/runs/{run-id}/qa.md` | 표준 14단계 중 실패 또는 미검증 지점, 다음 하네스 사용 가능 여부 |
| 5 | `artifacts/catalog/documents.jsonl`, `artifacts/catalog/files.jsonl` | `collection_status`, `primary_file_id`, `file_status`, `local_path`, `sha256`, `size_bytes` 관계 |

| 상태 | 의미 | 어디서 | 우선 행동 |
|---|---|---|---|
| `failed` | 수집 또는 QA 실패로 후속 사용이 위험함 | `runs.jsonl`, `qa.md` | `qa.md`와 `download-log.jsonl`에서 실패 단계와 attempt 오류를 확인 |
| `stopped` | 시작 전 조건 또는 transport/preflight/승인 문제로 중단 | `runs.jsonl`, `qa.md` | `config.md`, preflight 결과, User-Agent, 승인 조건을 확인 |
| `partial_success` | 일부는 성공했지만 일부 실패 또는 확인 필요가 있음 | `runs.jsonl`, `run-summary.md` | 사용 가능한 범위와 실패/보류 범위를 분리 |
| `repair_required` | 과거에는 수집 성공했으나 기존 파일 또는 catalog 관계가 깨짐 | `runs.jsonl` 집계, `files.jsonl`/`qa.md` 상세 | 자동 복구 금지, `files.jsonl.local_path`와 raw 파일 존재를 직접 확인 |
| `unverified` | catalog, raw 파일, index, run 관계 검증이 부족함 | `qa.md`, `index.md`의 `catalog_status` | 다음 하네스 확정 입력으로 사용하지 말고 QA 실패 지점을 보완 |
