# Global Harness v5 Phase 7-B Industry Primer First Slice Rubric Calibration Note

- 작성일: 2026-06-09
- 상태: Claude Code 교차검증 PASS, Section 12 minor fix 반영
- 단계: Phase 7-B Industry Primer blueprint 작성 직전
- 작업 지도 항목: Industry Primer blueprint 초안 작성 전 rubric calibration
- 기준 문서:
  - `global-harness-v5-work-map.md`
  - `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v1-2026-06-08.md`
  - `global-harness-v5-phase7b-industry-primer-blueprint-prep-note-2026-06-08.md`
  - `global-harness-v5-phase7b-industry-primer-blueprint-prep-section11-consensus-note-2026-06-09.md`
  - `Phase 2 - Step 4 Industry Primer 템플릿.md`
  - 필요 시 Step 5-7 template는 handoff 경계 확인용으로만 참조

이 문서는 Industry Primer blueprint가 아니다.

실제 `harness/` 파일, contract, procedure, schema, rubric, adapter를 만들지 않는다.
또한 작업 지도나 README 갱신 문서도 아니다.

이 문서의 목적은 Industry Primer first slice가 다루는 Section 1, 3, 5, 13에 대해 "좋은 답", "보완하면 통과 가능한 답", "차단해야 하는 답"의 실제 내용 기준을 calibration하는 것이다.

## 1. 이 문서를 따로 만드는 이유

지금까지의 Phase 7-B 논의는 Industry Primer 하네스의 구조를 충분히 정리했다.

- first slice 범위: Section 1, 3, 5, 13
- QA 구조: 5층 QA
- QA 결과: `pass`, `pass with adjustments`, `blocked`
- QA output: `qa.md` 또는 이에 준하는 8섹션 구조
- output schema: prose 본문 + `source_register` + Section 13 handoff table
- Section 13 v0 필드: `question`, `source_ref`, `status`
- module 연결: `design-preflight`, `qa-scaffold`, `signal-routing`, `approval-gate`, `type-schema` 등

하지만 분석형/판단형 하네스에서는 구조만으로 품질이 결정되지 않는다.

Industry Primer의 품질은 결국 아래 질문에 달려 있다.

> 이 답변이 산업을 이해하기 위한 좋은 기초 지도로 충분한가?

이 질문은 파일 존재 여부처럼 기계적으로만 판단할 수 없다.
LLM adapter가 rubric을 읽고 판단해야 한다.
따라서 blueprint 작성 전에 first slice 기준의 내용 calibration을 별도 문서로 고정한다.

## 2. First Slice Rubric과 Full Rubric을 분리하는 이유

first slice rubric과 full Industry Primer rubric/QA는 분리해야 한다.

| 구분 | first slice rubric | full Industry Primer rubric/QA |
|---|---|---|
| 대상 | Section 1, 3, 5, 13 | Section 1-14 전체 |
| 목적 | 하네스 구조, source tracking, 판단형 QA, handoff가 작동하는지 검증 | 완성형 Industry Primer 산출물의 품질 검증 |
| 깊이 | 좁고 실행 가능한 최소 기준 | 산업 구조, 성장 동인, 규제, 기술 변화, 리스크, 기업 연결, 결론까지 포함 |
| 위험 | 너무 무겁게 만들면 first pilot이 느려짐 | 너무 얕게 만들면 완성형 산출물 품질이 흔들림 |
| 사용 시점 | APP/adtech first slice pilot | first slice 완료 후 full pilot 진입 전 |

first slice rubric은 full rubric의 축소판이 아니다.

first slice rubric은 "작은 실행으로 하네스 메커니즘을 검증하기 위한 기준"이다.
full Industry Primer rubric/QA expansion은 first slice pilot 완료 후, full pilot 진입 전에 별도 작업으로 추적해야 한다.

즉 지금 문서가 Section 2, 4, 6-12, 14의 깊은 평가 기준까지 확정하지 않는다.
해당 확장은 pilot 결과를 본 뒤 진행한다.

## 3. Domain-Agnostic but Actionable 원칙

Industry Primer rubric은 산업별로 새로 만드는 checklist가 아니다.

전역 rubric은 공통 판단 기준을 제공하고, 실제 적용 시에는 아래 세 가지가 결합된다.

