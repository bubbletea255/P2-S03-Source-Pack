# Global Harness v5 Phase 7-B Industry Primer Blueprint Prep Section 11 Consensus Note

- 작성일: 2026-06-09
- 상태: Claude Code 교차검증 PASS
- 단계: Phase 7-B
- 작업 지도 항목: `blueprint 논의: blueprint-prep note Section 11 기준 6개 질문 커버`
- 기준 문서: `global-harness-v5-phase7b-industry-primer-blueprint-prep-note-2026-06-08.md`
- 기준 섹션: Section 11. `Blueprint가 반드시 답해야 할 질문`
- 목적: Industry Primer blueprint 작성 전에 Section 11의 6개 질문에 대한 사용자, Codex, Claude Code 논의와 합의를 고정한다.

이 문서는 Industry Primer blueprint가 아니다.
또한 실제 Industry Primer 하네스 파일, runbook, adapter, schema, rubric 파일도 아니다.

이 문서의 역할은 blueprint 작성 직전에 나온 핵심 설계 합의를 휘발시키지 않고 고정하는 것이다.
특히 판단형 QA, `qa.md` output format, 부분 구조화, Section 13 handoff schema, Step 5~7 calibration 경계, module 연결에 대한 합의를 이후 blueprint 초안 작성자가 그대로 따를 수 있게 만든다.

## 1. 이 문서를 따로 만드는 이유

`global-harness-v5-phase7b-industry-primer-blueprint-prep-note-2026-06-08.md`는 blueprint 작성 전 논의의 출발점이었다.
그 문서의 Section 11은 blueprint가 반드시 답해야 할 6개 질문을 제시했다.

그 뒤 사용자, Codex, Claude Code는 아래 주제를 두고 추가 논의를 진행했다.

- Industry Primer 하네스 구조
- 판단형 rubric
- `qa.md` output format
- output schema와 부분 구조화
- Section 13 handoff schema
- Step 5~7 템플릿의 역할
- `target_step`, `why_it_matters`, `question_type` 같은 후보 필드의 포함 여부
- `source_ref`를 출처 QA와 handoff QA에서 이중 확인하는 이유
- module tier를 실제 하네스 파일 구조에 어떻게 연결할지

이 논의는 단순한 의견 교환이 아니라 blueprint 작성 기준을 사실상 확정한 과정이다.
따라서 바로 blueprint 파일을 작성하면 기억 의존, 문서 간 충돌, 과거 합의 재해석 위험이 커진다.

이 문서는 그 위험을 줄이기 위한 consensus note다.

## 2. 현재 단계와 문서 위치

현재 Phase 7-B 흐름은 아래 상태다.

| 단계 | 산출물 | 상태 |
|---|---|---|
| Industry Primer design principles | `global-harness-v5-phase7b-industry-primer-design-principles-note-2026-06-08.md` | Claude Code PASS |
| pilot plan v0 | `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v0-2026-06-08.md` | Q1~Q12 합의 완료 |
| pilot plan v1 | `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v1-2026-06-08.md` | Claude Code PASS |
| blueprint prep note | `global-harness-v5-phase7b-industry-primer-blueprint-prep-note-2026-06-08.md` | Claude Code PASS |
| blueprint prep Section 11 consensus | 이 문서 | Claude Code 교차검증 전 |

이 문서는 작업 지도상 아래 항목에 대응한다.

```text
Phase 7-B
blueprint 논의: blueprint-prep note Section 11 기준 6개 질문 커버
```

즉 이 문서는 blueprint 작성 전 최종 설계 합의 문서이며, Claude Code 교차검증 PASS 후 작업 지도에서 해당 항목을 `done`으로 닫는 기준이 된다.

## 3. 기준 문서 우선순위

blueprint 작성 시 문서 우선순위는 아래와 같다.

| 우선순위 | 문서 | 역할 |
|---|---|---|
| 1 | `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v1-2026-06-08.md` | APP/adtech first slice pilot의 최신 source of truth |
| 2 | `global-harness-v5-phase7b-industry-primer-blueprint-prep-note-2026-06-08.md` | blueprint 작성 전 rubric/QA/schema/handoff 원칙 |
| 3 | 이 문서 | blueprint prep Section 11의 6개 질문에 대한 추가 합의 |
| 4 | `docs/reference/Phase 2 - Step 4 Industry Primer 템플릿.md` | Industry Primer 원본 산출물 구조 |
| 5 | Step 5~7 reference templates | 금지 영역 경계와 handoff fit calibration |
| 6 | `global-harness-core-structure-template-v1.md` | v5 core 구조, contracts/procedures/schemas/rubrics/adapter 경계 |

