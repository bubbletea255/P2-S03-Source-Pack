# Global Harness v5 Phase 7-B Industry Primer Pre-Build Risk Review Note

- 작성일: 2026-06-09
- 상태: Claude Code 교차검증 PASS
- 단계: Phase 7-B Industry Primer first slice pilot
- 대상 blueprint: `global-harness-v5-phase7b-industry-primer-blueprint-v0-2026-06-09.md`
- 문서 성격: 실제 하네스 파일 생성 전 첫 공식 challenge review 기록

이 문서는 새 설계 국면을 여는 문서가 아니다.

이 문서의 목적은 Industry Primer blueprint v0를 실제 `harness/`, `.agents/`, `.claude/`, `artifacts/` 파일로 만들기 전에, blueprint v0에 최소 반영해야 할 위험 항목을 확정하는 것이다.

## 1. 기준 문서와 우선순위

| 우선순위 | 문서 | 역할 |
|---|---|---|
| 1 | `global-harness-v5-work-map.md` | 현재 Phase 7-B 위치, 승인 게이트, 후속 항목 확인 |
| 2 | `global-harness-v5-phase7b-industry-primer-blueprint-v0-2026-06-09.md` | 실제 하네스 파일 생성 전 설계도 |
| 3 | `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v1-2026-06-08.md` | APP/adtech first slice pilot 실행 계획의 최신 source of truth |
| 4 | `global-harness-v5-phase7b-industry-primer-first-slice-rubric-calibration-note-2026-06-09.md` | Section 1/3/5/13 실제 내용 기준과 full rubric expansion 추적 기준 |

이 note는 위 문서의 결론을 뒤집는 것이 아니라, 실제 build 직전에 드러난 위험을 최소 수정 범위로 반영하기 위한 bridge 문서다.

## 2. 목적과 비범위

### 2.1 목적

이 note는 아래 질문에만 답한다.

```text
blueprint v0에서 실제 파일 생성 전에 무엇을 최소로 바꿔야 하는가?
```

구체 목적:

- Codex / Claude Code / 사용자 3자 논의에서 발견한 구조적 위험을 기록한다.
- conformance review와 challenge review를 분리한다.
- blueprint v0에 즉시 반영할 항목과 pilot 후 backlog/trigger로 둘 항목을 분리한다.
- 실제 하네스 파일 생성 승인 게이트 전에 사용자와 AI가 확인해야 할 위험을 드러낸다.

### 2.2 비범위

이 note에서는 아래를 새로 설계하지 않는다.

| 비범위 | 이유 |
|---|---|
| 가치투자 21단계 전체 재설계 | 현재 논의 범위를 벗어나며, 반복 적용 결과로 따로 검토해야 함 |
| v5 module 구조 재설계 | module 구조는 이미 Phase 5-7에서 검증했고, 이 note는 build 직전 위험만 다룸 |
| Source Pack 확장 설계 | transcript, 경쟁사 SEC, 산업 전문 매체 수집은 별도 expansion 후보 |
| JSONL migration 설계 | 현재는 병목 trigger만 정의하고, DB/SQLite 전환 설계는 하지 않음 |
| full Industry Primer rubric/QA 완성 | first slice pilot 완료 후 full pilot 진입 전 별도 작업으로 추적 |

## 3. 이 note가 challenge review인 이유

지금까지의 Claude Code / Codex 교차검증은 주로 conformance review였다.

| review 유형 | 질문 | 예시 |
|---|---|---|
| conformance review | 합의한 설계를 제대로 반영했는가? | blueprint v0가 pilot plan v1, calibration note, Section 11 consensus와 일치하는가 |
| challenge review | 설계 자체가 틀렸거나 과하거나 위험하지 않은가? | 구조는 통과해도 투자 분석 품질이 낮을 수 있지 않은가 |

이번 논의는 첫 공식 challenge review다.

이 논의에서는 blueprint v0가 합의 문서를 잘 반영했는지보다, 아래 질문을 물었다.

- 이 구조가 실제 투자 리서치에 위험한 거짓 확신을 줄 수 있는가?
- Codex / Claude Code 검증 루프가 자기검증에 머물렀는가?
- 사용자가 요청한 회사에 대해 긍정 편향을 강화할 수 있는가?
- source를 붙였지만 source를 잘못 해석한 경우를 어떻게 드러낼 것인가?
- 실제 하네스 파일 생성 전 최소 수정해야 할 항목은 무엇인가?

## 4. 발견한 핵심 위험

### 4.1 자기검증 편향

현재 검증 루프는 아래처럼 흐르기 쉽다.