| 요소 | 역할 |
|---|---|
| 공통 rubric | 좋은 산업 기초 지도의 품질 기준 |
| pilot spec / contract | 이번 실행의 산업, 기업 context, 입력, 범위 |
| LLM 도메인 지식과 source | 해당 산업에 맞게 기준을 적용하는 판단 근거 |

따라서 rubric은 아래 수준을 목표로 한다.

| 수준 | 예시 | 판정 |
|---|---|---|
| 너무 추상 | "산업을 잘 정의한다." | 실행자가 무엇을 해야 할지 모름 |
| 너무 구체 | "adtech에서는 DSP, SSP, ad network, SKAN을 반드시 포함한다." | 특정 산업에 과적합 |
| 적절 | "산업 경계, 해결하는 문제, 주요 참여자, target company의 생태계상 위치가 드러나야 한다." | 공통 기준이면서 실제 판단 가능 |

APP/adtech 예시는 calibration을 돕기 위한 예시일 뿐이다.
전역 rubric의 필수 조건으로 hard-code하지 않는다.

## 4. 금지 영역과 범위 원칙

Industry Primer first slice의 금지 영역은 pilot plan note v1 기준을 따른다.

금지 영역은 아래 7개다.

- Value Chain
- Business Model
- Market Share
- Competition
- Moat
- Valuation
- 투자 판단

Industry Primer는 산업을 이해하기 위한 기초 지도다.
후속 단계의 결론을 미리 내리면 안 된다.

허용되는 것은 descriptive mapping이다.
차단해야 하는 것은 evaluative conclusion이다.

| 허용 | 차단 |
|---|---|
| 이 산업에는 어떤 참여자가 있고, target company는 어디에 위치하는가 | 이 회사는 value chain상 좋은 위치에 있다 |
| 어떤 용어와 구조 변화가 산업 이해에 중요해 보이는가 | 이 구조 변화 때문에 이 회사의 moat가 강하다 |
| 다음 단계에서 확인해야 할 질문은 무엇인가 | 다음 단계의 답을 이미 결론으로 제시 |
| 시장 규모의 맥락적 중요성 | TAM, CAGR, 점유율, runway 결론 |

## 5. Industry Primer와 Value Chain을 분리해야 하는 이유

Industry Primer와 Value Chain은 모두 "대상 기업이 산업 안에서 어디에 있는가"를 다룰 수 있다.
하지만 두 하네스의 질문은 다르다.

| 구분 | Industry Primer | Value Chain |
|---|---|---|
| 핵심 질문 | 이 산업은 어떻게 구성되어 있고, target company는 어떤 맥락에서 이해해야 하는가 | 이 산업에서 돈과 비용은 어떻게 흐르고, target company의 위치는 경제적으로 좋은가 |
| target company 위치 | 산업 지도를 읽기 위한 context | 수익성, 병목, profit pool, pricing power 판단의 출발점 |
| 허용 수준 | descriptive position | economic position evaluation |
| 산출물 역할 | 다음 분석을 시작할 수 있게 하는 지도 | 회사의 산업 내 경제적 위치를 판단하는 분석 |

두 하네스가 공유하는 "대상 기업 위치" 기준은 중복이 아니다.
그것은 handoff 연결점이다.

Industry Primer는 "여기에 있는 것 같다"까지 정리한다.
Value Chain은 "그 위치가 경제적으로 어떤 의미를 가지는가"를 분석한다.

따라서 Industry Primer Section 3에서 target company 위치를 전혀 쓰지 않으면 handoff가 약해진다.
반대로 그 위치가 좋은지, profit pool에 가까운지, pricing power가 있는지까지 판단하면 Value Chain을 침범한다.

## 6. 공통 판정 레벨

각 Section의 판정은 아래 공통 의미를 가진다.

| 판정 | 의미 |
|---|---|
| 좋은 답 / `pass` | 후속 단계가 읽어도 안전하고, source로 추적 가능하며, Industry Primer 범위 안에서 구체적이다. |
| `pass with adjustments` | 핵심 방향은 맞지만, 범위 문구, source_ref, 구체성, 확인 필요 표시를 보완해야 한다. |
| `blocked` | 필수 내용이 없거나, 핵심 주장 근거가 없거나, 금지 영역 결론을 내리거나, 후속 단계가 사용하면 위험하다. |