충돌이 있을 때는 `pilot plan note v1`을 우선한다.
특히 금지 영역은 design principles note의 과거 5개 목록이 아니라 `pilot plan note v1`의 7개 목록을 사용한다.

금지 영역:

```text
Value Chain
Business Model
Market Share
Competition
Moat
Valuation
투자 판단
```

## 4. Section 11의 6개 질문과 최종 합의 요약

`blueprint-prep note` Section 11은 아래 6개 질문을 제시했다.

| 질문 | 최종 합의 |
|---|---|
| 하네스 구조 | contracts, procedures, schemas, rubrics, outputs, adapter 경계를 분리한다. 실제 파일 배치는 사용자 승인 후 결정한다. |
| Rubric | 기계 체크리스트가 아니라 판단형 서술 rubric으로 설계한다. |
| QA output format | `qa.md` output format을 별도 schema 계약으로 둔다. |
| Output schema | 전체 JSON 강제는 피하고, 본문 prose + `source_register` + Section 13 handoff table만 부분 구조화한다. |
| Handoff | Section 13 v0 필드는 `question`, `source_ref`, `status`만 필수로 둔다. Step 5~7은 calibration 자료이지 schema 필드가 아니다. |
| Module 연결 | pilot plan v1의 module tier를 실제 하네스 구조에 연결하되, comparison/checkpoint/candidate-ledger는 first slice에서 과하게 켜지 않는다. |

이 질문들은 순차적으로 따로 닫는 것이 아니라 하나의 blueprint 문서로 수렴한다.
다만 blueprint 작성자가 각 질문을 빠뜨리지 않도록 이 문서에서 질문별 합의를 따로 고정한다.

## 5. 질문 1: 하네스 구조 합의

### 5.1 기본 방향

Industry Primer 하네스는 Source Pack과 성격이 다르다.

Source Pack은 수집형 하네스다.
Industry Primer는 판단형 리서치 산출물을 만드는 하네스다.

따라서 Industry Primer 하네스는 아래 구조를 가져야 한다.

| 구성 | 역할 |
|---|---|
| contract | 목표, 입력, 출력, 완료 기준, 금지 영역, non-goals |
| runbook/procedure | preflight, slice 작성, QA, observation, 판정 순서 |
| output schema | slice output, source_register, handoff question, QA result 형식 |
| rubric | pass / pass with adjustments / blocked 판단 기준 |
| output artifacts | slice output, QA result, pilot observation note |
| adapter | Codex/Claude/ChatGPT 계열 실행자의 얇은 진입점 |
| module connections | approval, QA, signal, security, observability, docs, comparison, checkpoint, candidate 처리 |

### 5.2 후보 파일 구조

blueprint는 아래 논리 구조를 제안해야 한다.
정확한 실제 경로는 사용자 승인 후 harness build 단계에서 확정한다.

| 후보 파일 | 역할 |
|---|---|
| `harness/contracts/industry-primer.contract.md` | 목적, 입력, 출력, 금지 영역, 완료 기준 |
| `harness/procedures/industry-primer-runbook.md` | 전체 실행 순서 |
| `harness/procedures/industry-primer-slice.md` | first slice 작성 절차 |
| `harness/procedures/industry-primer-qa.md` | QA 실행 절차 |
| `harness/schemas/industry-primer-slice.schema.md` | slice output, source_register, Section 13 handoff table 형식 |
| `harness/schemas/industry-primer-qa.schema.md` | `qa.md` output format |
| `harness/rubrics/industry-primer-qa-rubric.md` | 판단형 QA rubric |
| adapter skill/agent | Codex/Claude 실행 진입점. 공통 업무 규칙은 복사하지 않음 |

