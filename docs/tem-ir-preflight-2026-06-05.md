# TEM IR Preflight - 2026-06-05

- 대상 티커: TEM
- 회사명: Tempus AI, Inc.
- 목적: IR taxonomy 확장 검증용 사전 조사
- 상태: preflight only
- 원칙: 다운로드 없음, raw/catalog/index 변경 없음

이 메모는 Tempus AI 공식 IR 사이트만 대상으로 IR 자료 유형을 확인하고, 다음 소규모 pilot 후보를 정리하기 위한 사전 조사 기록이다.
실제 raw 파일 다운로드, `artifacts/catalog/*.jsonl`, `artifacts/companies/TEM/index.md`, `artifacts/raw/` 수정은 수행하지 않는다.

PDF 링크는 공식 IR 페이지에 표시된 자료 유형과 URL 후보를 확인하기 위해 원격으로만 확인했다.
workspace에 raw 파일을 저장하지 않았고, hash/size QA도 수행하지 않았다.

## 1. 확인한 공식 출처

| 구분 | URL | 확인 내용 |
|---|---|---|
| IR Overview | https://investors.tempus.com/ | 공식 Investor Relations 진입점, News Releases, Recent Reports, Quarterly Results, Events 구조 확인 |
| Financial Information | https://investors.tempus.com/financials/financial-information | 분기별 Webcast, Results, Overview, Form 10-Q/10-K 자료 구조 확인 |
| Events | https://investors.tempus.com/news-events/investor-events | earnings call, Investor Day, healthcare conference, webcast, supporting materials 확인 |
| Presentations | https://investors.tempus.com/news-events/presentations | `Tempus 1Q26 Corporate Deck` 단독 presentation archive 확인 |
| News Releases | https://investors.tempus.com/news-events/news-releases | earnings release 외 제품, 과학, 임상, 플랫폼 관련 corporate news releases 존재 확인 |
| Q1 2026 earnings release | https://investors.tempus.com/news-releases/news-release-details/tempus-reports-first-quarter-2026-results | 공식 실적 발표 HTML, PDF Version 링크, webcast 안내, Investor Day 안내 확인 |
| Q1 2026 earnings event | https://investors.tempus.com/events/event-details/tempus-first-quarter-2026-earnings-conference-call | webcast, `Tempus 1Q26 Corporate Deck`, `Q1 2026 Overview` supporting materials 확인 |
| Investor Day event | https://investors.tempus.com/events/event-details/tempus-ai-investor-day | Investor Day webcast와 `Tempus AI Investor Day Presentation` supporting material 확인 |
| SEC Filings | https://investors.tempus.com/financials/sec-filings | IR 사이트 안의 SEC filings page 존재 확인. 이번 preflight에서는 수집/harvesting 대상 아님 |
| Terms of Use | https://www.tempus.com/terms-of-use/ | robot/automatic process 및 manual copying 제한 문구 확인. archive-wide crawling은 피한다 |

## 2. 사이트 구조 관찰

Tempus AI IR 사이트는 AAPL/APP보다 바이오/헬스케어 특성이 강하고, NTRA와 비슷하게 presentation 및 event 자료가 중요하다.

확인된 주요 섹션:

- News Releases
- Events
- Presentations
- Letter from the CEO
- Financial Information
- SEC Filings
- Investor FAQs
- Contact IR

Financial Information 쪽에서는 분기별로 아래 자료가 반복된다.

- Webcast
- Q1/Q2/Q3/Q4 Results
- Q1/Q2/Q3/Q4 Overview PDF
- Form 10-Q 또는 Form 10-K

Events 쪽에서는 아래 자료가 확인된다.

- earnings conference call webcast
- earnings-related corporate deck PDF
- quarterly overview PDF
- Investor Day webcast
- Investor Day presentation PDF
- healthcare/technology conference webcast
- 일부 과거 healthcare conference presentation PDF

News Releases 쪽에서는 earnings release뿐 아니라 제품, 임상, 과학, 연구, 플랫폼 관련 보도자료가 많이 나타난다.
다만 이번 Source Pack IR pilot에서는 News Releases 전체를 archive-wide로 수집하지 않는다.

## 3. 기존 document_type으로 분류 가능한 자료

