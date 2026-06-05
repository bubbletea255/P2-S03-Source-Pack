# NTRA IR Preflight - 2026-06-04

- 대상 티커: NTRA
- 회사명: Natera, Inc.
- 목적: IR pilot 2 사전 조사
- 상태: preflight only
- 원칙: 다운로드 없음, raw/catalog/index 변경 없음

이 메모는 NTRA 공식 IR 사이트만 대상으로 IR 자료 유형을 확인하고, 다음 소규모 pilot 후보를 정리하기 위한 사전 조사 기록이다.
실제 raw 파일 다운로드, `artifacts/catalog/*.jsonl`, `artifacts/companies/NTRA/index.md`, `artifacts/raw/` 수정은 수행하지 않는다.

## 1. 확인한 공식 출처

| 구분 | URL | 확인 내용 |
|---|---|---|
| IR Overview | https://investor.natera.com/overview/default.aspx | 공식 Investor Relations 진입점, News, Events & Presentations, Financials, SEC Filings, Governance, Resources 메뉴 확인 |
| Events & Presentations | https://investor.natera.com/events-and-presentations/default.aspx | Upcoming Events, Featured Presentation, Archived Events & Presentations 구조 확인 |
| Events page | https://investor.natera.com/events-and-presentations/events/default.aspx | earnings call, healthcare conference, post-ESMO investor call, webcast, presentation, press release 링크 유형 확인 |
| Quarterly Results | https://investor.natera.com/financials/quarterly-results/default.aspx | Quarterly Results 구조 확인. 일부 상세 항목은 JS 기반으로 보임 |
| Q1 2026 earnings press release | https://investor.natera.com/news/news-details/2026/Natera-Reports-First-Quarter-2026-Financial-Results/default.aspx | 공식 실적 발표 HTML 및 PDF download 링크 존재 확인 |
| Q1 2026 earnings event | https://investor.natera.com/events-and-presentations/events/event-details/2026/Natera-Inc-Q1-Earnings-Conference-Call/default.aspx | webcast, presentation PDF, press release 링크 확인 |
| Q1 2026 earnings presentation | https://investor.natera.com/events-and-presentations/presentations/presentation-details/2026/Natera-Inc-Q1-2026-Earnings-Presentation/default.aspx | presentation PDF, audio, video 링크 존재 확인 |
| J.P. Morgan Healthcare Conference event | https://investor.natera.com/events-and-presentations/events/event-details/2026/44th-Annual-JP-Morgan-Healthcare-Conference-2026-8khCChg0K_/default.aspx | investor conference event, webcast, presentation PDF 링크 확인 |
| J.P. Morgan Healthcare Conference presentation | https://investor.natera.com/events-and-presentations/presentations/presentation-details/2026/44th-Annual-JP-Morgan-Healthcare-Conference/default.aspx | investor conference presentation detail page 확인 |
| Post-ESMO Investor Call presentation | https://investor.natera.com/events-and-presentations/presentations/presentation-details/2025/Natera-Post-ESMO-Investor-Call/default.aspx | scientific/clinical update 성격의 investor call presentation 확인 |
| Terms of Use | https://www.natera.com/terms/ | automated extraction/scraping 제한 문구 확인. 대량 자동 수집은 피하고 승인된 단건 URL 중심 pilot 권장 |

## 2. 사이트 구조 관찰

Natera IR 사이트는 Apple보다 IR 자료 유형이 더 다양하다.

확인된 주요 섹션:

- News
- Events & Presentations
- Quarterly Results
- SEC Filings
- Proxy Reports
- Governance Documents
- Sustainability
- Investor FAQs
- Investor Contacts

Events & Presentations 쪽에서는 아래 자료 유형이 함께 나타난다.

- earnings conference call
- earnings presentation PDF
- earnings press release
- webcast
- audio/video 링크
- healthcare conference event
- healthcare conference presentation PDF
- thematic investor call presentation
- reconciliation of non-GAAP cash flow

이 구조는 AAPL pilot보다 IR taxonomy 검증에 더 적합하다.

## 3. 기존 document_type으로 분류 가능한 자료

