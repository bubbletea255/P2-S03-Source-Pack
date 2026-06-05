# AAPL IR 사전 조사 메모

- 작성일: 2026-06-04
- 대상: AAPL / Apple Inc.
- 범위: Apple 공식 IR 및 Apple 공식 Newsroom/Investor 페이지 확인
- 비범위: raw 다운로드, catalog 갱신, 회사별 index 갱신, SEC EDGAR 재수집, transcript 수집, webcast/audio 저장

이 문서는 첫 AAPL IR 소규모 pilot 전에 작성하는 사전 조사 메모다.
실행 산출물이 아니며, 운영 catalog 또는 QA 판정의 근거로 바로 사용하지 않는다.

## 1. Apple IR 실제 섹션

확인한 공식 페이지:

- Apple Investor Relations: [https://investor.apple.com/investor-relations/](https://investor.apple.com/investor-relations/)
- Apple SEC Filings: [https://investor.apple.com/investor-relations/sec-filings/default.aspx](https://investor.apple.com/investor-relations/sec-filings/default.aspx)
- Apple Earnings Call: [https://www.apple.com/investor/earnings-call/](https://www.apple.com/investor/earnings-call/)
- Apple Leadership and Governance: [https://investor.apple.com/investor-relations/leadership-and-governance/default.aspx](https://investor.apple.com/investor-relations/leadership-and-governance/default.aspx)
- Apple FAQ: [https://investor.apple.com/investor-relations/faq/default.aspx](https://investor.apple.com/investor-relations/faq/default.aspx)
- Apple FY2026 Q2 earnings press release: [https://www.apple.com/newsroom/2026/04/apple-reports-second-quarter-results/](https://www.apple.com/newsroom/2026/04/apple-reports-second-quarter-results/)
- Apple FY2026 Q2 consolidated financial statements PDF: [https://www.apple.com/newsroom/pdfs/fy2026q2/FY26_Q2_Consolidated_Financial_Statements.pdf](https://www.apple.com/newsroom/pdfs/fy2026q2/FY26_Q2_Consolidated_Financial_Statements.pdf)

관찰한 IR 구조:

- `Investor Updates`
- `Newsroom`
- `Financial Data`
- `Quarterly Earnings Reports`
- `SEC Filings`
- `Leadership and Governance`
- `Our Values`
- `FAQ`
- `Contact`

초기 판단:

- Apple IR 사이트는 Q4 Inc 기반 동적 페이지로 보인다.
- 정적 HTML 확인만으로는 모든 분기별 파일 링크가 안정적으로 드러나지 않을 수 있다.
- 실제 collector 단계에서는 렌더링된 페이지 또는 공식 링크 추출 방식을 별도로 확인해야 한다.

## 2. 다운로드 가능 자료 후보


| 후보                                          | 공식 URL                                                                                                                                                                                   | 형식                        | 날짜/기간                                                       | IR 수집 가치 | 비고                                                    |
| ------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------- | ----------------------------------------------------------- | -------- | ----------------------------------------------------- |
| FY2026 Q2 earnings press release            | [https://www.apple.com/newsroom/2026/04/apple-reports-second-quarter-results/](https://www.apple.com/newsroom/2026/04/apple-reports-second-quarter-results/)                             | HTML                      | FY2026 Q2 / quarter ended 2026-03-28 / published 2026-04-30 | 높음       | 공식 실적 발표 본문. SEC 8-K exhibit와 중복 가능성 높음.              |
| FY2026 Q2 consolidated financial statements | [https://www.apple.com/newsroom/pdfs/fy2026q2/FY26_Q2_Consolidated_Financial_Statements.pdf](https://www.apple.com/newsroom/pdfs/fy2026q2/FY26_Q2_Consolidated_Financial_Statements.pdf) | PDF                       | FY2026 Q2 / quarter ended 2026-03-28                        | 높음       | 작은 PDF라 pilot에 적합. SEC 8-K exhibit와 중복 가능성 높음.        |
| Quarterly earnings reports table            | [https://investor.apple.com/investor-relations/](https://investor.apple.com/investor-relations/)                                                                                         | Web table / dynamic links | 2023-2025 등 표시 확인                                           | 중간       | 전체 IR 수집 범위 설계에는 중요하지만 첫 pilot에서는 직접 수집보다 조사 대상.      |
| SEC Filings page                            | [https://investor.apple.com/investor-relations/sec-filings/default.aspx](https://investor.apple.com/investor-relations/sec-filings/default.aspx)                                         | Web table / dynamic links | 연도별 SEC filing                                              | 낮음       | SEC 자료는 EDGAR canonical 원칙을 유지. IR 페이지는 cross-check용. |
| Earnings call webcast                       | [https://www.apple.com/investor/earnings-call/](https://www.apple.com/investor/earnings-call/)                                                                                           | streaming audio           | 최신 실적 발표 후 약 2주 replay                                      | 낮음       | 첫 pilot에서는 out-of-scope. 링크/존재 여부만 기록 후보.             |
| Leadership and governance docs              | [https://investor.apple.com/investor-relations/leadership-and-governance/default.aspx](https://investor.apple.com/investor-relations/leadership-and-governance/default.aspx)             | Web/PDF 후보                | 상시 자료                                                       | 낮음       | IR 확장 2차 후보. 첫 실적자료 pilot에는 제외.                       |


## 3. SEC 중복 가능성

초기 원칙:

- SEC에 filed/furnished된 원자료는 EDGAR raw를 canonical로 본다.
- Apple IR/Newsroom 자료는 공식 회사 제공 자료이지만, SEC 8-K Item 2.02 및 exhibit와 중복될 수 있다.
- AAPL 2파일 pilot은 IR 수집 경로 검증이 목적이므로, 승인된 `run_scope` 안에서는 SEC 중복 가능성이 있어도 company-ir raw로 별도 저장할 수 있다.
- 이 pilot 원칙은 production 중복 처리 원칙이 아니다.

자료별 중복 판단:


| 자료                                              | SEC 중복 가능성                          | 권장 처리                                                               |
| ----------------------------------------------- | ----------------------------------- | ------------------------------------------------------------------- |
| FY2026 Q2 earnings press release HTML           | 높음                                  | `sec_overlap: likely`로 메모. 운영 반영 전 SEC accession/exhibit 확인 필요.     |
| FY2026 Q2 consolidated financial statements PDF | 높음                                  | `sec_overlap: likely`로 메모. 파일 내용이 SEC exhibit와 같은지 hash/size 비교 후보. |
| SEC Filings page                                | 본질적으로 SEC 중복                        | EDGAR canonical. IR SEC page는 수집 대상보다 cross-check 대상.               |
| Earnings call webcast                           | 낮음 또는 별도 자료                         | 첫 pilot out-of-scope.                                               |
| Governance page/docs                            | DEF 14A 및 governance docs와 일부 관계 가능 | 나중에 별도 IR governance 범위에서 판단.                                       |


Pilot 처리 원칙:

- `run_mode: test_collection`과 `run_scope`가 pilot 성격을 설명한다.
- 승인된 pilot 범위 안에서는 hash, size, source_url, retrieved_at을 기록한다.
- notes에는 필요한 경우 `sec_overlap: likely`, `canonical_source: sec-edgar`를 남긴다.
- `pilot_duplicate_allowed` 같은 별도 플래그는 쓰지 않는다.

Production 처리 원칙:

- Earnings-related로 명시된 IR `document_type`은 SEC overlap hash 비교 대상이다.
- Production에서는 먼저 임시 다운로드 후 hash와 size를 계산한다.
- 기존 SEC `files.jsonl`에 같은 hash가 있으면 company-ir raw로 승격하지 않고, 임시 파일은 삭제한다.
- hash가 다르면 company-ir raw로 별도 승격하고 notes에 overlap 관련 정보를 남긴다.
- 그 외 IR `document_type`은 SEC overlap hash 비교를 기본 요구하지 않는다.

## 4. access_check


| 항목               | 관찰                                                                                                                                               |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| 로그인 필요 여부        | 확인한 공식 페이지는 로그인 없이 접근 가능.                                                                                                                        |
| 직링크 여부           | Newsroom HTML과 FY2026 Q2 PDF는 직접 URL 접근 가능.                                                                                                      |
| JavaScript 필요 여부 | Investor Relations 메인/SEC Filings 표는 동적 페이지 성격이 있어 전체 링크 추출에는 렌더링 확인이 필요해 보임.                                                                    |
| robots.txt       | `www.apple.com/robots.txt`는 Newsroom/PDF 경로를 명시적으로 막는 패턴을 확인하지 못함. `investor.apple.com/robots.txt`는 이번 메모에서 확정 확인하지 못했으므로 collector 전에 별도 확인 권장. |
| 다운로드 안정성         | FY2026 Q2 PDF는 공식 Apple URL에서 열람 가능. 실제 raw 저장은 아직 수행하지 않음.                                                                                      |
| 요청 속도            | IR 사이트 요청 간격은 기존 collector 원칙대로 최소 2초 이상 유지 권장.                                                                                                  |


## 5. webcast/audio 처리

- 첫 AAPL IR pilot에서는 webcast/audio를 out-of-scope로 둔다.
- Apple Earnings Call 페이지는 streaming/replay 성격이며, replay는 일정 기간 후 사라질 수 있다.
- 이번 단계에서는 오디오 파일 저장, 재인코딩, transcript 생성, 요약을 하지 않는다.
- 필요하면 나중에 별도 transcript/audio 하네스 설계에서 다룬다.

## 6. document_id 생성 방식 초안

IR에는 SEC accession number 같은 공식 고유 ID가 없다.
따라서 문서 의미 기준의 안정적인 `document_id` 규칙이 필요하다.

기본 공식:

```text
ir-{ticker}-{document_type_slug}-{period_or_date}
```

기간 표기 규칙:


| 자료 성격             | 기간 표기                        |
| ----------------- | ---------------------------- |
| 실적 관련 자료          | `fy{YYYY}-q{N}`              |
| 연간 관련 자료          | `fy{YYYY}`                   |
| 이벤트 관련 자료         | `{YYYY}-{MM}-{DD}`           |
| 월 단위만 확인되는 자료     | `{YYYY}-{MM}`                |
| 연도만 확인되는 자료       | `{YYYY}`                     |
| 동일 type/period 충돌 | 뒤에 `{short-slug}` 또는 `v2` 추가 |


첫 pilot 후보의 `document_id` 초안:

```text
ir-aapl-earnings-release-fy2026-q2
ir-aapl-financial-supplement-fy2026-q2
```

구분 원칙:

- `document_id`: 문서 의미 기준 고유 이름
- `source_url`: 자료를 확인한 공식 페이지
- `download_url`: 실제 파일 다운로드 URL
- `sha256`: 파일 내용 동일성 검증용 지문

## 7. pilot 수집 후보 1-2건

권장 pilot 후보:

1. FY2026 Q2 earnings press release HTML
2. FY2026 Q2 consolidated financial statements PDF

추천 이유:

- 둘 다 Apple 공식 출처다.
- 최신 실적 관련 자료라 IR 수집 의미가 크다.
- HTML 1건과 PDF 1건으로 collector의 두 가지 기본 형식을 작게 테스트할 수 있다.
- 오디오, transcript, Q4 동적 테이블 전체 수집보다 훨씬 작고 안전하다.

제안 run_scope 초안:

```text
test only: AAPL company-ir official Apple FY2026 Q2 earnings press release HTML and consolidated financial statements PDF; no SEC download; no SEC filings page harvesting; no webcast/audio; no transcript; no operating catalog merge until user approval
```

주의:

- 이 run_scope는 아직 실행 승인된 범위가 아니다.
- 사용자가 승인하면 첫 AAPL IR 소규모 테스트의 실행 범위로 사용할 수 있다.

## 8. schema 변경 필요 여부

현재 schema에는 첫 AAPL IR pilot에 필요한 최소 IR `document_type`이 반영되어 있다.

- 현재 `document_type` 후보: `ir-deck`, `ir-earnings-release`, `ir-financial-supplement`, `other`
- 현재 `source_type` 후보: `company-ir` 사용 가능

첫 pilot에서 사용할 분류:


| 자료                                              | document_type             |
| ----------------------------------------------- | ------------------------- |
| FY2026 Q2 earnings press release HTML           | `ir-earnings-release`     |
| FY2026 Q2 consolidated financial statements PDF | `ir-financial-supplement` |


권장:

- 첫 pilot에서는 위 두 document_type만 사용한다.
- 새로운 IR 자료 유형이 발견되면 기존 값에 억지로 넣지 않고 `candidate_document_type`으로 기록한 뒤 사용자 승인 후 schema 확장을 검토한다.

## 9. 다음 단계 제안

다음 단계는 바로 전체 IR 수집이 아니라 아래 중 하나를 선택하는 것이다.

1. 위 pilot 후보 2건으로 소규모 IR test_collection을 승인한다.
2. Apple IR 페이지의 동적 테이블 링크 추출 방식을 추가 조사한다.
3. 다른 회사 IR pilot 전에 새 `candidate_document_type` 필요 여부를 별도 검토한다.

현재 추천은 1번으로 작게 진행하는 것이다.