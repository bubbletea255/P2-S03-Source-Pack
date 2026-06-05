# APP IR Preflight - 2026-06-04

- 대상 티커: APP
- 회사명: AppLovin Corporation
- 목적: IR pilot 3 사전 조사
- 상태: preflight only
- 원칙: 다운로드 없음, raw/catalog/index 변경 없음

이 메모는 APP 공식 IR 사이트만 대상으로 IR 자료 유형을 확인하고, 다음 소규모 pilot 후보를 정리하기 위한 사전 조사 기록이다.
실제 raw 파일 다운로드, `artifacts/catalog/*.jsonl`, `artifacts/companies/APP/index.md`, `artifacts/raw/` 수정은 수행하지 않는다.

## 1. 확인한 공식 출처

| 구분 | URL | 확인 내용 |
|---|---|---|
| IR Overview | https://investors.applovin.com/ | 공식 Investor Relations 진입점, News, Events & Presentations, Financials, Governance, Resources 메뉴 확인 |
| Quarterly Results | https://investors.applovin.com/financials/quarterly-results/default.aspx | Press Release, Shareholder Letter, Earnings Webcast, Financial Update, 10-Q, Annual Report 유형 확인 |
| Q1 2026 earnings press release | https://investors.applovin.com/news/news-details/2026/AppLovin-Announces-First-Quarter-2026-Financial-Results/default.aspx | 공식 실적 발표 HTML, financial update 게시 언급, webcast/replay 안내 확인 |
| Events & Presentations | https://investors.applovin.com/events-and-presentations/default.aspx | Events archive가 JS 기반으로 로딩되는 구조 확인 |
| Morgan Stanley TMT event detail | https://investors.applovin.com/events-and-presentations/event-details/2026/AppLovin-to-Participate-in-the-Morgan-Stanley-Technology-Media--Telecom-Conference--2026-OoIGfckpvU/default.aspx | investor conference event, webcast, transcript 링크 확인 |
| Morgan Stanley TMT press release | https://investors.applovin.com/news/news-details/2026/AppLovin-to-Participate-in-the-Morgan-Stanley-Technology-Media--Telecom-Conference/default.aspx | fireside chat 참여 공지와 replay 안내 확인 |
| Annual Reports | https://investors.applovin.com/financials/annual-reports/default.aspx | 10-K PDF archive 확인 |
| SEC Filings | https://investors.applovin.com/financials/sec-filings/default.aspx | IR 사이트 안의 SEC filings page 존재 확인. 이번 preflight에서는 수집/harvesting 대상 아님 |

## 2. 사이트 구조 관찰

AppLovin IR 사이트는 Q4 기반 IR 사이트이며, 아래 구조가 확인된다.

- News
- Events & Presentations
- Stock Information
- Financials
  - Quarterly Results
  - Annual Reports
  - SEC Filings
- Governance
  - Leadership
  - Board of Directors
  - Committee Composition
  - Governance Documents
- Resources
  - Investor FAQs
  - Information Request Form
  - Investor Email Alerts
  - Investor Contacts
  - Latest Shareholder Letter

Quarterly Results archive에는 아래 자료 유형이 함께 나타난다.

- Press Release
- Shareholder Letter
- Earnings Webcast
- Financial Update
- 10-Q
- Annual Report

Events & Presentations 쪽에서는 아래 유형이 나타난다.

- earnings call webcast
- investor conference webcast
- transcript 링크

이 구조는 AAPL보다 다양하고, NTRA와는 다르게 presentation PDF보다 webcast/transcript 중심 이벤트가 더 두드러진다.

## 3. 기존 document_type으로 분류 가능한 자료

| 발견 자료 유형 | 예시 | 기존 document_type | earnings-related | 판단 |
|---|---|---|---|---|
| 실적 발표 press release HTML | AppLovin Announces First Quarter 2026 Financial Results | `ir-earnings-release` | yes | 기존 type으로 분류 가능. SEC 8-K/EX-99.1 overlap 가능성 높음 |
| Financial Update PDF | Financial Update Q1 2026 | `ir-financial-supplement` | yes | 실적 관련 재무 보충자료로 기존 type 사용 가능 |
| Earnings Presentation PDF | Q1 2026 AppLovin Earnings Presentation | `ir-deck` | yes | 기존 `ir-deck` 사용 가능. 다만 earnings presentation 성격이 강해 candidate 기록 필요 |
| Annual Report PDF | 10-K of 2025 PDF | `10-K` | yes | SEC 수집 경로가 canonical. IR pilot 후보로는 낮은 우선순위 |
| Earnings webcast | Q1/Q4 earnings call webcast | out-of-scope | yes | webcast/audio/video는 현재 IR collector 범위 밖 |
| Earnings transcript | event detail의 Transcript 링크 | `transcript` 후보이나 이번 IR pilot out-of-scope | yes | transcript는 별도 transcript 설계에서 다룸 |
| Investor conference webcast/transcript | Morgan Stanley TMT Conference | out-of-scope | no | 현재 IR raw pilot 후보로는 부적합 |

