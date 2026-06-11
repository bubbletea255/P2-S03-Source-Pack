# Phase 2 - Step 6 Business Model 템플릿

# Phase 2 - Step 6 Business Model 템플릿

## 목적: 회사의 “매출 공식”과 “이익 공식”을 파악하기

## 1. 이 템플릿의 역할

**Business Model 분석**은 Phase 2에서 가장 중요한 세부 모듈입니다.

이 단계의 핵심은 단순히 “이 회사가 무엇을 한다”를 아는 것이 아닙니다.

> **이 회사가 누구에게, 무엇을, 왜 팔고, 어떻게 돈을 받으며, 매출이 늘 때 이익도 같이 늘어날 수 있는 구조인지 파악하는 것**
> 

입니다.

---

# 2. Business Model 분석의 최종 산출물

| 산출물 | 설명 |
| --- | --- |
| 고객 / 고객 문제 | 누가 왜 이 회사 제품을 사는지 |
| 제품 / 서비스 구조 | 무엇을 파는지 |
| 가격 / 과금 구조 | 구독, 사용량, 장비, 소모품, 수수료 등 |
| Revenue Engine | 매출이 증가하는 공식 |
| Cost Engine | 비용이 발생하는 공식 |
| Unit Economics 후보 | 고객 단위, 제품 단위, 거래 단위 경제성 |
| Operating Leverage 후보 | 매출 증가가 이익 증가로 이어질 가능성 |
| Phase 3 연결 질문 | 경쟁우위 / 해자 / 재무 품질에서 검증할 질문 |

---

# 3. Business Model 분석에서 가장 중요한 관점

## 3-1. 회사 설명을 “매출 공식”으로 바꾸기

좋지 않은 분석:

약한 설명

---

이 회사는 클라우드 보안 솔루션을 제공한다.

---

이 회사는 유전자 검사를 한다.

---

이 회사는 광고 플랫폼을 운영한다.

---

좋은 분석:

강한 설명

---

고객 수 × 제품 수 × 사용량 × retention × 가격 인상 여부로 매출이 증가한다.

---

검사 건수 × 검사당 수가 × 보험 보장률 × 검사 mix로 매출이 결정된다.

---

광고 spend × take rate × 광고 효율 × publisher inventory로 매출이 결정된다.

---

---

## 3-2. 매출 공식과 이익 공식을 분리하기

| 구분 | 핵심 질문 |
| --- | --- |
| Revenue Engine | 매출이 어떻게 늘어나는가? |
| Cost Engine | 비용은 어떤 구조로 늘어나는가? |
| Profit Engine | 매출 증가가 gross profit / operating profit / FCF로 얼마나 전환되는가? |

좋은 사업은 단순히 매출이 늘어나는 사업이 아닙니다.

> **매출이 늘수록 단위당 이익률이 좋아지거나, 고정비 레버리지가 발생하는 사업**이 좋은 사업입니다.
> 

---

# 4. Business Model 실제 프롬프트

아래 프롬프트를 그대로 복사해서 사용하면 됩니다.

