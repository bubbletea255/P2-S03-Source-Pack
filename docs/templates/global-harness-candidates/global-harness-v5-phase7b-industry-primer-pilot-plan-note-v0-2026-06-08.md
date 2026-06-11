# Global Harness v5 Phase 7-B Industry Primer Pilot Plan Note v0

- 작성일: 2026-06-08
- 상태: 살아있는 pilot plan note v0. 질문과 답변, 합의를 누적하는 중.
- 범위: Phase 7-B `Industry Primer` 하네스 실전 검증을 위한 pilot plan 논의판
- 최종화 원칙: 질문별 합의가 닫히면 별도 `v1` pilot plan note를 새로 만든다.
- 기준 문서:
  - `global-harness-v5-work-map.md`
  - `global-harness-v5-phase7-planning-consensus-note-2026-06-07.md`
  - `global-harness-v5-phase7b-industry-primer-design-principles-note-2026-06-08.md`
  - `docs/reference/가치투자 리서치 21단계 구조화 버전.md`
  - `docs/reference/Phase 2 - Step 4 Industry Primer 템플릿.md`
  - `artifacts/companies/APP/index.md`
  - `artifacts/runs/run-20260604-app-ir-pilot/qa.md`
  - `artifacts/runs/run-20260605-app-sec-ir-overlap-recheck/run-summary.md`

## 1. 목적

이 문서는 Phase 7-B `Industry Primer` 실전 검증을 바로 실행하기 위한 문서가 아니다.

역할은 다음과 같다.

- Industry Primer pilot을 어떻게 설계할지 질문별로 정리한다.
- 사용자, Codex, Claude Code의 논의와 합의를 누적한다.
- pilot plan v1 작성 전에 미정 질문, 결정된 질문, 보류 질문을 분리한다.
- 하네스 파일 생성 전에 입력, 출력, QA, module 선택, 승인 경계를 고정한다.

이 문서는 다음이 아니다.

- Industry Primer 하네스 blueprint
- Industry Primer 실행 runbook
- Industry Primer 최종 산출물
- Source Pack 보강 실행 계획서
- 전역 template bundle 배포 판단 문서

## 2. v0 운영 방식

이 note는 질문별로 업데이트한다.

권장 흐름:

```text
질문 제시
→ 사용자 의견
→ Codex 의견
→ Claude Code 교차검증
→ 합의 또는 보류
→ 이 note의 해당 질문 섹션 업데이트
```

모든 질문이 닫히면 이 v0를 그대로 최종본으로 고치지 않고, 별도 v1을 만든다.

v1 후보 파일명:

```text
global-harness-v5-phase7b-industry-primer-pilot-plan-note-v1-2026-06-08.md
```

v1은 아래 성격을 가진다.

- 질문/논의 과정을 줄이고 결정된 pilot plan만 남긴다.
- Industry Primer harness blueprint로 넘어갈 수 있는 실행 기준 문서가 된다.
- Claude Code 교차검증을 받은 뒤 하네스 구축 단계로 넘긴다.

## 3. 현재 확정된 결정

| 항목 | 현재 결정 | 상태 |
|---|---|---|
| Phase 7-B 대상 | Phase 2 - Step 4 `Industry Primer` | 확정 |
| 하네스 성격 | 산업 이해 / 구조화 / 분석 준비형 | 확정 |
| 첫 pilot 대상 | APP / adtech / mobile advertising | 확정 |
| APP Source Pack 상태 | full collection이 아닌 partial input | 확정 |
| APP 입력 판정 | `Conditional Proceed` | 확정 |
| Source Pack full 보강 선행 여부 | 선행하지 않음 | 확정 |
| Input Readiness Preflight | pilot plan에 포함 | 확정 |
| 실행 주체 | 도구 중립. Codex/Claude/ChatGPT 계열 adapter 가능 | 확정 |
| 산출물 단위 | 산업 중심 + target company context | 확정 |
| v0 산출물 | run 단위 standalone / immutable | 확정 |
| Source Pack + 웹검색 | 둘 다 사용 | 확정 |
| QA 구조 | 구조 / 출처 / 범위 / 판단 / handoff 5층 QA | 확정 |
| security-baseline | 최소 안전선으로 기본 적용 | 확정 |
| candidate-ledger | optional / lightweight | 확정 |
| 하네스 파일 생성 | pilot plan v1과 blueprint 교차검증 전에는 생성하지 않음 | 확정 |

## 4. APP Source Pack input readiness

APP는 pilot 대상으로 적합하지만, 현재 Source Pack은 full 상태가 아니다.

확인된 현재 상태:

| 항목 | 상태 | 근거 |
|---|---|---|
| APP company index | 있음 | `artifacts/companies/APP/index.md` |
| catalog status | `partial` | APP index header |
| collection mode | `partial_recheck` | APP index header |
| Company IR | Q1 2026 earnings release HTML, financial update PDF 2건 | APP index, APP IR pilot QA |
| SEC 자료 | FY2026 Q1 8-K Item 2.02 / EX-99.1 제한 수집 | APP overlap recheck summary |
| 10-K | 없음 / skipped | APP index |
| 10-Q | 없음 / skipped | APP index |
| Proxy | 없음 / skipped | APP index |
| Transcript | 없음 / skipped | APP index |
| Webcast/audio/video | 없음 / skipped | APP index |
| 산업·경쟁 자료 | 없음 / skipped | APP index |
| derived/text | 없음 / skipped | APP QA |

현재 판단:

```text
APP Source Pack은 Industry Primer pilot plan과 slice pilot에는 사용할 수 있다.
다만 full Industry Primer 품질 검증을 위한 충분한 원자료라고 보기는 어렵다.
```