| 발견 자료 유형 | 예시 | 기존 document_type | earnings-related | 판단 |
|---|---|---|---|---|
| 실적 발표 press release HTML | Tempus Reports First Quarter 2026 Results | `ir-earnings-release` | yes | 기존 type으로 분류 가능. SEC 8-K Item 2.02 / EX-99.1 overlap 가능성 높음 |
| 분기 Overview PDF | Q1 2026 Overview | `ir-financial-supplement` | yes | 분기 실적 관련 재무/운영 보충자료로 기존 type 사용 가능 |
| 분기 Corporate Deck PDF | Tempus 1Q26 Corporate Deck | `ir-deck` | yes | 기존 `ir-deck` 사용 가능. 다만 earnings presentation 성격이 있어 candidate 관찰 가치 있음 |
| Investor Day Presentation PDF | Tempus AI Investor Day Presentation | `ir-deck` | no | 기존 `ir-deck`으로 수집 가능. 다만 investor day 전용 type 후보로 관찰 가치 큼 |
| Healthcare conference webcast | Needham / Morgan Stanley / J.P. Morgan events | out-of-scope | no | webcast/audio/video는 현재 IR collector 범위 밖 |
| 과거 healthcare conference presentation PDF | J.P. Morgan Healthcare Conference 2025 Presentation | `ir-deck` | no | 기존 `ir-deck` 가능. `ir-investor-conference-presentation` 후보 관찰 가능 |
| SEC filing PDF/HTML | Form 10-Q, Form 10-K | SEC document_type | yes | SEC collector가 canonical. IR pilot에서는 수집하지 않음 |

## 4. candidate_document_type 후보

이번 preflight만으로 schema를 즉시 변경하지 않는다.
다만 Tempus는 새 후보를 관찰하기에 좋은 사례다.

| candidate_document_type | 후보 의미 | 관찰 근거 | 지금 schema 추가 여부 |
|---|---|---|---|
| `ir-earnings-presentation` | 분기 실적 발표용 deck 또는 corporate deck | `Tempus 1Q26 Corporate Deck`이 Q1 earnings call supporting material로 제공됨. NTRA/APP에서도 유사 후보가 관찰됨 | 보류. 우선 `ir-deck` + notes로 처리 |
| `ir-investor-day-presentation` | Investor Day 전용 장기전략/사업설명 presentation | `Tempus AI Investor Day Presentation`이 별도 event supporting material로 제공됨 | 보류. 실제 수집 성공 사례 확보 후 판단 |
| `ir-shareholder-letter` | CEO/CFO/주주 대상 letter | IR 메뉴에 `Letter from the CEO`가 있고 2025 Shareholder Letter PDF가 확인됨 | 보류. APP 사례와 함께 반복 관찰 후보 |
| `ir-investor-conference-presentation` | J.P. Morgan, Needham, Morgan Stanley 등 투자자 컨퍼런스 발표 deck | 과거 J.P. Morgan Healthcare Conference presentation PDF가 확인됨 | 보류. 현재 2026 최신 conference는 webcast 중심이라 pilot 우선순위 낮음 |
| `ir-scientific-update` | ASCO, 임상, 연구, scientific update 성격의 investor 자료 | News Releases에 ASCO, foundation model, product/clinical updates가 많음 | 보류. 현재 IR pilot 범위에서는 일반 corporate news 전체 수집을 하지 않음 |

현재 판단:

```text
TEM preflight는 기존 schema로 pilot 가능하다.
다만 `ir-earnings-presentation`, `ir-investor-day-presentation`, `ir-shareholder-letter`는 다음 taxonomy checkpoint에서 다시 볼 가치가 있다.
```

## 5. ir-taxonomy-candidates.jsonl 기록 판단

현재 `artifacts/catalog/ir-taxonomy-candidates.jsonl`은 존재하지만 비어 있다.
이번 작업은 preflight only이고 catalog 수정 금지 조건이 있으므로 후보 원장에는 기록하지 않았다.

다음 pilot에서 실제 수집 또는 QA 근거가 생기면 아래 후보를 원장에 기록할 수 있다.

| 후보 | 기록 권장 시점 | 근거 수준 |
|---|---|---|
| `ir-earnings-presentation` | `Tempus 1Q26 Corporate Deck` 또는 유사 earnings deck을 실제 수집할 때 | 중간. NTRA/APP/TEM에서 반복 관찰됐지만 성공 수집 근거는 아직 제한적 |
| `ir-investor-day-presentation` | `Tempus AI Investor Day Presentation`을 실제 수집할 때 | 중간. 자료 유형은 명확하지만 아직 단일 회사 관찰 |
| `ir-shareholder-letter` | `Letter from the CEO` 또는 shareholder letter를 실제 수집할 때 | 중간. APP/TEM에서 반복 관찰 |
| `ir-scientific-update` | 일반 news release가 아니라 별도 scientific/clinical investor PDF를 실제 수집할 때 | 약함. 현재는 news release 구조 관찰에 가까움 |