```
[기업명 / 티커]에 대해 Phase 2 Business Model 분석을 해줘.

분석 대상:
- 기업명:
- 티커:
- 산업 / 세부 산업:
- 1차 투자 유형:
  Compounder / Turnaround / Cyclical Recovery / Deep Value / Quality at Discount / Special Situation / 기타
- Source Pack에서 확보한 주요 자료:
- Phase 2 통합 분석에서 파악한 내용:
- Industry Primer에서 파악한 핵심 내용:
- Value Chain에서 파악한 회사의 위치:
- 내가 특히 헷갈리는 부분:
- 특별히 확인하고 싶은 질문:

목적:
- 이 회사가 누구에게, 무엇을, 왜 팔고, 어떻게 돈을 버는지 파악
- 회사 설명을 “매출 공식”과 “이익 공식”으로 바꾸기
- 매출 증가가 gross profit, operating profit, FCF로 전환될 수 있는 구조인지 확인
- 이후 Phase 3 Competition / Moat / Financial Quality 분석으로 넘길 질문을 도출

중요 원칙:
- Fact와 해석을 분리
- 모르는 내용은 “확인 필요”로 표시
- 최신 공시, earnings release, earnings call, investor presentation, 공식 제품 페이지를 우선
- 회사가 주장하는 장점과 실제 확인 가능한 구조를 구분
- 정교한 재무 분석, DCF, reverse valuation, 장기 forecast는 하지 말 것
- 경쟁사 우열 최종 판단은 하지 말 것
- 해자 최종 결론은 내리지 말 것
- 투자 매수 / 매도 판단은 하지 말 것
- 표 중심으로 작성
- 모든 재무 / 회계 숫자는 출처와 기준 기간을 표시

출력 형식:

1. Business Model 한 줄 요약
| 항목 | 내용 |
|---|---|
| 기업명 / 티커 |  |
| 사업 한 줄 정의 |  |
| 주요 고객 |  |
| 핵심 제품 / 서비스 |  |
| 주요 수익 구조 |  |
| 핵심 매출 driver |  |
| 핵심 비용 driver |  |
| 사업모델 예비 평가 | 단순 / 반복매출형 / 사용량 기반 / 플랫폼형 / 장비+소모품형 / 수수료형 / 기타 |

2. 고객 분석
- 이 회사의 직접 고객은 누구인가?
- 최종 사용자는 누구인가?
- 고객이 겪는 문제는 무엇인가?
- 고객이 이 회사 제품 / 서비스를 사는 경제적 이유는 무엇인가?
- 구매 결정자는 누구인가?
- 구매 주기는 긴가, 짧은가?
- 고객 예산은 어느 항목에서 나오는가?

표:
| 항목 | 내용 | 확인 필요 |
|---|---|---|
| 직접 고객 |  |  |
| 최종 사용자 |  |  |
| 고객 문제 |  |  |
| 구매 이유 |  |  |
| 구매 결정자 |  |  |
| 예산 출처 |  |  |
| 구매 주기 |  |  |
| 고객 집중도 |  |  |

3. 제품 / 서비스 구조
- 주요 제품군 / 서비스군을 구분
- 각 제품이 해결하는 문제와 매출 기여 방식을 정리
- 핵심 제품과 보조 제품을 구분
- 신제품 / 확장 제품이 있다면 표시

표:
| 제품 / 서비스 | 고객 문제 | 수익화 방식 | 중요도 | 확인 필요 |
|---|---|---|---|---|
|  |  |  | 핵심 / 보조 / 초기 |  |

4. 가격 / 과금 구조
- 회사가 어떻게 돈을 받는지 정리
- 구독, 사용량 기반, 좌석 수, 거래 수수료, 장비 판매, 소모품, 라이선스, 광고, 보험 / reimbursement 등을 구분
- 가격 인상 가능성과 가격 하락 위험을 후보 수준으로 정리

표:
| 과금 방식 | 설명 | 장점 | 리스크 | 해당 제품 / 고객 |
|---|---|---|---|---|
| 구독 |  | 반복매출 / 예측 가능성 | churn / 가격저항 |  |
| 사용량 기반 |  | 고객 성장과 함께 매출 증가 | 사용량 둔화 |  |
| 좌석 수 기반 |  | 고객 조직 확장과 연동 | seat growth 둔화 |  |
| 거래 수수료 / take rate |  | 거래액 증가와 연동 | take rate 압박 |  |
| 장비 판매 |  | 초기 매출 큼 | 일회성 / 경기 민감 |  |
| 소모품 / consumables |  | 설치 기반 확대 후 반복매출 | 장비 판매 둔화 영향 |  |
| 라이선스 |  | margin 높을 수 있음 | 갱신 리스크 |  |
| 광고 |  | scale과 데이터 효과 가능 | 경기 민감 / 정책 리스크 |  |
| 보험 / reimbursement |  | 수가 확보 시 성장 | 규제 / 지급 거절 |  |

5. Revenue Engine
- 매출이 증가하는 공식을 만들어줘.
- 가능하면 고객 수, 가격, 사용량, retention, 제품 mix, 지역 확장, 점유율 증가 등 driver로 분해.
- 확인 가능한 숫자와 추정 / 가설을 구분.

기본 형식:
Revenue = 고객 수 × ARPU × 사용량 × retention × 제품 확장 × 가격

단, 산업에 맞게 조정:
- SaaS: 고객 수 × ACV × NRR × 신규 제품 adoption
- Adtech: 광고 spend × take rate × 광고 효율 × publisher inventory
- Diagnostics: 검사 건수 × 검사당 수가 × reimbursement rate × test mix
- Hardware: 출하량 × ASP × mix × 교체주기
- Marketplace: GMV × take rate × transaction frequency
- Consumables: installed base × pull-through rate × consumable ASP

표:
| Revenue Driver | 설명 | 회사에 유리한 조건 | 악화될 수 있는 조건 | 확인 필요 |
|---|---|---|---|---|
| 고객 수 |  |  |  |  |
| 가격 / ARPU / ASP |  |  |  |  |
| 사용량 / 거래량 / 검사량 |  |  |  |  |
| retention / 재구매 |  |  |  |  |
| 제품 mix |  |  |  |  |
| 지역 확장 |  |  |  |  |
| 점유율 상승 |  |  |  |  |

6. Cost Engine
- 매출을 만들기 위해 어떤 비용이 필요한지 정리
- COGS, R&D, Sales & Marketing, G&A, capex, customer support, regulatory cost 등을 구분
- 고정비 / 변동비 성격을 표시
- 매출 성장 시 비용이 같은 속도로 늘어나는지, 느리게 늘어나는지 후보 수준으로 판단

표:
| 비용 항목 | 고정비 / 변동비 | 설명 | 매출 성장 시 변화 | 투자 분석상 의미 |
|---|---|---|---|---|
| COGS |  |  | 빠르게 / 비례 / 느리게 |  |
| R&D |  |  | 빠르게 / 비례 / 느리게 |  |
| Sales & Marketing |  |  | 빠르게 / 비례 / 느리게 |  |
| G&A |  |  | 빠르게 / 비례 / 느리게 |  |
| Capex |  |  | 빠르게 / 비례 / 느리게 |  |
| 고객 지원 / 서비스 |  |  | 빠르게 / 비례 / 느리게 |  |
| 규제 / 임상 / 인증 |  |  | 빠르게 / 비례 / 느리게 |  |
| 클라우드 / 인프라 |  |  | 빠르게 / 비례 / 느리게 |  |

7. Profit Engine
- 매출이 gross profit, operating profit, FCF로 어떻게 전환되는지 구조적으로 설명
- gross margin이 높아질 수 있는 이유와 낮아질 수 있는 이유를 구분
- operating leverage 가능성을 후보 수준으로 판단
- FCF 전환을 방해할 요소를 표시

표:
| 항목 | 개선 요인 | 악화 요인 | 확인 필요 |
|---|---|---|---|
| Gross margin |  |  |  |
| Operating margin |  |  |  |
| FCF margin |  |  |  |
| Working capital |  |  |  |
| Capex intensity |  |  |  |
| SBC / dilution |  |  |  |

8. Unit Economics 후보
- 회사가 제공하는 자료 안에서 unit economics를 확인할 수 있는지 검토
- CAC, LTV, payback period, gross retention, NRR, test margin, contribution margin, installed base pull-through 등 산업별 지표를 구분
- 숫자가 없으면 어떤 지표를 나중에 확인해야 하는지 표시

표:
| Unit Economics 지표 | 산업별 의미 | 확인 가능 여부 | 투자 분석상 의미 |
|---|---|---|---|
| CAC | 고객 획득 비용 |  |  |
| LTV | 고객 생애 가치 |  |  |
| Payback period | 고객 획득비 회수 기간 |  |  |
| Gross retention | 기존 고객 유지율 |  |  |
| NRR | 기존 고객 매출 확장률 |  |  |
| Contribution margin | 단위 경제성 |  |  |
| Installed base | 장비 / 플랫폼 기반 |  |  |
| Pull-through | 설치 기반당 반복매출 |  |  |
| 검사당 margin / 거래당 margin | 단위 수익성 |  |  |

9. 반복매출 / 안정성 분석
- 매출이 반복적인지, 일회성인지, 경기 민감한지 정리
- 고객 이탈 가능성, 계약 기간, 사용량 변동성, 예산 민감도를 후보 수준으로 판단

표:
| 항목 | 판단 | 이유 |
|---|---|---|
| 반복매출 비중 | 높음 / 중간 / 낮음 / 확인 필요 |  |
| 고객 retention 가능성 | 높음 / 중간 / 낮음 / 확인 필요 |  |
| 사용량 변동성 | 높음 / 중간 / 낮음 / 확인 필요 |  |
| 경기 민감도 | 높음 / 중간 / 낮음 / 확인 필요 |  |
| 계약 안정성 | 높음 / 중간 / 낮음 / 확인 필요 |  |
| 가격 인상 가능성 | 높음 / 중간 / 낮음 / 확인 필요 |  |

10. 확장성 / Operating Leverage 분석
- 매출이 늘 때 비용이 얼마나 같이 늘어나는지 후보 수준으로 판단
- 소프트웨어형 scale leverage, 플랫폼형 network leverage, 장비+소모품형 installed base leverage, 제조업형 utilization leverage 등을 구분

표:
| 확장성 요소 | 해당 여부 | 설명 | 확인 필요 |
|---|---|---|---|
| Software scale leverage |  |  |  |
| Platform / network leverage |  |  |  |
| Installed base leverage |  |  |  |
| Utilization leverage |  |  |  |
| Sales efficiency leverage |  |  |  |
| R&D leverage |  |  |  |
| G&A leverage |  |  |  |

11. Business Model Strength / Weakness
| 구분 | 내용 | Phase 3에서 검증할 질문 |
|---|---|---|
| 강점 후보 |  |  |
| 약점 후보 |  |  |
| 해자 후보 |  |  |
| 리스크 후보 |  |  |

12. Phase 3로 넘길 질문
| 후속 단계 | 넘길 질문 |
|---|---|
| Competition |  |
| Moat |  |
| Financial Quality |  |
| Management |  |
| Forecast / Valuation |  |

13. Business Model 결론
- 이 회사의 사업모델을 한 문단으로 요약
- 매출 공식:
- 이익 공식:
- 사업모델의 가장 매력적인 점 3개
- 사업모델의 가장 취약한 점 3개
- 가장 불확실한 부분 3개
- 추가로 확인해야 할 자료
- 다음 액션
```