## 4. candidate_document_type 후보

이번 preflight에서 즉시 schema에 추가할 필요는 없지만, 반복 등장 여부를 관찰할 후보가 있다.

| candidate_document_type | 후보 의미 | 왜 후보인가 | 지금 schema 추가 여부 |
|---|---|---|---|
| `ir-earnings-presentation` | 분기 실적 발표용 presentation 또는 prepared remarks PDF | NTRA와 APP 모두 earnings-related deck/presentation 후보가 나타남 | 보류. 우선 `ir-deck` + notes로 처리 |
| `ir-shareholder-letter` | 주주에게 보내는 quarterly/annual letter | APP Quarterly Results와 Resources에 Shareholder Letter가 명시됨 | 보류. 실제 pilot에서 반복 수집 필요 |
| `ir-financial-update` | 회사가 별도 명칭으로 게시하는 분기 재무 update PDF | APP는 `Financial Update`라는 이름을 명시적으로 사용함 | 보류. 현재는 `ir-financial-supplement`로 충분 |

현재 판단:

```text
APP pilot 3는 기존 schema로 진행 가능하다.
새 document_type은 추가하지 않고, earnings presentation / shareholder letter / financial update는 candidate로 관찰한다.
```

## 5. SEC overlap 가능성

| 자료 | overlap 가능성 | 이유 | 처리 원칙 |
|---|---|---|---|
| Q1 2026 earnings press release HTML/PDF | 높음 | 실적 발표 press release는 SEC 8-K Item 2.02/EX-99.1과 중복 가능성이 큼 | pilot에서는 `sec_overlap: likely`; production에서는 기존 SEC hash 비교 |
| Q1 2026 Financial Update PDF | 높음 | 실적 발표와 함께 게시되는 재무 보충자료이며 SEC exhibit 또는 earnings release 부속자료와 중복 가능성 있음 | `ir-financial-supplement`; earnings-related notes 기록 |
| Q1 2026 Earnings Presentation PDF | 중간~높음 | earnings call/presentation 성격이며 SEC exhibit 제출 여부 확인 필요 | `ir-deck`; notes에 `candidate_document_type: ir-earnings-presentation`, `earnings_related: true` 기록 |
| 10-Q / 10-K PDF | 매우 높음 | SEC filing 자체 또는 사본 | IR pilot에서는 수집하지 않고 SEC collector를 canonical로 둠 |
| Morgan Stanley TMT webcast/transcript | 낮음 | investor conference event이며 IR-native event 성격 | 이번 pilot out-of-scope. transcript/audio/video 수집하지 않음 |

주의:

```text
이번 preflight에서는 SEC 다운로드, SEC filings page harvesting, SEC exhibit 확인을 수행하지 않았다.
overlap 판단은 IR 사이트에서 관찰한 자료 성격 기준의 사전 판단이다.
```

## 6. Access check

| 항목 | 관찰 |
|---|---|
| 공식 출처 | `investors.applovin.com` 공식 IR 사이트에서 확인 |
| 로그인 | 확인한 News/Quarterly Results/Event detail page는 로그인 없이 열람 가능 |
| 파일 경로 | 일부 PDF는 `s21.q4cdn.com`으로 연결되는 공식 IR CDN 파일로 보임. 이번 preflight에서는 실제 PDF 다운로드 안 함 |
| JavaScript | Quarterly Results와 Events & Presentations 목록은 JS 기반 로딩 요소가 있음. News detail과 Event detail page는 정적 HTML로 확인 가능 |
| robots.txt | 이번 메모에서는 robots.txt 직접 확인하지 않았다 |
| 보안 주의 | NTRA pilot 2에서 PDF 보안 격리 사례가 있었으므로 APP PDF pilot도 다운로드 후 local_path 존재, hash, 보안 격리 여부를 반드시 QA한다 |
| 수집 권장 | archive-wide crawling 대신 사용자가 승인한 공식 detail page와 PDF 후보 1~3건만 단건 수집 권장 |

## 7. Webcast/audio/video/transcript 처리

