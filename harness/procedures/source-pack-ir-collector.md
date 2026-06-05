# Source Pack IR Collector

이 파일은 Source Pack의 Company IR 자료 수집 절차서다.
첫 버전은 AAPL IR 소규모 pilot을 위한 얇은 절차로 시작한다.

SEC 공시 수집 절차는 `harness/procedures/source-pack-collector.md`가 담당한다.
이 파일은 SEC 수집 절차를 대체하지 않는다.

## 목적

회사 공식 IR, Newsroom, 또는 투자자 페이지에서 확인 가능한 IR 원자료를 수집하고, Source Pack 공통 catalog 계약에 맞춰 기록한다.

IR 자료는 회사마다 구조가 다르므로 전체 taxonomy를 미리 크게 만들지 않는다.
승인된 `run_scope` 안에서 작게 수집하고, 새 자료 유형은 candidate로 기록한 뒤 사용자 승인으로 확장한다.

## 적용 범위

이 절차는 아래 요청에 사용한다.

- IR 자료만 수집하거나 재확인하는 요청
- `run_scope`가 Company IR 자료를 포함하는 `test_collection`
- SEC 수집 뒤 회사 IR 자료를 순차 보완하는 요청

아래는 이 절차의 범위가 아니다.

- SEC EDGAR 원자료 수집
- webcast/audio 저장
- transcript 생성, 요약, 번역
- 로그인, 유료벽, 봇 차단 우회
- 투자 판단, valuation, 매수/매도 의견

## 1. IR Preflight

IR 자료를 수집하기 전에 아래를 확인한다.

| 항목 | 확인 내용 |
|---|---|
| 공식 출처 | 회사 공식 IR, Newsroom, 또는 회사가 명시한 투자자 자료 페이지인지 확인한다 |
| 접근성 | 로그인 없이 접근 가능한지 확인한다 |
| 파일 경로 | PDF/HTML 직링크 또는 안정적인 원문 URL이 있는지 확인한다 |
| 렌더링 | JavaScript 렌더링이 필요한지 확인한다 |
| 접근 제한 | robots.txt 또는 사이트 정책상 명백한 제한이 있는지 확인한다 |
| 범위 | 사용자가 승인한 `run_scope` 안의 자료인지 확인한다 |

`run_scope` 밖의 IR 자료를 발견해도 즉시 수집하지 않는다.
필요하면 run-summary의 확인 필요 또는 preflight 메모에 후보로 남긴다.

## 2. IR document_id 규칙

IR 자료에는 SEC accession number 같은 공식 고유 ID가 없다.
따라서 문서 의미 기준으로 `document_id`를 만든다.

기본 형식:

```text
ir-{ticker}-{document_type_slug}-{period_or_date}
```

기간 표기:

| 자료 성격 | 표기 |
|---|---|
| 실적 관련 자료 | `fy{YYYY}-q{N}` |
| 연간 관련 자료 | `fy{YYYY}` |
| 이벤트 관련 자료 | `{YYYY}-{MM}-{DD}` |
| 월 단위만 확인되는 자료 | `{YYYY}-{MM}` |
| 연도만 확인되는 자료 | `{YYYY}` |
| 동일 type/period 충돌 | 뒤에 `{short-slug}` 또는 `v2` 추가 |

예시:

```text
ir-aapl-earnings-release-fy2026-q2
ir-aapl-financial-supplement-fy2026-q2
```

`document_id`, `source_url`, `download_url`, `sha256`를 섞지 않는다.

- `document_id`: 문서 의미 기준 고유 이름
- `source_url`: 자료를 확인한 공식 페이지
- `download_url`: 실제 파일 다운로드 URL
- `sha256`: 파일 내용 동일성 검증용 지문

## 3. document_type 선택과 확장

초기 IR `document_type`은 아래 값을 우선 사용한다.

| document_type | 의미 | earnings-related |
|---|---|---|
| `ir-deck` | IR Presentation | 자료 성격에 따라 판단 |
| `ir-earnings-release` | 회사 IR 또는 Newsroom의 공식 실적 발표 자료 | yes |
| `ir-financial-supplement` | 회사 IR 또는 Newsroom의 실적 관련 재무 보충자료 | yes |

IR `document_type`은 controlled but extensible vocabulary다.

새 IR 자료 유형을 발견했을 때:

1. 기존 `document_type`으로 정확히 분류 가능한지 확인한다.
2. 억지 분류가 필요하면 기존 값에 넣지 않는다.
3. preflight 메모 또는 run-summary에 `candidate_document_type`으로 기록한다.
4. 사용자 승인 후 schema 추가 여부를 결정한다.
5. 새 값을 추가할 때 의미, 제외 기준, 기본 `file_role`, earnings-related 여부를 함께 정한다.

`other`는 임시 보류나 예외 기록에만 제한적으로 사용한다.
반복해서 등장하는 `other`는 taxonomy 확장 후보로 본다.

