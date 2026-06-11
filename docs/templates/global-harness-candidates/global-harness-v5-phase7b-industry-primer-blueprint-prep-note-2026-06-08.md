# Global Harness v5 Phase 7-B Industry Primer Blueprint Prep Note

- 작성일: 2026-06-08
- 상태: blueprint 작성 전 합의 note. Claude Code 교차검증 PASS.
- 범위: APP/adtech `Industry Primer` 하네스 blueprint 작성 전에 고정할 설계 원칙, 기준 문서, rubric/QA/schema/handoff 합의
- 최신 실행 계획: `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v1-2026-06-08.md`

이 문서는 blueprint 자체가 아니다.
또한 Industry Primer 실행 runbook이나 실제 하네스 파일도 아니다.

이 문서의 목적은 `pilot plan note v1` 작성 후, 실제 Industry Primer blueprint를 만들기 전에 사용자, Codex, Claude Code가 논의하고 합의한 내용을 보존하는 것이다.
특히 판단형 QA, rubric, QA output format, output schema, Section 13 handoff schema에 대한 합의를 휘발시키지 않기 위해 작성한다.

## 1. 이 note를 따로 만드는 이유

`pilot plan note v1`은 APP/adtech first slice pilot 실행 계획서로 닫혔다.
따라서 blueprint 착수 전 논의까지 v1에 계속 추가하면 v1의 실행 계획서 성격이 흐려진다.

`work map`은 항해 지도다.
무엇을 했고 다음에 무엇을 할지 추적하는 데 적합하지만, rubric/QA/schema 철학과 논쟁을 길게 보존하기에는 맞지 않다.

`design principles note`는 pilot plan 작성 전 배경 원칙이다.
지금 논의는 pilot plan v1 이후, blueprint 작성 직전의 구체 설계 논의이므로 별도 note가 더 안전하다.

따라서 이 문서는 아래 역할을 맡는다.

| 역할 | 설명 |
|---|---|
| 합의 보존 | blueprint 작성 전 논의한 기준 문서, QA/rubric/schema/handoff 결론을 기록 |
| drift 방지 | 이후 blueprint 작성자가 과거 논의를 재해석하거나 누락하지 않도록 기준 고정 |
| Claude Code 교차검증 입력 | 이 note를 Claude Code에 검토시켜 남은 이견을 닫기 위한 기준 |
| blueprint 착수 기준 | PASS 후 Industry Primer blueprint 작성의 직접 입력 |

## 2. 현재 단계

현재 위치는 Phase 7-B `Industry Primer` 실전 검증 안이다.

지금까지 완료한 단계:

| 단계 | 산출물 | 상태 |
|---|---|---|
| Industry Primer design principles | `global-harness-v5-phase7b-industry-primer-design-principles-note-2026-06-08.md` | Claude Code PASS |
| pilot plan v0 | `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v0-2026-06-08.md` | Q1~Q12 합의 완료 |
| pilot plan v1 | `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v1-2026-06-08.md` | Claude Code PASS |

다음 큰 단계는 Industry Primer blueprint다.

작업 흐름:

```text
pilot plan v1 PASS
→ blueprint prep note 작성
→ Claude Code 교차검증
→ work map / README 반영
→ Industry Primer blueprint 논의/작성
→ blueprint Claude Code 교차검증
→ 사용자 승인
→ 실제 하네스 파일 생성
```

## 3. Blueprint의 의미

`pilot plan note v1`은 "어떤 pilot을 할 것인가"를 정한 실행 계획서다.

`blueprint`는 "그 pilot을 수행할 Industry Primer 하네스를 어떤 구조로 만들 것인가"를 정하는 설계도다.

