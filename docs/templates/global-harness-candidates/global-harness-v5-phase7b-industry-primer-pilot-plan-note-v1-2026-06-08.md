# Global Harness v5 Phase 7-B Industry Primer Pilot Plan Note v1

- 작성일: 2026-06-08
- 상태: v1 실행 계획서. Claude Code 교차검증 전.
- 대상: APP / adtech / mobile advertising
- 범위: Phase 2 - Step 4 `Industry Primer` first slice pilot
- 최신 기준: `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v0-2026-06-08.md`
- 배경 원칙: `global-harness-v5-phase7b-industry-primer-design-principles-note-2026-06-08.md`

이 문서는 논의 기록이 아니라 실행 계획서다.
`pilot plan note v0`의 Q1~Q12 합의를 기준으로, 실행자가 이 문서만 읽고 APP/adtech Industry Primer first slice pilot을 수행할 수 있게 정리한다.

원칙 충돌 시 우선순위:

1. `pilot plan note v0`
2. 이 v1 실행 계획서
3. design principles note

## 1. 목적

이 pilot의 목적은 APP/adtech Industry Primer 전체를 완성하는 것이 아니라, v5 template 후보가 다음 하네스에서 실제로 작동하는지 작게 검증하는 것이다.

검증 대상:

- Industry Primer의 산업 정의, 참여자 구조, 용어 정리, handoff 질문이 실행 가능한가
- APP partial Source Pack과 웹/외부자료를 함께 사용할 때 입력 부족을 안전하게 다루는가
- source tracking, 5층 QA, signal-routing, approval-gate, top-up trigger가 과하지 않게 작동하는가
- slice 결과를 바탕으로 Industry Primer blueprint로 넘어가도 되는가

이 문서는 다음이 아니다.

- Industry Primer 하네스 blueprint
- Industry Primer runbook
- full Industry Primer 최종 산출물
- Source Pack 보강 실행 계획
- 전역 template bundle 배포 판단 문서

## 2. 실행 전제

첫 pilot 대상은 APP / adtech / mobile advertising이다.

APP Source Pack은 full collection이 아니라 partial input이다. 따라서 이 pilot은 `Conditional Proceed`로 진행한다.

의미:

- 현재 APP Source Pack으로 first slice를 시작할 수 있다.
- 부족한 산업 구조, 참여자, 용어, platform policy 정보는 웹/외부자료로 보완한다.
- 없는 자료는 추측하지 않고 gap으로 기록한다.
- Source Pack top-up은 기본값이 아니라 예외다.
- top-up은 approval-gate를 거쳐 사용자 승인 후 별도 작업으로 수행한다.

Industry Primer v0 산출물은 산업 중심으로 작성하되 target company context를 둔다.

```text
산업 중심: adtech / mobile advertising 구조를 설명한다.
target company context: APP가 그 구조 안에서 어디에 위치하는지 필요한 만큼만 표시한다.
```

이번 v0 pilot에서는 run 단위 standalone / immutable 산출물로 취급한다.
산업별 master primer, update, company appendix, reuse/reconcile 모델은 pilot 이후 판단한다.

## 3. First Slice 범위

첫 slice는 아래 4개 섹션만 작성한다.

| Industry Primer 섹션 | 처리 | 이유 |
|---|---|---|
| Section 1. 산업 한 줄 정의 | 필수 | 산업 범위와 APP context를 빠르게 검증 |
| Section 3. 산업 참여자 구조 | 필수 | adtech/mobile advertising의 구조 이해에 중요 |
| Section 5. 핵심 용어 정리 | 필수 | glossary/type-schema 연결 검증 |
| Section 13. 다음 단계로 넘길 질문 | 필수 | handoff QA 검증 |

Section 4 `산업 하위 시장 구분`은 별도 필수 섹션으로 작성하지 않는다.
다만 adtech에서는 참여자 구조와 하위 시장 맥락이 겹치므로, Section 3 안에서 APP 위치를 이해하는 데 필요한 만큼만 간략히 다룬다.

full pilot에서 검증할 항목:

| 항목 | 처리 |
|---|---|
| Section 2. 산업이 존재하는 이유 | full pilot에서 검증 |
| Section 4. 산업 하위 시장 구분 | full pilot에서 별도 검증 |
| Section 6. 산업 성장 동인 | full pilot에서 검증 |
| Section 7. 산업 수익 구조 | full pilot에서 검증 |
| Section 8. 산업 비용 구조 | full pilot에서 검증 |
| Section 9. 규제 / 제도 / 표준 | full pilot에서 검증 |
| Section 10. 기술 변화 / 구조 변화 | full pilot에서 검증 |
| Section 11. 산업의 구조적 리스크 | full pilot에서 검증 |
| Section 12. 분석 대상 기업과의 연결 | full pilot에서 검증 |
| Section 14. Industry Primer 결론 | full pilot에서 검증 |

금지 영역은 Value Chain, Business Model, Market Share, Competition, Moat, Valuation, 투자 판단을 뜻한다.
첫 slice는 이 영역의 결론을 미리 내리지 않는다.

## 4. Input Readiness Preflight

slice 작성 전 아래를 확인한다.

| 체크 | 기준 | 처리 |
|---|---|---|
| APP index 접근 가능 여부 | `artifacts/companies/APP/index.md`를 읽을 수 있다. | 필수 control input |
| APP Q1 IR 2건 접근 가능 여부 | APP Q1 2026 earnings release HTML, APP Q1 2026 financial update PDF를 읽을 수 있다. | 필수 content input |
| FY2026 Q1 8-K / EX-99.1 접근 가능 여부 | APP FY2026 Q1 8-K / EX-99.1을 읽을 수 있다. | 필수 content input |
| QA/run-summary 확인 | APP IR pilot QA와 APP SEC-IR overlap recheck run-summary를 확인할 수 있다. | 필수 입력이 아니라 preflight 참조 |
| partial input gap 식별 | 10-K, 10-Q, proxy, transcript, webcast/audio/video, 산업/경쟁 자료 부재를 확인한다. | gap으로 기록 |
| top-up trigger 미발동 여부 | 필수 input이 있고 Section 1, 3, 5, 13 작성이 가능하다. | top-up 없이 진행 |

preflight 참조 자료:

| 구분 | 파일 | 역할 |
|---|---|---|
| control input | `artifacts/companies/APP/index.md` | APP Source Pack 현재 상태, partial 범위, 누락 자료 확인 |
| preflight 참조 | `artifacts/runs/run-20260604-app-ir-pilot/qa.md` | APP IR 2건이 정상 반영됐는지 검산 |
| preflight 참조 | `artifacts/runs/run-20260605-app-sec-ir-overlap-recheck/run-summary.md` | SEC-IR overlap 관계와 partial 상태 검산 |

QA/run-summary는 first slice의 필수 입력이 아니다.
이 파일들은 Source Pack 상태를 검산하는 preflight 참조다.

preflight 결과 처리:

| 결과 | 처리 |
|---|---|
| 필수 control/content input 접근 가능 | slice 진행 |
| 필수 input 중 하나가 없거나 열 수 없음 | top-up 승인 요청 후보 |
| 10-K/10-Q/proxy/transcript 없음 | 첫 slice blocking 아님. gap 기록 |
| 산업/경쟁 자료 없음 | Source Pack top-up이 아니라 웹/외부자료로 보완 |
| source gap을 표시하면 안전하게 진행 가능 | `pass` 또는 `pass with adjustments` 후보 |
| source gap 때문에 산업 정의, 참여자 구조, 핵심 용어를 판단할 수 없음 | `blocked` 후보 |

## 5. Source Pack 입력 범위

첫 slice는 APP partial Source Pack을 `Conditional Proceed` 입력으로 사용한다.

필수 입력:

| 구분 | 입력 | 역할 |
|---|---|---|
| 필수 control input | APP company index | Source Pack 현재 상태와 partial 범위 확인 |
| 필수 content input | APP Q1 2026 earnings release HTML | 회사가 직접 설명하는 사업, 제품, 시장 언어 확인 |
| 필수 content input | APP Q1 2026 financial update PDF | 회사 공식 KPI, segment, product/market 표현 확인 |
| 필수 content input | APP FY2026 Q1 8-K / EX-99.1 | SEC canonical 자료로 earnings release 교차 확인 |