따라서 APP 입력은 `Conditional Proceed`로 둔다.

의미:

- 현재 자료로 pilot plan과 slice pilot을 진행할 수 있다.
- 부족한 산업/경쟁/규제/용어 정보는 웹검색과 외부자료로 보완한다.
- 없는 자료는 추측하지 않고 `확인 필요`, `needs_external_research`, `source_pack_top_up_needed`로 표시한다.
- Source Pack full 보강은 pilot 시작 전에 무조건 선행하지 않는다.

## 5. Source Pack top-up 원칙

Source Pack 보강은 Industry Primer pilot을 막는 선행조건이 아니다.

기본 원칙:

| 상황 | 처리 |
|---|---|
| pilot plan 작성 | APP partial Source Pack으로 진행 |
| preflight | 현재 입력 부족분을 명시 |
| slice pilot | 현재 Source Pack + 웹검색 / 외부자료로 진행 |
| full pilot | 우선 현재 Source Pack + 웹검색으로 진행하되 blocking gap이 있으면 top-up 후보 기록 |
| Source Pack top-up 필요 | 별도 승인 후 별도 작업으로 처리 |

`source_pack_top_up_needed`로 표시할 수 있는 조건:

- 10-K 또는 10-Q 부재 때문에 산업 내 회사 위치를 확인할 수 없다.
- 회사 segment / product / customer 설명이 부족해 Section 12 target company connection을 쓸 수 없다.
- transcript 또는 management commentary 부재가 Business Model 이후 단계의 blocking dependency가 된다.
- 산업/경쟁 자료가 없어 웹검색으로도 핵심 구조를 확인할 수 없다.
- Source Pack catalog와 실제 raw 사이에 불일치가 발견된다.

현재 결정:

```text
APP Source Pack full 보강은 pilot 전에 선행하지 않는다.
필요성은 Input Readiness Preflight와 slice/full pilot 중 관찰한다.
```

## 6. pilot 진행 단계 초안

현재 권장 순서:

| 단계 | 목적 | 산출물 | 상태 |
|---|---|---|---|
| 1. pilot plan v0 | 질문과 합의 누적 | 이 문서 | 진행 중 |
| 2. pilot plan v1 | 결정된 plan 정리 | v1 note | todo |
| 3. Claude Code 교차검증 | plan 누락/과잉 확인 | review | todo |
| 4. Industry Primer blueprint | 실제 하네스 구조 설계 | blueprint note | todo |
| 5. blueprint 교차검증 | 파일 생성 전 승인 게이트 | review | todo |
| 6. harness build | Industry Primer 하네스 파일 작성 | `harness/` 후보 파일 | todo |
| 7. Input Readiness Preflight | 입력 충분성 확인 | preflight note | todo |
| 8. slice pilot | 일부 섹션 실행 | slice output + QA | todo |
| 9. full pilot | 완성형 Industry Primer 작성 | final output + QA | todo |
| 10. evaluation | v5 template 사용성 평가 | evaluation note | todo |

## 7. 논의 질문 보드

| ID | 질문 | 현재 상태 | 현재 답변 / 합의 |
|---|---|---|---|
| Q1 | 첫 slice 범위는 어디까지로 할까? | 합의됨 | Section 1, 3, 5, 13. Section 4 하위 시장 구분은 Section 3에서 간략한 컨텍스트로만 다루고 full pilot에서 검증 |
| Q2 | Source Pack 입력 범위는 어디까지 쓸까? | 합의됨 | APP partial Source Pack을 `Conditional Proceed` 입력으로 사용. 필수 control input은 APP index, 필수 content input은 APP Q1 IR 2건과 FY2026 Q1 8-K / EX-99.1 |
| Q3 | 웹검색 / 외부자료 범위는 어디까지 허용할까? | 합의됨 | 구조/용어/참여자/플랫폼 정책은 허용, 시장 규모는 맥락적 규모만 허용, TAM/CAGR/점유율/경쟁우위/valuation 판단은 제외 |
| Q4 | source_register는 어디에 둘까? | 합의됨 | 첫 slice에서는 slice output 내부 필수 섹션으로 둔다. 별도 artifact는 full pilot 후보 |
| Q5 | slice pilot 산출물 계약은 무엇인가? | 합의됨 | 필수 산출물 3개: Industry Primer slice output, Slice QA result, Pilot observation note |
| Q6 | 어떤 module을 실제로 적용할까? | 합의됨 | 필수/기본/좁게/가볍게/조건부/관찰만 tier로 적용. comparison/checkpoint는 조건부, candidate-ledger는 관찰만 |
| Q7 | QA pass / pass with adjustments / blocked 기준은 무엇인가? | 합의됨 | `pass`, `pass with adjustments`, `blocked` 3단계. 5층 QA 중 하나라도 `blocked`이면 전체 `blocked`, blocked 없이 하나라도 adjustments면 전체 `pass with adjustments` |
| Q8 | comparison mode를 첫 pilot에서 사용할까? | 합의됨 | 조건부 적용. 첫 slice 기본 실행에서는 비활성화하고 결과가 애매하거나 교차검증이 필요할 때만 사용 |
| Q9 | candidate-ledger를 실제로 켤까, 관찰만 할까? | 합의됨 | 별도 ledger 파일 없이 Pilot observation note Section B에 후보만 기록 |
| Q10 | checkpoint를 pilot 중 언제 사용할까? | 합의됨 | 세션 전환, 컨텍스트 압축 위험, 사용자 저장 요청 시에만 사용 |
| Q11 | APP Source Pack top-up trigger는 충분한가? | 합의됨 | Source Pack top-up은 기본값이 아니라 예외. 현재 APP partial Source Pack으로 진행 / top-up 후보 기록 / 실제 top-up 승인 요청 3단계로 구분 |
| Q12 | pilot plan v1 전환 조건은 무엇인가? | 합의됨 | 실행자가 v1만 읽고 APP/adtech Industry Primer first slice pilot을 수행할 수 있으면 v1로 전환. v1에는 실행 계획만 담고, v0 질문 보드/논의 흔적/진행 메타 문구는 옮기지 않음 |