| 구분 | pilot plan note v1 | blueprint |
|---|---|---|
| 핵심 질문 | APP/adtech first slice pilot을 어떻게 검증할까? | Industry Primer 하네스를 어떤 파일과 계약으로 만들까? |
| 성격 | 실행 계획 | 하네스 설계도 |
| 주요 내용 | 목적, 범위, 입력, 산출물, QA, top-up, module tier, 실행 순서 | contracts, procedures, schemas, rubrics, outputs, adapter 경계, module 연결 |
| 파일 생성 여부 | 실제 하네스 파일 생성 전 계획 | 실제 파일 생성 전 청사진 |
| 다음 단계 | blueprint 작성 | 사용자 승인 후 harness build |

blueprint는 아직 실제 하네스 파일을 만드는 단계가 아니다.
하네스 파일 생성은 blueprint 교차검증과 사용자 승인 후에만 진행한다.

## 4. Blueprint 기준 문서 세트

기준 문서는 많이 볼수록 좋은 것이 아니라, 문서별 역할을 구분해서 읽어야 한다.
모든 문서를 같은 무게로 읽으면 과거 논의가 다시 열리고 blueprint의 초점이 흐려진다.

### 4.1 필수로 깊게 읽을 문서

| 문서 | 역할 |
|---|---|
| `global-harness-v5-work-map.md` | 현재 위치, 다음 산출물, 진행 상태 확인 |
| `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v1-2026-06-08.md` | 최신 source of truth. Q1~Q12 합의와 first slice 실행 기준 |
| `docs/reference/Phase 2 - Step 4 Industry Primer 템플릿.md` | Industry Primer 산출물 원본. 실제 작성해야 할 도메인 구조 |
| `global-harness-core-structure-template-v1.md` | v5 하네스 구조 틀. contracts/procedures/schemas/rubrics/adapter 경계 확인 |

`pilot plan note v1`과 다른 문서가 충돌하면 `pilot plan note v1`을 우선한다.

### 4.2 경계와 handoff fit 확인용 문서

| 문서 | 소극적 역할 | 적극적 역할 |
|---|---|---|
| `docs/reference/가치투자 리서치 21단계 구조화 버전.md` | Step 4가 21단계 안에서 어디까지 해야 하는지 확인 | Step 4가 다음 단계로 무엇을 넘겨야 하는지 위치 확인 |
| `docs/reference/Phase 2 - Step 5 Value Chain 템플릿.md` | Value Chain 영역을 Industry Primer가 침범하지 않게 함 | Section 13 handoff 질문이 Value Chain에서 실제로 쓸 수 있는지 확인 |
| `docs/reference/Phase 2 - Step 6 Business Model 템플릿.md` | Business Model 결론을 Industry Primer가 미리 내지 않게 함 | Section 13 handoff 질문이 Business Model에서 실제로 쓸 수 있는지 확인 |
| `docs/reference/Phase 2 - Step 7 Market Share 템플릿.md` | Market Share 결론을 Industry Primer가 미리 내지 않게 함 | Section 13 handoff 질문이 Market Share에서 실제로 쓸 수 있는지 확인 |

Step 5~7 문서는 단순한 "금지 영역 울타리"가 아니다.
Section 13 handoff 질문이 다음 하네스의 시작점에서 실제로 유용한지 확인하는 적극적 기준이기도 하다.

### 4.3 필요 시 참조할 문서

| 문서 | 사용할 때 |
|---|---|
| `global-harness-v5-phase7b-industry-primer-design-principles-note-2026-06-08.md` | pilot plan v1의 배경 의도나 설계 원칙을 확인할 때 |
| `global-harness-v5-phase7-planning-consensus-note-2026-06-07.md` | Phase 7 전체 구조, 전역화 판단, Source Pack 소급 검증 맥락이 필요할 때 |

이 두 문서는 blueprint의 직접 source of truth가 아니다.
이미 많은 내용이 `pilot plan note v1`에 흡수됐으므로, 충돌 확인과 배경 이해용으로만 사용한다.

## 5. Step 5~7 문서의 이중 역할

Step 5~7 문서는 blueprint에서 두 역할을 한다.