```text
합의된 설계 작성
-> Codex가 문서화
-> Claude Code가 합의 대비 반영 여부 검토
-> PASS
```

이 흐름은 유용하지만, 설계 자체가 맞는지는 충분히 묻지 않는다.

따라서 앞으로는 아래를 분리한다.

- conformance review: 합의 대비 구현 일치 여부 확인
- challenge review: 설계 자체의 위험, 과잉, 누락, 편향 확인

### 4.2 구조 품질과 투자 분석 품질의 차이

현재 5층 QA는 아래를 잘 본다.

- 필수 섹션이 있는가
- `source_ref`가 있는가
- 금지 영역을 넘지 않았는가
- fact / interpretation / 확인 필요가 분리됐는가
- Section 13 handoff 질문이 구체적인가

하지만 아래를 자동으로 보장하지는 않는다.

- 산업 구조 설명이 실제로 맞는가
- source를 올바르게 해석했는가
- 분석이 투자 판단에 유용한 현실 이해를 제공하는가
- 구조적으로 완성된 문서가 잘못된 확신을 주지 않는가

구조적으로 완벽하지만 내용이 틀린 Industry Primer는 없는 것보다 위험할 수 있다.

### 4.3 LLM hallucination / source 오해석 위험

LLM은 source를 붙인 채로도 source를 잘못 해석할 수 있다.

위험한 실패 모드:

```text
source_ref 있음
+ 섹션 구조 완성
+ 금지 영역 미침범
+ 문장 논리 자연스러움
= 5층 QA 통과 가능

하지만 source 해석 자체가 틀림
```

따라서 "source가 있는가"와 "source를 올바르게 해석했는가"는 분리해서 다뤄야 한다.

단, first slice v0에서 source interpretation QA를 LLM 자동검증으로 완전히 해결한다고 가정하면 안 된다.
현실적인 방어선은 고위험 claim을 드러내고, 사용자가 원문/source와 도메인 지식으로 확인할 수 있게 만드는 것이다.

### 4.4 company bias / anti-cheerleading 필요성

Industry Primer는 사용자가 관심을 가진 회사를 분석 대상으로 삼는다.
하지만 "관심 있는 회사"는 "투자할 만한 회사"와 다르다.

위험:

- 요청된 회사를 좋게 설명하려는 편향
- 회사의 공식 narrative를 산업 구조 설명처럼 받아들이는 편향
- target company context가 산업 이해를 돕는 수준을 넘어 투자 thesis를 정당화하는 방향으로 흐르는 편향

Industry Primer는 투자 후보를 정당화하는 문서가 아니다.
투자하지 말아야 할 회사를 걸러내기 위해서도 같은 냉정함으로 작성되어야 한다.

### 4.5 source quality 통제 필요성

웹 source를 허용하면 출처 품질 차이가 커진다.

`source_register`는 source를 기록하지만, source의 질을 자동으로 보장하지 않는다.
따라서 source quality tier가 필요하다.

특히 익명 블로그, 주식 토론방, 커뮤니티 댓글, AI 요약문 같은 source는 투자 분석의 핵심 근거로 쓰면 위험하다.

### 4.6 시스템 복잡성

현재 v5 template 후보는 많은 module과 note를 가진다.
이 구조는 안전장치를 제공하지만, 단일 비개발자 운영자에게 부담이 될 수 있다.

위험:

- 문서가 많아져 실제 실행자가 어떤 문서를 읽어야 하는지 헷갈림
- module tier와 approval/signal/QA 경계가 과하게 복잡해짐
- 실제 투자 판단보다 시스템 관리가 더 큰 일이 됨

first slice pilot은 이 복잡성이 실제 운영 가능한지 확인하는 첫 테스트다.

### 4.7 21단계 프로세스는 아직 검증 중인 가설

가치투자 21단계는 현재 작업의 상위 구조지만, 아직 실전 검증이 끝난 확정 체계가 아니다.

따라서 아래처럼 기록한다.

```text
가치투자 21단계는 현재 검증 중인 투자 리서치 프로세스 가설이다.
검증 기준은 적용 횟수 자체가 아니라, 반복 적용 과정에서 사용자가 실제 투자 판단에 실질적으로 도움이 됐다고 판단하는 사례와 패턴이 확인되는가이다.
```

수량은 보조 지표일 수 있지만, 검증의 중심은 실제 투자 판단 기여도다.

### 4.8 Source Pack 범위 한계

현재 Source Pack은 SEC/IR 중심이다.

Industry Primer와 이후 단계에서는 아래 source가 중요해질 수 있다.

- earnings call transcript
- 경쟁사 SEC filings
- 산업 전문 매체
- 산업 리포트 / 컨설팅 리포트
- multi-year 자료