## 8. Q1. 첫 slice 범위

현재 합의됨.

첫 slice 범위:

| 섹션 | 이유 |
|---|---|
| 1. 산업 한 줄 정의 | 산업의 범위와 target company context를 빠르게 검증 |
| 3. 산업 참여자 구조 | adtech/mobile advertising의 구조 이해에 중요 |
| 5. 핵심 용어 정리 | Industry Primer의 glossary/type-schema 연결 검증 |
| 13. 다음 단계로 넘길 질문 | handoff QA 검증 |

이 조합의 장점:

- Industry Primer의 핵심인 산업 지도, 용어, downstream handoff를 한 번에 검증한다.
- full output보다 비용이 작다.
- Source Pack partial input 상태에서 부족한 부분을 드러내기 쉽다.
- type-schema, qa-scaffold, signal-routing을 모두 작게 시험할 수 있다.

첫 slice에서 제외하는 항목:

| 제외 항목 | 처리 |
|---|---|
| Section 2. 산업이 존재하는 이유 | full pilot에서 검증. 첫 slice에서는 Section 1의 산업 정의와 해결 문제 행에서 일부만 간접 확인 |
| Section 4. 산업 하위 시장 구분 | full pilot에서 검증. 단 adtech에서는 참여자 구조와 하위 시장이 겹치므로 Section 3에서 하위 시장 맥락을 간략히 포함 |
| Section 6. 산업 성장 동인 | full pilot에서 반드시 검증 |
| Section 7. 산업 수익 구조 | full pilot에서 검증 |
| Section 8. 산업 비용 구조 | full pilot에서 검증 |
| Section 9. 규제 / 제도 / 표준 | full pilot에서 반드시 검증 |
| Section 10. 기술 변화 / 구조 변화 | full pilot에서 반드시 검증 |
| Section 11. 산업의 구조적 리스크 | full pilot에서 검증 |
| Section 12. 분석 대상 기업과의 연결 | full pilot에서 검증 |
| Section 14. Industry Primer 결론 | full pilot에서 검증. 첫 slice에서는 별도 결론 대신 observation note로 대체 |

제외 이유:

```text
첫 slice의 목적은 완성형 산업 분석이 아니라 하네스 메커니즘 검증이다.
adtech에서 성장 동인, 규제/플랫폼 정책, 기술 변화는 중요하지만,
첫 slice에 포함하면 범위가 커져 Source Pack partial input과 하네스 구조 검증이 섞일 수 있다.
따라서 첫 slice는 Section 1, 3, 5, 13으로 제한하고,
Section 4 하위 시장 구분은 Section 3에서 APP 위치를 이해하는 데 필요한 만큼만 간략히 다룬다.
규제/제도/표준과 성장/기술 변화는 full pilot에서 반드시 다룬다.
```

## 9. Q2. Source Pack 입력 범위

현재 합의됨.

Q2 합의:

```text
첫 slice는 APP partial Source Pack을 Conditional Proceed 입력으로 사용한다.

필수 control input은 APP company index 또는 이에 준하는 Source Pack handoff summary로 둔다.
이번 APP pilot에서는 artifacts/companies/APP/index.md를 사용한다.

필수 content input은 APP Q1 2026 earnings release, APP Q1 2026 financial update,
FY2026 Q1 8-K / EX-99.1로 둔다.

APP IR pilot QA와 APP SEC-IR overlap recheck run-summary는 필수 slice 입력이 아니라
Input Readiness Preflight에서 partial 상태와 overlap 관계를 검산하는 참조 자료로 둔다.

full 10-K, 10-Q, proxy, transcript, webcast/audio/video 부재는 첫 slice blocking 조건이 아니다.

산업/경쟁 자료 부재도 Source Pack top-up 조건이 아니라 Q3 웹검색/외부자료에서 보완할 대상이다.

누락 자료는 gap 항목으로 기록하되,
정확한 status/signal 어휘와 기록 위치는 Q5 산출물 계약에서 확정한다.

Source Pack은 APP 공식 관점과 자료 한계를 제공하고,
산업 구조 자체는 Q3 웹검색/외부자료에서 보완한다.
```

입력 구분:

| 구분 | 입력 | 처리 |
|---|---|---|
| 필수 control input | `artifacts/companies/APP/index.md` | APP Source Pack 현재 상태, partial 범위, 누락 자료, 다음 하네스 전달 조건 확인 |
| 필수 content input | APP Q1 2026 earnings release HTML | 회사가 직접 설명하는 사업, 제품, 시장 언어 확인 |
| 필수 content input | APP Q1 2026 financial update PDF | 회사 공식 KPI, segment, product/market 표현 확인 |
| 필수 content input | APP FY2026 Q1 8-K / EX-99.1 | SEC canonical 자료로 earnings release 쪽 교차 확인 |
| preflight 참조 | `run-20260604-app-ir-pilot/qa.md` | IR 2건이 catalog/raw에 정상 반영됐는지 검산 |
| preflight 참조 | `run-20260605-app-sec-ir-overlap-recheck/run-summary.md` | SEC-IR overlap 관계와 APP partial 상태 검산 |
| 선택 검증 참조 | catalog JSONL, file metadata, download-log | source_id, file path, provenance 확인이 필요할 때만 참고 |