### 5.1 소극적 역할: 금지 영역 경계

Industry Primer는 산업의 기본 구조를 설명하지만, 다음 단계의 결론을 미리 내리지 않는다.

`pilot plan note v1` 기준 금지 영역은 7개다.

```text
Value Chain
Business Model
Market Share
Competition
Moat
Valuation
투자 판단
```

blueprint의 scope, procedure, rubric, QA output format은 이 7개 금지 영역을 유지해야 한다.
design principles note의 과거 5개 목록을 사용하면 안 된다.

### 5.2 적극적 역할: handoff fit 검증

Industry Primer Section 13은 "다음 단계로 넘길 질문"이다.

이 질문은 단순히 있어야 하는 것이 아니라, 다음 하네스가 바로 사용할 수 있을 만큼 구체적이어야 한다.

따라서 Step 5~7 문서는 아래를 확인하는 데 쓴다.

| 확인 질문 | 의미 |
|---|---|
| 이 질문이 Value Chain / Business Model / Market Share 단계에서 실제 분석 시작점이 되는가? | handoff fit |
| 질문이 너무 일반적이거나 "추가 조사 필요" 수준에 머물지 않는가? | handoff QA |
| 질문이 다음 단계 결론을 미리 포함하지 않는가? | scope QA |
| 질문의 근거 source가 추적 가능한가? | source QA |

이 적극적 역할은 blueprint의 rubric과 QA output format에 반영해야 한다.

## 6. 판단형 QA 원칙

Industry Primer QA는 Source Pack QA와 성격이 다르다.

Source Pack QA는 비교적 기계적이다.

- 파일이 있는가
- catalog에 등록됐는가
- raw 경로가 맞는가
- index와 catalog가 일치하는가

Industry Primer QA는 판단형이다.

- 이 문장이 산업 구조 설명인가, Business Model 결론인가
- 이 설명이 참여자 구조인가, Competition 분석인가
- 이 질문이 다음 단계에서 실제로 쓸 수 있는가
- fact와 interpretation이 분리됐는가
- source gap이 있어도 downstream으로 넘겨도 안전한가

따라서 blueprint는 QA를 단순 체크리스트로 설계하면 안 된다.

합의된 표현:

```text
Industry Primer QA는 자동 검증이 아니라 LLM 판단 기반 QA다.
Codex든 Claude Code든 실행 adapter는 같은 rubric을 읽고 판단해야 한다.
```

이 말은 "Claude Code만 판단한다"는 뜻이 아니다.
모델 중립 원칙에 따라, 어떤 LLM adapter가 실행하든 같은 rubric을 읽고 같은 기준으로 판단해야 한다.

## 7. Rubric 설계 원칙

Industry Primer rubric은 예/아니오 체크리스트가 아니라 판단 기준 서술형 rubric이어야 한다.

좋은 rubric은 아래를 제공해야 한다.

| 필요 요소 | 설명 |
|---|---|
| 판단 경계 | `pass`, `pass with adjustments`, `blocked`의 차이를 설명 |
| 예외 처리 | source gap, partial input, 확인 필요 표시가 있는 경우의 처리 |
| 금지 영역 기준 | 어떤 표현이 scope creep인지 설명 |
| handoff 품질 기준 | 다음 하네스가 바로 사용할 수 있는 질문인지 판단 |
| comparison trigger | 판단이 애매할 때 교차검증을 언제 발동할지 설명 |

예시:

| QA 항목 | 약한 기준 | 판단형 rubric 기준 |
|---|---|---|
| 범위 QA | 금지 영역이 없는가? | 문장이 산업 구조 설명을 넘어 Value Chain / Business Model / Market Share / Competition / Moat / Valuation / 투자 판단 결론을 암시하는가? 암시 수준이면 `pass with adjustments`, 결론 수준이면 `blocked` |
| 판단 QA | fact와 해석이 분리됐는가? | 수치, 제도, 기업 발언은 fact로, 산업 구조 해석은 interpretation으로 표시했는가? 출처 없는 해석을 fact처럼 쓰지 않았는가? |
| handoff QA | 질문이 있는가? | 질문 자체만 읽어도 다음 단계에서 왜 필요한지 드러나는가? 다음 하네스가 바로 분석을 시작할 만큼 구체적인가? |

