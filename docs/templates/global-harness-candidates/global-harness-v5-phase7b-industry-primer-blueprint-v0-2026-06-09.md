# Global Harness v5 Phase 7-B Industry Primer Blueprint v0

- 작성일: 2026-06-09
- 상태: Claude Code 재교차검증 PASS, 추가 gray-zone/risk-control 보강 반영
- 단계: Phase 7-B Industry Primer first slice pilot
- 대상 pilot: APP / adtech / mobile advertising
- 문서 성격: 실제 하네스 파일 생성 전 설계도
- 최신 source of truth: `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v1-2026-06-08.md`

이 문서는 실제 하네스 파일이 아니다.

이 문서는 `harness/`, `.agents/`, `.claude/`, `artifacts/` 파일을 생성하거나 수정하지 않는다.
또한 기존 Source Pack 하네스 구조를 수정하지 않는다.

이 문서의 목적은 Industry Primer 하네스가 가져야 할 contract, procedure, schema, rubric, output, adapter 경계, module 연결을 blueprint 수준에서 설계하는 것이다.
사용자가 이 blueprint를 검토하고 명시적으로 승인한 뒤에만 실제 하네스 파일 생성을 논의할 수 있다.

## 1. 기준 문서와 우선순위

blueprint 작성 기준은 아래 순서를 따른다.

| 우선순위 | 문서 | 역할 |
|---|---|---|
| 1 | `global-harness-v5-work-map.md` | 현재 작업 단계와 완료/후속 항목 |
| 2 | `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v1-2026-06-08.md` | APP/adtech first slice pilot 실행 계획의 최신 source of truth |
| 3 | `global-harness-v5-phase7b-industry-primer-blueprint-prep-note-2026-06-08.md` | blueprint 작성 전 rubric, QA output, schema, handoff 원칙 |
| 4 | `global-harness-v5-phase7b-industry-primer-blueprint-prep-section11-consensus-note-2026-06-09.md` | blueprint-prep Section 11의 6개 질문에 대한 합의 |
| 5 | `global-harness-v5-phase7b-industry-primer-first-slice-rubric-calibration-note-2026-06-09.md` | Section 1/3/5/13의 실제 내용 기준 |
| 6 | `global-harness-v5-phase7b-industry-primer-pre-build-risk-review-note-2026-06-09.md` | build 직전 risk control, challenge review, 사용자 검토 기준 |
| 7 | `Phase 2 - Step 4 Industry Primer 템플릿.md` | 원본 Industry Primer 산출물 구조 |
| 8 | `global-harness-core-structure-template-v1.md` | v5 core의 contracts/procedures/schemas/rubrics/adapter 구조 |
| 보조 | Step 5-7 templates | handoff fit과 금지 영역 경계 calibration |

주제별 source of truth:

- 기본 실행 범위, 입력, 산출물은 `pilot plan note v1`을 따른다.
- build 직전 risk control 항목은 `pre-build risk review note`를 따른다. 여기에는 중립성, source quality tier, source interpretation risk, User Review Required Claims, challenge review timing, 21단계 가설 문구가 포함된다.
- 현재 작업 상태, done/todo, 다음 단계는 `work-map`을 따른다.

특히 금지 영역은 아래 7개를 사용한다.

```text
Value Chain
Business Model
Market Share
Competition
Moat
Valuation
투자 판단
```

## 2. 하네스 목적과 범위

### 2.1 목적

Industry Primer 하네스는 특정 기업을 분석하기 전에 그 기업이 속한 산업의 기초 지도를 만든다.

first slice pilot의 목적은 full Industry Primer를 완성하는 것이 아니라, 아래 메커니즘이 작동하는지 검증하는 것이다.

- APP partial Source Pack과 웹/외부자료를 함께 사용할 수 있는가
- Section 1, 3, 5, 13만으로 작은 slice를 작성할 수 있는가
- source tracking과 `source_register`가 작동하는가
- 판단형 QA가 구조/출처/범위/판단/handoff를 나눠 판정할 수 있는가
- Section 13 handoff 질문이 다음 하네스의 출발점으로 쓸 수 있는가
- signal-routing, approval-gate, top-up trigger가 과하지 않게 작동하는가

Industry Primer는 target company를 좋게 포장하거나 투자 결론을 강화하는 문서가 아니다.
요청된 기업이 투자 후보든 제외 후보든 같은 기준으로 산업 구조를 설명해야 한다.

가치투자 21단계는 현재 검증 중인 투자 리서치 프로세스 가설이다.
이 blueprint는 그 가설 안의 Step 4 Industry Primer first slice를 검증하지만, 21단계 전체가 이미 검증 완료된 체계라고 전제하지 않는다.

### 2.2 First Slice 범위

first slice에서 작성할 섹션은 아래 4개다.

| 섹션 | 처리 | 목적 |
|---|---|---|
| Section 1. 산업 한 줄 정의 | 필수 | 산업 경계와 target company context 확인 |
| Section 3. 산업 참여자 구조 | 필수 | 주요 참여자와 target company 위치 확인 |
| Section 5. 핵심 용어 정리 | 필수 | glossary/type-schema와 이해 가능성 확인 |
| Section 13. 다음 단계로 넘길 질문 | 필수 | handoff QA 확인 |

Section 4 `산업 하위 시장 구분`은 별도 필수 섹션으로 작성하지 않는다.
다만 Section 3에서 target company 위치를 이해하는 데 필요한 만큼만 하위 시장 맥락을 간략히 다룰 수 있다.

### 2.3 비범위

이 blueprint와 first slice pilot은 아래를 하지 않는다.

- full Industry Primer 14개 섹션 완성
- Value Chain, Business Model, Market Share, Competition, Moat, Valuation, 투자 판단 결론
- target company를 홍보하거나 정당화하는 cheerleading 문서 작성
- Source Pack top-up 실행
- Earnings Call 하네스 설계
- Industry Primer 실제 하네스 파일 생성
- Codex/Claude adapter 파일 생성
- 전역 template bundle 배포 판단