top-up 후보로만 기록할 입력:

| 입력 | 첫 slice 처리 |
|---|---|
| full 10-K / 10-Q | blocking 아님. full pilot에서 사업 설명, segment, risk 확인이 막히면 후보 |
| Proxy | Industry Primer first slice에는 필요 없음 |
| transcript / webcast / audio / video | Earnings Call 또는 Business Model 쪽 후보 |
| 산업/경쟁 자료 | Source Pack top-up이 아니라 웹/외부자료에서 처리 |
| derived text | 편의 문제. Source Pack 보강 필수 조건 아님 |

Source Pack은 APP 공식 관점과 자료 한계를 제공한다.
산업 구조 자체는 웹/외부자료로 보완한다.

## 6. 웹검색 / 외부자료 범위

웹검색과 외부자료는 산업 정의, 참여자 구조, 핵심 용어, APP가 속한 하위 시장 맥락을 확인하는 데 사용한다.

허용 범위:

| 범위 | 처리 |
|---|---|
| adtech / mobile advertising / app monetization 산업 정의 | 허용 |
| 주요 참여자 유형 | 허용 |
| 핵심 용어 | 허용 |
| APP가 속한 하위 시장 맥락 | 허용 |
| ATT / SKAN / IDFA / attribution / Privacy Sandbox | 허용 |

제한적으로 허용:

| 범위 | 처리 |
|---|---|
| 시장 규모 / 성장률 | 맥락적 규모만 허용. 정밀 TAM, CAGR, 점유율 추정 금지 |
| 최신 규제·정책 변화 | 용어와 구조 이해에 필요한 만큼만 사용 |
| 경쟁사 자료 | 참여자 예시나 용어 이해용으로만 사용 |
| 컨설팅 / 리서치 / 블로그 자료 | 보조 자료. 핵심 fact는 가능한 원천 자료로 교차 확인 |
| Wikipedia / 일반 설명 자료 | 방향 잡기용. 최종 핵심 근거로는 약함 |
| paywall 자료 | 접근 가능한 공개 내용만 사용하고 보이지 않는 내용은 추정 금지 |

금지:

| 범위 | 이유 |
|---|---|
| 경쟁사 우열 판단 | Competition / Moat 영역 |
| 시장점유율 결론 | Market Share 영역 |
| moat / valuation / 투자 판단 | Industry Primer 비범위 |
| AI 요약문을 원천 출처로 사용 | 실제 source가 아니므로 source_register 원천으로 쓰지 않음 |

웹 source는 source_register에 기록한다.
웹 source에는 `accessed_at`을 반드시 남긴다.

## 7. source_register 계약

첫 slice에서는 source_register를 Industry Primer slice output 내부 필수 섹션으로 둔다.
별도 source_register artifact는 만들지 않는다.

별도 artifact는 full pilot에서 source 수가 많아지거나 여러 산출물이 같은 source_register를 공유할 때 후보로 올린다.

source_register 최소 필드:

| 필드 | 필수 여부 | 설명 |
|---|---|---|
| `source_id` | 필수 | 산출물 본문에서 `source_ref`로 참조할 식별자 |
| `source_type` | 필수 | `source_pack`, `sec`, `company_ir`, `web`, `industry_reference` 등 |
| `title` | 필수 | 사람이 식별할 수 있는 source 제목 |
| `url_or_path` | 필수 | 웹 URL 또는 로컬 파일 경로 |
| `accessed_at` | 웹 source 필수 | 웹 source를 확인한 날짜 |
| `reliability` | v0 제외 | 신뢰도 평가는 v0에서 필드로 강제하지 않고 pilot 이후 후보로만 둔다. |

작성 원칙:

- source_register는 본문 뒤에 둔다.
- 핵심 fact와 산업 구조 주장에는 `source_ref`를 붙인다.
- Source Pack 파일과 웹 source를 같은 source_register 안에서 구분한다.
- 웹 source는 최소한 title, URL, source_type, accessed_at을 기록한다.

## 8. 필수 산출물