`industry-primer-runbook.md`는 preflight, slice 작성, QA, observation, 완료 판정의 전체 실행 흐름을 관리하고, `industry-primer-slice.md`는 Section 1/3/5/13 작성 규칙과 Source Pack/웹 source 사용 방식을 담당한다.

### 5.3 실제 배치 유의점

현재 작업 루트는 Source Pack 하네스다.
Industry Primer는 Phase 2 Step 4 하네스이므로 Source Pack과 동일한 `harness/`에 바로 섞어 넣으면 경계가 흐려질 수 있다.

따라서 blueprint는 먼저 논리 구조를 제시하고, 실제 생성 위치는 별도 승인 단계에서 결정해야 한다.

가능한 배치 후보:

| 배치 후보 | 의미 |
|---|---|
| 새 Industry Primer 전용 하네스 루트 | Source Pack과 독립된 Step 4 하네스 |
| 현재 repo 안의 후보/실험 위치 | Phase 7-B pilot 전용 임시 구조 |
| 21단계 상위 하네스 아래 Step 4 하네스 | 장기적으로 가장 자연스러운 구조 후보 |

blueprint는 이 선택지를 열어두되, 사용자 승인 없이 실제 파일을 생성하지 않는다.

## 6. 질문 2: Rubric 합의

### 6.1 판단형 rubric 원칙

Industry Primer QA는 자동 체크리스트가 아니다.
LLM adapter가 같은 rubric을 읽고 판단하는 모델 중립 QA다.

rubric은 아래를 서술형으로 정의해야 한다.

| 항목 | 필요 내용 |
|---|---|
| 판정 상태 | `pass`, `pass with adjustments`, `blocked` |
| 5층 QA | 구조, 출처, 범위, 판단, handoff |
| source gap 처리 | gap이 있어도 진행 가능한 경우와 blocked인 경우 |
| 금지 영역 | 7개 금지 영역으로 넘어간 정도 |
| fact/interpretation 분리 | 추측을 fact처럼 쓰지 않았는지 |
| handoff 품질 | 다음 하네스가 바로 사용할 수 있는 질문인지 |
| comparison trigger | 판단이 애매할 때 교차검증을 켜는 조건 |

### 6.2 범위 QA 기준

범위 QA는 아래 금지 영역 7개를 기준으로 한다.

```text
Value Chain
Business Model
Market Share
Competition
Moat
Valuation
투자 판단
```

판정 기준:

| 판정 | 기준 |
|---|---|
| `pass` | 산업 정의, 참여자 구조, 핵심 용어, handoff 질문에 머문다. |
| `pass with adjustments` | 일부 문장이 금지 영역 쪽으로 기울지만 수정 가능하다. |
| `blocked` | 금지 영역 결론을 미리 냈거나 후속 단계 분석으로 넘어갔다. |

### 6.3 handoff QA 기준

handoff QA는 Section 13 질문의 품질을 본다.

권장 기준:

| 판정 | 기준 |
|---|---|
| `pass` | 질문이 구체적이고 `source_ref`가 있으며, 후속 하네스가 바로 조사나 분석의 출발점으로 사용할 수 있다. |
| `pass with adjustments` | 질문 방향은 유용하지만 너무 넓거나, `source_ref`가 약하거나, 표현을 좁히면 더 좋아진다. |
| `blocked` | 질문이 없거나, `source_ref`가 없거나, 다음 단계 결론을 미리 내렸거나, 특정 후속 단계로의 배분 방향을 질문에 포함한다. |

여기서 "특정 후속 단계로의 배분 방향"은 아래처럼 질문 안에 downstream routing을 포함하는 경우다.

좋지 않은 예:

```text
이 질문은 Value Chain 단계로 넘긴다.
이 항목은 Business Model 분석에서 검증해야 한다.
이 질문은 Market Share 단계의 입력이다.
```

좋은 예:

```text
APP가 광고 생태계에서 어떤 참여자와 수익 흐름으로 연결되는지 확인할 필요가 있다.
```

이 기준은 내용 판단 기준이다.
`target_step` 같은 필드가 schema에 존재하는지 여부는 structure/schema QA에서 처리한다.

## 7. 질문 3: QA output format 합의

### 7.1 QA 3파일 분리

Industry Primer QA는 세 층으로 분리한다.