rubric은 `harness/rubrics/industry-primer-qa-rubric.md` 같은 별도 후보 파일로 설계하는 것이 자연스럽다.
정확한 파일명은 blueprint에서 확정한다.

## 8. QA output format 설계 원칙

rubric만 있고 QA 결과 형식이 없으면 실행자마다 `qa.md` 구조가 달라진다.

따라서 blueprint는 `qa.md` 또는 이에 준하는 QA result의 구조를 함께 설계해야 한다.

첫 slice 기준 QA output format 후보:

| 섹션 | 역할 |
|---|---|
| Overall verdict | `pass` / `pass with adjustments` / `blocked` |
| 5-layer QA table | 구조, 출처, 범위, 판단, handoff별 판정 |
| Findings | 주요 문제, severity, 관련 섹션, 수정 필요 여부 |
| Source gap review | 부족한 Source Pack / 웹 source와 처리 방식 |
| Forbidden area check | 7개 금지 영역 침범 여부를 독립적으로 확인 |
| Handoff QA | Section 13 질문이 Step 5~7에서 쓸 수 있는지 확인 |
| Comparison trigger | Codex/Claude 비교가 필요한지 판단 |
| Next action | blueprint 진행, 보완, top-up 요청 등 |

특히 `Forbidden area check`는 독립 섹션으로 두는 것이 좋다.
구조 QA나 출처 QA에 묻혀 범위 침범을 놓치지 않기 위해서다.

QA output은 아래 두 방향을 모두 기록해야 한다.

| 방향 | 설명 |
|---|---|
| 산출물 품질 | Industry Primer slice가 후속 단계로 넘겨도 안전한가 |
| 하네스 적합성 | v5 module, rubric, schema, signal-routing이 과하거나 부족하지 않았는가 |

산출물 품질은 QA result에 중심적으로 기록하고, 하네스 적합성은 Pilot observation note Section B와 연결한다.

## 9. Output schema 설계 원칙

Industry Primer는 기본적으로 prose 산출물이다.
하지만 모든 것을 자유 prose로 두면 다음 하네스가 읽기 어렵다.
반대로 전체 JSON/JSONL을 강제하면 v0에서 과하다.

따라서 합의된 방향은 부분 구조화다.

| 산출물 부분 | 처리 |
|---|---|
| 본문 Section 1, 3, 5 | 사람이 읽는 markdown prose + 표 |
| source_register | 구조화된 table 필수 |
| source_ref | 본문에서 source_id 참조 |
| Section 13 handoff 질문 | 최소 구조화된 table |
| 전체 Industry Primer | v0에서는 JSON/JSONL 강제 안 함 |

blueprint는 아래 질문에 답해야 한다.

```text
Industry Primer 산출물 중 어디까지 prose로 두고,
어디부터 구조화된 schema 또는 table contract로 둘 것인가?
```

현재 합의는 다음과 같다.

- 전체 문서를 기계용 JSON으로 만들지 않는다.
- 사람이 읽는 markdown 산출물을 기본으로 한다.
- source_register와 Section 13 handoff 질문은 downstream 재사용을 위해 구조화한다.
- schema 파일이 필요하다면 source_register와 handoff question format부터 검토한다.

## 10. Section 13 handoff schema 합의

Section 13은 많은 논의가 있었던 쟁점이다.
초기에는 `target_step`, `why_it_matters`, `question_id`, `question_type` 같은 필드를 검토했다.
Claude Code는 `target_step`과 `why_it_matters`에 강하게 반대했고, Codex도 설계 원칙상 그 반대를 수용했다.