APP IR 사이트의 Events & Presentations에는 webcast와 transcript 링크가 있다.

이번 pilot에서는 out-of-scope:

- webcast 저장
- audio/video 다운로드
- transcript 다운로드
- transcript 생성, 요약, 번역
- webcast replay 수집

필요하면 존재 여부만 notes에 남기고, 실제 처리는 별도 transcript/audio 설계에서 다룬다.

## 8. Pilot 후보 1~3건

추천 pilot 후보:

| 우선순위 | 후보 | source_url | document_type | 수집 목적 | SEC overlap |
|---:|---|---|---|---|---|
| 1 | AppLovin Announces First Quarter 2026 Financial Results | https://investors.applovin.com/news/news-details/2026/AppLovin-Announces-First-Quarter-2026-Financial-Results/default.aspx | `ir-earnings-release` | HTML press release 경로 검증, SEC overlap 처리 재사용 | likely |
| 2 | Financial Update Q1 2026 PDF | https://s21.q4cdn.com/165405286/files/doc_financials/2026/q1/Financial-Update-Q1-2026.pdf | `ir-financial-supplement` | APP 특유의 financial update 자료를 기존 type으로 처리 가능한지 확인 | likely |
| 3 | Q1 2026 AppLovin Earnings Presentation PDF | https://s21.q4cdn.com/165405286/files/doc_financials/2026/q1/Q1-2026-AppLovin-Earnings-Presentation.pdf | `ir-deck` | `ir-earnings-presentation` 후보 관찰, NTRA와 반복성 비교 | likely |

가장 작은 pilot 권장안:

```text
1번 HTML + 2번 Financial Update PDF
```

확장 pilot 권장안:

```text
1번 HTML + 2번 Financial Update PDF + 3번 Earnings Presentation PDF
```

3번을 포함하면 NTRA에서 보였던 `ir-earnings-presentation` 후보의 반복성을 더 잘 확인할 수 있다.
다만 PDF 2건을 포함하므로 NTRA pilot 2의 Bitdefender 격리 사례를 감안해 보안 격리 여부를 QA에서 반드시 확인한다.

## 9. document_id 초안

| 후보 | document_id 초안 | 비고 |
|---|---|---|
| Q1 2026 earnings release | `ir-app-earnings-release-fy2026-q1` | 실적 관련 자료이므로 fiscal quarter 사용 |
| Q1 2026 financial update | `ir-app-financial-supplement-fy2026-q1` | `ir-financial-supplement` 사용. notes에 `financial_update` 기록 |
| Q1 2026 earnings presentation | `ir-app-deck-fy2026-q1` | `ir-deck` 사용. notes에 `candidate_document_type: ir-earnings-presentation` 기록 |

## 10. 추천 run_scope

보수적인 2건 pilot:

```text
test only: APP company-ir official AppLovin Q1 2026 earnings press release HTML and Q1 2026 financial update PDF; no SEC download; no SEC filings page harvesting; no webcast/audio/video; no transcript; no archive-wide crawling; no operating catalog/index merge until user approval
```

확장 3건 pilot:

```text
test only: APP company-ir official AppLovin Q1 2026 earnings press release HTML, Q1 2026 financial update PDF, and Q1 2026 earnings presentation PDF; no SEC download; no SEC filings page harvesting; no webcast/audio/video; no transcript; no archive-wide crawling; no operating catalog/index merge until user approval
```

## 11. Schema 변경 필요 여부

현재 APP pilot 3만 기준으로는 schema 변경이 필수는 아니다.

사용할 수 있는 기존 type:

- `ir-earnings-release`
- `ir-financial-supplement`
- `ir-deck`

보류할 후보:

- `ir-earnings-presentation`
- `ir-shareholder-letter`
- `ir-financial-update`

결론:

```text
APP pilot 3는 기존 schema로 진행 가능하다.
새 document_type 추가는 APP pilot 결과와 NTRA/APP 반복 출현 여부를 함께 본 뒤 결정한다.
```

## 12. 다음 단계

사용자가 승인하면 APP IR pilot 3를 `test_collection`으로 실행한다.

권장 순서:

1. 2건 pilot로 할지 3건 pilot로 할지 선택한다.
2. SEC 다운로드 없이 company-ir raw만 run-local로 저장한다.
3. earnings-related 자료는 기존 SEC catalog/files 안에서 overlap 후보만 확인한다.
4. PDF 파일은 다운로드 후 local_path 존재, hash, 보안 격리 여부를 QA한다.
5. 운영 catalog/index 반영은 raw 수집과 QA 후 사용자 승인으로 결정한다.