| 파일 유형 | 역할 |
|---|---|
| procedure | 언제, 어떤 순서로 QA를 실행하는가 |
| rubric | 무엇을 기준으로 판단하는가 |
| schema | 판단 결과를 어떤 형식으로 기록하는가 |

따라서 blueprint에서는 `industry-primer-qa.schema.md`를 별도 후보 파일로 둔다.

결정:

```text
industry-primer-qa.schema.md는 별도 파일로 둔다.
rubric 파일은 "어떻게 판단하는가"를 담고,
schema 파일은 "판단 결과를 qa.md에 어떤 형식으로 기록하는가"를 담는다.
```

이 분리는 Source Pack보다 Industry Primer에서 더 중요하다.
Source Pack QA는 파일 존재, catalog 정합성 같은 기계적 검증이 많지만, Industry Primer QA는 판단형이기 때문이다.

### 7.2 qa.md output format

`qa.md` 또는 이에 준하는 QA result는 아래 8섹션을 가진다.

| 섹션 | 역할 |
|---|---|
| Overall verdict | 전체 판정: `pass`, `pass with adjustments`, `blocked` |
| 5-layer QA table | 구조, 출처, 범위, 판단, handoff별 판정 |
| Findings | 문제, 영향, 필요한 조치 |
| Source gap review | Source Pack gap, 웹 source gap, top-up 후보 |
| Forbidden area check | 7개 금지 영역 침범 여부 독립 확인 |
| Handoff QA | Section 13 질문의 구체성, 근거, 후속 사용 가능성 확인 |
| Comparison trigger | Codex/Claude 비교가 필요한지 판단 |
| Next action | 진행, 보완, top-up 요청, blocked 처리 |

`Forbidden area check`는 독립 섹션으로 둔다.
범위 침범은 Industry Primer에서 가장 중요한 위험 중 하나이므로 구조 QA나 출처 QA 안에 묻히면 안 된다.

### 7.3 source_ref 이중 확인

`source_ref`는 출처 QA와 handoff QA 양쪽에서 확인한다.

이것은 단순 중복이 아니라 의도적 방어다.

| QA 층 | `source_ref` 확인 목적 |
|---|---|
| 출처 QA | 본문 전체의 핵심 주장, fact, 산업 구조 설명이 근거 source와 연결되는지 확인 |
| handoff QA | Section 13 질문이 다음 하네스가 이어받을 수 있는 근거 source를 가지는지 확인 |

즉 출처 QA의 `source_ref`는 "본문 근거성"이고, handoff QA의 `source_ref`는 "다음 하네스로 이어지는 연결고리"다.

blueprint에는 이 이중 검증이 의도적이라는 점을 한 줄로 명시해야 한다.

## 8. 질문 4: Output schema 합의

### 8.1 부분 구조화 원칙

Industry Primer는 사람이 읽는 prose 산출물이다.
v0에서 전체 JSON/JSONL 산출물을 강제하지 않는다.

다만 downstream handoff와 source tracking을 위해 일부는 구조화한다.

| 산출물 영역 | 처리 |
|---|---|
| Section 1, 3, 5 본문 | markdown prose + table |
| source_register | 구조화된 table 필수 |
| 본문 source_ref | `source_id` 참조 |
| Section 13 handoff 질문 | 구조화된 table 필수 |
| 전체 산출물 | JSON/JSONL 강제 안 함 |

### 8.2 source_register schema

`source_register`는 slice output 내부 필수 섹션이다.
별도 source_register artifact는 first slice에서 만들지 않는다.

최소 필드:

| 필드 | 필수 여부 | 설명 |
|---|---|---|
| `source_id` | 필수 | 본문과 handoff 질문에서 참조할 식별자 |
| `source_type` | 필수 | `source_pack`, `sec`, `company_ir`, `web`, `industry_reference` 등 |
| `title` | 필수 | 사람이 식별할 수 있는 제목 |
| `url_or_path` | 필수 | URL 또는 로컬 파일 경로 |
| `accessed_at` | 웹 source 필수 | 웹 source 확인 날짜 |
| `reliability` | v0 제외 | pilot 이후 후보로만 둠 |

### 8.3 slice output schema

first slice output은 아래를 포함한다.