## 3. 논리 파일 구조 후보

아래는 승인 후 만들 수 있는 논리적 파일 구조 후보다.
이 blueprint는 파일을 생성하지 않는다.

```text
harness/
  contracts/
    industry-primer.contract.md
  procedures/
    industry-primer-runbook.md
    industry-primer-slice.md
    industry-primer-qa.md
  schemas/
    industry-primer-slice.schema.md
    industry-primer-qa.schema.md
  rubrics/
    industry-primer-qa-rubric.md
```

adapter 후보:

```text
.agents/skills/industry-primer-orchestrator/SKILL.md
.claude/skills/industry-primer-orchestrator/SKILL.md
```

adapter 위치는 실제 하네스 생성 단계에서 결정한다.
공통 업무 의미는 adapter에 복사하지 않고 `harness/` 원장에 둔다.

### 3.1 후보 파일 역할

| 후보 파일 | 역할 |
|---|---|
| `industry-primer.contract.md` | 목표, 입력, 출력, 완료 기준, 금지 영역, non-goals |
| `industry-primer-runbook.md` | 1회 실행 전체 흐름: preflight, slice, QA, observation, 판정 |
| `industry-primer-slice.md` | Section 1/3/5/13 작성 규칙과 Source Pack/웹 source 사용 방식 |
| `industry-primer-qa.md` | QA 실행 순서와 판정 집계 절차 |
| `industry-primer-slice.schema.md` | slice output, `source_register`, Section 13 table 형식 |
| `industry-primer-qa.schema.md` | `qa.md` output format 8섹션 |
| `industry-primer-qa-rubric.md` | 판단형 QA rubric과 Section별 pass/adjustments/blocked 기준 |

### 3.2 `runbook`과 `slice procedure` 역할 분리

`industry-primer-runbook.md`는 전체 실행 흐름을 관리한다.

- run-id 결정
- Input Readiness Preflight
- slice 작성 호출
- QA 호출
- observation note 작성
- top-up/approval/comparison/checkpoint 조건 확인
- 완료 판정

`industry-primer-slice.md`는 실제 slice 작성 규칙을 담당한다.

- Section 1 작성 규칙
- Section 3 작성 규칙
- Section 5 작성 규칙
- Section 13 작성 규칙
- Source Pack source와 웹 source 사용 방식
- `source_ref`와 `source_register` 연결
- APP/adtech 예시가 전역 필수 조건이 되지 않게 하는 기준

이 분리는 중요하다.
runbook이 모든 작성 기준을 흡수하면 절차 파일이 너무 무거워지고, slice 작성 규칙을 수정할 때 전체 실행 흐름까지 흔들린다.

## 4. Contract 설계

### 4.1 목표

Industry Primer first slice contract의 목표는 아래와 같다.

```text
APP/adtech Industry Primer first slice를 작성한다.
산출물은 Section 1, 3, 5, 13과 내부 source_register를 포함한다.
QA는 5층 기준으로 pass, pass with adjustments, blocked 중 하나를 판정한다.
```

중립성 원칙:

- target company를 좋게 포장하지 않는다.
- 투자 찬성/반대 결론을 암시하지 않는다.
- 회사 공식 narrative는 source로 사용하되, 산업 구조의 독립 설명과 구분한다.
- 불확실하거나 source 해석 위험이 큰 claim은 숨기지 않고 사용자 검토 대상으로 표시한다.

21단계 위치:

```text
가치투자 21단계는 현재 검증 중인 투자 리서치 프로세스 가설이다.
first slice 결과는 이 가설의 Step 4 적용 가능성을 작게 검증하는 입력이다.
```

### 4.2 입력

필수 control input:

| 입력 | 역할 |
|---|---|
| `artifacts/companies/APP/index.md` | APP Source Pack 상태, partial 범위, 누락 자료 확인 |

필수 content input:

| 입력 | 역할 |
|---|---|
| APP Q1 2026 earnings release HTML | 회사 공식 사업/시장 언어 |
| APP Q1 2026 financial update PDF | 회사 공식 KPI, segment, product/market 표현 |
| APP FY2026 Q1 8-K / EX-99.1 | SEC canonical 교차 확인 |

preflight 참조:

| 입력 | 역할 |
|---|---|
| APP IR pilot QA | IR 2건 반영 상태 검산 |
| APP SEC-IR overlap recheck run-summary | SEC-IR overlap과 partial 상태 검산 |

QA/run-summary는 필수 content input이 아니다.
Source Pack 상태를 검산하는 preflight 참조다.

웹/외부자료:

| 입력 | 처리 |
|---|---|
| 산업 정의, 참여자 구조, 핵심 용어, 플랫폼 정책 자료 | 허용 |
| ATT/SKAN/IDFA/Privacy Sandbox 등 adtech 구조 이해 자료 | APP/adtech pilot context에서 허용 |
| 정밀 TAM/CAGR/점유율 자료 | first slice에서는 금지 또는 full pilot 후보 |
| AI 요약문 | 원천 source로 사용 금지 |

source quality tier:

| tier | source 예시 | 사용 원칙 |
|---|---|---|
| Preferred | SEC, 회사 공식 IR, 정부/규제기관, 공식 통계 | 우선 사용. 핵심 fact와 회사 공식 발언 검증에 사용 |
| Strong external | 신뢰 가능한 산업 리포트, 컨설팅/리서치, 주요 금융/산업 매체 | 산업 구조, 용어, 외부 맥락 보강에 사용 |
| Context only | Wikipedia, 일반 블로그, 커뮤니티, 비전문 매체 | 용어 감 잡기나 후보 탐색용. 핵심 근거로 쓰지 않음 |
| Disallowed as evidence | AI 요약문, 출처 없는 댓글, 익명성 강한 주장 | evidence로 사용 금지 |

### 4.3 출력