---

# 5. 산업별 Revenue Engine 예시

Business Model 템플릿을 사용할 때 가장 중요한 부분은 **Revenue Engine을 산업별로 다르게 잡는 것**입니다.

| 산업 | Revenue Engine 예시 |
| --- | --- |
| SaaS | 신규 고객 수 × ACV × NRR × multi-product adoption |
| Cybersecurity | 고객 수 × 제품 모듈 수 × endpoint / workload 수 × renewal rate |
| Observability | 고객 수 × 데이터 ingest / 사용량 × retention × 제품 확장 |
| Adtech | 광고 spend × take rate × 광고 효율 × publisher / advertiser scale |
| Diagnostics | 검사 건수 × 검사당 수가 × reimbursement rate × test mix |
| Life science tools | 장비 설치 대수 × consumables pull-through × assay adoption |
| Semiconductor | 출하량 × ASP × mix × utilization |
| Marketplace | GMV × take rate × transaction frequency |
| Hardware + Consumables | installed base × replacement cycle × consumables usage |
| Digital health | 가입자 / 환자 수 × ARPU × retention × 보험 / employer 계약 |

---

# 6. Business Model 분석에서 자주 생기는 실수

| 실수 | 설명 |
| --- | --- |
| 제품 설명에서 멈춤 | 제품을 안다고 사업모델을 이해한 것은 아님 |
| 고객을 넓게만 정의 | 실제 구매자와 최종 사용자를 구분해야 함 |
| TAM만 보고 성장 판단 | 매출 driver와 점유율 확대 근거를 봐야 함 |
| Gross margin만 보고 좋은 사업이라 판단 | S&M, R&D, SBC, capex까지 봐야 함 |
| 반복매출을 과대평가 | 계약 갱신, churn, 사용량 변동성 확인 필요 |
| 사용량 기반 매출을 무조건 좋게 봄 | 사용량 둔화 시 매출이 바로 둔화될 수 있음 |
| 플랫폼이라는 말에 속음 | 실제 network effect와 take rate 방어력이 있는지 봐야 함 |
| 장비 판매와 소모품 매출을 구분하지 않음 | 매출 질과 반복성이 완전히 다름 |