`pass with adjustments`는 "나쁜 답"이 아니다.
first slice pilot에서는 이 판정이 중요하다.
어떤 보완이 필요한지 드러내는 것이 pilot의 목적 중 하나이기 때문이다.

## 7. Section 1. 산업 한 줄 정의 Calibration

### 7.1 Section 1의 역할

Section 1은 산업을 한 문장 또는 매우 짧은 표로 정의한다.
여기서의 목적은 완벽한 산업론이 아니라, 이후 Section 3, 5, 13이 같은 산업 경계를 보고 움직이게 하는 것이다.

좋은 Section 1은 아래를 드러낸다.

- 이 산업이 해결하는 문제
- 주요 고객, 사용자, payer 또는 demand source
- 주요 참여자들이 연결되는 방식의 큰 윤곽
- target company가 어떤 맥락에서 이 산업과 연결되는지
- 너무 넓거나 좁은 산업 정의가 아닌지
- source_ref 또는 source_register와 연결 가능한 근거

### 7.2 좋은 답 / Pass

| 기준 | 설명 |
|---|---|
| 산업 경계가 보인다 | "무엇을 포함하고 무엇은 제외하는지"가 최소한 암시된다. |
| 해결하는 문제가 보인다 | 단순 카테고리 이름이 아니라 산업이 존재하는 이유가 드러난다. |
| 고객/수요자가 보인다 | 누가 이 산업의 서비스를 필요로 하는지 알 수 있다. |
| target company context가 있다 | 대상 기업이 왜 이 산업 primer의 trigger인지 보인다. |
| descriptive 수준에 머문다 | 경쟁우위, 시장점유율, 밸류에이션, 투자 판단을 하지 않는다. |
| 추적 가능하다 | 핵심 fact나 회사 context가 source_ref로 연결된다. |

좋은 답은 아래처럼 읽힌다.

> 이 산업은 특정 고객 문제를 해결하기 위해 여러 참여자가 연결되는 구조이며, target company는 그 구조 안의 특정 역할을 수행한다.

### 7.3 Pass with Adjustments

아래 경우는 보완하면 통과 가능하다.

| 경우 | 필요한 보완 |
|---|---|
| 산업 정의가 너무 넓다 | slice 범위에 맞게 하위 산업 또는 적용 맥락을 좁힌다. |
| 산업 정의가 너무 좁다 | target company의 일부 제품만 설명하지 않도록 경계를 넓힌다. |
| target company 위치가 모호하다 | "이 회사가 왜 이 산업 primer의 대상인지"를 한 줄 추가한다. |
| 고객/사용자/payer가 섞여 있다 | 누가 쓰고, 누가 돈을 내고, 누가 영향을 받는지 분리한다. |
| source_ref가 약하다 | 핵심 회사 context 또는 산업 정의에 source_ref를 붙인다. |
| 용어가 어렵다 | Section 5에서 풀어야 할 용어 후보로 넘긴다. |

### 7.4 Blocked

아래 경우는 차단한다.

| 경우 | 이유 |
|---|---|
| 산업을 거의 정의하지 못한다 | 이후 Section 3, 5, 13이 같은 경계를 볼 수 없다. |
| target company와 산업의 연결이 없다 | pilot 대상 맥락이 사라진다. |
| 핵심 정의가 source 없이 단정된다 | 출처 QA가 blocked가 된다. |
| 후속 단계 결론을 미리 낸다 | Value Chain, Business Model, Market Share, Competition, Moat, Valuation, 투자 판단 침범 |
| 투자 thesis처럼 작성된다 | Industry Primer가 아니라 투자 판단으로 변질된다. |

### 7.5 APP/adtech 비규범 예시

APP/adtech에서는 "모바일 앱 광고/수익화 생태계" 또는 "광고주와 앱 publisher/developer를 연결해 사용자 획득과 앱 monetization을 돕는 산업 구조" 같은 방향이 calibration 예시가 될 수 있다.

하지만 전역 rubric은 `DSP`, `SSP`, `SKAN`, `IDFA` 같은 특정 용어를 Section 1 필수 조건으로 요구하지 않는다.
그 용어가 중요하면 Section 5에서 다룬다.