| 섹션 | 필수 여부 | 비고 |
|---|---|---|
| Section 1. 산업 한 줄 정의 | 필수 | APP/adtech context 포함 |
| Section 3. 산업 참여자 구조 | 필수 | Section 4 하위 시장 맥락은 필요한 만큼만 간략히 포함 |
| Section 5. 핵심 용어 정리 | 필수 | glossary/type-schema 검증 |
| Section 13. 다음 단계로 넘길 질문 | 필수 | handoff QA 검증 |
| source_register | 필수 | 내부 섹션 |

full Industry Primer 14개 섹션 전체 schema는 first slice 이후 확장 후보로 둔다.

## 9. 질문 5: Handoff schema 합의

### 9.1 Step 5~7의 역할

Step 5~7 템플릿은 두 가지 역할을 한다.

| 역할 | 설명 |
|---|---|
| 소극적 경계 | Industry Primer가 Value Chain, Business Model, Market Share 영역 결론을 미리 내지 않게 막는다. |
| 적극적 calibration | Section 13 질문이 실제 후속 분석의 출발점으로 쓸 수 있는지 검산한다. |

그러나 Step 5~7은 Section 13 schema 필드가 아니다.

합의 문구:

```text
Step 5~7은 설계자의 calibration 자료이지, Section 13 산출물 schema의 필드가 아니다.
```

### 9.2 Section 13 v0 필수 필드

Section 13 handoff table은 v0에서 아래 3필드만 필수로 둔다.

| 필드 | 의미 |
|---|---|
| `question` | 후속 하네스가 바로 사용할 수 있을 만큼 구체적인 질문 |
| `source_ref` | 질문의 근거 source |
| `status` | `ready` 또는 `needs_more_source` |

status:

| status | 의미 |
|---|---|
| `ready` | 후속 하네스가 바로 사용할 수 있다. |
| `needs_more_source` | 질문은 유효하지만 추가 source가 필요하다. |

### 9.3 v0에서 제외하는 필드

| 필드 | 처리 | 이유 |
|---|---|---|
| `target_step` | 제외 | downstream 하네스 구조와 과결합될 위험 |
| `why_it_matters` | 제외 | 질문 자체와 rubric 기준으로 처리 |
| `question_type` / `analysis_lens` | 후보만 유지 | 필요성은 관찰하되 v0 필수 아님 |
| `question_id` | 선택 후보 | 질문이 많거나 QA 참조가 필요할 때만 고려 |

### 9.4 target_step 제외 이유

`target_step`은 처음에는 유용해 보인다.

예:

```text
target_step: Value Chain
```

하지만 이 필드를 넣으면 Industry Primer가 downstream 하네스의 내부 라우팅을 알아야 한다.
이는 upstream 산출물이 downstream 구조에 결합되는 결과를 만든다.

따라서 v0에서는 `target_step`을 넣지 않는다.

Step 5~7 이름은 blueprint 논의나 calibration 설명에서는 사용할 수 있다.
하지만 output schema, 필수 산출물 table, rubric 판정 필드에는 넣지 않는다.

### 9.5 why_it_matters 제외 이유

`why_it_matters`는 질문의 중요성을 설명하려는 필드다.
하지만 v0에서는 작성 부담과 중복을 늘린다.

대신 handoff rubric에 아래 기준을 둔다.

```text
질문 자체만 읽어도 다음 분석에서 왜 필요한지 드러나는가?
```

즉 `why_it_matters`는 필드가 아니라 rubric 기준으로 처리한다.

### 9.6 question_type / analysis_lens 후보

`question_type` 또는 `analysis_lens`는 나중에 필요할 수 있다.
다만 v0 필수 필드로 두지 않는다.

필요성이 pilot에서 반복 확인되면 candidate로 기록한다.

주의:

Step 이름 기반 naming은 피한다.

좋지 않은 후보:

```text
value_chain_candidate
business_model_candidate
market_share_candidate
```

더 나은 후보:

```text
structural
definitional
regulatory
monetization
participant_map
source_gap
```

이 후보는 지금 확정하지 않는다.
Pilot observation note Section B 또는 candidate-ledger 후보로 남긴다.

## 10. 질문 6: Module 연결 합의

pilot plan v1의 module tier를 blueprint에서 실제 하네스 구조에 연결한다.

