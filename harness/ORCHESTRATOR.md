# Source Pack Orchestrator

이 문서는 Source Pack v2 하네스의 공통 실행 원장이다.
Claude Code와 Codex adapter는 이 문서를 단일 원본으로 읽고 실행한다.

## 목적

Source Pack은 가치투자 리서치 21단계 중 Phase 2 Step 3의 원자료 인덱스 하네스다.
투자 결론을 내리지 않고, 다음 하네스들이 신뢰할 수 있는 공시, IR, 실적 발표 자료, 대본, 산업/경쟁 자료 링크를 회사별로 정리한다.

최종 산출물:

- `artifacts/{TICKER}/phase2/step3-source-pack/index.md`

## 실행 모드

| 모드 | 조건 | 산출물 |
|---|---|---|
| 신규 수집 | 회사별 `index.md`가 없음 | 전체 범위 수집 |
| 증분 업데이트 | 회사별 `index.md`가 있음 | `last_updated` 이후 새 자료 추가 |
| 부분 재검토 | 특정 섹션만 요청 | 해당 섹션 갱신 및 QA |
| 비교 모드 | Claude/Codex 비교 요청 | 동일 입력 기준 결과 비교 |

## 비교 모드 원칙

비교 모드의 기본값은 `입력 공유`다.
같은 사용자 요청, 같은 `watchlist.md`, 같은 `config.md`, 같은 기존 `index.md`를 Claude와 Codex가 함께 읽어야 모델 차이를 해석할 수 있다.

| 방식 | 사용 상황 | 비교 범위 |
|---|---|---|
| 입력 공유 | 기본값. 모델 차이만 보고 싶을 때 | 같은 입력을 기준으로 수집, 정리, QA 품질 비교 |
| 각자 생성 | 입력 해석 차이까지 보고 싶을 때 | 티커 해석, 설정 요약, 수집 판단까지 포함한 전체 실행 차이 비교 |

비교 모드에서는 기존 회사별 누적 `index.md`를 바로 덮어쓰지 않는다.
각 실행 결과를 `artifacts/run-YYYYMMDD-source-pack-{ticker}-claude/`와 `artifacts/run-YYYYMMDD-source-pack-{ticker}-codex/`에 분리 저장한 뒤 비교 리포트를 작성한다.
누적 `index.md` 반영은 사용자 승인 후에만 진행한다.

## 입력

- `watchlist.md`: 관심 종목 티커 목록
- `config.md`: 수집 범위, 속도 제한, 필터, User-Agent
- 기존 `artifacts/{TICKER}/phase2/step3-source-pack/index.md`
- 사용자 직접 입력 티커

## 실행 흐름

1. 사용자 요청이 신규 수집, 증분 업데이트, 부분 재검토, 비교 모드 중 무엇인지 판단한다.
2. `watchlist.md`와 사용자 입력을 기준으로 대상 티커를 확정한다.
3. `config.md`를 읽어 수집 범위와 속도 제한을 확인한다.
4. 수집 전 사용자에게 대상 티커와 핵심 설정을 요약하고 승인받는다.
5. 대상 티커를 순차 처리한다. SEC/IR 요청은 병렬화하지 않는다.
6. 각 티커에 대해 `harness/procedures/source-pack-collector.md`를 따른다.
7. 생성 또는 갱신된 `index.md`를 `harness/procedures/source-pack-qa.md`와 `harness/rubrics/source-pack-qa.rubric.md`로 점검한다.
8. `artifacts/README.md`와 `artifacts/improvement-log.md`를 필요한 만큼 갱신한다.
9. 사용자에게 성공, 부분 성공, 실패, 확인 필요 항목을 요약한다.

## run-id 규칙

Source Pack은 회사별 누적 인덱스를 유지하므로 기본 산출물은 티커 경로에 저장한다.
비교 모드나 별도 평가 실행은 아래 형식을 사용한다.

```text
artifacts/run-YYYYMMDD-source-pack-{ticker}/
artifacts/run-YYYYMMDD-source-pack-{ticker}-claude/
artifacts/run-YYYYMMDD-source-pack-{ticker}-codex/
artifacts/comparison-run-YYYYMMDD-source-pack-{ticker}.md
```

기존 폴더가 있으면 덮어쓰지 않고 `-v2`, `-v3`를 붙인다.

## 중단 조건

- `config.md`의 User-Agent가 비어 있음
- SEC 429가 반복되어 재시도 한도를 초과함
- CIK 조회가 실패하고 수동 확인도 불가능함
- 사용자가 설정 확인 단계에서 진행을 승인하지 않음

중단 시에도 가능한 범위의 결과와 확인 필요 항목을 파일에 남긴다.

## 수정 경계

| 바꾸고 싶은 것 | 수정 위치 |
|---|---|
| 하네스 목적, 실행 모드, 전체 흐름 | `harness/ORCHESTRATOR.md` |
| 수집 대상, 완료 기준, 금지사항 | `harness/contracts/source-pack.contract.md` |
| 실제 수집 절차 | `harness/procedures/source-pack-collector.md` |
| 출력 형식 | `harness/schemas/source-pack-index.schema.md` |
| 품질 검증 기준 | `harness/rubrics/source-pack-qa.rubric.md` |
| Codex 실행 방식 | `.agents/skills/` |
| Claude 실행 방식 | `.claude/agents/`, `.claude/skills/` |

공통 업무 규칙은 adapter에 길게 복사하지 않는다.