필수 산출물은 3개다.

| 산출물 | 권장 placeholder | 역할 |
|---|---|---|
| Industry Primer slice output | `artifacts/runs/{run-id}/industry-primer-slice.md` | Section 1/3/5/13과 내부 `source_register` |
| Slice QA result | `artifacts/runs/{run-id}/qa.md` | 5층 QA 판정과 finding |
| Pilot observation note | `artifacts/runs/{run-id}/pilot-observation-note.md` | 내용 품질 관찰과 v5 template 적용 평가 |

정확한 경로는 실제 하네스 생성 단계에서 확정한다.
이 blueprint는 권장 placeholder만 제시한다.

### 4.4 완료 기준

| 판정 | 완료 기준 |
|---|---|
| `pass` | 필수 산출물 3개가 있고, 5층 QA에서 blocked가 없으며, 후속 단계로 넘겨도 안전하다. |
| `pass with adjustments` | 필수 산출물은 있으나 source gap, 범위 문구, handoff 질문, source_register, 확인 필요 표시 보완이 필요하다. |
| `blocked` | 필수 섹션/source_register 누락, 핵심 주장 출처 부재, 금지 영역 결론, 또는 input 부족 때문에 안전한 handoff가 불가능하다. |

### 4.5 금지 영역

first slice는 아래 결론을 내리지 않는다.

- Value Chain 결론
- Business Model 결론
- Market Share 결론
- Competition 결론
- Moat 결론
- Valuation 결론
- 투자 판단

## 5. Procedure / Runbook 설계

### 5.1 전체 실행 순서

`industry-primer-runbook.md` 후보는 아래 순서를 가진다.

| 순서 | 단계 | 주요 확인 | 산출물 |
|---|---|---|---|
| 1 | Input Readiness Preflight | APP index, IR 2건, 8-K/EX-99.1, partial gap, top-up trigger | preflight 기록 |
| 2 | Slice 작성 | Section 1/3/5/13과 source_register 작성 | slice output |
| 3 | Slice QA | 5층 QA, 금지 영역, source gap, handoff QA | QA result |
| 4 | User Review Required Claims 표시 | source 해석 위험, company bias 위험, 후속 분석 오염 위험이 큰 claim 최대 5개 | QA result Findings 하위 섹션 |
| 5 | Challenge review | 사용자가 claim을 원문/source와 도메인 지식으로 검토 | 사용자 승인/보완/blocked 판단 입력 |
| 6 | Pilot observation | 내용 품질과 v5 template fit 기록 | observation note |
| 7 | 완료 판정과 다음 단계 결정 | pass / pass with adjustments / blocked, blueprint 보완, top-up 후보, comparison 여부, full rubric expansion 후보 | QA result / observation note / work map |

### 5.2 Input Readiness Preflight

preflight는 아래 checklist를 가진다.

| 체크 | 기준 | 실패 시 |
|---|---|---|
| APP index 접근 가능 | `artifacts/companies/APP/index.md`를 읽을 수 있다. | top-up 또는 Source Pack 상태 확인 후보 |
| APP Q1 IR 2건 접근 가능 | earnings release HTML, financial update PDF를 읽을 수 있다. | top-up 승인 요청 후보 |
| FY2026 Q1 8-K / EX-99.1 접근 가능 | SEC canonical 자료를 읽을 수 있다. | top-up 승인 요청 후보 |
| QA/run-summary 참조 가능 | 운영 기록을 preflight 참조로 확인할 수 있다. | 필수 입력 실패는 아님 |
| partial input gap 식별 | 10-K, transcript, proxy 등 누락을 기록한다. | gap 기록 |
| top-up trigger 미발동 | 필수 input으로 Section 1/3/5/13 작성 가능 | 진행 |

### 5.3 Slice 작성 절차

`industry-primer-slice.md` 후보는 아래 작성 원칙을 가진다.

| 섹션 | 작성 원칙 |
|---|---|
| Section 1 | 산업 경계, 해결 문제, 고객/수요자, target company context를 짧게 정의 |
| Section 3 | 참여자 category, 역할, 흐름, target company 위치를 descriptive하게 정리 |
| Section 5 | Section 1/3 이해에 필요한 핵심 용어를 쉬운 설명으로 정리 |
| Section 13 | 다음 하네스가 바로 사용할 수 있는 질문을 `question`, `source_ref`, `status`로 정리 |
| source_register | 본문 뒤 내부 섹션으로 작성 |

APP/adtech 예시는 pilot context로만 쓴다.
전역 rubric 필수 조건으로 DSP/SSP/SKAN 같은 adtech 용어를 hard-code하지 않는다.

source 사용 원칙:

- Preferred source와 Strong external source를 우선 사용한다.
- Context only source는 핵심 evidence가 아니라 탐색과 맥락 보조로만 쓴다.
- Disallowed as evidence source는 source_register의 evidence로 넣지 않는다.
- 회사 공식 source는 회사 narrative로 표시하고, 독립 산업 구조 설명과 혼동하지 않는다.

### 5.4 QA 절차

`industry-primer-qa.md` 후보는 아래를 수행한다.

1. 필수 산출물 3개 존재 확인
2. slice output 구조 확인
3. `source_register`와 `source_ref` 연결 확인
4. 7개 금지 영역 독립 확인
5. 5층 QA 판정
6. source interpretation risk와 company bias 위험 claim 식별
7. User Review Required Claims 최대 5개를 Findings 하위 섹션에 기록
8. 전체 판정 집계
9. comparison 발동 여부 판단
10. top-up trigger 여부 판단
11. next action 기록

`User Review Required Claims`는 QA status가 아니다.
사용자가 산출물 승인 전에 원문/source와 도메인 지식으로 직접 확인할 고위험 claim 목록이다.

### 5.5 Observation 절차

Pilot observation note는 두 섹션을 가진다.