### 10.1 최종 v0 필수 필드

Section 13 handoff 질문은 v0에서 최소 구조화한다.

필수 필드:

| 필드 | 의미 |
|---|---|
| `question` | 다음 하네스가 바로 사용할 수 있을 만큼 구체적인 질문 |
| `source_ref` | 질문의 근거 source |
| `status` | `ready` / `needs_more_source` |

`status` 의미:

| status | 의미 |
|---|---|
| `ready` | 다음 하네스가 바로 사용할 수 있는 질문 |
| `needs_more_source` | 질문은 유효하지만 추가 source가 필요함 |

`needs_more_source`가 많으면 Source Pack top-up 후보, 웹 source gap, 또는 full pilot 보완 후보로 연결할 수 있다.

### 10.2 v0 필수에서 제외한 필드

| 필드 | 제외 이유 |
|---|---|
| `target_step` | Industry Primer 산출물이 downstream 하네스 구조에 결합될 위험이 있음 |
| `why_it_matters` | 좋은 질문은 질문 자체에서 목적이 드러나야 하며, 별도 필드로 두면 작성 부담과 중복이 생김 |
| `question_type` / `analysis_lens` | 필요성은 열어두지만 v0 필수로 두기에는 이르다 |
| `question_id` | 질문이 많거나 QA에서 참조할 때만 필요. v0 필수는 아님 |

### 10.3 target_step을 제외한 이유

`target_step`은 다음처럼 보기에 유용해 보인다.

```text
target_step: Value Chain
```

그러나 이 필드를 필수화하면 Industry Primer가 다음 하네스의 내부 구조를 알아야 한다.
이는 upstream 산출물이 downstream 구조에 결합되는 결과를 만든다.

나중에 Step 5/6/7 구조가 바뀌거나, Value Chain과 Business Model의 경계가 바뀌면 Industry Primer 산출물 schema까지 흔들릴 수 있다.
따라서 v0에서는 `target_step`을 넣지 않는다.

### 10.4 why_it_matters를 제외한 이유

`why_it_matters`의 의도는 좋다.
다음 하네스가 질문의 목적을 이해하도록 돕기 때문이다.

그러나 v0에서는 필드로 만들지 않는다.
질문 자체가 구체적이면 왜 중요한지 자연스럽게 드러나야 한다.

대신 handoff QA rubric에 아래 기준을 넣는다.

```text
질문 자체만 읽어도 다음 단계에서 왜 필요한지 드러나는가?
```

즉, `why_it_matters` 필드는 제외하지만 그 문제의식은 rubric으로 유지한다.

### 10.5 question_type / analysis_lens 후보 처리

`question_type` 또는 `analysis_lens`가 필요할 수 있다는 문제의식은 유지한다.
다만 v0 필수 필드로 넣지 않고 pilot observation 후보로 둔다.

주의:

Step 이름 기반 naming은 피한다.

좋지 않은 후보:

```text
value_chain_candidate
business_model_candidate
market_share_candidate
```

이 이름들은 사실상 `target_step`의 다른 이름이므로 downstream coupling을 남긴다.

필요성이 확인되면 아래처럼 개념 기반 naming을 검토한다.

```text
structural
definitional
regulatory
monetization
participant_map
source_gap
```

이 결정은 지금 확정하지 않는다.
first slice 후 Pilot observation note Section B에 "handoff 질문 분류 필드가 필요한가"를 후보로 기록한다.

## 11. Blueprint가 반드시 답해야 할 질문

blueprint 논의는 아래 질문을 중심으로 진행한다.