첫 slice의 필수 산출물은 3개다.

| 산출물 | 필수 여부 | 권장 파일명 / placeholder | 역할 |
|---|---|---|---|
| Industry Primer slice output | 필수 | `artifacts/runs/{run-id}/industry-primer-slice.md` 또는 blueprint에서 확정 | Section 1, 3, 5, 13과 내부 source_register를 담은 실제 slice 산출물 |
| Slice QA result | 필수 | `artifacts/runs/{run-id}/qa.md` 또는 blueprint에서 확정 | 5층 QA 기준의 통과/보완/차단 판단 |
| Pilot observation note | 필수 | `artifacts/runs/{run-id}/pilot-observation-note.md` 또는 blueprint에서 확정 | Industry Primer 내용 품질 관찰과 v5 template 적용 평가 |

내부 필수 섹션:

| 항목 | 위치 |
|---|---|
| `source_register` | Industry Primer slice output 내부 필수 섹션 |

선택 섹션:

| 항목 | 위치 |
|---|---|
| signal list | QA result 또는 Pilot observation note 내부 선택 섹션 |

signal list는 별도 필수 파일로 만들지 않는다.
중요 signal은 QA result 또는 Pilot observation note 안에 기록한다.

Pilot observation note는 두 섹션으로 나눈다.

| 섹션 | 내용 |
|---|---|
| Section A. Industry Primer 내용 품질 관찰 | source gap, 범위 초과 위험, handoff 품질, 확인 필요 항목 |
| Section B. v5 template 적용 평가 | module fit, 과한 module, 빠진 module, signal-routing/candidate-ledger 후보, 다음 pilot 또는 Phase 7-C 반영 후보 |

## 9. Module 적용

첫 Industry Primer slice에서는 모든 module을 전부 켜지 않는다.
최소 구조가 실제로 작동하는지 확인하는 tier 방식으로 적용한다.

| tier | module | first slice 처리 |
|---|---|---|
| 필수 적용 | `design-preflight` | Industry Primer 유형, slice 범위, 금지 영역, 산출물 역할을 고정 |
| 필수 적용 | `approval-gate` | top-up, 범위 확장, 하네스 파일 생성, 별도 artifact 추가 전 승인 경계 |
| 필수 적용 | `qa-scaffold` | Slice QA result와 5층 QA 실행 |
| 필수 적용 | `signal-routing` | 별도 signal 파일 없이 QA/observation note 안에서 공통 signal 표현 사용 |
| 필수 적용 | `pilot-first / testing` | preflight -> slice -> full pilot 순서 유지 |
| 기본 적용 | `security-baseline` | 웹자료, 로컬 source 파일, checkpoint를 다루므로 최소 안전선 적용 |
| 좁게 적용 | `type-schema` | source_register 필드, source_ref, 확인 필요, handoff question 구조만 잡음 |
| 가볍게 적용 | `observability` | Pilot observation note로 병목, 과잉/누락, module fit 기록 |
| 가볍게 적용 | `docs-organization` | 새 폴더 구조를 강제하지 않고 산출물 위치와 README/작업지도 참조 유지 |
| 조건부 적용 | `comparison` | 기본 실행에서는 사용하지 않고, 결과 품질이 애매하거나 교차검증이 필요할 때만 사용 |
| 조건부 적용 | `checkpoint` | 세션 전환, 컨텍스트 압축 위험, 사용자 저장 요청 시에만 사용 |
| 관찰만 | `candidate-ledger` | 별도 ledger 파일 없이 반복 후보를 Pilot observation note Section B에 기록 |

comparison 발동 조건은 Section 10의 QA 기준을 따른다.
candidate-ledger는 첫 slice에서 별도 파일을 만들지 않는다.
checkpoint는 품질 module이 아니라 세션 연속성 도구로만 사용한다.

## 10. QA 완료 판정

QA 기준은 "완벽한 산출물인가"가 아니라 "후속 단계로 넘겨도 안전한가"이다.

전체 판정:

| 판정 | 의미 |
|---|---|
| `pass` | 필수 산출물 3개가 있고, 5층 QA에서 치명적 결함이 없으며, downstream 하네스에 넘겨도 안전하다. |
| `pass with adjustments` | 큰 구조는 작동하지만 source gap, 범위 문구, handoff 질문, source_register, 확인 필요 표시를 보완해야 한다. |
| `blocked` | 필수 섹션/source_register 누락, 핵심 주장 출처 부재, 금지 영역 결론 선제 도출, 또는 입력 부족 때문에 산업 정의/참여자/핵심 용어를 판단할 수 없다. |

5층 QA 기준:

| QA 층 | `pass` | `pass with adjustments` | `blocked` |
|---|---|---|---|
| 구조 QA | Section 1, 3, 5, 13과 `source_register`가 정해진 구조로 있다. | 일부 표/필드/문체 정리가 필요하지만 의미는 사용할 수 있다. | 필수 섹션 또는 `source_register`가 없거나 산출물이 slice 범위와 맞지 않는다. |
| 출처 QA | 핵심 fact와 산업 구조 주장에 `source_ref`가 있고, 웹 source가 `source_register`에 기록됐다. | 일부 보조 주장이나 비핵심 문장의 `source_ref` 정리가 필요하다. | 핵심 주장의 출처가 다수 없거나, `source_register`가 없거나 깨졌거나, 웹 fact를 추적할 수 없다. |
| 범위 QA | Industry Primer 범위 안에 머문다. | 일부 문장이 금지 영역 쪽으로 기울지만 수정 가능. | 금지 영역 결론을 미리 냈거나, Industry Primer가 아닌 후속 단계 분석으로 넘어갔다. |
| 판단 QA | fact, interpretation, 확인 필요가 분리되고 불확실성이 드러난다. | 일부 확인 필요 표시나 interpretation label을 보완해야 한다. | 추측을 fact처럼 쓰거나, 불확실성을 숨기거나, 근거 없는 강한 결론을 낸다. |
| handoff QA | 다음 하네스가 바로 사용할 수 있는 구체적 질문을 남긴다. | 질문은 유용하지만 더 구체화해야 한다. | handoff 질문이 없거나 너무 막연하거나, 후속 단계 결론을 미리 결정한다. |

전체 판정 집계:

| 조건 | 전체 판정 |
|---|---|
| 5층 QA 중 하나라도 `blocked` | `blocked` |
| `blocked`는 없고 하나라도 `pass with adjustments` | `pass with adjustments` |
| 5층 QA가 모두 `pass` | `pass` |

Source gap 처리:

| 상황 | 처리 |
|---|---|
| 10-K, transcript, proxy 같은 자료가 없지만 slice의 산업 정의/참여자/핵심 용어 판단에는 직접 필요하지 않음 | `pass` 또는 `pass with adjustments` 가능. gap을 명시 |
| source gap 때문에 산업 정의, 참여자 구조, 핵심 용어를 판단할 수 없음 | `blocked` |
| APP Source Pack이 partial이라는 사실을 숨기거나 source gap을 표시하지 않음 | `blocked` |

comparison 발동:

| QA 상황 | comparison 처리 |
|---|---|
| 판정이 명확한 `pass` | comparison 불필요 |
| `pass with adjustments`이고 수정 방향이 명확함 | comparison 선택 사항 |
| QA 판정이 애매하거나, 보완 방향이 불명확하거나, `blocked` 원인이 data gap인지 실행 품질인지 구분이 안 됨 | comparison 권장 |
| 범위 위반 여부가 논쟁적이거나, 사용자가 핵심 해석 차이를 직접 판단하기 어려움 | comparison 권장 |

## 11. Source Pack Top-up과 Approval Gate

Source Pack top-up은 기본값이 아니라 예외다.

역할 분리:

| 항목 | 역할 |
|---|---|
| `approval-gate` | 범위 변경, Source Pack top-up, 하네스 파일 생성, 별도 artifact 추가를 승인할지 판단 |
| Source Pack top-up trigger | 자료 부족 때문에 현재 slice를 멈추고 Source Pack 보강을 요청해야 하는지 판단 |
| `qa-scaffold` | 자료 부족이 `pass`, `pass with adjustments`, `blocked` 중 어디에 해당하는지 판정 |