| 섹션 | 내용 |
|---|---|
| Section A. Industry Primer 내용 품질 관찰 | source gap, 범위 초과 위험, handoff 품질, 확인 필요 항목 |
| Section B. v5 template 적용 평가 | module fit, 과한 module, 빠진 module, signal-routing/candidate-ledger 후보, 다음 pilot 반영 후보 |

candidate-ledger는 first slice에서 별도 파일로 만들지 않는다.
반복 후보는 observation note Section B에 기록한다.

## 6. Schema 설계

### 6.1 Slice Output Schema

`industry-primer-slice.schema.md` 후보는 아래 구조를 정의한다.

| 섹션 | 필수 여부 | 형식 |
|---|---|---|
| metadata | 필수 | run-id, target company, industry context, date |
| Section 1. 산업 한 줄 정의 | 필수 | markdown table 또는 짧은 prose |
| Section 3. 산업 참여자 구조 | 필수 | markdown table + short notes |
| Section 5. 핵심 용어 정리 | 필수 | markdown table |
| Section 13. 다음 단계로 넘길 질문 | 필수 | structured table |
| source_register | 필수 | structured table |

### 6.2 Source Register Schema

`source_register`는 slice output 내부 필수 섹션이다.

| 필드 | 필수 여부 | 설명 |
|---|---|---|
| `source_id` | 필수 | 본문과 handoff 질문에서 참조할 식별자 |
| `source_type` | 필수 | `source_pack`, `sec`, `company_ir`, `web`, `industry_reference` 등 |
| `title` | 필수 | 사람이 식별할 수 있는 source 제목 |
| `url_or_path` | 필수 | 웹 URL 또는 로컬 파일 경로 |
| `accessed_at` | 웹 source 필수 | 웹 source 확인 날짜 |
| `reliability` | v0 제외 | pilot 이후 후보로만 둠 |

source quality tier는 v0에서 `source_register` 필드로 추가하지 않는다.
대신 procedure와 rubric에서 source 사용 원칙으로 적용한다.
`source_register` 필드화 여부는 pilot observation note에서 평가한다.
판단형 하네스 전반에 source quality tier 원칙을 적용할지 여부는 Phase 7-C에서 별도 판단한다.

### 6.3 Section 13 Handoff Schema

Section 13 v0 필수 필드는 3개다.

| 필드 | 필수 여부 | 설명 |
|---|---|---|
| `question` | 필수 | 다음 하네스가 바로 사용할 수 있을 만큼 구체적인 질문 |
| `source_ref` | 필수 | 질문의 근거 source |
| `status` | 필수 | `ready` 또는 `needs_more_source` |

허용 status:

| status | 의미 |
|---|---|
| `ready` | 후속 하네스가 바로 사용할 수 있다. |
| `needs_more_source` | 질문은 유효하지만 추가 source가 필요하다. |

### 6.4 제외 필드

아래 필드는 v0에서 제외한다.

| 필드 | 제외 이유 |
|---|---|
| `target_step` | downstream 하네스 구조와 과결합된다. |
| `why_it_matters` | 질문 자체와 rubric 기준으로 처리한다. |
| `question_type` / `analysis_lens` | pilot observation 후보로만 둔다. |
| `question_id` | 질문이 많아지거나 QA 참조가 필요할 때 선택 후보로 둔다. |

Step 5-7은 설계자의 calibration 자료이지 schema 필드가 아니다.

### 6.5 QA Result Schema

`industry-primer-qa.schema.md`는 별도 후보 파일로 둔다.

분리 이유:

- `industry-primer-qa.md`: QA 실행 절차
- `industry-primer-qa-rubric.md`: 판단 기준
- `industry-primer-qa.schema.md`: `qa.md` 기록 형식

Industry Primer QA는 판단형이므로 결과 기록 형식도 별도 계약으로 둬야 drift를 줄일 수 있다.

## 7. `qa.md` Output Format 8섹션

`qa.md` 또는 이에 준하는 QA result는 아래 8섹션을 가진다.

| 섹션 | 내용 |
|---|---|
| 1. Overall verdict | 전체 판정: `pass`, `pass with adjustments`, `blocked` |
| 2. 5-layer QA table | 구조, 출처, 범위, 판단, handoff별 판정 |
| 3. Findings | 문제, 영향, 필요한 조치. Cross-Section Consistency 연결성 문제와 User Review Required Claims 포함 |
| 4. Source gap review | Source Pack gap, 웹 source gap, top-up 후보 |
| 5. Forbidden area check | 7개 금지 영역 침범 여부 독립 확인 |
| 6. Handoff QA | Section 13 질문의 구체성, 근거, 후속 사용 가능성 확인 |
| 7. Comparison trigger | Codex/Claude 비교가 필요한지 판단 |
| 8. Next action | 진행, 보완, top-up 요청, blocked 처리 |

`Forbidden area check`는 독립 섹션으로 둔다.
범위 침범은 Industry Primer에서 가장 중요한 위험 중 하나이므로 구조 QA나 출처 QA 안에 묻히면 안 된다.

Findings 안에는 아래 하위 섹션을 둔다.

```md
### User Review Required Claims

| priority | claim | why_user_review_required | source_ref | suggested_user_check |
|---|---|---|---|---|

### Additional Gray-Zone Claims

| claim | why_gray_zone | source_ref |
|---|---|---|
```

원칙:

- `User Review Required Claims`는 사용자가 직접 우선 검토할 최대 5개 claim이다.
- `Additional Gray-Zone Claims`는 5개 우선순위에는 못 들었지만 회색 지대로 기록해야 할 claim이다.
- 우선순위 순으로 작성한다.
- 단순 fact보다 해석 claim을 우선한다.
- 후속 분석을 크게 오염시킬 수 있는 claim을 우선한다.
- 사용자 지식과 충돌 가능성이 있는 claim을 우선한다.
- source quality가 낮거나 간접적인 source에 의존한 claim을 우선한다.
- 이 하위 섹션들은 새 QA 상태값이 아니며, 사용자 승인 게이트의 입력 정보다.