---

# 7. 실행 시 추천 모드와 웹검색

## 7-1. 템플릿 작성용

| 작업 | 추천 모드 | 웹검색 |
| --- | --- | --- |
| Business Model 템플릿 작성 | **Thinking 표준** | 불필요 |

---

## 7-2. 실제 기업에 적용할 때

| 상황 | 추천 모드 | 웹검색 |
| --- | --- | --- |
| 이미 Source Pack을 충분히 확보한 경우 | **Thinking 표준** | 선택 |
| 처음 보는 기업 | **Thinking 표준 + 웹검색** | 권장 |
| 사업모델이 단순한 기업 | **Thinking 라이트 또는 표준** | 선택 |
| 사업모델이 복잡한 기업 | **Thinking 표준 또는 Pro 표준** | 권장 |
| 의료 / 진단 / 보험 / 규제 산업 | **Pro 표준 또는 Pro 확장** | 필수에 가까움 |
| 반도체 / 하드웨어 / 제조업 | **Thinking 표준 또는 Pro 표준** | 권장 |
| 최신 제품 구조 / pricing / KPI 확인 필요 | **Pro 표준** | 필수 |
| 특수상황 / 구조조정 기업 | **Pro 표준 또는 Pro 확장** | 필수 |
| 여러 출처를 종합해야 하는 경우 | **Pro 확장 또는 Deep Research** | 필수 |