범위 확장과 top-up의 관계:

```text
scope 확장은 top-up trigger가 아니라 별도 approval-gate 사안이다.
scope 확장이 승인되면 새 범위 기준으로 source 충분성을 다시 평가한다.
그 재평가에서 blocking gap이 발견될 때만 Source Pack top-up 요청으로 이어진다.
```

현재 APP partial Source Pack으로 계속 진행하는 경우:

| 상황 | 처리 |
|---|---|
| APP index와 필수 content input을 읽을 수 있다. | 계속 진행 |
| APP Q1 IR 자료와 8-K / EX-99.1로 APP의 공식 사업 설명을 확인할 수 있다. | 계속 진행 |
| 부족한 정보가 산업 구조, 용어, 참여자, ATT/SKAN 같은 외부 산업 맥락이다. | 웹검색 / 외부자료로 보완 |
| 10-K, proxy, transcript가 없지만 Section 1, 3, 5, 13 작성에는 치명적이지 않다. | gap 명시 후 진행 |
| 일부 source가 더 있으면 좋지만 핵심 정의와 참여자 구조를 쓸 수 있다. | `pass` 또는 `pass with adjustments` 후보 |
| 확인 필요로 표시하면 downstream에 위험 없이 넘길 수 있다. | 계속 진행 |

top-up 후보로만 기록하는 경우:

| 상황 | 처리 |
|---|---|
| 10-K / 10-Q가 있으면 full pilot 품질이 좋아질 것 같다. | top-up 후보 기록 |
| transcript가 있으면 Business Model 또는 Earnings Call 계열에서 유용할 것 같다. | 후속 하네스 후보 기록 |
| proxy가 있으면 governance, incentive, ownership 분석에 유용할 것 같다. | 후속 단계 후보 기록 |
| APP의 segment/product 용어를 더 정확히 잡고 싶지만 현재 slice는 진행 가능하다. | top-up 후보 기록 |
| 같은 종류의 source gap이 반복되어 future Source Pack 개선 후보가 된다. | Pilot observation note Section B에 기록 |
| source gap 때문에 전체 판정이 `pass with adjustments`이지만 `blocked`는 아니다. | 보강 후보로 기록하고 slice는 계속 진행 |

실제 top-up 승인 요청이 필요한 경우:

| trigger | 이유 |
|---|---|
| 필수 control input인 APP index가 없거나 신뢰할 수 없다. | Source Pack 현재 상태와 partial 범위를 확인할 수 없다. |
| 필수 content input인 APP Q1 IR 자료 또는 8-K / EX-99.1이 없거나 열 수 없다. | APP 공식 관점 확인이 불가능하다. |
| APP의 산업 정의, 제품 위치, 참여자 구조가 공식 자료만으로 전혀 잡히지 않는다. | Section 1 또는 Section 3 작성이 불안정하다. |
| 핵심 용어가 APP 공식 자료와 외부자료 사이에서 충돌하고 현재 자료로 해결할 수 없다. | 잘못된 glossary가 downstream으로 넘어갈 위험이 있다. |
| `source_ref`를 붙일 수 없는 핵심 주장이 많다. | 출처 QA가 `blocked`가 된다. |
| 보안 격리, 접근 불가, quarantined source 때문에 필수 자료를 확인할 수 없다. | security-baseline과 approval-gate 확인이 필요하다. |
| 범위 확장 승인 후, 새 범위 기준 source 충분성 재평가에서 blocking gap이 발견된다. | scope 변경 자체가 아니라 새 범위의 자료 부족이 top-up 사유다. |

top-up 요청 형식:

```text
Source Pack top-up 필요

- 필요한 자료:
- 필요한 이유:
- 영향을 받는 slice 섹션:
- 없으면 blocked가 되는 이유:
- 수행 범위:
- 기존 APP partial Source Pack으로 대체할 수 없는 이유:

사용자 승인 후 별도 Source Pack top-up 실행
```

## 12. 실행 순서

first slice pilot은 아래 순서로 진행한다.

