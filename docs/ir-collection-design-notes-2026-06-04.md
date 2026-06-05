# IR 자료 수집 구축 설계 메모 1

- 작성일: 2026-06-04
- 대상 하네스: P2-S03 Source Pack
- 주제: Company IR 자료 수집 확장 설계
- 상태: 구현 전 설계 합의 보관
- 적용 전제: 아직 `harness/` 운영 규칙에는 반영하지 않음

이 문서는 AAPL IR 소규모 pilot을 준비하면서 Codex와 Claude Code가 교차 논의한 내용을 보존하기 위한 설계 메모다.
현재 파일은 논의 보관용이며, 이 자체가 schema, collector, QA 규칙을 변경하지 않는다.

## 1. 왜 이 메모를 남기는가

SEC 수집 규칙은 이미 Source Pack 하네스 안에 비교적 안정적으로 구축되어 있다.
반면 IR 자료는 회사마다 페이지 구조, 자료명, 파일 형식, 보관 방식이 다르기 때문에 SEC와 같은 방식으로 바로 일반화하기 어렵다.

따라서 IR 규칙을 곧바로 `harness/`에 추가하기 전에, 다음 내용을 먼저 기록한다.

- 실제 Apple IR 사전 조사에서 확인한 사항
- IR 자료 식별 방식
- SEC-IR 중복 처리 원칙
- IR document_type 확장 원칙
- pilot 단계와 production 단계의 차이
- 앞으로 하네스 구조를 어떻게 분리하거나 모듈화할지 논의할 때 필요한 기준

핵심 목적은 논의 내용을 대화에만 남기지 않고, 이후 구조 설계와 구현의 기준점으로 보존하는 것이다.

## 2. 현재 합의된 큰 방향

IR 자료 수집은 바로 전체 taxonomy를 크게 만들지 않는다.
대신 작은 active taxonomy로 시작하고, 새로운 자료 유형이 발견되면 승인된 절차로 확장한다.

합의된 방향:

- SEC 수집 안정성을 건드리지 않는다.
- IR 수집 규칙은 작게 시작한다.
- `other`에 모든 IR 자료를 뭉뚱그리지 않는다.
- 아직 확인되지 않은 IR 유형을 미리 대량으로 schema에 넣지 않는다.
- 새 IR 유형은 발견, 후보 기록, 승인, schema 반영 순서로 확장한다.
- Apple IR pilot은 전체 IR 자동 수집이 아니라 작은 수집 경로 검증으로 본다.

## 3. AAPL IR 사전 조사에서 확인한 사항

별도 사전 조사 메모:

```text
docs/aapl-ir-preflight-2026-06-04.md
```

확인한 공식 출처:

- Apple Investor Relations: https://investor.apple.com/investor-relations/
- Apple SEC Filings: https://investor.apple.com/investor-relations/sec-filings/default.aspx
- Apple Earnings Call: https://www.apple.com/investor/earnings-call/
- Apple FY2026 Q2 earnings press release: https://www.apple.com/newsroom/2026/04/apple-reports-second-quarter-results/
- Apple FY2026 Q2 consolidated financial statements PDF: https://www.apple.com/newsroom/pdfs/fy2026q2/FY26_Q2_Consolidated_Financial_Statements.pdf

확인한 Apple IR 주요 섹션:

- Investor Updates
- Newsroom
- Financial Data
- Quarterly Earnings Reports
- SEC Filings
- Leadership and Governance
- Our Values
- FAQ
- Contact

첫 pilot 후보:

1. FY2026 Q2 earnings press release HTML
2. FY2026 Q2 consolidated financial statements PDF

이 두 후보는 Apple 공식 출처이며, HTML과 PDF 수집 경로를 작게 검증할 수 있다는 장점이 있다.
다만 SEC 8-K Item 2.02 및 exhibit와 중복될 가능성이 높다.

## 4. IR document_id 생성 원칙

SEC 문서에는 accession number가 있어 고유 식별이 쉽다.
IR 자료에는 그에 해당하는 공식 고유번호가 없으므로 Source Pack 내부 규칙으로 `document_id`를 만들어야 한다.

기본 공식:

```text
ir-{ticker}-{document_type_slug}-{period_or_date}
```

기간 표기 규칙:

| 자료 성격 | 기간 표기 |
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

구분 원칙:

- `document_id`: 문서 의미 기준 고유 이름
- `source_url`: 자료를 확인한 공식 페이지
- `download_url`: 실제 파일 다운로드 URL
- `sha256`: 파일 내용 동일성 검증용 지문

URL이나 hash를 `document_id` 자체로 쓰지 않는다.
URL은 바뀔 수 있고, hash는 파일 내용의 지문이지 문서 의미의 이름표가 아니기 때문이다.

## 5. IR document_type 초기 후보

현재 논의에서 첫 pilot에 필요한 최소 추가 후보:

```text
ir-earnings-release
ir-financial-supplement
```

기존 유지:

```text
ir-deck
```

명칭 조정:

- `ir-financial-statements`보다 `ir-financial-supplement`를 선호한다.
- 이유: Apple PDF 제목은 consolidated financial statements지만, 일반 IR taxonomy에서는 supplement가 더 넓고 회사 간 적용 가능성이 높다.

아직 추가하지 않는 후보:

- Investor Day
- conference presentation
- ESG report
- governance policy
- product strategy deck
- shareholder letter
- 기타 회사별 IR-native 자료

이 후보들은 실제 다른 회사 IR 사전 조사에서 반복적으로 발견되면 candidate로 기록한 뒤 schema 반영을 검토한다.

## 6. Open-Controlled Vocabulary 원칙

IR `document_type`은 closed list처럼 고정하지 않는다.
동시에 아무 값이나 즉석에서 쓰는 free text로도 두지 않는다.

권장 원칙:

```text
IR document_type은 controlled but extensible vocabulary다.
```

쉬운 뜻:

- 아무 이름이나 즉석에서 만들지 않는다.
- 기존 type에 억지로 끼워 넣지 않는다.
- 새 공식 IR 자료 유형이 발견되면 후보로 기록한다.
- 사용자 승인 후 schema에 추가한다.
- 새 type을 추가할 때 의미와 처리 규칙을 같이 정한다.

새 IR document_type 추가 시 함께 정할 항목:

| 항목 | 설명 |
|---|---|
| 이름 | `ir-...` 형식의 document_type |
| 의미 | 어떤 자료를 이 type으로 볼지 |
| 제외 기준 | 비슷하지만 이 type이 아닌 자료 |
| earnings-related 여부 | SEC overlap hash 비교 대상인지 |
| 기본 file_role | `primary`, `ir_deck` 등 |
| 예시 | 실제 회사 IR 페이지에서 확인한 예 |

## 7. 새 IR 유형 발견 시 처리 규칙

새 자료 유형을 발견했을 때의 기본 흐름:

```text
새 IR 자료 발견
→ 기존 document_type으로 정확히 분류 가능한지 확인
→ 억지 분류가 필요하면 기존 type에 넣지 않음
→ preflight 또는 run-summary에 candidate_document_type으로 기록
→ 사용자 승인 후 schema 추가 여부 결정
→ schema 반영 후 pilot 또는 정식 수집
```

중요 원칙:

- 모르는 자료를 `ir-deck`이나 `other`에 습관적으로 밀어 넣지 않는다.
- `other`는 임시 보류나 예외 기록에만 제한적으로 사용한다.
- 반복해서 나오는 `other`는 taxonomy 확장 후보로 본다.

### 7.1 issuer label과 handled_as

회사가 자료에 붙인 이름과 Source Pack의 `document_type`은 다를 수 있다.
회사별 자료명 차이를 모두 새 `document_type`으로 만들지 않는다.

구분:

| 메모 | 의미 | 사용 상황 |
|---|---|---|
| `source_label` | 회사가 자료에 붙인 원래 이름 | 회사마다 부르는 이름만 다르고 기존 `document_type`으로 처리 가능한 경우 |
| `handled_as` | Source Pack에서 실제로 적용한 `document_type` | source label을 기존 taxonomy 안에 흡수한 경우 |
| `candidate_document_type` | 새 schema 값 후보 | 기존 `document_type`으로 처리하면 의미가 왜곡되고 반복 관찰되는 경우 |

예시:

```text
source_label: Financial Update; handled_as: ir-financial-supplement
```