top-up 후보로만 기록할 입력:

| 입력 | Q2 처리 |
|---|---|
| full 10-K / 10-Q | 첫 slice blocking 조건 아님. full pilot에서 회사 사업 설명, segment, risk 확인이 막히면 top-up 후보 |
| Proxy | Industry Primer slice에는 필요 없음 |
| transcript / webcast / audio / video | Industry Primer blocking dependency 아님. Earnings Call 또는 Business Model 쪽 후보 |
| 산업/경쟁 자료 | Source Pack top-up이 아니라 Q3 웹검색/외부자료에서 처리 |
| derived text | 편의 문제이지 Source Pack 보강 필수 조건 아님 |

Forward note:

```text
blueprint 단계에서 Source Pack handoff summary가 control input으로 인정받기 위한 최소 조건을 정의한다.
```

## 10. Q3. 웹검색 / 외부자료 범위

현재 합의됨.

Q3 합의:

```text
첫 slice에서 웹검색 / 외부자료는 산업 정의, 참여자 구조, 핵심 용어,
APP가 속한 하위 시장 맥락을 확인하는 데 사용한다.

웹검색은 Source Pack을 대체하지 않는다.
Source Pack은 APP 공식 관점과 자료 한계를 제공하고,
웹검색 / 외부자료는 산업 수준의 외부 검증과 용어·구조 보완에 사용한다.

adtech/mobile advertising에서는 ATT, SKAN, IDFA, attribution, Privacy Sandbox 같은
platform policy / privacy 기본 용어를 허용한다.
이들은 첫 slice의 Section 3 참여자 구조와 Section 5 핵심 용어 이해에 필요하기 때문이다.
단, 정책 변화가 APP의 경쟁우위, 수익성, 투자 판단에 주는 결론은 full pilot 이후 단계로 넘긴다.

시장 규모는 맥락적 규모에 한해 허용한다.
정밀 TAM, CAGR, 시장점유율 추정은 Market Share 또는 full pilot 단계로 넘긴다.

경쟁사 우열, 시장점유율 결론, moat, valuation, 투자 판단은 금지한다.

웹 자료는 source_register에 기록해야 하며,
웹 source는 최소한 title, url, source_type, accessed_at을 남긴다.
정확한 source_register 위치와 필드 구조는 Q4/Q5에서 확정한다.
```

웹/외부자료 범위:

| 범위 | Q3 처리 | 이유 |
|---|---|---|
| adtech / mobile advertising / app monetization 산업 정의 | 허용 | Section 1 산업 한 줄 정의에 필요 |
| 주요 참여자 유형 | 허용 | Section 3 참여자 구조에 필요 |
| 핵심 용어 | 허용 | Section 5 glossary/type-schema 연결 검증에 필요 |
| APP가 속한 하위 시장 맥락 | 허용 | Section 3에서 APP 위치를 간략히 잡기 위해 필요 |
| ATT / SKAN / IDFA / attribution / Privacy Sandbox | 허용 | adtech 산업 구조와 용어 이해에 필수 |
| 시장 규모 / 성장률 | 맥락적 규모만 허용 | 정밀 TAM, CAGR, 시장점유율은 Market Share 또는 full pilot로 넘김 |
| 최신 규제·정책 변화 | 제한적 허용 | 용어와 구조 이해에 필요한 만큼만 사용 |
| 경쟁사 자료 | 제한적 허용 | 참여자 예시나 용어 이해용으로만 사용 |
| 컨설팅 / 리서치 / 블로그 자료 | 제한적 허용 | 산업 구조 파악용 보조 자료. 핵심 fact는 가능한 원천 자료로 교차 확인 |
| Wikipedia / 일반 설명 자료 | 제한적 허용 | 방향 잡기용. 최종 근거로는 약함 |
| paywall 자료 | 제한적 허용 | 접근 가능한 공개 내용만 사용하고 보이지 않는 내용은 추정 금지 |
| 경쟁사 우열 판단 | 금지 | Competition / Moat 영역 |
| 시장점유율 결론 | 금지 | Market Share 영역 |
| moat / valuation / 투자 판단 | 금지 | Industry Primer 비범위 |
| AI 요약문을 원천 출처로 사용 | 금지 | 실제 source가 아니므로 source_register 원천으로 쓰지 않음 |

Source Pack과 웹검색의 역할:

| 입력 | 역할 |
|---|---|
| Source Pack | APP가 자기 사업과 시장을 어떻게 설명하는지 보여주는 회사 공식 관점 |
| 웹검색 / 외부자료 | APP의 설명이 산업 구조상 어디에 위치하는지 검증하고, 산업 용어와 참여자 구조를 보완 |
| 둘의 관계 | Source Pack은 회사 내부 시야, 웹검색은 산업 외부 시야 |

## 11. Q4. source_register 위치

현재 합의됨.

```text
첫 slice에서는 source_register를 slice output 내부 섹션으로 둔다.
별도 source_register artifact는 만들지 않는다.
full pilot에서 source 수가 많아지거나 기계 재사용 필요성이 생기면 별도 artifact 후보로 기록한다.
```

판단 근거:

| 근거 | 설명 |
|---|---|
| 첫 slice 범위가 작음 | Section 1, 3, 5, 13만 다루므로 source_register를 별도 파일로 분리할 만큼 크지 않다. |
| 사람이 읽는 흐름이 중요함 | Industry Primer는 내용 판단형 산출물이므로 본문과 출처를 같은 문서에서 확인하는 편이 낫다. |
| 별도 artifact는 늦게 결정 가능 | full pilot에서 source가 30개 이상으로 늘거나 여러 산출물이 같은 source_register를 공유하면 분리 후보로 올린다. |

## 12. Q5. slice pilot 산출물 계약

현재 합의됨.

첫 slice의 필수 산출물은 3개로 둔다.

| 산출물 | 필수 여부 | 형태 | 역할 |
|---|---|---|---|
| Industry Primer slice output | 필수 | 별도 파일 | 선택된 Industry Primer 섹션의 실제 초안 |
| source_register | 필수 | slice output 내부 섹션 | Source Pack / 웹 / 외부자료 출처 |
| Slice QA result | 필수 | 별도 파일 | 5층 QA 기준의 통과/보완/차단 판단 |
| Pilot observation note | 필수 | 별도 파일 | Industry Primer 내용 품질 관찰과 v5 template 적용 평가 |
| signal list | 선택 | QA result 또는 observation note 내부 섹션 | 중요한 signal만 기록. 별도 파일은 만들지 않음 |

Industry Primer slice output에 포함할 섹션:

| 섹션 | 처리 |
|---|---|
| Section 1. 산업 한 줄 정의 | 필수 |
| Section 3. 산업 참여자 구조 | 필수. Section 4 하위 시장 맥락을 간략히 포함 |
| Section 5. 핵심 용어 정리 | 필수 |
| Section 13. 다음 단계로 넘길 질문 | 필수 |
| source_register | 내부 필수 섹션 |

Slice QA result는 아래 5층 QA를 포함한다.

| QA 층 | 확인할 것 |
|---|---|
| 구조 QA | 선택한 섹션이 정해진 구조로 작성됐는가 |
| 출처 QA | 핵심 주장에 `source_ref`가 붙었는가 |
| 범위 QA | Value Chain / Business Model / Market Share / Competition / Moat 결론을 미리 내리지 않았는가 |
| 판단 QA | fact / interpretation / 확인 필요가 분리됐는가 |
| handoff QA | 다음 단계 질문이 실제로 쓸 수 있을 만큼 구체적인가 |

Pilot observation note는 두 섹션으로 나누어 작성한다.

| 섹션 | 내용 |
|---|---|
| Section A. Industry Primer 내용 품질 관찰 | source gap, 범위 초과 위험, handoff 품질, 확인 필요 항목 |
| Section B. v5 template 적용 평가 | module fit, 과한 module, 빠진 module, signal-routing/candidate-ledger 후보, 다음 pilot 또는 Phase 7-C 반영 후보 |

signal list 처리:

```text
signal list는 별도 필수 파일로 만들지 않는다.
중요 signal은 QA result 또는 Pilot observation note 내부 섹션에 기록한다.
반복되거나 routing 필요성이 커지면 full pilot 이후 별도 signal artifact 후보로 올린다.
```

## 13. Q6. module 적용

현재 합의됨.

Q6 합의:

```text
첫 Industry Primer slice에서는 필수 module과 lightweight module만 적용한다.

필수 적용:
design-preflight, approval-gate, qa-scaffold, signal-routing, pilot-first/testing

기본 적용:
security-baseline

좁게 적용:
type-schema

가볍게 적용:
observability, docs-organization

조건부 적용:
comparison, checkpoint

관찰만:
candidate-ledger

comparison은 첫 slice 기본 실행에서는 사용하지 않고,
slice 결과가 애매하거나 Claude/Codex 비교가 필요할 때만 적용한다.

candidate-ledger는 별도 ledger 파일을 만들지 않고,
반복 후보를 Pilot observation note Section B에 기록한다.

checkpoint는 세션 전환, 컨텍스트 압축 위험, 사용자 저장 요청이 있을 때만 사용한다.
```

module 적용 tier:

| tier | module | 첫 slice 처리 |
|---|---|---|
| 필수 적용 | `design-preflight` | Industry Primer 유형, slice 범위, 금지 범위, 산출물 역할을 고정한다. |
| 필수 적용 | `approval-gate` | Source Pack top-up, 범위 확장, 하네스 파일 생성, 별도 artifact 추가 전에 승인 경계를 둔다. |
| 필수 적용 | `qa-scaffold` | Q5에서 정한 Slice QA result와 5층 QA를 실행한다. |
| 필수 적용 | `signal-routing` | 별도 signal 파일 없이 QA/observation note 안에서 공통 signal 표현만 사용한다. |
| 필수 적용 | `pilot-first / testing` | preflight -> slice -> full pilot 순서를 지킨다. |
| 기본 적용 | `security-baseline` | 웹자료, 로컬 source 파일, checkpoint를 다루므로 최소 안전선을 적용한다. |
| 좁게 적용 | `type-schema` | source_register 필드, source_ref, 확인 필요, handoff question 구조만 잡는다. |
| 가볍게 적용 | `observability` | Pilot observation note로 병목, 과잉/누락, module fit을 기록한다. |
| 가볍게 적용 | `docs-organization` | 새 폴더 구조를 강제하지 않고 산출물 위치와 README/작업지도 참조만 유지한다. |
| 조건부 적용 | `comparison` | 첫 slice 기본 실행에서는 사용하지 않는다. 결과 품질이 애매하거나 교차검증이 필요할 때만 사용한다. |
| 조건부 적용 | `checkpoint` | 세션 전환, 컨텍스트 압축 위험, 사용자 저장 요청 시에만 사용한다. |
| 관찰만 | `candidate-ledger` | 별도 ledger 파일을 만들지 않고 반복 후보를 Pilot observation note Section B에 기록한다. |