OpenAI 도움말에 따르면 Thinking은 더 복잡한 작업을 위한 깊은 추론, Pro는 research-grade intelligence 성격으로 제공됩니다. 또한 Thinking이나 Pro를 선택하면 모델 선택기에서 추론 강도 수준을 조절할 수 있습니다. 따라서 Business Model 템플릿 작성 자체는 Thinking 표준이 적절하고, 실제 기업에 적용하면서 최신 공시·제품·pricing·KPI 확인이 필요하면 Pro와 웹검색을 함께 쓰는 편이 적합합니다. ([OpenAI Help Center](https://help.openai.com/en/articles/11909943-gpt-53-and-54-in-chatgpt?utm_source=chatgpt.com))

---

# 8. 모드 선택 기준 요약

| 목적 | 추천 |
| --- | --- |
| 템플릿 자체 작성 | Thinking 표준 |
| 실제 기업 기본 분석 | Thinking 표준 |
| 빠른 사업모델 초안 | Thinking 라이트 |
| 최신 제품 / 가격 / KPI 확인 | Pro 표준 + 웹검색 |
| 복잡한 산업 / 규제 / 다중 사업부 | Pro 표준 또는 Pro 확장 |
| 고난도 특수상황 | Pro 확장 |
| Thinking 확장 / 헤비 | 템플릿 작성에는 과함. 실제 분석이 매우 복잡할 때 제한적으로 사용 |

---

# 9. 최종 정리

| 항목 | 내용 |
| --- | --- |
| 템플릿 이름 | Business Model |
| Phase | Phase 2-4 세부 모듈 |
| 목적 | 고객, 제품, 가격, 매출 공식, 비용 공식, 이익 전환 구조 파악 |
| 산출물 | Revenue Engine, Cost Engine, Profit Engine, Unit Economics 후보 |
| 템플릿 작성 추천 모드 | Thinking 표준 |
| 실제 실행 기본 모드 | Thinking 표준 |
| 복잡한 기업 실행 모드 | Pro 표준 / Pro 확장 |
| 웹검색 | 실제 기업 적용 시 권장~필수 |
| 다음 단계 | Market / Share, Financial Quality, Competition, Moat |

## 한 줄 결론

> **Business Model 템플릿은 회사 설명을 “매출 공식”과 “이익 공식”으로 바꿔, 이후 경쟁우위·재무 품질·밸류에이션 분석의 기반을 만드는 Phase 2 핵심 템플릿입니다.**
>