다만 first slice에서는 이것을 blocking dependency로 보지 않는다.
부족분은 source gap, top-up 후보, full pilot expansion 후보로 기록한다.

### 4.9 JSONL 확장성 한계

현재 JSONL catalog는 초기 Source Pack에는 충분하다.
하지만 10-50개 기업, 수백-수천 문서, 기업 간 비교 쿼리로 확장되면 병목이 생길 수 있다.

지금 DB/SQLite migration을 설계하지 않는다.
대신 migration trigger만 backlog로 둔다.

## 5. 즉시 blueprint v0에 반영해야 할 항목

아래 항목은 실제 하네스 파일 생성 전 blueprint v0에 최소 반영해야 한다.

| 항목 | 반영 위치 후보 | 이유 |
|---|---|---|
| 중립성 / anti-cheerleading 원칙 | contract, rubric, slice procedure | 요청된 회사를 좋게 포장하는 편향 방지 |
| source quality tier | contract, slice procedure, source_register guidance | 웹/외부 source 품질 drift 방지 |
| source interpretation risk 처리 | rubric, QA output format | source_ref만으로는 source 해석 정확성을 보장할 수 없음 |
| User Review Required Claims | QA output format 또는 Findings 하위 섹션, runbook 승인 단계 | 고위험 claim을 사용자 승인 게이트 전에 드러냄 |
| Challenge review 실행 순서와 타이밍 | runbook, QA procedure, approval-gate 연결 | 사용자가 산출물 승인 전에 challenge reviewer 역할을 수행하게 함 |
| 21단계 가설 문구 | contract 또는 blueprint note | 상위 프로세스가 아직 검증 중임을 명시 |

이 항목은 새 module 설계가 아니다.
blueprint v0의 contract/procedure/rubric/QA output에 최소 문구와 section을 보강하는 수준으로 반영한다.

## 6. User Review Required Claims 설계 원칙

### 6.1 성격

`User Review Required Claims`는 QA 상태값이 아니다.

아래 상태값 체계는 유지한다.

```text
pass
pass with adjustments
blocked
```

`User Review Required Claims`는 사용자 승인 게이트의 입력 정보다.

역할:

- AI가 source 해석 위험이 큰 claim을 사용자에게 드러낸다.
- 사용자가 해당 claim을 원문/source와 자기 도메인 지식으로 확인한다.
- 사용자는 그 결과를 바탕으로 승인, 보완 요청, blocked 판단을 한다.

### 6.2 건수 제한

`User Review Required Claims`는 최대 5개만 표시한다.

건수 제한 이유:

- 너무 많으면 사용자가 실제로 검토하지 못한다.
- 모든 claim을 표시하면 우선순위 신호가 사라진다.
- first slice에서는 가장 위험한 claim만 사용자에게 올리는 것이 목적이다.

### 6.3 우선순위 기준

아래 순서로 우선순위를 둔다.

| 우선순위 | 기준 | 설명 |
|---|---|---|
| 1 | 후속 분석을 크게 오염시킬 수 있는 claim | Value Chain, Business Model, Market Share 등 다음 하네스의 전제를 흔들 수 있음 |
| 2 | 단순 fact보다 해석이 들어간 claim | source 내용을 요약하는 수준이 아니라 구조적 의미를 부여한 문장 |
| 3 | 사용자 지식과 충돌 가능성이 있는 claim | APP/adtech에 대해 사용자가 이미 아는 내용과 다를 수 있는 주장 |
| 4 | 회사 공식 narrative를 그대로 산업 사실처럼 사용한 claim | company bias 가능성이 있음 |
| 5 | source quality가 낮거나 간접적인 claim | preferred source가 아닌 자료에 의존한 주장 |

### 6.4 자동 blocked가 아님

`User Review Required Claims`가 있다는 사실만으로 자동 blocked가 되지는 않는다.

판단 방식:

| 상황 | 처리 |
|---|---|
| claim이 중요하지만 사용자가 확인 가능 | 사용자 승인 게이트 입력으로 표시 |
| 사용자가 확인 후 맞다고 판단 | 승인 가능 |
| 사용자가 확인 후 틀렸거나 과장됐다고 판단 | 보완 요청 또는 blocked |
| claim이 너무 핵심이고 확인 없이는 안전한 handoff가 불가능 | 사용자가 blocked로 결정 가능 |

즉 이 섹션은 자동 차단 장치가 아니라 승인 전 검토 장치다.

### 6.5 권장 기록 형식

`qa.md` 또는 equivalent QA result 안에 아래와 같은 하위 섹션을 둔다.

