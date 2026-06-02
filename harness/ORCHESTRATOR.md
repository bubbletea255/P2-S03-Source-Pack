# Source Pack Orchestrator

이 문서는 Source Pack v2 하네스의 상위 실행 원장이다.
Claude Code와 Codex adapter는 이 문서를 단일 원본으로 읽고 실행한다.

`source-pack-orchestrator`는 사용자 요청을 받는 adapter 진입점 이름이다.
실제 1회 실행 절차는 `harness/procedures/source-pack-runbook.md`가 담당한다.

## 목적

Source Pack은 가치투자 리서치 21단계 중 P2-S03의 원자료 수집 하네스다.
투자 결론을 내리지 않고, 다음 하네스들이 신뢰할 수 있는 SEC 공시, IR 자료, 실적 발표 원자료, 가능한 경우 transcript 원문을 로컬 파일과 catalog 원장으로 정리한다.

이 하네스는 링크 북마크가 아니다.
다음 하네스가 반복해서 읽을 수 있는 원자료 저장소와 기계용 catalog를 만드는 것이 목적이다.

## 하네스 유형과 품질 축

| 항목 | 선언 |
|---|---|
| 하네스 유형 | 수집형 |
| 산출물 역할 | 원자료 저장소, 원자료 인덱스, 다음 하네스 입력 패키지 |
| 품질 축 | 출처 정확성, catalog 일관성, raw 파일 재현성, 실패 기록 투명성 |
| 수준 선언 | 수집 엄격도 - 표준 수집 |

Source Pack에는 분석형 리포트의 깊이 기준을 그대로 적용하지 않는다.
판단이 필요한 내용은 다음 분석 하네스로 넘기고, Source Pack에는 원자료 위치와 확인 필요 표시만 남긴다.

## 운영 산출물

필수 산출물:

- `artifacts/companies/{TICKER}/index.md`
- `artifacts/catalog/entities.jsonl`
- `artifacts/catalog/documents.jsonl`
- `artifacts/catalog/files.jsonl`
- `artifacts/catalog/runs.jsonl`
- `artifacts/runs/{run-id}/download-log.jsonl`
- `artifacts/runs/{run-id}/run-summary.md`
- `artifacts/runs/{run-id}/qa.md`

조건부 산출물:

- `artifacts/raw/sec-edgar/...`
- `artifacts/raw/company-ir/...`
- `artifacts/raw/transcripts/...`
- `artifacts/derived/text/...`

`artifacts/raw/`와 `artifacts/derived/`는 대용량 원자료이므로 git에 커밋하지 않는다.

## 실행 모드

| 모드 | 조건 | 처리 |
|---|---|---|
| `new_collection` | 해당 티커의 catalog/index가 없음 | 새 run을 만들고 원자료를 수집한다 |
| `incremental_update` | 기존 catalog/index가 있음 | 기존 원장을 기준으로 새 문서만 추가한다 |
| `partial_recheck` | 특정 영역만 요청 | 요청된 SEC/IR/transcript/QA 영역만 다시 확인한다 |
| `test_collection` | 수집 파이프라인 검증을 위한 제한 범위 요청 | 선언된 `run_scope` 안에서만 수집하고 평가한다 |
| `comparison` | Claude/Codex 또는 두 실행 비교 | 입력을 공유하고 결과를 분리 저장한다 |

## 비교 모드 원칙

비교 모드의 기본값은 `입력 공유`다.
같은 사용자 요청, 같은 `watchlist.md`, 같은 `config.md`, 같은 기존 catalog와 index를 Claude와 Codex가 함께 읽어야 모델 차이를 해석할 수 있다.

| 방식 | 사용 상황 | 비교 범위 |
|---|---|---|
| 입력 공유 | 기본값. 모델 차이만 보고 싶을 때 | 같은 입력을 기준으로 수집, 기록, QA 품질 비교 |
| 각자 생성 | 입력 해석 차이까지 보고 싶을 때 | 티커 해석, 설정 요약, 수집 판단까지 포함한 전체 실행 차이 비교 |

비교 모드에서는 운영 `catalog/*.jsonl`과 `artifacts/companies/{TICKER}/index.md`를 바로 덮어쓰지 않는다.
각 실행 결과를 분리된 `artifacts/runs/{run-id}/`에 저장한 뒤 비교 리포트를 작성한다.
운영 catalog/index 반영은 사용자 승인 후에만 진행한다.

## 입력