## 8. Section 3. 산업 참여자 구조 Calibration

### 8.1 Section 3의 역할

Section 3은 산업 안의 주요 참여자와 역할 관계를 정리한다.
first slice에서는 완성형 value chain map을 만들지 않는다.
다만 target company가 어떤 참여자들과 연결되는지 알 수 있을 정도의 산업 지도가 필요하다.

Section 3은 Step 5 Value Chain과 가장 가까운 경계에 있다.
따라서 "위치 설명"은 허용하지만 "위치의 경제적 매력도 판단"은 금지한다.

### 8.2 좋은 답 / Pass

| 기준 | 설명 |
|---|---|
| 주요 참여자 category가 있다 | 회사명 나열이 아니라 역할 category로 정리한다. |
| 각 참여자의 역할이 설명된다 | 누가 무엇을 제공하고, 누구에게 가치를 주는지 드러난다. |
| 흐름이 보인다 | 돈, 데이터, 제품, 서비스, demand, supply 중 해당 산업에서 중요한 흐름이 최소한 보인다. |
| target company 위치가 descriptive하게 표시된다 | 대상 기업이 어느 category 또는 연결 지점에 있는지 보인다. |
| 하위 시장 맥락을 필요한 만큼만 포함한다 | Section 4를 완전히 수행하지 않고, Section 3 이해에 필요한 정도로만 다룬다. |
| 예시는 예시로 남는다 | 대표 기업이나 사례를 category 자체와 혼동하지 않는다. |
| source_ref가 있다 | 핵심 구조나 target company 위치가 근거 source와 연결된다. |

좋은 Section 3은 "누가 있는가"보다 "누가 어떤 역할로 연결되는가"를 보여준다.

### 8.3 Pass with Adjustments

| 경우 | 필요한 보완 |
|---|---|
| 참여자 category는 있으나 흐름이 약하다 | 최소한 demand/supply/revenue/data 흐름 중 중요한 흐름을 한 줄 추가한다. |
| 회사명 예시가 너무 많다 | 대표 예시는 줄이고 category 중심으로 정리한다. |
| target company 위치가 암시만 된다 | 별도 행 또는 문장으로 명시한다. |
| 하위 시장이 필요한데 빠졌다 | Section 3 이해에 필요한 만큼만 간략히 추가한다. |
| buyer/user/payer가 섞여 있다 | 산업에 맞는 구분으로 보완한다. |
| 일부 역할 설명에 source_ref가 없다 | 핵심 역할에 source_ref를 붙인다. |

### 8.4 Blocked

| 경우 | 이유 |
|---|---|
| 단순 회사명 리스트다 | 산업 구조를 이해할 수 없다. |
| 참여자 역할을 잘못 배치한다 | downstream 분석이 잘못된 지도를 읽게 된다. |
| target company 위치가 없다 | Industry Primer가 특정 기업 context를 잃는다. |
| profit pool, pricing power, 병목, moat를 결론낸다 | Value Chain 또는 Moat 침범 |
| 시장점유율이나 TAM 결론으로 넘어간다 | Market Share 침범 |
| source 없이 구조를 단정한다 | 출처 QA가 blocked가 된다. |

### 8.5 APP/adtech 비규범 예시

APP/adtech에서는 광고주, agency, app developer/publisher, ad network, mediation, attribution/measurement, mobile platform 정책 같은 category가 예시가 될 수 있다.

그러나 이 목록은 APP/adtech calibration 예시다.
전역 rubric은 모든 산업에 "광고주/퍼블리셔/플랫폼" 구조를 요구하지 않는다.

## 9. Section 5. 핵심 용어 정리 Calibration

### 9.1 Section 5의 역할

Section 5는 산업을 이해하는 데 필요한 핵심 용어를 정리한다.
목적은 용어 사전을 많이 만드는 것이 아니라, Section 1과 Section 3을 읽는 데 필요한 개념적 마찰을 줄이는 것이다.

좋은 glossary는 다음 단계 질문의 품질도 높인다.
용어가 불명확하면 Section 13 질문도 막연해진다.

### 9.2 좋은 답 / Pass