```md
## User Review Required Claims

| priority | claim | source_ref | 왜 직접 확인이 필요한가 | 확인할 원문/source 위치 |
|---|---|---|---|---|
| 1 | ... | src-001 | 산업 구조 핵심 해석 / 사용자 지식과 충돌 가능성 | ... |
```

이 section은 새 QA status를 만들지 않는다.
필요하면 기존 `Findings` 또는 `Next action` 안의 하위 섹션으로 구현할 수 있다.

## 7. Challenge review 실행 순서

Challenge review는 산출물 승인 이전에 수행한다.

권장 실행 순서:

| 순서 | 수행자 | 작업 |
|---|---|---|
| 1 | AI adapter | Industry Primer first slice 작성 |
| 2 | AI adapter | 5층 QA 수행 |
| 3 | AI adapter | `User Review Required Claims` 최대 5개 표시 |
| 4 | 사용자 | 해당 claim을 원문/source와 자기 도메인 지식으로 검토 |
| 5 | 사용자 | 승인 / 보완 요청 / blocked 판단 |

이 순서가 없으면 challenge review는 사후 감상이나 회고가 될 위험이 있다.

APP/adtech first slice에서는 사용자가 APP과 adtech에 대해 가진 지식이 중요한 challenge review 자료다.
AI는 고위험 claim을 표면화하고, 사용자는 핵심 claim의 현실 정확성을 최종 검토한다.

## 8. Source Quality Tier 초안

first slice v0에서 source quality를 너무 무겁게 설계하지 않는다.
하지만 source type별 사용 원칙은 명시해야 한다.

| tier | source 유형 | 사용 원칙 |
|---|---|---|
| Preferred | SEC filings, 회사 공식 IR, 정부/규제기관 자료, 공식 통계 | 핵심 fact와 회사 공식 context의 1순위 근거 |
| Strong external | 신뢰 가능한 산업 리포트, 컨설팅/리서치 회사 자료, 주요 금융/산업 매체 | 산업 구조, 외부 시야, 용어/정책 맥락 보강 |
| Context only | Wikipedia, 일반 블로그, 커뮤니티, 비전문 매체 | 배경 이해에는 참고 가능하나 핵심 판단 근거로 사용하지 않음 |
| Disallowed as evidence | AI 요약문, 출처 없는 댓글, 익명성 강한 주장, 홍보성/근거 불명 자료 | source_register의 핵심 근거로 사용하지 않음 |

참고:

- 사용자가 명시적으로 커뮤니티 반응이나 시장 심리를 요청한 경우에는 별도 승인/범위 확인 후 제한적으로 볼 수 있다.
- 그런 경우에도 투자 판단의 핵심 근거로 승격하지 않는다.
- 웹 source는 title, url, source_type, accessed_at를 기록해야 한다.

## 9. Company Bias / Anti-Cheerleading 원칙

Industry Primer는 target company를 좋게 말하기 위한 문서가 아니다.

원칙:

- target company context는 산업 구조 안에서 회사가 어디에 위치하는지 설명하는 데만 사용한다.
- target company의 장점, moat, 경쟁우위, valuation, 투자 매력 판단은 금지 영역이다.
- 회사 공식 IR narrative는 회사 관점 source로 취급하고, 산업 사실로 바로 승격하지 않는다.
- 투자하지 말아야 할 회사를 걸러낼 때도 같은 Industry Primer 구조를 사용할 수 있어야 한다.
- "요청된 회사"와 "좋은 회사"를 구분한다.

blueprint v0에는 이 원칙을 contract/rubric에 명시해야 한다.

## 10. First Slice 이후 검증 질문

first slice 실행 후 아래 질문을 반드시 묻는다.

| 질문 | 목적 |
|---|---|
| 후속 분석을 위한 좋은 질문을 만드는가? | Industry Primer가 Value Chain/Business Model/Market Share 등 후속 하네스의 출발점으로 기능하는지 확인 |
| 사용자가 알고 있는 APP/adtech 현실과 얼마나 일치하는가? | AI의 현실 이해와 source 해석 정확성 확인 |
| source 해석 오류나 company bias가 발견됐는가? | structure QA를 통과한 분석 품질 오류 포착 |
| 구조는 작동했지만 분석 품질이 낮은 지점은 무엇인가? | full rubric/QA expansion 전 보강 후보 수집 |

first slice의 성공 기준은 투자 결론을 내는 것이 아니다.
성공 기준은 후속 분석을 더 안전하고 날카롭게 시작할 수 있는가이다.

## 11. 지금 수정하지 않고 backlog/trigger로 둘 항목