### 10.1 필수 적용

| module | blueprint 반영 |
|---|---|
| `design-preflight` | Industry Primer 유형, 범위, 산출물 역할, 금지 영역을 실행 전 고정 |
| `approval-gate` | top-up, 범위 확장, 하네스 파일 생성, 별도 artifact 추가 전 승인 |
| `qa-scaffold` | 5층 QA와 QA result 구조의 기반 |
| `signal-routing` | QA/observation note 안에서 signal envelope 사용. 별도 signal file 강제 안 함 |
| `pilot-first / testing` | preflight -> slice -> QA -> observation -> blueprint/full pilot 순서 유지 |

### 10.2 기본 / 좁게 / 가볍게 적용

| module | blueprint 반영 |
|---|---|
| `security-baseline` | 웹자료, 로컬 파일, checkpoint, 비공개 대화 원문 안전선 |
| `type-schema` | source_register, source_ref, handoff table, status 값의 최소 구조 |
| `observability` | Pilot observation note Section A/B를 통해 병목과 module fit 기록 |
| `docs-organization` | 산출물 위치, README/색인, 새 폴더 과잉 생성 방지 |

### 10.3 조건부 / 관찰 적용

| module | blueprint 반영 |
|---|---|
| `comparison` | 기본 비활성. QA 판정이 애매하거나 해석 차이가 큰 경우 조건부 발동 |
| `checkpoint` | 세션 전환, context 압축 위험, 사용자 저장 요청 시만 사용 |
| `candidate-ledger` | 별도 ledger 파일 없음. Pilot observation note Section B에 후보만 기록 |

### 10.4 module 연결의 핵심 원칙

모든 module을 처음부터 강하게 켜지 않는다.
첫 slice 목적은 Industry Primer 하네스의 최소 구조가 작동하는지 보는 것이다.

따라서 blueprint는 module을 "적용 여부"뿐 아니라 "적용 두께"로 설계해야 한다.

```text
필수 적용
기본 적용
좁게 적용
가볍게 적용
조건부 적용
관찰만
```

이 tier 구조는 pilot plan v1 Section 9와 일치해야 한다.

## 11. 논쟁 지점과 최종 합의

### 11.1 QA schema 별도 파일

논쟁:

Source Pack은 QA 절차 안에 output format을 인라인으로 포함했다.
Industry Primer도 같은 방식으로 갈 수 있는가?

결론:

Industry Primer는 판단형 QA가 복잡하므로 `industry-primer-qa.schema.md`를 별도 후보 파일로 둔다.

이유:

- procedure는 실행 순서다.
- rubric은 판단 기준이다.
- schema는 결과 기록 형식이다.
- 셋을 분리해야 나중에 각각 독립적으로 수정할 수 있다.

### 11.2 target_step 제외

논쟁:

Section 13 질문에 `target_step`을 두면 다음 단계와 연결이 쉬워 보인다.

결론:

v0 schema에서 제외한다.

이유:

- Industry Primer가 downstream 하네스 구조에 결합된다.
- Step 5~7 구조가 바뀌면 Industry Primer schema도 흔들린다.
- Industry Primer의 역할은 질문을 남기는 것이지 배달 경로를 지정하는 것이 아니다.

### 11.3 why_it_matters 제외

논쟁:

질문의 중요성을 별도 필드로 설명하면 더 친절할 수 있다.

결론:

v0 schema에서 제외한다.

이유:

- 좋은 handoff 질문은 질문 자체에서 중요성이 드러나야 한다.
- 필드가 늘어나면 작성 부담이 커진다.
- rubric 기준으로 충분히 처리 가능하다.

### 11.4 schema 기준과 rubric 기준 분리

논쟁:

handoff blocked 기준에 "라우팅 필드에 의존한다"는 표현을 넣을 수 있는가?

결론:

그 표현은 schema/structure QA 기준에 가깝다.
handoff rubric에서는 내용 기준으로 표현한다.

정리:

| 층 | 처리 |
|---|---|
| schema/structure QA | Section 13에 허용되지 않은 필드가 들어갔는지 확인 |
| handoff rubric | 질문 내용이 특정 후속 단계로의 배분 방향을 포함하는지 확인 |

### 11.5 source_ref 이중 검증