주의:

```text
후보 원장 기록은 schema 변경이 아니다.
누적 후보가 기준에 도달해도 schema는 자동 변경하지 않고 사용자 승인 요청만 남긴다.
```

## 6. SEC overlap 가능성

| 자료 | overlap 가능성 | 이유 | 처리 원칙 |
|---|---|---|---|
| Q1 2026 earnings release HTML/PDF | 높음 | 실적 발표 press release는 SEC 8-K Item 2.02 / EX-99.1과 중복 가능성이 큼 | `ir-earnings-release`; pilot에서는 company-ir raw 가능, production에서는 SEC hash 비교 |
| Q1 2026 Overview PDF | 높음 | 분기 실적 보충자료이며 8-K supplemental material 또는 IR website reference와 연결될 가능성 있음 | `ir-financial-supplement`; earnings-related notes 기록 |
| Tempus 1Q26 Corporate Deck PDF | 중간~높음 | earnings call supporting material로 제공되며 SEC 8-K Item 7.01 또는 IR website posting과 연결될 수 있음 | `ir-deck`; `candidate_document_type: ir-earnings-presentation` 관찰 가능 |
| Investor Day Presentation PDF | 중간 | Investor Day 자료는 SEC 8-K Item 7.01로 제출될 수 있으나 earnings-related는 아님 | `ir-deck`; 기본 earnings overlap hash 비교 대상은 아님 |
| CEO/Shareholder Letter PDF | 중간 | shareholder communication 또는 annual materials와 연결될 수 있음 | 현재 pilot 후보에서는 제외. 수집 시 candidate notes 필요 |
| Form 10-Q / Form 10-K | 매우 높음 | SEC filing 자체 또는 IR 사이트 사본 | IR pilot에서는 수집하지 않고 SEC collector를 canonical로 둠 |
| Product/scientific news releases | 낮음~중간 | 일부 material news는 SEC 8-K 가능성이 있지만 일반 earnings overlap 대상은 아님 | 이번 pilot에서는 archive-wide 수집하지 않음 |

주의:

```text
이번 preflight에서는 SEC 다운로드, SEC filings page harvesting, SEC exhibit 확인을 수행하지 않았다.
overlap 판단은 공식 IR 사이트에서 관찰한 자료 성격 기준의 사전 판단이다.
```

## 7. Access check

| 항목 | 관찰 |
|---|---|
| 공식 출처 | `investors.tempus.com` 공식 IR 사이트에서 확인 |
| 로그인 | 확인한 IR overview, financial information, event detail, news release page는 로그인 없이 열람 가능 |
| 파일 경로 | PDF 자료는 `investors.tempus.com/static-files/...` 형태의 공식 IR 파일 URL로 연결됨 |
| JavaScript | 주요 목록과 detail page는 텍스트로 확인 가능. 일부 동적 요소가 있을 수 있으므로 archive-wide crawling은 피함 |
| robots.txt | 이번 메모에서는 `investors.tempus.com/robots.txt`를 직접 확정 확인하지 못함 |
| Terms of Use | Tempus Terms of Use에는 robot/automatic process, unauthorized manual copying 제한 문구가 있음 |
| 보안 주의 | NTRA pilot 2에서 PDF 격리 사례가 있었으므로 TEM PDF pilot도 local_path 존재, hash, 보안 격리 여부를 반드시 QA한다 |
| 수집 권장 | 전체 News Releases 또는 Events archive를 긁지 말고, 사용자가 승인한 detail page와 PDF 후보 1~3건만 단건 수집한다 |

## 8. Webcast/audio/video/transcript 처리

TEM IR 사이트에는 earnings call, Investor Day, healthcare conference webcast 링크가 많다.

이번 pilot에서는 out-of-scope:

- webcast 저장
- audio/video 다운로드
- transcript 다운로드
- transcript 생성, 요약, 번역
- replay 수집
- external webcast provider 대량 접근

필요하면 존재 여부만 notes에 남기고, 실제 처리는 별도 transcript/audio 설계에서 다룬다.

## 9. Pilot 후보 1~3건

추천 pilot 후보:

| 우선순위 | 후보 | source_url | document_type | 수집 목적 | SEC overlap |
|---:|---|---|---|---|---|
| 1 | Tempus Reports First Quarter 2026 Results | https://investors.tempus.com/news-releases/news-release-details/tempus-reports-first-quarter-2026-results | `ir-earnings-release` | 공식 earnings release HTML 경로 검증, SEC overlap 처리 재사용 | likely |
| 2 | Q1 2026 Overview PDF | https://investors.tempus.com/static-files/e940c10c-914e-4761-95c8-709dc2669c2f | `ir-financial-supplement` | 분기 overview를 financial supplement로 처리 가능한지 확인 | likely |
| 3 | Tempus 1Q26 Corporate Deck PDF | https://investors.tempus.com/static-files/c5344fd9-1707-4c32-a622-136d824ca623 | `ir-deck` | `ir-earnings-presentation` 후보 반복성 확인 | likely |

가장 작은 pilot 권장안:

```text
1번 HTML + 2번 Q1 2026 Overview PDF
```

taxonomy 학습 가치가 더 큰 pilot 권장안:

```text
1번 HTML + 2번 Q1 2026 Overview PDF + 3번 Tempus 1Q26 Corporate Deck PDF
```

Investor Day Presentation은 학습 가치가 크지만 26.7 MB로 파일이 크고, 첫 TEM pilot에서는 부담이 있다.
따라서 이번 pilot에서는 제외하고, 별도 `ir-investor-day-presentation` 후보 검증 run으로 남기는 편이 낫다.

## 10. document_id 초안

| 후보 | document_id 초안 | 비고 |
|---|---|---|
| Q1 2026 earnings release | `ir-tem-earnings-release-fy2026-q1` | 실적 관련 자료이므로 fiscal quarter 사용 |
| Q1 2026 Overview | `ir-tem-financial-supplement-fy2026-q1-overview` | `ir-financial-supplement` 사용. source_label은 `Q1 2026 Overview` |
| Tempus 1Q26 Corporate Deck | `ir-tem-deck-fy2026-q1-corporate-deck` | `ir-deck` 사용. notes에 `candidate_document_type: ir-earnings-presentation` 가능 |
| Tempus AI Investor Day Presentation | `ir-tem-deck-2026-05-29-investor-day` | 이번 pilot 제외. 추후 `ir-investor-day-presentation` 후보 검증 가능 |
| 2025 Shareholder Letter | `ir-tem-deck-fy2025-shareholder-letter` | 이번 pilot 제외. 현 schema에는 별도 shareholder letter type 없음 |

## 11. 추천 run_scope

보수적인 2건 pilot:

```text
test only: TEM company-ir official Tempus AI Q1 2026 earnings release HTML and Q1 2026 Overview PDF; no SEC download; no SEC filings page harvesting; no webcast/audio/video; no transcript; no archive-wide crawling; no operating catalog/index merge until user approval
```

taxonomy 학습용 3건 pilot:

```text
test only: TEM company-ir official Tempus AI Q1 2026 earnings release HTML, Q1 2026 Overview PDF, and Tempus 1Q26 Corporate Deck PDF; no SEC download; no SEC filings page harvesting; no webcast/audio/video; no transcript; no archive-wide crawling; no operating catalog/index merge until user approval
```

## 12. Schema 변경 필요 여부

현재 TEM preflight만 기준으로는 schema 변경이 필수는 아니다.

사용할 수 있는 기존 type:

- `ir-earnings-release`
- `ir-financial-supplement`
- `ir-deck`

보류할 후보:

- `ir-earnings-presentation`
- `ir-investor-day-presentation`
- `ir-shareholder-letter`
- `ir-investor-conference-presentation`
- `ir-scientific-update`

결론:

```text
TEM pilot은 기존 schema로 진행 가능하다.
다만 `ir-earnings-presentation`은 반복 관찰이 쌓이고 있으므로, 실제 수집 성공 사례가 더 생기면 후보 원장 기록과 taxonomy checkpoint 업데이트가 필요하다.
```

## 13. 다음 단계

사용자가 승인하면 TEM IR pilot을 `test_collection`으로 실행한다.

권장 순서:

1. 2건 pilot로 할지 3건 pilot로 할지 선택한다.
2. SEC 다운로드 없이 company-ir raw만 run-local로 저장한다.
3. earnings-related 자료는 기존 SEC catalog/files 안에서 overlap 후보만 확인한다.
4. PDF 파일은 다운로드 후 local_path 존재, hash, 보안 격리 여부를 QA한다.
5. `Tempus 1Q26 Corporate Deck`을 수집하면 `ir-taxonomy-candidates.jsonl`에 `ir-earnings-presentation` 후보 기록 여부를 판단한다.
6. 운영 catalog/index 반영은 raw 수집과 QA 후 사용자 승인으로 결정한다.