아래 항목은 중요하지만, 실제 하네스 파일 생성 전 즉시 수정 범위에는 넣지 않는다.

| 항목 | 지금 처리 | trigger |
|---|---|---|
| JSONL 확장성 | 현 구조 유지, migration trigger만 기록 | catalog 검색/중복검사/기업 간 비교 쿼리 병목 발생 |
| Source Pack 범위 확장 | first slice blocker 아님, expansion 후보 | transcript, 경쟁사 SEC, 산업 전문 매체가 반복적으로 필요 |
| 21단계 전체 프로세스 재검토 | 현재는 검증 중인 가설로 기록 | 반복 적용 후 투자 판단 기여도가 낮거나 특정 단계가 계속 무용/과중 |
| full Industry Primer rubric/QA expansion | 작업 지도에 이미 후속 todo로 추적 | first slice pilot 완료 후 full pilot 진입 전 |

이 항목을 pre-build 단계에서 열면 실제 파일 생성 전 설계 범위가 다시 폭발한다.
따라서 이 note에서는 trigger와 추적 위치만 남긴다.

## 12. Blueprint v0 최소 수정 후보

Claude Code 교차검증 후 확정되면 blueprint v0에 아래를 최소 반영한다.

| 수정 후보 | blueprint 반영 위치 후보 | 반영 방식 |
|---|---|---|
| 중립성 / anti-cheerleading | Section 2 범위, Section 4 contract, Section 8 rubric | target company를 좋게 포장하지 않는 원칙 추가 |
| source quality tier | Section 5 procedure, Section 6 schema/source_register, Section 8 rubric | tier 표와 사용 원칙 추가 |
| source interpretation risk | Section 8 rubric, Section 7 QA output format | source_ref만으로 충분하지 않으며 high-risk claim은 user review로 표시 |
| User Review Required Claims | Section 7 QA output format, Section 12 approval/stop, Section 13 output | 최대 5개, 승인 게이트 입력 정보로 추가 |
| Challenge review timing | Section 5 runbook/procedure, Section 12 approval/stop | 산출물 승인 전 사용자 검토 단계 추가 |
| 21단계 가설 문구 | Section 2 또는 Section 4 contract | 상위 리서치 프로세스가 검증 중인 가설임을 명시 |

주의:

- `User Review Required Claims`를 새 QA status로 추가하지 않는다.
- 5층 QA를 6층으로 늘리지 않는다.
- full Industry Primer 전체 rubric을 여기서 완성하지 않는다.
- Source Pack expansion이나 JSONL migration을 blueprint v0 수정으로 끌어오지 않는다.

## 13. Claude Code 교차검증 요청 기준

Claude Code에게 아래를 확인 요청한다.

| 검증 항목 | 질문 |
|---|---|
| 핵심 위험 누락 여부 | 자기검증 편향, 구조 품질/분석 품질 차이, hallucination/source 오해석, company bias, source quality, 시스템 복잡성, 21단계 가설, Source Pack 범위, JSONL 한계가 빠짐없이 기록됐는가 |
| 범위 통제 | 이 note가 새 설계 국면으로 확장되지 않고 blueprint v0 최소 수정사항 확정에 머무르는가 |
| 즉시 반영 / backlog 분리 | blueprint에 바로 반영할 항목과 pilot 후 trigger/backlog 항목이 섞이지 않았는가 |
| User Review Required Claims | 새 QA 상태값처럼 표현되지 않고 사용자 승인 게이트 입력 정보로 표현됐는가 |
| 건수 제한 | User Review Required Claims가 최대 5개, 우선순위 순으로 제한됐는가 |
| Challenge review 타이밍 | 산출물 승인 이전에 수행한다고 명시됐는가 |
| 21단계 가설 | 검증 기준이 단순 적용 횟수가 아니라 실제 투자 판단 기여도 중심으로 표현됐는가 |
| source quality tier | Preferred / Strong external / Context only / Disallowed as evidence 구분이 실무적으로 맞는가 |
| 다음 단계 | note PASS 후 blueprint v0 최소 수정으로 이어지는가 |

## 14. 현재 판정

현재 판정:

```text
pre-build risk review note 작성 및 Claude Code 교차검증 PASS 완료.
실제 하네스 파일 생성 전이며, 이 PASS는 실제 파일 생성 승인이 아니라 blueprint v0 최소 수정 항목이 확정됐다는 의미이다.
```

다음 단계:

1. 작업 지도와 README에 이 note를 기준 문서로 반영한다.
2. note 기준으로 blueprint v0 최소 수정사항을 반영한다.
3. 수정된 blueprint v0를 다시 검증한 뒤 사용자 승인 게이트로 이동한다.