| 발견 자료 유형 | 예시 | 기존 document_type | earnings-related | 판단 |
|---|---|---|---|---|
| 실적 발표 press release | Natera Reports First Quarter 2026 Financial Results | `ir-earnings-release` | yes | 기존 type으로 분류 가능. SEC 8-K/EX-99.1 overlap 가능성 높음 |
| 실적 발표 presentation PDF | Natera, Inc. Q1 2026 Earnings Presentation | `ir-deck` | yes | 기존 `ir-deck`으로 분류 가능. 다만 notes에 earnings-related 성격 기록 필요 |
| 투자자/헬스케어 컨퍼런스 presentation PDF | 44th Annual J.P. Morgan Healthcare Conference | `ir-deck` | no | 기존 `ir-deck`으로 분류 가능. SEC overlap 기본 검사 대상 아님 |
| thematic investor call presentation PDF | Natera Post-ESMO Investor Call | `ir-deck` | no | PDF deck으로는 기존 `ir-deck` 가능. 다만 반복되면 별도 type 후보 검토 가능 |

## 4. candidate_document_type 후보

이번 preflight에서 즉시 schema에 추가할 필요는 없지만, 반복 등장 여부를 관찰할 후보는 있다.

| candidate_document_type | 후보 의미 | 왜 후보인가 | 지금 schema 추가 여부 |
|---|---|---|---|
| `ir-earnings-presentation` | 분기 실적 발표용 slide deck | `ir-deck`으로 처리 가능하지만, SEC overlap 가능성과 분기 실적 성격이 강함 | 보류. 우선 `ir-deck` + notes로 처리 |
| `ir-investor-conference-presentation` | J.P. Morgan, Wolfe, Jefferies, UBS 등 투자자 컨퍼런스 발표 deck | NTRA는 healthcare conference event가 반복적으로 나타남 | 보류. 우선 `ir-deck`으로 처리 |
| `ir-scientific-update` | ESMO, AACR, ASCO 등 학회/임상 데이터 기반 investor update | Natera Post-ESMO Investor Call처럼 일반 IR deck과 성격이 다를 수 있음 | 보류. 반복 수집 후 판단 |
| `ir-non-gaap-reconciliation` | Reconciliation of Non-GAAP Cash Flow 같은 보충자료 | earnings call 주변에 반복 등장 가능, `ir-financial-supplement`와 겹칠 수 있음 | 보류. 실제 파일 확인 후 판단 |

현재 판단:

```text
지금은 새 document_type을 추가하지 않는다.
NTRA pilot 2에서는 기존 type만 사용하고, candidate_document_type은 메모에 남긴다.
```

## 5. SEC overlap 가능성

| 자료 | overlap 가능성 | 이유 | 처리 원칙 |
|---|---|---|---|
| Q1 2026 earnings press release | 높음 | 실적 발표 press release는 SEC 8-K Item 2.02/EX-99.1과 중복 가능성이 큼 | pilot에서는 `sec_overlap: likely`; production에서는 기존 SEC hash 비교 |
| Q1 2026 earnings presentation PDF | 중간~높음 | 회사가 earnings call event에 PDF presentation을 연결함. SEC exhibit로 제출됐을 가능성 확인 필요 | `ir-deck`이지만 earnings-related notes 기록 |
| Q4 2025 earnings call presentation / reconciliation | 높음 | earnings event 주변 자료이며 reconciliation 자료는 SEC 또는 earnings release 부속자료와 중복 가능성 있음 | pilot 후보로 쓰면 SEC overlap 확인 필요 |
| J.P. Morgan Healthcare Conference presentation | 낮음 | investor conference IR-native 자료에 가까움 | SEC overlap 기본 검사 불필요 |
| Post-ESMO Investor Call presentation | 낮음~중간 | IR-native scientific update로 보이나, 보도자료와 연결될 수 있음 | pilot에서는 SEC overlap 필수 아님, notes에 source context 기록 |

## 6. Access check

| 항목 | 관찰 |
|---|---|
| 공식 출처 | `investor.natera.com` 공식 IR 사이트에서 확인 |
| 로그인 | 확인한 IR detail page는 로그인 없이 열람 가능 |
| 파일 경로 | PDF 링크는 `s201.q4cdn.com`으로 연결되는 경우가 있음. 이번 preflight에서는 실제 PDF 다운로드 안 함 |
| JavaScript | Quarterly Results와 Events archive 일부 목록은 JS로 로딩되는 구조로 보임. 검색 결과와 detail page는 접근 가능 |
| robots.txt | 이번 메모에서는 robots.txt 직접 확인 실패. 대신 IR footer의 Terms of Use를 확인했고, automated extraction/scraping 제한 문구가 있음 |
| 수집 권장 | 대량 자동 탐색보다 사용자가 승인한 detail page의 PDF/HTML 1~3건만 단건 수집하는 방식 권장 |

주의:

```text
NTRA pilot 2에서는 site-wide crawl, archive pagination 대량 탐색, webcast/audio/video 수집을 하지 않는다.
```

## 7. Webcast/audio/video 처리

NTRA IR detail page에는 webcast, audio, video 링크가 함께 표시된다.

이번 pilot에서는 out-of-scope:

- webcast 저장
- audio 다운로드
- video 다운로드
- transcript 생성
- webcast replay 수집

필요하면 존재 여부만 notes에 남기고, 실제 처리는 별도 transcript/audio 설계에서 다룬다.

## 8. Pilot 후보 1~3건

추천 pilot 후보:

| 우선순위 | 후보 | source_url | document_type | 수집 목적 | SEC overlap |
|---:|---|---|---|---|---|
| 1 | Natera Reports First Quarter 2026 Financial Results | https://investor.natera.com/news/news-details/2026/Natera-Reports-First-Quarter-2026-Financial-Results/default.aspx | `ir-earnings-release` | HTML press release 경로 검증, SEC overlap 처리 재사용 | likely |
| 2 | Natera, Inc. Q1 2026 Earnings Presentation | https://investor.natera.com/events-and-presentations/presentations/presentation-details/2026/Natera-Inc-Q1-2026-Earnings-Presentation/default.aspx | `ir-deck` | earnings presentation PDF 경로 검증, `ir-deck` earnings-related 처리 확인 | likely |
| 3 | 44th Annual J.P. Morgan Healthcare Conference | https://investor.natera.com/events-and-presentations/events/event-details/2026/44th-Annual-JP-Morgan-Healthcare-Conference-2026-8khCChg0K_/default.aspx | `ir-deck` | IR-native healthcare conference deck 처리 확인 | unlikely |

가장 작은 pilot 권장안:

```text
1번 + 2번만 먼저 수집
```

확장 pilot 권장안:

```text
1번 + 2번 + 3번 수집
```

3번을 포함하면 Apple에서 보지 못한 IR-native investor conference deck을 검증할 수 있다.
따라서 NTRA pilot 2의 학습 가치만 보면 3건 pilot이 더 좋다.

## 9. document_id 초안

| 후보 | document_id 초안 | 비고 |
|---|---|---|
| Q1 2026 earnings release | `ir-ntra-earnings-release-fy2026-q1` | 실적 관련 자료이므로 fiscal quarter 사용 |
| Q1 2026 earnings presentation | `ir-ntra-deck-fy2026-q1` | `ir-deck` 사용. notes에 earnings_presentation 또는 earnings_related 기록 |
| J.P. Morgan Healthcare Conference | `ir-ntra-deck-2026-01-13-jpm-healthcare-conference` | 이벤트 관련 자료이므로 event date 사용 |

## 10. 추천 run_scope

```text
test only: NTRA company-ir official Natera IR Q1 2026 earnings press release HTML, Q1 2026 earnings presentation PDF, and 44th Annual J.P. Morgan Healthcare Conference presentation PDF; no SEC download; no SEC filings page harvesting; no webcast/audio/video; no transcript; no archive-wide crawling; no operating catalog/index merge until user approval
```

더 보수적인 2건 pilot을 원하면:

```text
test only: NTRA company-ir official Natera IR Q1 2026 earnings press release HTML and Q1 2026 earnings presentation PDF; no SEC download; no SEC filings page harvesting; no webcast/audio/video; no transcript; no archive-wide crawling; no operating catalog/index merge until user approval
```

## 11. Schema 변경 필요 여부

현재 pilot 2만 기준으로는 schema 변경이 필수는 아니다.

사용할 수 있는 기존 type:

- `ir-earnings-release`
- `ir-deck`

보류할 후보:

- `ir-earnings-presentation`
- `ir-investor-conference-presentation`
- `ir-scientific-update`
- `ir-non-gaap-reconciliation`

결론:

```text
NTRA pilot 2는 기존 schema로 진행 가능하다.
새 document_type 추가는 pilot 결과와 반복 출현 여부를 본 뒤 결정한다.
```

## 12. 다음 단계

사용자가 승인하면 NTRA IR pilot 2를 `test_collection`으로 실행한다.

권장 순서:

1. 3건 pilot로 할지 2건 pilot로 할지 선택한다.
2. SEC 다운로드 없이 company-ir raw만 run-local로 저장한다.
3. earnings-related 2건은 기존 SEC catalog/files 안에서 overlap 후보만 확인한다.
4. 운영 catalog/index 반영은 raw 수집과 QA 후 사용자 승인으로 결정한다.