특히 중요한 세부 결정:

| 항목 | 결정 |
|---|---|
| `comparison` | 기본 실행에서는 끄고, QA가 `pass with adjustments`이거나 결과가 애매할 때 후발 적용한다. |
| `candidate-ledger` | 별도 ledger 파일을 만들지 않는다. source type, status/signal, handoff question type, glossary 구조 후보는 observation note Section B에 기록한다. |
| `checkpoint` | 품질 module이 아니라 세션 연속성 도구다. 긴 세션 전환, 컨텍스트 압축 위험, 사용자 요청 시에만 사용한다. |
| `signal-routing` | lightweight 적용. 별도 signal artifact는 만들지 않고 QA result 또는 observation note 내부에 공통 표현으로 기록한다. |
| `approval-gate` | top-up, 범위 확장, 하네스 파일 생성, 별도 artifact 추가 전 승인 조건으로 사용한다. |

추가 module 확장 원칙:

```text
첫 slice에서 모든 module을 전부 켜지 않는다.
먼저 최소 구조가 실제로 작동하는지 확인한다.
full pilot 또는 Phase 7-C에서 필요성이 확인되면 module 적용 수준을 올릴 수 있다.
```

## 14. Q7. QA 완료 판정

현재 합의됨.

Q7의 기준은 "완벽한 산출물인가"가 아니라 "후속 단계로 넘겨도 안전한가"이다.

전체 판정:

| 판정 | 의미 |
|---|---|
| `pass` | 필수 산출물 3개가 있고, 5층 QA에서 치명적 결함이 없으며, downstream 하네스에 넘겨도 안전하다. |
| `pass with adjustments` | 큰 구조는 작동하지만 source gap, 범위 문구, handoff 질문, source_register, 확인 필요 표시를 보완해야 한다. |
| `blocked` | 필수 섹션/source_register 누락, 핵심 주장 출처 부재, 금지 영역 결론 선제 도출, 또는 입력 부족 때문에 산업 정의/참여자/핵심 용어를 판단할 수 없다. |

5층 QA 기준:

금지 영역은 Value Chain, Business Model, Market Share, Competition, Moat, Valuation, 투자 판단을 뜻한다.

| QA 층 | `pass` | `pass with adjustments` | `blocked` |
|---|---|---|---|
| 구조 QA | Section 1, 3, 5, 13과 `source_register`가 정해진 구조로 있다. | 일부 표/필드/문체 정리가 필요하지만 의미는 사용할 수 있다. | 필수 섹션 또는 `source_register`가 없거나, 산출물이 slice 범위와 맞지 않는다. |
| 출처 QA | 핵심 fact와 산업 구조 주장에 `source_ref`가 있고, 웹 source가 `source_register`에 기록됐다. | 일부 보조 주장이나 비핵심 문장의 `source_ref` 정리가 필요하다. | 핵심 주장의 출처가 다수 없거나, `source_register`가 없거나 깨졌거나, 웹 fact를 추적할 수 없다. |
| 범위 QA | Industry Primer 범위 안에 머문다. | 일부 문장이 금지 영역 쪽으로 기울지만 수정 가능. | 금지 영역 결론을 미리 냈거나, Industry Primer가 아닌 후속 단계 분석으로 넘어갔다. |
| 판단 QA | fact, interpretation, 확인 필요가 분리되고 불확실성이 드러난다. | 일부 확인 필요 표시나 interpretation label을 보완해야 한다. | 추측을 fact처럼 쓰거나, 불확실성을 숨기거나, 근거 없는 강한 결론을 낸다. |
| handoff QA | 다음 하네스가 바로 사용할 수 있는 구체적 질문을 남긴다. | 질문은 유용하지만 더 구체화해야 한다. | handoff 질문이 없거나 너무 막연하거나, 후속 단계 결론을 미리 결정한다. |

전체 판정 집계 규칙:

| 조건 | 전체 판정 |
|---|---|
| 5층 QA 중 하나라도 `blocked` | `blocked` |
| `blocked`는 없고 하나라도 `pass with adjustments` | `pass with adjustments` |
| 5층 QA가 모두 `pass` | `pass` |

Source gap 처리:

| 상황 | 처리 |
|---|---|
| 10-K, transcript, proxy 같은 자료가 없지만 slice의 산업 정의/참여자/핵심 용어 판단에는 직접 필요하지 않음 | `pass` 또는 `pass with adjustments` 가능. gap을 명시한다. |
| source gap 때문에 산업 정의, 참여자 구조, 핵심 용어를 판단할 수 없음 | `blocked` |
| APP Source Pack이 partial이라는 사실을 숨기거나 source gap을 표시하지 않음 | `blocked` |

comparison 발동 조건:

| QA 상황 | comparison 처리 |
|---|---|
| 판정이 명확한 `pass` | comparison 불필요 |
| `pass with adjustments`이고 수정 방향이 명확함 | comparison 선택 사항 |
| QA 판정이 애매하거나, 보완 방향이 불명확하거나, `blocked` 원인이 data gap인지 실행 품질인지 구분이 안 됨 | comparison 권장 |
| 범위 위반 여부가 논쟁적이거나, 사용자가 핵심 해석 차이를 직접 판단하기 어려움 | comparison 권장 |

## 15. Q11. Source Pack top-up trigger

현재 합의됨.

Q11의 핵심 원칙:

```text
Industry Primer 첫 slice는 APP partial Source Pack으로 Conditional Proceed 한다.
Source Pack top-up은 기본값이 아니라 예외다.
웹검색, 확인 필요 표시, QA result, Pilot observation note로 처리 가능한 부족분은 top-up하지 않는다.
APP 공식 자료 부족 때문에 Section 1, 3, 5, 13을 안전하게 작성할 수 없을 때만 approval-gate를 거쳐 별도 top-up을 요청한다.
```

역할 분리:

| 항목 | 역할 |
|---|---|
| `approval-gate` | 범위 변경, Source Pack top-up, 하네스 파일 생성, 별도 artifact 추가를 승인할지 판단한다. |
| Source Pack top-up trigger | 자료 부족 때문에 현재 slice를 멈추고 Source Pack 보강을 요청해야 하는지 판단한다. |
| `qa-scaffold` | 자료 부족이 `pass`, `pass with adjustments`, `blocked` 중 어디에 해당하는지 판정한다. |

범위 확장과 top-up의 관계:

```text
scope 확장은 top-up trigger가 아니라 별도 approval-gate 사안이다.
scope 확장이 승인되면 새 범위 기준으로 source 충분성을 다시 평가한다.
그 재평가에서 blocking gap이 발견될 때만 Source Pack top-up 요청으로 이어진다.
```

### 15.1 현재 APP partial Source Pack으로 계속 진행하는 경우

아래 상황에서는 Source Pack을 보강하지 않고 첫 slice를 진행한다.

| 상황 | 처리 |
|---|---|
| APP index와 필수 content input을 읽을 수 있다. | 계속 진행 |
| APP Q1 IR 자료와 8-K / EX-99.1로 APP의 공식 사업 설명을 확인할 수 있다. | 계속 진행 |
| 부족한 정보가 산업 구조, 용어, 참여자, ATT/SKAN 같은 외부 산업 맥락이다. | Q3 웹검색 / 외부자료로 보완 |
| 10-K, proxy, transcript가 없지만 Section 1, 3, 5, 13 작성에는 치명적이지 않다. | gap 명시 후 진행 |
| 일부 source가 더 있으면 좋지만 핵심 정의와 참여자 구조를 쓸 수 있다. | `pass` 또는 `pass with adjustments` 후보로 진행 |
| 확인 필요로 표시하면 downstream에 위험 없이 넘길 수 있다. | 계속 진행 |

### 15.2 top-up 후보로만 기록하는 경우

아래 상황은 실제 보강을 바로 수행하지 않고 QA result 또는 Pilot observation note에 후보로 기록한다.

| 상황 | 처리 |
|---|---|
| 10-K / 10-Q가 있으면 full pilot 품질이 좋아질 것 같다. | top-up 후보 기록 |
| transcript가 있으면 Business Model 또는 Earnings Call 계열에서 유용할 것 같다. | 후속 하네스 후보 기록 |
| proxy가 있으면 governance, incentive, ownership 분석에 유용할 것 같다. | 후속 단계 후보 기록 |
| APP의 segment/product 용어를 더 정확히 잡고 싶지만 현재 slice는 진행 가능하다. | top-up 후보 기록 |
| 같은 종류의 source gap이 반복되어 future Source Pack 개선 후보가 된다. | Pilot observation note Section B에 기록 |
| source gap 때문에 전체 판정이 `pass with adjustments`이지만 `blocked`는 아니다. | 보강 후보로 기록하고 slice는 계속 진행 |

### 15.3 실제 top-up 승인 요청이 필요한 경우

아래 상황에서는 조용히 보강하지 않고 approval-gate를 거쳐 사용자에게 Source Pack top-up을 요청한다.

| trigger | 이유 |
|---|---|
| 필수 control input인 APP index가 없거나 신뢰할 수 없다. | Source Pack 현재 상태와 partial 범위를 확인할 수 없다. |
| 필수 content input인 APP Q1 IR 자료 또는 8-K / EX-99.1이 없거나 열 수 없다. | APP 공식 관점 확인이 불가능하다. |
| APP의 산업 정의, 제품 위치, 참여자 구조가 공식 자료만으로 전혀 잡히지 않는다. | Section 1 또는 Section 3 작성이 불안정하다. |
| 핵심 용어가 APP 공식 자료와 외부자료 사이에서 충돌하고 현재 자료로 해결할 수 없다. | 잘못된 glossary가 downstream으로 넘어갈 위험이 있다. |
| `source_ref`를 붙일 수 없는 핵심 주장이 많다. | 출처 QA가 `blocked`가 된다. |
| 보안 격리, 접근 불가, quarantined source 때문에 필수 자료를 확인할 수 없다. | security-baseline과 approval-gate 확인이 필요하다. |
| 범위 확장 승인 후, 새 범위 기준 source 충분성 재평가에서 blocking gap이 발견된다. | scope 변경 자체가 아니라 새 범위의 자료 부족이 top-up 사유다. |

Source Pack top-up 요청 형식:

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

Q11 결론:

```text
첫 slice 기준으로는 top-up 없이 진행하는 것이 기본값이다.
실제 top-up은 APP 공식 자료 부족 때문에 Section 1, 3, 5, 13을 안전하게 쓸 수 없는 경우에만 발동한다.
자료가 있으면 더 좋다는 이유만으로는 top-up하지 않고 후보로 기록한다.
```

## 16. Q12. pilot plan v1 전환 조건

현재 합의됨.

v1 전환 기준:

```text
pilot plan note v1은 실행자가 v1만 읽고
APP/adtech Industry Primer first slice pilot을 수행할 수 있을 때 작성한다.
```

v1 작성 전 충족 조건:

| 조건 | 의미 |
|---|---|
| Q1~Q12가 모두 `합의됨` 상태다. | 더 이상 미정 또는 부분 합의 질문이 없어야 한다. |
| slice 범위가 고정됐다. | Section 1, 3, 5, 13을 첫 slice로 두고, Section 4 하위 시장 맥락은 Section 3 안에서 간략히 다룬다. |
| 입력 계약이 고정됐다. | APP partial Source Pack, 필수 control/content input, 웹검색/외부자료 범위가 확정됐다. |
| 산출물 계약이 고정됐다. | Industry Primer slice output, Slice QA result, Pilot observation note 3개가 필수 산출물이다. |
| QA 기준이 고정됐다. | `pass`, `pass with adjustments`, `blocked`, 5층 QA, comparison 발동 조건이 확정됐다. |
| module 적용 tier가 고정됐다. | 필수/기본/좁게/가볍게/조건부/관찰만 적용 기준이 확정됐다. |
| Source Pack top-up trigger가 고정됐다. | 계속 진행, 후보 기록, 실제 top-up 승인 요청의 3단계가 확정됐다. |
| v1에 넣을 내용과 뺄 내용이 구분됐다. | v1은 논의 기록이 아니라 실행 계획서로 작성한다. |

v1에 포함할 내용:

| 포함 항목 | 내용 |
|---|---|
| 목적 | APP/adtech Industry Primer first slice pilot의 목적 |
| 전제 | APP Source Pack은 `Conditional Proceed` 입력으로 사용 |
| 범위 | 첫 slice 섹션, 제외 섹션, 금지 영역 |
| 입력 | APP index, APP IR/SEC 필수 content input, 웹검색/외부자료 허용 범위 |
| 산출물 | slice output, QA result, Pilot observation note, 내부 `source_register` |
| module 적용 | Q6의 module tier와 조건부 적용 기준 |
| QA | Q7의 5층 QA, 전체 판정 집계, source gap 처리, comparison 발동 조건 |
| top-up | Q11의 계속 진행 / 후보 기록 / 실제 승인 요청 기준 |
| 실행 순서 | Input Readiness Preflight -> slice 작성 -> QA -> observation -> 완료 판정 |
| 완료 후 판단 | blueprint로 넘어갈지, plan 조정이 필요한지 판단 |

v1에 옮기지 않을 내용:

| 제외 항목 | 이유 |
|---|---|
| Q1~Q12 질문 보드 자체 | v1은 논의판이 아니라 실행 계획서이기 때문 |
| Codex / Claude Code / 사용자 의견 교환 흔적 | 실행자가 따라야 할 절차가 아니기 때문 |
| `현재 미정`, `부분 합의`, `다음 논의` 같은 진행 메타 문구 | v1은 확정 계획서여야 하기 때문 |
| rejected alternative의 긴 설명 | 필요한 결정 이유만 짧게 남긴다. |
| 작업 지도 상태 관리 문구 | 상태 관리는 work map에서 다룬다. |
| v1 작성 후 교차검증 요청문 자체 | 교차검증 기준만 남기고 요청 대화문은 남기지 않는다. |

Claude Code 교차검증 기준:

| 검증 항목 | 확인 질문 |
|---|---|
| Q1~Q12 합의 누락 여부 | Q1~Q12의 핵심 결정이 v1에 빠짐없이 들어갔는가 |
| 추가 내용 통제 | v0 합의에 없는 새 조건, 기준, 예외, 필수 산출물, 승인 조건이 v1에 추가되지 않았는가 |
| 실행 가능성 | 실행자가 v1만 읽고 preflight와 first slice pilot을 시작할 수 있는가 |
| scope creep 방지 | Value Chain, Business Model, Market Share, Competition, Moat, Valuation, 투자 판단이 금지 영역으로 유지되는가 |
| 산출물 계약 | 필수 산출물 3개와 `source_register` 위치가 명확한가 |
| QA 정합성 | Q7의 5층 QA와 `pass` / `pass with adjustments` / `blocked` 기준이 정확한가 |
| module 정합성 | Q6의 module tier와 각 module 역할이 충돌하지 않는가 |
| top-up 정합성 | Q11에서 정한 top-up 예외 원칙과 approval-gate 경계가 유지되는가 |
| v1 문서 성격 | v1이 논의 로그가 아니라 실행 계획서로 정리됐는가 |

Q12 결론:

```text
pilot plan note v1은 Q1~Q12가 모두 합의된 뒤 작성한다.
v1은 논의 기록이 아니라 APP/adtech Industry Primer first slice pilot 실행 계획서다.
v1에는 실행에 필요한 목적, 범위, 입력, 산출물, module 적용, QA, top-up, 실행 순서만 담는다.
v0의 질문 보드, 논의 흔적, 미정/진행 메타 문구는 v1에 옮기지 않는다.
v1 작성 후 Claude Code는 합의 누락과 합의 외 추가를 모두 교차검증한다.
```

## 17. 현재 미결 질문 요약

현재 미결 질문:

```text
없음.
```

그 다음 순서:

```text
pilot plan note v1 작성
```

## 18. 다음 단계

1. 이 v0 논의판을 기준으로 pilot plan note v1을 새 파일로 작성한다.
2. v1에서 Q1~Q12 합의 누락과 합의 외 추가가 없는지 자체 검산한다.
3. v1을 Claude Code에 교차검증한다.
4. 교차검증 PASS 후 Industry Primer blueprint로 넘어간다.

현재 이 문서는 pilot plan note v0이며, Q1~Q12 합의가 모두 반영된 상태다.