| 기준 | 설명 |
|---|---|
| 용어 선정이 산업 이해와 연결된다 | 단순 buzzword가 아니라 Section 1/3 이해에 필요한 용어다. |
| 정의가 쉬운 언어로 되어 있다 | 도메인 전문가가 아니어도 의미를 이해할 수 있다. |
| 왜 중요한지가 드러난다 | 투자 결론이 아니라 산업 구조 이해에 왜 필요한지 설명한다. |
| 유사 용어가 구분된다 | 혼동되기 쉬운 개념은 차이를 짧게 분리한다. |
| source_ref 또는 확인 필요가 있다 | 핵심 용어의 정의나 사용 맥락이 추적 가능하다. |
| 과도한 결론을 피한다 | 용어 설명이 moat, valuation, investment thesis로 넘어가지 않는다. |

### 9.3 Pass with Adjustments

| 경우 | 필요한 보완 |
|---|---|
| 용어가 너무 많다 | first slice 이해에 필요한 핵심 용어로 줄인다. |
| 용어가 너무 적다 | Section 1/3에서 이해를 막는 용어를 추가한다. |
| 정의가 원문 복붙에 가깝다 | 쉬운 설명으로 바꾼다. |
| 중요도 설명이 약하다 | "왜 이 산업 구조를 이해하는 데 필요한가"를 보완한다. |
| source_ref가 일부 없다 | 핵심 용어에 source_ref 또는 확인 필요를 붙인다. |
| 용어 설명이 판단으로 기운다 | descriptive definition으로 낮춘다. |

### 9.4 Blocked

| 경우 | 이유 |
|---|---|
| 핵심 용어 섹션이 없다 | Section 1/3의 이해 가능성이 떨어진다. |
| 중요한 용어가 명백히 잘못 정의됐다 | 이후 판단이 왜곡된다. |
| 용어 설명이 산업 구조가 아니라 투자 결론이 된다 | 금지 영역 침범 |
| source 없이 핵심 technical/regulatory 용어를 단정한다 | 출처 QA가 blocked가 된다. |
| 특정 산업 용어를 모른 채 참여자 구조를 작성한다 | Section 3까지 흔들릴 수 있다. |

### 9.5 APP/adtech 비규범 예시

APP/adtech에서는 ATT, SKAN/SKAdNetwork, IDFA, attribution, mediation, ROAS, ad network, DSP/SSP 같은 용어가 후보가 될 수 있다.

하지만 전역 rubric은 이 용어들을 필수 목록으로 고정하지 않는다.
다른 산업에서는 전혀 다른 용어가 같은 역할을 한다.

## 10. Section 13. 다음 단계로 넘길 질문 Calibration

### 10.1 Section 13의 역할

Section 13은 Industry Primer가 끝난 뒤 다음 하네스가 이어받을 수 있는 질문을 남긴다.

v0 schema의 필수 필드는 아래 3개다.

| 필드 | 의미 |
|---|---|
| `question` | 다음 하네스가 바로 사용할 수 있을 만큼 구체적인 질문 |
| `source_ref` | 질문의 근거가 되는 source |
| `status` | `ready` 또는 `needs_more_source` |

`target_step`은 넣지 않는다.
Step 5-7 이름은 설계자의 calibration 자료로 사용할 수 있지만, Section 13 output schema의 필드가 아니다.

### 10.2 좋은 답 / Pass

| 기준 | 설명 |
|---|---|
| 질문이 구체적이다 | "무엇을 더 분석할까"가 아니라 확인할 대상과 이유가 드러난다. |
| source_ref가 있다 | 다음 하네스가 질문의 근거를 추적할 수 있다. |
| status가 있다 | 바로 사용할 질문인지, 추가 source가 필요한지 구분된다. |
| 질문 자체에서 중요성이 드러난다 | `why_it_matters` 필드 없이도 왜 물어보는지 이해된다. |
| 결론이 아니라 질문이다 | 후속 단계가 판단해야 할 것을 미리 결론내지 않는다. |
| 후속 단계에 배분하지 않는다 | "Value Chain으로 보내라" 같은 routing을 질문에 포함하지 않는다. |

좋은 질문은 다음 하네스가 "이 질문을 어디서부터 확인해야 하는지"를 알 수 있게 만든다.
하지만 "이 질문은 어떤 후속 단계의 소유다"라고 upstream에서 결정하지 않는다.