## 4. SEC overlap 처리

실적 관련 IR 자료는 SEC 8-K Item 2.02 또는 EX-99.1과 중복될 수 있다.
Pilot 단계와 production 단계를 구분한다.

### 4.1 Pilot 단계

`test_collection`의 목적은 IR 수집 경로 검증이다.
승인된 pilot 범위 안에서는 SEC 중복 가능성이 있어도 company-ir raw로 별도 저장할 수 있다.

Pilot 기록 원칙:

- hash, size, source_url, retrieved_at을 기록한다.
- notes에는 필요한 경우 `sec_overlap: likely`, `canonical_source: sec-edgar`를 남긴다.
- `pilot_duplicate_allowed` 같은 별도 플래그는 쓰지 않는다.
- `run_mode: test_collection`과 `run_scope`가 pilot 성격을 설명한다.
- 이 원칙은 production 중복 처리 원칙이 아니다.

### 4.2 Production 단계

Production에서는 운영 raw 저장소와 catalog의 중복을 줄인다.
Earnings-related로 명시된 IR `document_type`은 SEC overlap hash 비교 대상이다.

처리 흐름:

1. IR 후보 파일을 임시 위치에 다운로드한다.
2. hash와 size를 계산한다.
3. 기존 SEC `files.jsonl`에서 같은 hash가 있는지 찾는다.
4. hash가 같으면 company-ir raw로 승격하지 않는다.
5. hash가 같은 임시 파일은 삭제한다.
6. `documents.jsonl`에는 IR 출처 확인 또는 cross-reference만 남긴다.
7. notes에는 `canonical_source: sec-edgar`를 남긴다.
8. hash가 다르면 company-ir raw로 승격하고 notes에 overlap 관련 정보를 남긴다.

그 외 IR `document_type`은 SEC overlap hash 비교를 기본 요구하지 않는다.
다만 명백한 SEC 중복 의심 근거가 있으면 notes에 남기고 사람 확인 대상으로 둘 수 있다.

## 5. raw 저장 경로

Company IR raw 경로는 공통 계약을 따른다.

```text
artifacts/raw/company-ir/{TICKER}/{YYYY-MM-DD}_{slug}/
```

첫 pilot에서는 승인된 파일만 이 경로에 저장한다.
실패한 다운로드 시도는 `artifacts/runs/{run-id}/download-log.jsonl`에 남긴다.
검증된 파일만 `artifacts/catalog/files.jsonl`로 승격한다.

보안 제품이 다운로드된 IR 파일을 격리하거나 삭제한 경우 해당 파일을 복구하거나 열지 않는다.
이 파일은 운영 `documents.jsonl`, `files.jsonl`, 회사별 `index.md`에 승격하지 않는다.
download-log는 보존하고, run-summary/QA에는 `security_quarantined` 또는 이에 준하는 상태와 탐지 정보를 기록한다.

## 6. Webcast/Audio

첫 IR pilot에서 webcast/audio는 out-of-scope다.

하지 않는 일:

- streaming audio 저장
- replay audio 다운로드
- transcript 생성
- 오디오 요약, 번역, 분석
- 접근 제한 우회

필요하면 링크 또는 존재 여부만 확인 필요 항목으로 기록한다.
실제 audio/transcript 처리는 별도 transcript/audio 설계에서 다룬다.

## 7. AAPL 2파일 pilot 기준

AAPL IR 첫 pilot의 권장 범위는 아래 2건이다.

| 후보 | document_type | 형식 | 비고 |
|---|---|---|---|
| FY2026 Q2 earnings press release | `ir-earnings-release` | HTML | SEC overlap 가능성 높음 |
| FY2026 Q2 consolidated financial statements PDF | `ir-financial-supplement` | PDF | SEC overlap 가능성 높음 |

권장 `run_scope`:

```text
test only: AAPL company-ir official Apple FY2026 Q2 earnings press release HTML and consolidated financial statements PDF; no SEC download; no SEC filings page harvesting; no webcast/audio; no transcript; no operating catalog merge until user approval
```

이 범위는 사용자가 승인한 경우에만 실행한다.

## 8. 완료 전 점검

IR collector는 완료 전 아래를 확인한다.

- 승인된 `run_scope` 밖 자료를 수집하지 않았는가?
- 공식 출처와 접근 가능성을 확인했는가?
- `document_id`가 IR 규칙을 따르는가?
- `document_type`이 schema 허용값 안에 있는가?
- 새 유형을 억지 분류하지 않고 candidate로 기록했는가?
- earnings-related 자료의 SEC overlap 메모를 남겼는가?
- webcast/audio/transcript를 수집하지 않았는가?
- 검증된 파일만 `files.jsonl`로 승격했는가?
- 보안 제품이 격리한 파일을 복구/열람/승격하지 않았는가?