위 예시는 회사가 자료를 `Financial Update`라고 부르지만, Source Pack에서는 실적 관련 재무 보충자료로 보아 `ir-financial-supplement`로 처리한다는 뜻이다.
이 경우 `ir-financial-update`는 candidate vocabulary가 아니다.

반대로, 기존 `document_type`으로 처리하면 의미가 왜곡되는 자료가 여러 회사에서 반복 관찰되면 아래처럼 기록한다.

```text
candidate_document_type: ir-shareholder-letter
```

이 구분은 taxonomy가 회사별 명칭 차이 때문에 불필요하게 늘어나는 것을 막기 위한 규칙이다.

## 8. SEC-IR 중복 처리 원칙

Apple 실적 자료처럼 IR/Newsroom 자료와 SEC 8-K exhibit가 겹칠 수 있다.
따라서 pilot 단계와 production 단계를 분리한다.

### 8.1 Pilot 단계

Pilot의 목적은 IR 수집 경로 검증이다.
중복 제거 최적화가 목적이 아니다.

Pilot 원칙:

- 승인된 pilot 범위 안에서는 SEC 중복 가능성이 있어도 company-ir raw로 별도 저장할 수 있다.
- hash, size, source_url, retrieved_at을 기록한다.
- notes에는 `sec_overlap: likely`, `canonical_source: sec-edgar` 정도를 남긴다.
- `pilot_duplicate_allowed` 같은 별도 플래그는 쓰지 않는다.
- `run_mode: test_collection`과 `run_scope`가 pilot 성격을 설명한다.
- 이 원칙은 production 원칙이 아니다.

### 8.2 Production 단계

Production의 목적은 운영 raw 저장소와 catalog를 깨끗하게 유지하는 것이다.
같은 파일을 중복 저장하지 않는 방향을 기본으로 한다.

Earnings-related로 명시된 IR document_type은 SEC overlap hash 비교를 필수로 한다.

처리 흐름:

```text
1. IR 후보 파일을 임시 위치에 다운로드한다.
2. hash와 size를 계산한다.
3. 기존 SEC files.jsonl에서 같은 hash가 있는지 찾는다.
4. hash가 같으면:
   - company-ir raw로 승격하지 않는다.
   - 임시 파일은 삭제한다.
   - documents.jsonl에는 IR 출처 확인 또는 cross-reference만 남긴다.
   - canonical_source: sec-edgar로 기록한다.
5. hash가 다르면:
   - company-ir raw로 승격한다.
   - notes에 sec_overlap 관련 정보를 남긴다.
```

그 외 IR document_type은 SEC overlap hash 비교를 기본 요구하지 않는다.
다만 명백한 SEC 중복 의심 근거가 있으면 notes에 남기고 사람 확인 대상으로 둘 수 있다.

## 9. Earnings-Related 판단 기준

현재 earnings-related로 볼 후보:

```text
ir-earnings-release
ir-financial-supplement
```

원칙:

- 실적 발표, 분기 결과, 재무 보충자료처럼 SEC 8-K Item 2.02 또는 EX-99.1과 겹칠 가능성이 큰 자료는 earnings-related로 본다.
- 새 IR document_type을 추가할 때 earnings-related 여부를 함께 정한다.
- 아직 schema에 없는 document_type 목록을 미리 확정된 것처럼 나열하지 않는다.

## 10. Access Check 원칙

IR 수집 전에는 접근 가능성을 확인한다.

확인 항목:

- 공식 출처인지
- 로그인 없이 접근 가능한지
- PDF/HTML 직링크가 있는지
- JavaScript 렌더링이 필요한지
- robots.txt 또는 사이트 정책상 명백한 제한이 있는지
- 다운로드가 안정적으로 되는지

이번 AAPL pilot의 경우:

- `www.apple.com/newsroom/...` HTML과 PDF 직링크만 사용한다면 `investor.apple.com/robots.txt` 미확인은 pilot 블로커가 아니다.
- `investor.apple.com` 페이지를 자동 탐색해서 링크를 수집한다면 `investor.apple.com/robots.txt` 확인은 pilot 전 필수다.

## 11. Webcast/Audio 처리

첫 AAPL IR pilot에서 webcast/audio는 out-of-scope다.

이유:

- streaming/replay 성격이 강하다.
- replay가 일정 기간 후 사라질 수 있다.
- 다운로드, 보관, transcript 변환, 저작권, 접근 제한 문제가 섞인다.
- Source Pack IR pilot의 난이도를 불필요하게 올린다.

초기에는 링크 또는 존재 여부만 기록 후보로 두고, 실제 수집은 별도 transcript/audio 하네스에서 다룬다.

## 12. Preflight 메모 정리 필요 사항

현재 preflight 메모에서 나중에 정리할 항목:

```text
docs/aapl-ir-preflight-2026-06-04.md
```

정리 후보:

- `ir-financial-statements` 표현을 `ir-financial-supplement`로 변경
- pilot/production SEC-IR 중복 처리 원칙 반영
- `하네스 운영 관찰` 섹션 제거
- schema와 collector 문서가 확정된 뒤 그 용어를 반영

관측 섹션은 원래 `run-summary.md`에 붙는 선택 섹션이다.
preflight 메모는 run이 아니므로 관측 메트릭 위치로는 적절하지 않다.

## 13. 예상 파일 수정 순서

실제 하네스에 반영한다면 권장 순서는 다음과 같다.

```text
1. harness/schemas/source-pack-catalog.schema.md
   - ir-earnings-release 추가
   - ir-financial-supplement 추가
   - open-controlled vocabulary 원칙 추가
   - 새 IR type 추가 시 earnings-related 여부 판단 규칙 추가

2. harness/procedures/source-pack-collector.md
   - 새 IR 유형 candidate 기록 규칙 추가
   - earnings-related IR hash 비교 흐름 추가
   - pilot과 production 중복 처리 차이 명시

3. docs/aapl-ir-preflight-2026-06-04.md
   - 확정된 schema/collector 용어 반영
   - 관측 섹션 제거
   - pilot run_scope 문구 정리
```

preflight 메모는 schema와 collector에서 정의된 이름과 원칙을 참조하므로 마지막에 수정하는 것이 좋다.

## 14. Pilot 이후 구조 논의 아젠다

IR 규칙을 기존 `harness/` 문서에 그대로 추가하면 지침이 더 무거워질 수 있다.
다만 아래 질문들은 AAPL IR 2파일 pilot의 기술적 전제 조건은 아니다.

이 질문들은 Source Pack이 SEC, IR, transcript 자료를 모두 다루면서 커질 때 중요해질 구조 설계 아젠다다.
따라서 pilot을 막는 블로커가 아니라, pilot 이후 실제 실행 경험을 바탕으로 다시 검토할 후보로 보존한다.

다음 질문을 별도로 검토한다.

- SEC와 IR 규칙을 같은 collector 문서에 계속 둘 것인가?
- `harness2/`처럼 별도 폴더를 만들 것인가?
- 아니면 `harness/` 안에서 `sec`, `ir`, `transcript` source module로 나눌 것인가?
- 공통 catalog schema는 하나로 유지할 것인가?
- Orchestrator는 모든 지침을 매번 읽을 것인가, 요청 범위별로 필요한 source module만 읽을 것인가?
- IR taxonomy와 SEC taxonomy를 같은 파일에 둘 것인가, 별도 vocabulary 파일로 분리할 것인가?

## 15. 현재 진행 상태

기술적으로는 13섹션의 순서대로 최소 수정 후 AAPL IR 2파일 pilot을 진행할 수 있다.
즉, 아래 순서는 여전히 유효하다.

```text
schema 수정
→ collector 수정
→ preflight 메모 정리
→ AAPL IR 2파일 pilot
```

다만 사용자가 전체 하네스 구조 분리 가능성을 먼저 논의하고 싶다고 판단했으므로, 현재는 구현을 잠시 보류하고 구조 논의를 진행할 수 있다.

이 보류는 기술적 블로커가 아니라 진행 전략상의 선택이다.
구조 논의를 먼저 하더라도, pilot 실행에 필요한 IR 규칙 합의는 1~13섹션에 보존되어 있다.

한 줄 요약:

```text
IR 수집 pilot은 최소 수정으로 진행 가능하며, source별 모듈화 논의는 pilot 이후 또는 사용자가 원할 때 별도 아젠다로 다룬다.
```