### 10.3 Pass with Adjustments

| 경우 | 필요한 보완 |
|---|---|
| 질문은 유용하지만 너무 넓다 | 확인 대상, 관계, source를 좁힌다. |
| source_ref가 약하거나 일부 빠졌다 | 질문별 source_ref를 보완한다. |
| status가 없다 | `ready` 또는 `needs_more_source`를 명시한다. |
| 질문이 약간 결론처럼 읽힌다 | 열린 질문 형태로 바꾼다. |
| downstream step 이름을 설명용으로 썼다 | output에서는 step routing을 제거하고 질문 자체로 남긴다. |

### 10.4 Blocked

| 경우 | 이유 |
|---|---|
| handoff 질문이 없다 | 다음 하네스가 이어받을 시작점이 없다. |
| 질문이 너무 막연하다 | "경쟁력을 분석하라"처럼 실행 가능한 질문이 아니다. |
| source_ref가 없다 | 다음 하네스가 근거를 추적할 수 없다. |
| 후속 단계 결론을 미리 포함한다 | downstream 판단을 upstream이 대체한다. |
| 특정 후속 단계로의 배분 방향을 질문에 포함한다 | downstream coupling이 생긴다. |
| `needs_more_source`를 숨긴다 | data gap이 다음 단계로 조용히 전파된다. |

### 10.5 APP/adtech 비규범 예시

비규범 예시:

| 좋은 방향 | 이유 |
|---|---|
| "APP가 연결하는 주요 counterparties 중 광고주와 app developer/publisher 사이의 value flow를 더 확인해야 하는가? source_ref: src-003, status: ready" | 질문이 구체적이고 source로 이어진다. |
| "SKAN/ATT 이후 attribution 변화가 참여자 역할 구분에 어떤 영향을 주는지 추가 source가 필요한가? source_ref: src-009, status: needs_more_source" | data gap을 숨기지 않는다. |

나쁜 방향:

| 나쁜 방향 | 이유 |
|---|---|
| "Value Chain에서 APP의 위치가 좋은지 분석하라" | 후속 단계 routing과 결론 방향이 섞인다. |
| "APP는 adtech value chain에서 좋은 위치에 있다" | 질문이 아니라 결론이다. |

## 11. Cross-Section Consistency 기준

Section 1, 3, 5, 13은 따로 평가하지만 서로 연결되어야 한다.

| 연결 | 확인 기준 |
|---|---|
| Section 1 -> Section 3 | 산업 정의에 등장한 주요 참여자/관계가 Section 3에서 구조화되는가 |
| Section 3 -> Section 5 | Section 3 이해에 필요한 용어가 Section 5에서 풀리는가 |
| Section 5 -> Section 13 | 용어/구조상 불확실성이 다음 단계 질문으로 이어지는가 |
| Section 13 -> source_register | handoff 질문의 source_ref가 실제 source_register와 연결되는가 |
| 전체 -> 금지 영역 | 어떤 섹션도 7개 금지 영역 결론으로 넘어가지 않는가 |

first slice에서 가장 위험한 실패는 "각 섹션은 그럴듯하지만 서로 이어지지 않는 것"이다.
따라서 QA는 개별 섹션뿐 아니라 연결성을 함께 본다.

## 12. First Slice QA에서 Full Rubric Expansion으로 넘길 항목

이 문서는 first slice 기준이다.
아래 항목은 여기서 확정하지 않고 first slice pilot 완료 후, full pilot 진입 전에 별도 작업으로 추적한다.

