# Source Pack QA Rubric

## 목적

이 rubric은 Source Pack QA 결과를 100점 만점으로 환산하기 위한 기준이다.

Source Pack은 분석형 리포트가 아니다.
점수는 해석의 깊이가 아니라 raw 원자료 확보, catalog 일관성, 파일 검증, 실패 기록, 다음 하네스 전달성으로 평가한다.

이 rubric은 `harness/procedures/source-pack-qa.md`를 대체하지 않는다.
QA procedure가 검증 절차이고, 이 rubric은 그 검증 결과를 점수화하는 기준이다.

## 평가 항목

| 항목 | 가중치 | 기준 |
|---|---:|---|
| Catalog 구조와 schema 준수 | 20 | `entities/documents/files/runs.jsonl` 형식, 허용값, 식별자 규칙 |
| Raw/File 무결성 | 20 | raw 파일 존재, SHA-256, size, `primary_file_id` 연결 |
| 수집 완전성과 출처 추적성 | 15 | Tier 1 범위, CIK, SEC/IR source URL, fallback, 누락 사유 |
| Run log와 실패 처리 | 15 | `download-log.jsonl`, `run-summary.md`, `qa.md`, 실패/보류/재시도 기록 |
| Index와 다음 하네스 전달성 | 10 | 회사별 `index.md`, catalog 참조, raw/text 경로, 다음 하네스 안내 |
| IR/Transcript optional 처리 | 10 | IR file_role, transcript 냉각 기간, 시도 제한, optional 실패 처리 |
| 금지 내용과 모델 중립성 | 10 | 분석/투자 판단 금지, adapter drift 방지, 공통 원장 사용 |

총점은 100점이다.

## 채점 방식

각 항목은 1-5점으로 평가한다.

최종 점수:

```text
sum(항목 점수 / 5 * 가중치)
```

자동 실패 조건이 있으면 점수와 무관하게 전체 판정은 `fail`이다.

여러 판정 조건이 겹치면 더 보수적인 판정을 우선한다.
예를 들어 optional source 실패만 있으면 `partial_pass`일 수 있지만, catalog 관계나 raw 파일 검증이 불완전하면 `unverified`를 우선한다.

## 항목별 1-5점 기준

| 항목 | 1점 | 3점 | 5점 |
|---|---|---|---|
| Catalog 구조와 schema 준수 | catalog 파일이 없거나 JSONL 파싱 불가, 허용값 위반 다수 | 핵심 catalog는 있으나 일부 필드, null 처리, 허용값, 상태 매핑 보완 필요 | 모든 catalog 파일이 JSONL 형식이고 ID, 허용값, 상태 매핑이 schema와 일치 |
| Raw/File 무결성 | `available` 파일이 없거나 hash/size/path가 부정확 | 주요 raw 파일은 있으나 일부 hash, size, content_type, file_role, 관계 검증 보완 필요 | raw 파일이 존재하고 `file_id`, SHA-256, size, `primary_file_id`, `files.jsonl` 관계가 일관 |
| 수집 완전성과 출처 추적성 | CIK, Tier 1 자료, source URL, 누락 사유가 불명확 | 핵심 SEC 자료는 있으나 일부 범위, fallback, filing/primary URL 구분이 부족 | CIK, Tier 1 범위, SEC primary URL, IR 출처, fallback, 누락 사유가 명확 |
| Run log와 실패 처리 | 다운로드/실패 시도가 기록되지 않거나 실패가 생략됨 | `download-log.jsonl`은 있으나 승격/실패/재시도 근거가 일부 부족 | 모든 시도와 실패가 `download-log`, `run-summary`, `qa`에 남고 files 승격과 분리됨 |
| Index와 다음 하네스 전달성 | `index.md` 필수 섹션 또는 다음 하네스 전달 요약 누락 | index는 있으나 catalog 참조, raw/text 경로, 확인 필요 연결이 일부 약함 | index가 catalog 지도 역할을 하며 다음 하네스가 읽을 record와 경로가 명확 |
| IR/Transcript optional 처리 | optional 실패를 전체 실패처럼 처리하거나 차단/유료벽 우회 시도 | optional 실패는 기록됐으나 IR file_role, transcript 냉각 기간, 시도 제한 일부 보완 필요 | IR `ir_deck/primary`, transcript optional, 90일 냉각 기간, 시도 제한, 실패 기록이 명확 |
| 금지 내용과 모델 중립성 | thesis, valuation, 투자 판단, transcript 분석이 포함되거나 adapter에 공통 규칙이 복사됨 | 금지 내용은 없으나 일부 표현이 분석처럼 보이거나 adapter/common 경계가 약함 | 수집 상태와 경로 안내만 있고 Claude/Codex adapter가 같은 공통 원장만 참조 |

## 판정

점수 판정은 자동 실패 조건이 없을 때만 적용한다.

| 점수 | QA `overall_status` | `runs.jsonl.status` | `index.md.catalog_status` | 의미 |
|---:|---|---|---|---|
| 90-100 | `pass` | `success` | `valid` | 다음 하네스가 사용할 수 있음 |
| 75-89 | `partial_pass` | `partial_success` | `partial` | 핵심 자료는 사용 가능하나 경미한 확인 필요 |
| 60-74 | `unverified` | `partial_success` 또는 `failed` | `unverified` | 후속 하네스 사용 전 검증 또는 보완 필요 |
| 0-59 | `fail` | `failed` | `failed` | 재수집, schema 수정, 또는 사람 확인 필요 |