| 순서 | 단계 | 수행 내용 | 산출물 |
|---|---|---|---|
| 1 | Input Readiness Preflight | APP index, APP Q1 IR 2건, FY2026 Q1 8-K / EX-99.1 접근 가능 여부와 top-up trigger 미발동 여부 확인 | preflight 결과는 QA result 또는 observation note에 기록 |
| 2 | Slice 작성 | Section 1, 3, 5, 13과 내부 source_register 작성 | Industry Primer slice output |
| 3 | Slice QA | 5층 QA, 전체 판정 집계, source gap, scope, handoff 확인 | Slice QA result |
| 4 | Pilot observation | 내용 품질 관찰과 v5 template 적용 평가 기록 | Pilot observation note |
| 5 | 완료 판정 | `pass`, `pass with adjustments`, `blocked` 중 하나로 정리 | QA result |
| 6 | 다음 단계 결정 | blueprint로 넘어갈지, plan을 조정할지, top-up 후보를 남길지 결정 | observation note / work map 업데이트 |

이 단계에서는 실제 Industry Primer 하네스 파일을 만들지 않는다.
하네스 blueprint 작성과 파일 생성은 v1 교차검증 후 별도 approval-gate를 거쳐 진행한다.

## 13. 완료 후 판단

first slice pilot 결과는 아래 기준으로 다음 행동을 정한다.

| 결과 | 다음 행동 |
|---|---|
| `pass` | Industry Primer blueprint 작성 논의로 진행 |
| `pass with adjustments` | 보완 항목을 observation note에 기록하고, blueprint에 반영할지 결정 |
| `blocked` | blocked 원인이 input gap, scope 문제, module 설계 문제 중 무엇인지 분리한 뒤 재계획 |

Phase 7-B의 목적은 v5 template 후보의 실전 사용성을 확인하는 것이다.
따라서 Pilot observation note에는 산출물 품질뿐 아니라 module fit, 과한 규칙, 빠진 규칙, signal-routing/candidate-ledger 후보를 함께 기록한다.

## 14. v1 자체 검산

v1 작성 시 아래를 자체 확인한다.

| 검산 항목 | 확인 |
|---|---|
| Q1 합의 | first slice가 Section 1, 3, 5, 13으로 제한됐고 Section 4는 Section 3 안의 간략한 맥락으로 처리됐는가 |
| Q2 합의 | APP index, APP Q1 IR 2건, FY2026 Q1 8-K / EX-99.1 입력 계약과 QA/run-summary preflight 참조가 반영됐는가 |
| Q3 합의 | 웹/외부자료 허용, 제한, 금지 범위가 반영됐는가 |
| Q4 합의 | source_register가 slice output 내부 필수 섹션으로 반영됐는가 |
| Q5 합의 | 필수 산출물 3개와 signal list 비분리 원칙이 반영됐는가 |
| Q6 합의 | module 적용 tier와 comparison/checkpoint/candidate-ledger 처리가 반영됐는가 |
| Q7 합의 | 7개 금지 영역, 5층 QA, 전체 판정 집계, comparison 발동 조건이 반영됐는가 |
| Q8 합의 | comparison이 기본 비활성, 조건부 적용으로 반영됐는가 |
| Q9 합의 | candidate-ledger가 별도 파일 없이 observation note 후보 기록으로 반영됐는가 |
| Q10 합의 | checkpoint가 세션 전환, 압축 위험, 사용자 요청 시에만 사용되는 것으로 반영됐는가 |
| Q11 합의 | top-up 3단계와 approval-gate/top-up/qa-scaffold 역할 분리가 반영됐는가 |
| Q12 합의 | v1이 논의 기록이 아니라 실행 계획서로 작성됐는가 |

합의 외 추가 통제:

| 금지 | 확인 |
|---|---|
| v0 합의에 없는 새 조건 추가 | 추가하지 않음 |
| v0 합의에 없는 새 기준 추가 | 추가하지 않음 |
| v0 합의에 없는 새 예외 추가 | 추가하지 않음 |
| v0 합의에 없는 필수 산출물 추가 | 추가하지 않음 |
| v0 합의에 없는 승인 조건 추가 | 추가하지 않음 |

Claude Code 교차검증에서는 위 자체 검산표를 기준으로 누락과 과잉을 모두 확인한다.