## 8. 판단형 Rubric 설계

Industry Primer rubric은 기계 checklist가 아니다.
LLM adapter가 같은 기준으로 판단할 수 있게 해주는 서술형 rubric이다.

### 8.1 5층 QA

| QA 층 | 보는 것 |
|---|---|
| 구조 QA | 필수 섹션과 필드가 있는가 |
| 출처 QA | 핵심 fact와 source가 연결되는가 |
| 범위 QA | 7개 금지 영역으로 넘어가지 않았는가 |
| 판단 QA | fact, interpretation, 확인 필요, source 해석 위험, company bias 위험이 분리됐는가 |
| handoff QA | 다음 하네스가 바로 사용할 수 있는 질문이 있는가 |

### 8.2 전체 판정 집계

| 조건 | 전체 판정 |
|---|---|
| 5층 QA 중 하나라도 `blocked` | `blocked` |
| blocked는 없고 하나라도 `pass with adjustments` | `pass with adjustments` |
| 5층 QA가 모두 `pass` | `pass` |

### 8.3 Section 1 Rubric

Section 1은 산업 정의와 target company context를 짧게 고정한다.

| 판정 | 기준 |
|---|---|
| `pass` | 산업 경계, 해결 문제, 고객/수요자, target company context가 드러나고 descriptive 수준에 머물며, 핵심 fact나 company context가 `source_ref`로 추적 가능하다. |
| `pass with adjustments` | 산업 정의가 너무 넓거나 좁거나, target company 위치가 약하거나, source_ref/용어 설명 보완이 필요하다. |
| `blocked` | 산업을 정의하지 못하거나, target company 연결이 없거나, source 없이 단정하거나, 후속 단계 결론을 미리 낸다. |

anti-cheerleading check:

- target company가 해당 산업에서 유리하다는 암시를 하려면 source와 구조 근거가 있어야 한다.
- source 없이 target company의 포지션을 긍정적으로 포장하면 `pass with adjustments` 또는 `blocked` finding으로 기록한다.

### 8.4 Section 3 Rubric

Section 3은 참여자 구조와 target company 위치를 descriptive하게 정리한다.

| 판정 | 기준 |
|---|---|
| `pass` | 주요 참여자 category, 역할, 중요한 흐름, target company 위치, 필요한 하위 시장 맥락, source_ref가 있다. |
| `pass with adjustments` | 참여자 category는 있으나 흐름/위치/source_ref가 약하거나, 회사명 예시가 많고 category 중심성이 약하다. |
| `blocked` | 단순 회사명 리스트이거나, 참여자 역할을 잘못 배치하거나, target company 위치가 없거나, 병목/profit pool/pricing power/moat/market share 결론으로 넘어간다. |
| `blocked` | source 없이 산업 구조나 참여자 역할을 단정한다. |

source interpretation risk:

- source에 있는 회사 표현을 산업 전체 구조로 확대 해석하지 않는다.
- 회사 공식 narrative와 외부 산업 구조 설명이 충돌하거나 긴장 관계에 있으면 User Review Required Claims 후보로 표시한다.

묘사 / 회색 지대 / 금지 구분:

| 구분 | 기준 | 예시 |
|---|---|---|
| 허용 | 참여자 역할, 자산, 데이터 접근, 유통 경로를 descriptive하게 설명한다. | "Google과 Meta는 대규모 first-party data와 owned inventory를 가진 주요 참여자다." |
| 회색 지대 | 구조 설명이 누가 유리하거나 불리해 보이는지 암시하지만, moat/초과이익/투자 판단 결론까지는 가지 않는다. | "Google과 Meta는 owned inventory와 first-party data를 갖고 있어 ATT 이후 독립 DSP보다 데이터 손실이 적을 수 있다." |
| 금지 | 지속 우위, 초과이익, moat, 승자/패자, 투자 판단으로 결론화한다. | "따라서 Google/Meta는 지속 가능한 moat를 가진다." |

first slice pilot용 임시 threshold:

- 회색 지대 claim이 6개 이상이면 최소 `pass with adjustments`로 판정한다.
- 이 숫자 기준은 first slice pilot용 임시 기준이며, full pilot이나 다른 산업 하네스에 그대로 일반화하지 않는다.
- descriptive 구조 설명과 금지 영역 결론을 분리할 수 없으면 `blocked` 후보로 본다.
- threshold가 너무 느슨하거나 엄격했는지는 pilot observation note에서 평가한다.

### 8.5 Section 5 Rubric

Section 5는 Section 1/3 이해에 필요한 핵심 용어를 쉬운 언어로 정리한다.

| 판정 | 기준 |
|---|---|
| `pass` | 용어 선정이 산업 이해와 연결되고, 쉬운 정의와 중요성, source_ref/확인 필요가 있으며 투자 결론으로 흐르지 않는다. |
| `pass with adjustments` | 용어가 너무 많거나 적고, 정의가 어렵거나, 중요도/source_ref 보완이 필요하다. |
| `blocked` | 핵심 용어가 없거나, 중요한 용어를 잘못 정의하거나, 용어 설명이 투자 결론으로 변하거나, source 없이 핵심 용어를 단정하거나, 핵심 산업 용어를 이해하지 못한 채 참여자 구조를 작성한다. |

### 8.6 Section 13 Rubric

Section 13은 다음 하네스가 이어받을 수 있는 질문을 남긴다.

| 판정 | 기준 |
|---|---|
| `pass` | 질문이 구체적이고, `source_ref`와 `status`가 있으며, 결론이 아니라 질문이고, 후속 단계 배분 방향을 포함하지 않는다. |
| `pass with adjustments` | 질문은 유용하지만 너무 넓거나, source_ref/status가 약하거나, 결론처럼 읽히거나, step 이름을 설명용으로 사용했다. |
| `blocked` | 질문이 없거나 막연하고, source_ref가 없거나, 후속 단계 결론을 미리 포함하거나, 특정 후속 단계로 배분 방향을 질문에 포함하거나, `needs_more_source`를 숨기거나 data gap을 `ready`처럼 넘긴다. |