| 질문 | 설명 |
|---|---|
| 하네스 구조 | Industry Primer 하네스는 어떤 contracts/procedures/schemas/rubrics/outputs를 가져야 하는가 |
| Rubric | 판단형 QA의 기준을 어떻게 서술할 것인가 |
| QA output format | `qa.md`는 어떤 구조로 판단 결과와 근거를 기록해야 하는가 |
| Output schema | Industry Primer 산출물 중 어디까지 prose로 두고 어디부터 구조화할 것인가 |
| Handoff | Section 13이 Step 5~7에서 실제로 쓸 수 있으려면 어떤 필드와 QA 기준이 필요한가 |
| Module 연결 | pilot plan v1의 module tier를 실제 하네스 파일 구조에 어떻게 연결할 것인가 |

이 질문들은 순차라기보다 서로 맞물려 있다.
특히 rubric, QA output format, output schema는 함께 설계해야 한다.

## 12. Blueprint 작성 시 유의할 점

blueprint 작성자는 아래를 지켜야 한다.

| 유의점 | 이유 |
|---|---|
| `pilot plan note v1`을 최신 source of truth로 둔다 | Q1~Q12 합의가 반영된 최신 기준이기 때문 |
| 금지 영역은 7개를 사용한다 | design principles note의 과거 5개 목록을 쓰면 Q7 합의가 깨짐 |
| Step 5~7은 경계와 handoff fit을 동시에 확인한다 | 단순 scope 방지가 아니라 Section 13 품질 검증에 필요 |
| QA는 판단형 rubric으로 설계한다 | Industry Primer는 단순 파일 검증이 아니라 판단 기반 산출물이기 때문 |
| QA output format을 함께 설계한다 | rubric만 있으면 QA 결과 기록이 실행자마다 달라짐 |
| 전체 JSON 강제는 피한다 | v0에서 과도한 구조화는 pilot 목적을 흐림 |
| source_register와 Section 13은 구조화한다 | 출처 추적과 downstream handoff 재사용에 필요 |
| `target_step` 필수화를 피한다 | downstream 하네스 구조와 과결합을 막기 위함 |
| `why_it_matters`는 필드가 아니라 rubric으로 처리한다 | 작성 부담을 줄이고 질문 자체의 품질을 높이기 위함 |

## 13. 다음 작업

권장 다음 순서:

1. 이 blueprint prep note를 Claude Code에 교차검증한다.
2. 이견이 있으면 이 note를 수정한다.
3. PASS 후 `work map`과 `README`에 이 note를 반영한다.
4. 이 note를 기준으로 Industry Primer blueprint 논의를 시작한다.
5. blueprint 초안 작성 후 Claude Code 교차검증을 받는다.
6. 사용자 승인 후 실제 Industry Primer 하네스 파일을 만든다.

## 14. Claude Code 교차검증 요청 기준

Claude Code에는 아래를 확인하게 한다.

| 검증 항목 | 확인 질문 |
|---|---|
| 기준 문서 세트 | 필수/경계/handoff/배경 문서 구분이 적절한가 |
| Step 5~7 역할 | 금지 영역 경계와 handoff fit 검증의 이중 역할이 반영됐는가 |
| 판단형 QA | Source Pack식 기계 QA가 아니라 LLM 판단 기반 QA로 설계됐는가 |
| rubric 원칙 | 판단 경계, source gap, 금지 영역, handoff 품질을 서술형 rubric으로 다루는가 |
| QA output format | `qa.md` 구조를 blueprint에서 설계해야 한다는 점이 반영됐는가 |
| output schema | 전체 JSON 강제 없이 source_register와 Section 13만 부분 구조화하는 방향이 타당한가 |
| Section 13 schema | 필수 필드 `question`, `source_ref`, `status`가 충분한가 |
| 제외 필드 | `target_step`, `why_it_matters`를 v0 필수에서 제외한 이유가 타당한가 |
| 후보 필드 | `question_type` / `analysis_lens`를 Step 이름이 아닌 개념 기반 후보로만 둔 결정이 타당한가 |
| 다음 작업 | 이 note를 기준으로 blueprint 논의로 넘어가도 되는가 |

현재 이 문서는 Claude Code 교차검증 PASS 상태이며, Industry Primer blueprint 논의의 기준 문서로 사용한다.