| 후속 항목 | 이유 |
|---|---|
| Section 2. 산업이 존재하는 이유 | full Industry Primer에서 산업의 근본 수요를 더 깊게 다뤄야 함 |
| Section 4. 산업 하위 시장 구분 | first slice에서는 Section 3 안의 간략 맥락으로만 처리 |
| Section 6. 성장 동인 / 억제 요인 | 시장 성장과 회사 성장의 혼동 위험이 큼 |
| Section 7. 산업 수익 구조 | Value Chain/Business Model과 경계가 민감함 |
| Section 8. 산업 비용 구조 | Value Chain/Business Model과 경계가 민감함 |
| Section 9. 규제 / 제도 / 표준 | 산업별 깊이 차이가 커서 별도 calibration 필요 |
| Section 10-11. 기술 변화, 구조적 리스크 | Competition/Moat으로 넘어갈 위험이 있음 |
| Section 12. 분석 대상 기업과의 연결 | Industry Primer와 기업 분석 사이의 경계가 중요함 |
| Section 14. Industry Primer 결론 | 결론 섹션은 투자 판단으로 기울 위험이 큼 |
| full QA rubric | first slice 5층 QA보다 더 넓은 품질 기준 필요 |
| full output schema | 14개 섹션 전체 구조화 여부를 별도로 판단해야 함 |

작업 지도에는 이 후속 작업을 `first slice pilot 완료 후 -> full pilot 진입 전` 위치에 명시해야 한다.
단, 이 문서 작성 단계에서는 작업 지도와 README를 수정하지 않는다.

## 13. Blueprint 작성 시 반영할 방식

이 calibration note는 blueprint에 아래 방식으로 반영되어야 한다.

| blueprint 영역 | 반영 방식 |
|---|---|
| contract | first slice 범위와 금지 영역을 명시 |
| slice procedure | Section 1/3/5/13 작성 규칙에 calibration 기준 반영 |
| rubric | 각 Section의 pass / adjustments / blocked 기준으로 반영 |
| QA schema | 5층 QA와 Section별 finding 기록 구조에 연결 |
| output schema | source_register와 Section 13 handoff table 구조에 연결 |
| observation note | full rubric expansion 후보와 calibration 실패 사례 기록 |

중요한 점:
이 문서의 APP/adtech 예시는 blueprint의 전역 필수 조건으로 복사하면 안 된다.
예시는 rubric 설명이나 pilot observation의 calibration sample로만 사용한다.

## 14. Claude Code 교차검증 요청 기준

Claude Code에는 아래 기준으로 교차검증을 요청한다.

| 검증 항목 | 확인 질문 |
|---|---|
| 단계 정합성 | 이 문서가 blueprint가 아니라 first slice rubric calibration note로 유지됐는가 |
| 기준 문서 정합성 | pilot plan v1, blueprint-prep note, Section 11 consensus note와 충돌하지 않는가 |
| first slice / full 분리 | first slice rubric과 full Industry Primer rubric/QA expansion을 명확히 분리했는가 |
| full expansion 추적 | full rubric/QA expansion이 first slice pilot 후, full pilot 전 작업으로 명시됐는가 |
| domain-agnostic | APP/adtech 예시가 전역 필수 조건으로 굳지 않았는가 |
| actionable | 기준이 너무 추상적이지 않고 실행자가 판정할 수 있는가 |
| 과구체화 방지 | 특정 산업 용어, 기업, 시장 구조를 공통 필수 조건으로 넣지 않았는가 |
| Industry Primer / Value Chain 경계 | target company 위치 기준이 중복이 아니라 handoff 연결점으로 설명됐는가 |
| Section 1 rubric | 좋은 답 / adjustments / blocked 기준이 충분히 구체적인가 |
| Section 3 rubric | 참여자 구조와 Value Chain 침범 경계가 명확한가 |
| Section 5 rubric | glossary가 산업 이해를 돕되 투자 결론으로 흐르지 않게 잡혔는가 |
| Section 13 rubric | `question`, `source_ref`, `status` 중심이며 `target_step` coupling을 피하는가 |
| 금지 영역 | 7개 금지 영역을 pilot plan v1 기준으로 사용했는가 |
| 작업 범위 | 실제 하네스 파일, 작업 지도, README를 수정하지 않았는가 |

## 15. 현재 판정

이 문서는 first slice pilot을 위한 rubric calibration 기준 문서다.

현재 판정:

1. Claude Code 교차검증 PASS.
2. Section 12 full rubric expansion 추적표의 섹션 번호 오류를 pilot plan v1 기준으로 수정했다.
3. Industry Primer blueprint 초안 작성 시 이 note를 first slice rubric 내용 기준으로 사용한다.
4. full Industry Primer rubric/QA expansion은 first slice pilot 완료 후, full pilot 진입 전에 별도 작업으로 추적한다.