### 8.7 Source Ref 이중 검증

`source_ref`는 출처 QA와 handoff QA에서 모두 확인한다.

| 위치 | 목적 |
|---|---|
| 출처 QA | 본문 전체의 핵심 주장과 fact가 source와 연결되는지 확인 |
| handoff QA | Section 13 질문이 다음 하네스가 이어받을 수 있는 근거 source를 가지는지 확인 |

이것은 중복 실수가 아니라 의도적 방어다.

### 8.8 Cross-Section Consistency Check

Cross-Section Consistency는 5층 QA에 새 층을 추가하는 것이 아니다.
개별 섹션 rubric을 적용한 뒤, Section 1 -> 3 -> 5 -> 13 -> `source_register` 연결성을 별도로 확인하는 보조 기준이다.

| 연결 | 확인 기준 |
|---|---|
| Section 1 -> Section 3 | 산업 정의의 경계와 주요 참여자/관계가 Section 3에서 구조화되는가 |
| Section 3 -> Section 5 | Section 3 이해에 필요한 핵심 용어가 Section 5에서 풀리는가 |
| Section 5 -> Section 13 | 용어/구조상 불확실성이 다음 단계 질문으로 이어지는가 |
| Section 13 -> `source_register` | handoff 질문의 `source_ref`가 실제 `source_register` 항목과 연결되는가 |

각 섹션은 그럴듯하지만 이 연결성이 깨져 있으면 `pass with adjustments` 또는 `blocked` finding으로 기록한다.
발견된 문제는 `qa.md` Section 3 Findings에 기록한다.

### 8.9 Source Interpretation Risk and User Review Required Claims

`source_ref`가 있다는 사실만으로 source 해석이 정확하다고 보장하지 않는다.
아래 경우는 `qa.md` Findings의 `User Review Required Claims` 하위 섹션에 우선 기록한다.

| 우선순위 | 표시 대상 |
|---|---|
| 1 | source 문장을 해석해 산업 구조 claim으로 확장한 경우 |
| 2 | 후속 Value Chain, Business Model, Market Share, Competition, Moat, Valuation, 투자 판단을 크게 오염시킬 수 있는 claim |
| 3 | 사용자가 알고 있는 APP/adtech 현실과 충돌할 가능성이 있는 claim |
| 4 | 회사 공식 narrative에 강하게 의존해 target company에 유리하게 읽힐 수 있는 claim |
| 5 | Preferred source가 아니라 낮은 tier source에 의존한 claim |

`User Review Required Claims`는 최대 5개만 표시한다.
5개 상한은 모든 위험 claim이 제거됐다는 뜻이 아니라, 승인 전 사용자가 우선 확인해야 할 상위 위험 claim을 표시한다는 뜻이다.

5개를 초과하는 gray-zone claim은 `Additional Gray-Zone Claims`에 간략히 기록한다.
권장 필드는 `claim`, `why_gray_zone`, `source_ref`다.

`User Review Required Claims`와 `Additional Gray-Zone Claims`는 exhaustive guarantee가 아니다.
AI가 모든 회색 지대 claim을 완전히 식별했다는 뜻은 아니며, first slice observation note에서 누락 여부와 운영 부담을 평가한다.

이 목록들은 자동 blocked 조건이 아니라, 사용자 승인 게이트의 입력 정보다.

## 9. Section 13 Handoff 원칙

### 9.1 Step 5-7은 calibration 자료다

Step 5-7 template는 두 가지 용도로 사용한다.

| 용도 | 설명 |
|---|---|
| 소극적 경계 | Industry Primer가 Value Chain, Business Model, Market Share 결론을 미리 내지 않게 막는다. |
| 적극적 handoff fit | Section 13 질문이 실제 후속 분석의 출발점으로 쓸 수 있는지 검산한다. |

하지만 Step 5-7 이름은 Section 13 schema 필드가 아니다.

### 9.2 Industry Primer와 Value Chain의 경계

Industry Primer와 Value Chain은 모두 target company 위치를 언급할 수 있다.
이는 중복이 아니라 handoff 연결점이다.

| 구분 | Industry Primer | Value Chain |
|---|---|---|
| target company 위치 | 산업 지도를 읽기 위한 descriptive context | 경제적 위치, profit pool, 병목, pricing power 분석의 출발점 |
| 허용 | 어디에 있는지 설명 | 그 위치가 좋은지 분석 |
| 금지 | 좋은 위치인지 결론 | Industry Primer의 기초 산업 정의를 대체 |

Industry Primer는 "어디에 있는가"를 남긴다.
Value Chain은 "그 위치가 경제적으로 어떤 의미인가"를 판단한다.

## 10. Module Tier 연결

blueprint는 pilot plan v1의 module tier를 따른다.

| tier | module | blueprint 연결 |
|---|---|---|
| 필수 적용 | `design-preflight` | 유형, 범위, 금지 영역, 산출물 역할 고정 |
| 필수 적용 | `approval-gate` | top-up, 범위 확장, 하네스 파일 생성, 별도 artifact 추가 승인 |
| 필수 적용 | `qa-scaffold` | 5층 QA, QA result, repair/recheck 기준 |
| 필수 적용 | `signal-routing` | QA/observation note 내부 signal 표현 |
| 필수 적용 | `pilot-first / testing` | preflight -> slice -> QA -> observation -> 다음 단계 |
| 기본 적용 | `security-baseline` | 웹자료, 로컬 파일, checkpoint, 비공개 대화 원문 안전선 |
| 좁게 적용 | `type-schema` | source_register, source_ref, handoff table, status 값 |
| 가볍게 적용 | `observability` | pilot observation note로 병목, drift, module fit 기록 |
| 가볍게 적용 | `docs-organization` | 산출물 위치와 색인 기준만 적용 |
| 조건부 적용 | `comparison` | QA 판정이 애매하거나 해석 차이가 클 때만 발동 |
| 조건부 적용 | `checkpoint` | 세션 전환, context 압축 위험, 사용자 요청 시만 사용 |
| 관찰만 | `candidate-ledger` | 별도 ledger 없이 observation note Section B에 후보 기록 |