`unverified`는 영향 범위에 따라 `runs.jsonl.status`를 `partial_success` 또는 `failed`로 둘 수 있다.
후속 하네스가 사용하면 위험한 경우에는 `failed`를 우선한다.

## 자동 실패 조건

아래 조건 중 하나라도 있으면 전체 QA 판정은 `fail`이다.

### Catalog 자동 실패

- 필수 catalog 파일이 없음: `entities.jsonl`, `documents.jsonl`, `files.jsonl`, `runs.jsonl`
- catalog JSONL 파싱 불가
- schema에 없는 `document_type`, `collection_status`, `file_status`, `file_role` 사용
- `link_only` 또는 동등한 링크-only 운영 상태 사용
- `companies/{TICKER}/sources.jsonl` 같은 이중 원장을 운영 산출물로 생성
- CIK가 없고 실패 사유도 없음
- `entity_id`가 `sec-cik-{10자리 CIK}` 형식을 따르지 않음

### Document/File 자동 실패

- `collection_status: collected`인데 `primary_file_id`가 없음
- `primary_file_id`가 `files.jsonl.file_id`와 연결되지 않음
- `files.jsonl.document_id`가 `documents.jsonl.document_id`와 연결되지 않음
- `files.jsonl.entity_id`가 `entities.jsonl.entity_id`와 연결되지 않음
- `file_status: available`인데 `local_path` 파일이 없음
- `size_bytes <= 0`
- `sha256`이 없거나 명백히 잘못됨
- 실패 다운로드가 `available` 파일로 승격됨
- 성공 attempt가 있고 검증 실패 사유도 없는데 `files.jsonl`에 승격되지 않음

### Index 자동 실패

- `artifacts/companies/{TICKER}/index.md` 없음
- 필수 index 섹션 누락
- Markdown 표 파싱 불가
- 표 구분선과 첫 데이터 행이 같은 줄에 붙어 있음
- 누락, 실패, 보류 건수가 음수
- 링크만 있고 raw 파일이 없는 문서를 `collected`로 표시
- 다음 하네스 전달 요약 누락

### Transcript/접근 자동 실패

- transcript 번역, 요약, Q&A 구조화, 투자 관점 분석이 catalog/raw/index/qa에 들어감
- Cloudflare, bot 차단, 로그인, 유료벽 우회 시도
- transcript 반복 실패를 무한 탐색하거나 냉각 기간 규칙을 무시함

### 분석/투자 판단 자동 실패

- 원자료 내용을 분석 리포트처럼 해석
- 투자 thesis 작성
- valuation 의견 작성
- 목표주가 작성
- 매수/매도/보유 판단 작성
- 경영진 또는 사업 품질에 대한 분석 결론 작성

## 부분 통과로 볼 수 있는 조건

아래 조건만 존재하고 catalog/raw/index 핵심 관계가 안전하면 `partial_pass`로 볼 수 있다.

- transcript 원문을 찾지 못했으나 `download-log.jsonl`과 `index.md`에 실패 이유가 있음
- IR 자료 일부를 찾지 못했으나 후보 URL, 실패 이유, 수동 확인 방법이 있음
- SEC exhibit 일부가 누락됐으나 primary document와 누락 사유가 있음
- text 추출을 실행하지 않았고 index의 `text 추출` 칸에 `-`로 표시함
- 성공 attempt가 있었으나 hash/size/content type 검증 실패 사유가 명확해 `files.jsonl` 승격을 의도적으로 차단함

## 미검증으로 볼 수 있는 조건

아래 조건은 후속 하네스 사용 전에 확인이 필요하므로 `unverified`를 우선한다.

- catalog 관계 일부가 불완전함
- raw 파일은 있으나 hash 재검증을 하지 못함
- `files.jsonl` record는 있으나 local path 접근을 확인하지 못함
- `documents.jsonl.source_url`이 filing folder URL인지 primary document URL인지 불명확함
- `index.md`와 catalog 숫자 또는 상태가 충돌함
- QA가 일부 파일을 읽지 못했지만 실패로 단정할 근거가 부족함

## 모델 중립성 기준

모델 중립성 항목은 아래를 본다.

- Claude/Codex adapter가 공통 규칙을 길게 복사하지 않고 `harness/` 원장을 참조하는가?
- 같은 입력에서 `catalog/*.jsonl` field 이름과 허용값이 모델별로 달라지지 않는가?
- 비교 모드에서 운영 catalog를 사용자 승인 없이 덮어쓰지 않는가?
- 모델별 산출물 차이는 `qa.md` 또는 비교 리포트에 남기고, 공통 원장을 drift시키지 않는가?

## 채점 결과 작성 형식

`qa.md` 또는 비교 리포트에 점수를 남길 때 아래 형식을 사용한다.

```md
## Rubric Score

total_score: {0-100}
overall_status: pass | partial_pass | unverified | fail | stopped

| 항목 | 가중치 | 점수(1-5) | 환산점 | 근거 |
|---|---:|---:|---:|---|
| Catalog 구조와 schema 준수 | 20 |  |  |  |
| Raw/File 무결성 | 20 |  |  |  |
| 수집 완전성과 출처 추적성 | 15 |  |  |  |
| Run log와 실패 처리 | 15 |  |  |  |
| Index와 다음 하네스 전달성 | 10 |  |  |  |
| IR/Transcript optional 처리 | 10 |  |  |  |
| 금지 내용과 모델 중립성 | 10 |  |  |  |

## Rubric 판정 근거
- 자동 실패 조건:
- partial_pass 근거:
- unverified 근거:
- 사람 승인 필요:
```