- `watchlist.md`: 관심 종목 티커 목록
- `config.md`: 수집 범위, 속도 제한, 필터, SEC User-Agent
- `artifacts/catalog/*.jsonl`: 기존 기계용 원장
- `artifacts/companies/{TICKER}/index.md`: 기존 사람용 회사별 지도
- `artifacts/runs/{run-id}/download-log.jsonl`: 이전 실패와 cooldown 판단 근거
- 사용자 직접 입력 티커

## 실행 흐름

1. 사용자 요청이 신규 수집, 증분 업데이트, 부분 재검토, 테스트 실행, 비교 모드 중 무엇인지 판단한다.
2. `harness/procedures/source-pack-runbook.md`를 읽고 실행 단계를 따른다.
3. `watchlist.md`와 사용자 입력을 기준으로 대상 티커를 확정한다.
4. `config.md`를 읽어 수집 범위, 속도 제한, User-Agent를 확인한다.
5. 수집 전 사용자에게 대상 티커와 핵심 설정을 요약하고 승인받는다.
6. 대상 티커를 순차 처리한다. SEC/IR/transcript 요청은 병렬화하지 않는다.
7. 티커별 수집은 `harness/procedures/source-pack-collector.md`를 따른다.
8. 티커별 검증은 `harness/procedures/source-pack-qa.md`와 `harness/rubrics/source-pack-qa.rubric.md`를 따른다.
9. `artifacts/README.md`와 `artifacts/improvement-log.md`를 필요한 만큼 갱신한다.
10. 사용자에게 성공, 부분 성공, 실패, 확인 필요 항목을 요약한다.

## run-id 규칙

기본 형식:

```text
run-{YYYYMMDD}-{ticker-lower}
```

예시:

```text
artifacts/runs/run-20260602-aapl/
artifacts/runs/run-20260602-aapl-v2/
artifacts/runs/run-20260602-aapl-codex/
artifacts/runs/run-20260602-aapl-claude/
```

기존 폴더가 있으면 덮어쓰지 않고 `-v2`, `-v3` 또는 모델 suffix를 붙인다.

## 중단 조건

- `config.md`의 SEC User-Agent가 비어 있음
- SEC 429가 반복되어 재시도 한도를 초과함
- CIK 조회가 실패하고 수동 확인도 불가능함
- 사용자가 설정 확인 단계에서 진행을 승인하지 않음
- 운영 catalog/index 덮어쓰기가 필요한데 사용자 승인이 없음
- transcript 유료벽, 로그인, 봇 차단 우회가 필요한 요청

중단 시에도 가능한 범위의 결과와 확인 필요 항목을 `artifacts/runs/{run-id}/`에 남긴다.

## 금지 사항

- Source Pack에서 투자 thesis, valuation, 매수/매도 의견을 작성하지 않는다.
- SEC/IR/transcript 원자료를 요약, 번역, 해석하지 않는다.
- link-only 산출물을 운영 출력으로 만들지 않는다.
- `sources.jsonl`을 만들지 않는다.
- 예전 경로 `artifacts/{TICKER}/phase2/step3-source-pack/index.md`를 운영 산출물로 사용하지 않는다.
- 비교 모드 결과를 사용자 승인 없이 운영 catalog에 병합하지 않는다.

## 수정 경계

| 바꾸고 싶은 것 | 수정 위치 |
|---|---|
| 하네스 목적, 실행 모드, 전체 흐름, 중단 조건 | `harness/ORCHESTRATOR.md` |
| 1회 실행 순서와 사용자 승인 절차 | `harness/procedures/source-pack-runbook.md` |
| 수집 대상, 완료 기준, 금지사항 | `harness/contracts/source-pack.contract.md` |
| 실제 수집 절차 | `harness/procedures/source-pack-collector.md` |
| QA 절차 | `harness/procedures/source-pack-qa.md` |
| catalog 원장 형식 | `harness/schemas/source-pack-catalog.schema.md` |
| 회사별 index 형식 | `harness/schemas/source-pack-index.schema.md` |
| 실행 요약 형식 | `harness/schemas/source-pack-run-summary.schema.md` |
| 품질 검증 기준 | `harness/rubrics/source-pack-qa.rubric.md` |
| Codex 실행 방식 | `.agents/skills/` |
| Claude 실행 방식 | `.claude/agents/`, `.claude/skills/` |

공통 업무 규칙은 adapter에 길게 복사하지 않는다.