모든 module을 강하게 켜지 않는다.
first slice는 최소 구조 검증이 목적이다.

## 11. Adapter 경계

Industry Primer 하네스는 도구 중립 구조로 설계한다.

| 영역 | 소유 위치 |
|---|---|
| 업무 의미, 금지 영역, 입력/출력 계약 | `harness/contracts/` |
| 실행 순서 | `harness/procedures/` |
| 산출물 형식 | `harness/schemas/` |
| 판단 기준 | `harness/rubrics/` |
| Codex/Claude/ChatGPT 실행 방식 | 각 adapter |

adapter는 얇게 유지한다.

adapter가 해도 되는 일:

- 기준 문서 읽기
- source 접근
- 웹검색 도구 호출
- 산출물 작성
- QA 실행
- 사용자 승인 요청
- checkpoint 저장

adapter가 하면 안 되는 일:

- 금지 영역을 새로 정의
- rubric을 adapter 내부에 길게 복사
- `target_step` 같은 schema 필드를 임의 추가
- Source Pack top-up을 조용히 실행
- 사용자가 승인하지 않은 하네스 파일 생성

웹검색이 불가능한 adapter는 해당 항목을 `needs_more_source` 또는 `blocked` 후보로 기록한다.
웹검색 불가 상태에서 웹 source를 추측하지 않는다.

## 12. Approval / Stop 조건

### 12.1 사용자 승인 필요

아래는 반드시 사용자 승인을 받는다.

| 상황 | 이유 |
|---|---|
| Source Pack top-up 실행 | 기존 Source Pack 수집 범위 변경 |
| first slice 범위 확장 | pilot plan v1의 범위 변경 |
| 별도 source_register artifact 추가 | Q4 합의 변경 |
| comparison mode 정식 실행 | 비용과 판단 흐름 증가 |
| User Review Required Claims 검토 완료 전 산출물 승인 | challenge review가 산출물 승인 이전에 필요 |
| 실제 `harness/`, `.agents/`, `.claude/`, `artifacts/` 파일 생성 | blueprint 승인 후 별도 build 단계 필요 |
| 기존 Source Pack 하네스 구조 수정 | 현재 작업 범위 밖 |

### 12.2 중단 조건

아래 경우는 멈추고 사용자에게 확인한다.

| 조건 | 처리 |
|---|---|
| 필수 input에 접근할 수 없음 | top-up 또는 경로 확인 요청 |
| 산업 정의/참여자/핵심 용어를 source 기반으로 판단할 수 없음 | `blocked` 후보 |
| 보안 격리 파일, credential, 비공개 대화 원문 등이 필요해짐 | security-baseline과 approval-gate 적용 |
| 금지 영역 결론 없이는 산출물을 작성할 수 없다고 판단됨 | scope 재확인 |
| 사용자가 논의만 원했는데 실행 요청처럼 보이는 표현이 섞임 | approval-gate 확인 |

### 12.3 Challenge Review Timing

Challenge review는 산출물 승인 이전에 수행한다.

순서:

1. AI adapter가 first slice를 작성한다.
2. AI adapter가 5층 QA를 수행한다.
3. AI adapter가 `User Review Required Claims`를 최대 5개 표시한다.
4. 사용자가 해당 claim을 원문/source와 자기 도메인 지식으로 검토한다.
5. 그 뒤에 사용자 승인, 보완 요청, blocked 판단을 한다.

### 12.4 Top-up 원칙

Source Pack top-up은 기본값이 아니라 예외다.

3단계:

| 단계 | 의미 |
|---|---|
| 계속 진행 | 현재 APP partial Source Pack으로 first slice 작성 가능 |
| 후보 기록 | 있으면 좋지만 blocking은 아닌 source gap |
| 승인 요청 | 없으면 Section 1/3/5/13을 안전하게 작성할 수 없는 blocking gap |

scope 확장은 top-up trigger가 아니다.
scope 확장이 승인된 뒤 source 충분성을 다시 평가하고, 그때 blocking gap이 생긴 경우에만 top-up 요청으로 이어진다.

## 13. Output Artifacts 설계

실제 경로는 build 단계에서 확정한다.
blueprint 수준의 권장 placeholder는 아래와 같다.

```text
artifacts/runs/{run-id}/industry-primer-slice.md
artifacts/runs/{run-id}/qa.md
artifacts/runs/{run-id}/pilot-observation-note.md
```

각 산출물 역할:

| 파일 | 역할 |
|---|---|
| `industry-primer-slice.md` | Section 1/3/5/13, source_register |
| `qa.md` | 8섹션 QA result, 전체 판정, comparison trigger |
| `pilot-observation-note.md` | 내용 품질 관찰과 v5 template 적용 평가 |

`source_register`는 first slice에서 별도 파일이 아니라 `industry-primer-slice.md` 내부 섹션으로 둔다.
signal list는 별도 파일이 아니라 `qa.md` 또는 `pilot-observation-note.md` 내부 선택 섹션으로 둔다.
`User Review Required Claims`는 별도 파일이 아니라 `qa.md` Findings 하위 섹션으로 둔다.

## 14. Full Rubric / QA Expansion 후속 작업

이 blueprint는 first slice용 하네스 설계다.
full Industry Primer rubric/QA 전체 확장은 지금 완성하지 않는다.

후속 작업 위치:

```text
first slice pilot 완료 후
full pilot 진입 전
```

확장 대상:

| 항목 | 이유 |
|---|---|
| Section 2. 산업이 존재하는 이유 | 산업의 근본 수요와 고객 문제를 더 깊게 평가 |
| Section 4. 산업 하위 시장 구분 | Section 3과 분리해 하위 시장 구조를 평가 |
| Section 6. 산업 성장 동인 | 시장 성장과 회사 성장의 혼동 방지 |
| Section 7. 산업 수익 구조 | Value Chain/Business Model 경계가 민감 |
| Section 8. 산업 비용 구조 | Business Model 경계가 민감 |
| Section 9. 규제 / 제도 / 표준 | 산업별 깊이 차이가 큼 |
| Section 10-11. 기술 변화, 구조적 리스크 | Competition/Moat으로 넘어갈 위험 |
| Section 12. 분석 대상 기업과의 연결 | 기업 분석과 Industry Primer 경계가 민감 |
| Section 14. Industry Primer 결론 | 투자 판단으로 기울 위험 |
| full QA rubric | first slice 5층 QA보다 넓은 기준 필요 |
| full output schema | 14개 섹션 전체 구조화 여부 판단 필요 |

## 15. 실제 파일 생성 전 승인 조건

이 blueprint가 승인되더라도 바로 파일을 만들지 않는다.
실제 하네스 파일 생성 전에는 아래를 사용자에게 확인해야 한다.

| 확인 항목 | 선택지 |
|---|---|
| 실제 하네스 위치 | 새 Industry Primer repo / 현재 repo 후보 위치 / 21단계 상위 하네스 아래 |
| first slice artifact 경로 | `artifacts/runs/{run-id}/` 유지 / 별도 pilot folder |
| adapter 생성 여부 | Codex만 / Claude만 / 둘 다 / 아직 생성 안 함 |
| blueprint 범위 | first slice 전용 / full pilot까지 일부 포함 |
| Source Pack 입력 연결 | 현재 APP partial 기준 / 추가 Source Pack top-up 후 |

사용자 승인 문구 예:

```text
이 blueprint 기준으로 실제 Industry Primer first slice 하네스 파일 생성을 진행해줘.
```

그 전에는 실제 `harness/`, `.agents/`, `.claude/`, `artifacts/` 파일을 만들지 않는다.

## 16. Claude Code 교차검증 체크리스트

Claude Code에는 아래를 검증하게 한다.

| 검증 항목 | 확인 질문 |
|---|---|
| blueprint 성격 | 실제 하네스 파일이 아니라 설계도인가 |
| source of truth | Section 1의 주제별 source of truth 구조와 일치하는가: 기본 실행 범위/입력/산출물은 pilot plan note v1, build 직전 risk control은 pre-build risk review note, 작업 상태는 work-map을 따르는가 |
| 기준 문서 반영 | blueprint-prep note, Section 11 consensus, calibration note가 반영됐는가 |
| 파일 구조 후보 | contract/procedure/schema/rubric/output/adapter 경계가 명확한가 |
| runbook/slice 분리 | 전체 실행 흐름과 Section 1/3/5/13 작성 절차가 분리됐는가 |
| contract | 목표, 입력, 출력, 완료 기준, 금지 영역이 충분한가 |
| schema | source_register, Section 13, QA result schema가 pilot plan v1과 일치하는가 |
| QA schema 분리 | `industry-primer-qa.schema.md` 별도 파일 결정이 반영됐는가 |
| rubric | 판단형 rubric과 Section 1/3/5/13 기준이 calibration note와 일치하는가 |
| QA output | `qa.md` 8섹션 구조가 반영됐는가 |
| handoff | `question`, `source_ref`, `status` 3필드만 v0 필수인가 |
| 제외 필드 | `target_step`, `why_it_matters` 제외 이유가 명확한가 |
| Step 5-7 | calibration 자료이지 schema 필드가 아님이 명확한가 |
| source_ref 이중 검증 | 출처 QA와 handoff QA의 목적 차이가 설명됐는가 |
| module tier | pilot plan v1 Section 9와 충돌하지 않는가 |
| approval/stop | top-up, 범위 확장, 파일 생성 승인 조건이 명확한가 |
| pre-build risk 반영 | 중립성, source quality tier, source interpretation risk, User Review Required Claims, challenge review timing, 21단계 가설 문구가 반영됐는가 |
| User Review Required Claims | 새 QA status가 아니라 Findings 하위 섹션과 사용자 승인 게이트 입력 정보로 표현됐는가 |
| Additional Gray-Zone Claims | Findings 하위 섹션으로 들어갔고, URRC와 역할이 구분되는가 |
| Section 3 gray-zone rubric | Section 8.4에 허용/회색지대/금지 예시가 있으며 adtech 회색지대 예시가 포함됐는가 |
| gray-zone threshold | 6개 threshold가 first slice pilot용 임시 기준으로 표시됐는가 |
| exhaustive guarantee 방지 | URRC와 Additional 목록이 모든 위험 claim을 완전히 식별했다는 보장이 아님을 명시했는가 |
| challenge review | 산출물 승인 이전에 사용자 검토 단계가 명시됐는가 |
| full expansion | full rubric/QA expansion이 first slice 후 full pilot 전 작업으로 남아 있는가 |
| 과잉 추가 | pilot plan v1 합의에 없는 필수 산출물, 승인 조건, schema 필드가 추가되지 않았는가 |

## 17. 현재 판정과 다음 단계

현재 판정:

```text
Industry Primer blueprint v0 초안 작성 완료.
기존 Claude Code 교차검증 PASS 후 pre-build risk review 최소 수정 6개 반영.
Claude Code 재교차검증 PASS, 추가 gray-zone/risk-control 보강 반영.
실제 하네스 파일 생성 전.
```

다음 단계:

1. 실제 Industry Primer first slice 하네스 파일 생성 여부를 사용자 승인 게이트에서 결정한다.
2. 승인 전에는 `harness/`, `.agents/`, `.claude/`, `artifacts/` 파일을 만들지 않는다.