논쟁:

`source_ref`가 출처 QA와 handoff QA 양쪽에 들어가면 중복 아닌가?

결론:

의도적 방어적 중복이다.

이유:

- 출처 QA는 본문 전체의 근거성을 본다.
- handoff QA는 다음 하네스가 질문을 이어받을 수 있는 근거 연결을 본다.

blueprint에 이 의도를 명시한다.

## 12. Blueprint 작성 시 반드시 반영할 체크포인트

blueprint 초안에는 아래가 빠지면 안 된다.

| 체크포인트 | 확인 |
|---|---|
| 하네스 구조 | contract, procedure, schema, rubric, output, adapter 경계가 있다. |
| QA 3파일 분리 | `procedure`, `rubric`, `schema`가 역할 분리되어 있다. |
| QA schema 별도 파일 | `industry-primer-qa.schema.md` 후보가 명시되어 있다. |
| 판단형 rubric | pass / adjustments / blocked의 경계가 서술형으로 정의된다. |
| QA output format | `qa.md` 8섹션 구조가 있다. |
| 부분 구조화 | 전체 JSON 강제 없이 source_register와 Section 13만 구조화한다. |
| source_register | 필수 4필드 + 웹 `accessed_at` + `reliability` v0 제외가 반영된다. |
| Section 13 | `question`, `source_ref`, `status`만 v0 필수 필드다. |
| target_step 제외 | schema 필드로 넣지 않는다. |
| why_it_matters 제외 | schema 필드로 넣지 않고 rubric 기준으로 처리한다. |
| Step 5~7 | calibration 자료이지 schema 필드가 아니다. |
| handoff rubric | 특정 후속 단계로의 배분 방향을 질문에 포함하면 blocked로 본다. |
| source_ref 이중 확인 | 출처 QA와 handoff QA에서 의도적으로 모두 확인한다. |
| module tier | pilot plan v1 Section 9와 일치한다. |
| 실제 파일 생성 금지 | blueprint 승인 전 하네스 파일을 만들지 않는다. |

## 13. Claude Code 교차검증 요청 기준

Claude Code에는 이 문서에 대해 아래를 확인하게 한다.

| 검증 항목 | 확인 질문 |
|---|---|
| 단계 맥락 | 이 문서가 Phase 7-B work map의 `blueprint 논의: blueprint-prep note Section 11 기준 6개 질문 커버`에 대응하는가 |
| Section 11 대응 | 6개 질문이 모두 다뤄졌는가 |
| 하네스 구조 | contracts/procedures/schemas/rubrics/outputs/adapter 경계가 적절한가 |
| QA 3파일 분리 | procedure/rubric/schema 분리가 타당한가 |
| QA schema 별도 파일 | `industry-primer-qa.schema.md` 분리 결정이 타당한가 |
| 판단형 rubric | 체크리스트가 아니라 판단형 rubric으로 정리됐는가 |
| QA output format | `qa.md` 8섹션 구조가 적절한가 |
| output schema | 부분 구조화 원칙이 pilot plan v1과 일치하는가 |
| Section 13 schema | `question`, `source_ref`, `status` 3필드가 v0 필수로 충분한가 |
| 제외 필드 | `target_step`, `why_it_matters` 제외 이유가 타당한가 |
| Step 5~7 역할 | calibration 자료와 schema 필드의 경계가 명확한가 |
| source_ref 이중 확인 | 출처 QA와 handoff QA의 목적 차이가 설명됐는가 |
| module 연결 | pilot plan v1의 module tier와 충돌하지 않는가 |
| 다음 단계 | 이 문서를 기준으로 blueprint 초안 작성으로 넘어가도 되는가 |

## 14. 다음 작업

권장 순서:

1. 이 문서를 Claude Code에 교차검증한다.
2. 이견이 있으면 이 문서를 수정한다.
3. Claude Code PASS 후 작업 지도에 이 문서를 반영한다.
4. Phase 7-B의 `blueprint 논의: blueprint-prep note Section 11 기준 6개 질문 커버` 항목을 `done`으로 전환한다.
5. 이 문서를 직접 입력으로 삼아 Industry Primer blueprint 초안을 작성한다.

현재 상태:

```text
Claude Code 교차검증 PASS
```
