# Global Harness v5 Phase 7-B Industry Primer Design Principles Note

- 작성일: 2026-06-08
- 상태: Claude Code 교차검증 PASS, minor fix 반영
- 범위: Phase 7-B `Industry Primer` 하네스 pilot plan 작성 전 설계 원칙 합의
- 기준 문서:
  - `global-harness-v5-phase7-planning-consensus-note-2026-06-07.md`
  - `global-harness-v5-phase7a-source-pack-retro-validation-note-2026-06-08.md`
  - `docs/reference/가치투자 리서치 21단계 구조화 버전.md`
  - `docs/reference/Phase 2 - Step 3 Source Pack Minimum Check 템플릿.md`
  - `docs/reference/Phase 2 - Step 4 Industry Primer 템플릿.md`
  - `docs/reference/Phase 2 - Step 5 Value Chain 템플릿.md`
  - `docs/reference/Phase 2 - Step 6 Business Model 템플릿.md`
  - `docs/reference/Phase 2 - Step 7 Market Share 템플릿.md`

## 1. 목적

이 문서는 Phase 7-B `Industry Primer` 실전 검증에 들어가기 전에, 사용자, Codex, Claude Code가 논의한 설계 원칙을 고정하기 위한 note다.

Phase 7-B의 다음 작업은 곧바로 Industry Primer 하네스를 만드는 것이 아니다. 먼저 아래 순서로 진행한다.

```text
설계 원칙 note 작성
→ Claude Code 교차검증
→ 이견 반영
→ Industry Primer pilot plan 작성
→ 하네스 blueprint 작성
→ 교차검증
→ 하네스 구축
→ preflight / slice / full pilot
```

이 문서의 목적은 pilot plan과 하네스 blueprint가 흔들리지 않게 기준을 세우는 것이다.

이 문서는 다음이 아니다.

- Industry Primer 하네스 본문
- Industry Primer pilot plan
- Industry Primer 최종 산출물
- Phase 2 전체 통합 템플릿
- 전역 template bundle 배포 결정 문서

## 2. 현재 합의된 Phase 7-B 위치

Phase 7은 아래 구조로 진행한다.

| 단계 | 의미 | 현재 상태 |
|---|---|---|
| Phase 7-0 | signal-routing scope note + v0 | 완료 |
| Phase 7-A | Source Pack 소급 검증 | 완료, 판정 `pass` |
| Phase 7-B | Industry Primer 실전 검증 | 설계 원칙 정리 중 |
| Phase 7-C | 전역 배포 판단 | 미착수 |

Phase 7-B의 첫 실전 pilot 대상은 Phase 2 - Step 4 `Industry Primer`로 한다.

이 선택의 이유는 다음과 같다.

| 이유 | 설명 |
|---|---|
| workflow상 자연스러움 | `Source Pack` 다음 단계가 `Industry Primer`다. |
| Source Pack과 충분히 다름 | Source Pack은 수집형이고, Industry Primer는 산업 이해 / 구조화 / 분석 준비형이다. |
| 앞단 의존성이 과하지 않음 | `Moat`나 `Financial Quality`처럼 많은 이전 단계 산출물에 강하게 의존하지 않는다. |
| 실전 가치가 있음 | 실제 가치투자 21단계의 다음 하네스 후보로 의미가 있다. |

`Earnings Call`은 Step 3 Source Pack의 하위 분화 후보로 둔다. 다만 `Industry Primer`의 blocking dependency는 아니다.

## 3. 참고 자료에서 확인한 Phase 2 경계

가치투자 21단계 구조화 버전에서 Phase 2는 “사업 이해 / 산업 구조 파악” 단계다.

| Step | 이름 | 핵심 역할 |
|---|---|---|
| 3 | Source Pack | 공시, 컨콜, IR, 산업자료 등 원자료 확보 |
| 4 | Industry Primer | 산업 구조와 핵심 용어 이해 |
| 5 | Value Chain | profit pool map, 돈의 흐름, 병목, 회사 위치 |
| 6 | Business Model | 제품, 고객, 가격, 매출 공식, 이익 공식 |
| 7 | Market / Share | TAM / SAM / SOM, 현재 점유율, 성장 runway |

Industry Primer는 Phase 2 전체를 대체하지 않는다. Step 4의 역할은 산업의 기본 지도를 만드는 것이다.

Step 4가 다뤄야 할 것과 넘겨야 할 것은 아래처럼 구분한다.

| 영역 | Industry Primer에서 다룰 것 | 넘길 것 |
|---|---|---|
| 산업 정의 | 산업이 무엇이고 왜 존재하는지 설명 | 산업 내 돈의 세부 흐름은 Value Chain |
| 참여자 | 주요 참여자 유형과 구매자/판매자 구분 | 각 위치의 profit pool 판단은 Value Chain |
| 하위시장 | 하위시장과 용어 구조를 정리 | 특정 회사의 매출 공식은 Business Model |
| 성장 동인 | 수요, 공급, 제도, 기술 동인을 산업 수준에서 설명 | 시장 규모 착시, 점유율, runway 판단은 Market / Share |
| 수익/비용 구조 | 산업 수준의 일반적 수익/비용 패턴 | 특정 회사의 revenue/cost/profit engine은 Business Model |
| 리스크 | 산업 구조상 위험을 예비적으로 정리 | 경쟁우위, 해자, 최종 우열 판단은 Competition / Moat |
| 다음 질문 | 후속 단계에서 검증할 질문 생성 | 후속 단계 결론을 미리 내리지 않음 |

## 4. Industry Primer의 역할과 비범위

Industry Primer의 핵심 목적은 “산업을 이해하기 위한 기초 지도”를 만드는 것이다.

Industry Primer가 답해야 하는 질문은 다음에 가깝다.

- 이 산업은 왜 존재하는가?
- 고객은 누구이고 무엇을 해결하기 위해 돈을 내는가?
- 산업 참여자는 어떤 유형으로 나뉘는가?
- 핵심 용어와 하위시장은 무엇인가?
- 산업 성장을 움직이는 수요, 공급, 제도, 기술 요인은 무엇인가?
- 산업 구조상 위험은 무엇인가?
- 다음 단계에서 반드시 확인해야 할 질문은 무엇인가?

Industry Primer가 하지 말아야 할 것은 다음과 같다.

| 하지 말 것 | 이유 |
|---|---|
| 투자 매수 / 매도 판단 | Phase 5 Final Memo 전에는 투자 결정을 내리지 않는다. |
| 경쟁사 우열 최종 판단 | Competition / Moat 단계의 역할이다. |
| 해자 최종 판단 | Moat 단계의 역할이다. |
| 밸류에이션 판단 | Phase 4 Valuation의 역할이다. |
| 정교한 장기 forecast | Forecast / Market / Share 이후의 역할이다. |
| 특정 회사의 매출 공식 결론 | Business Model 단계의 역할이다. |
| profit pool 최종 결론 | Value Chain 단계의 역할이다. |

Industry Primer는 분석을 시작하기 위한 지도이지, 결론을 끝내는 리포트가 아니다.

## 5. 실행 주체 원칙

Industry Primer 하네스는 특정 모델이나 특정 ChatGPT 프롬프트에 묶지 않는다.

합의된 원칙은 다음과 같다.

| 항목 | 원칙 |
|---|---|
| 실행 주체 | 도구 중립. Codex, Claude Code, ChatGPT 계열 실행 모두 가능해야 한다. |
| 업무 의미 | `harness/` 공통 원장에 둔다. |
| adapter | Codex / Claude / 기타 실행 환경은 얇은 adapter로 둔다. |
| 기존 GPT 프롬프트 | reference input으로 흡수한다. 하네스 본문으로 그대로 복사하지 않는다. |
| 웹검색 불가 환경 | 내용을 지어내지 않고 `needs_external_research`, `blocked`, `user_input_needed`로 표시한다. |

즉 Industry Primer 하네스는 “ChatGPT에게 붙여넣는 프롬프트”가 아니라, 어떤 실행자도 같은 입력/출력/QA 기준을 따르게 하는 구조여야 한다.

## 6. 산업 단위와 기업 context

Industry Primer는 기업 단위로 트리거되지만, 산출물의 중심은 산업 단위다.

예:

```text
APP를 분석하기 위해 adtech / mobile advertising 산업을 이해한다.
```

합의된 구조는 다음과 같다.

| 항목 | 결정 |
|---|---|
| content center | 산업 / 하위산업 |
| trigger context | target company / ticker |
| v0 산출물 | run 단위 standalone, immutable 문서 |
| 재사용 모델 | v0에서는 보류 |
| update 모델 | v0에서는 보류 |
| company appendix 모델 | v0에서는 보류 |

중요한 점은 v0 pilot에서 “산업별 master primer”를 만들지 않는다는 것이다.

예를 들어 APP를 위해 작성한 adtech primer를 나중에 TTD 분석 때 update하거나 company appendix로 확장하는 모델은 아직 도입하지 않는다.

이유는 다음과 같다.

| 위험 | 설명 |
|---|---|
| version 관리 복잡도 | 기존 primer를 update하면 이전 버전과 새 버전의 기준이 꼬일 수 있다. |
| 기준 회사 혼동 | APP 관점 primer와 TTD 관점 primer 중 무엇이 기준인지 불명확해질 수 있다. |
| mutable knowledge base 문제 | Source Pack처럼 append/idempotent로 피했던 상태 관리 문제가 다시 생긴다. |
| v0 과잉 설계 | 첫 pilot에서 재사용/통합 모델까지 설계하면 하네스가 너무 무거워진다. |

따라서 v0 원칙은 다음이다.

```text
Industry Primer v0는 산업 중심 + target company context 구조로 작성한다.
각 run은 standalone / immutable 산출물로 둔다.
산업별 master primer, update, reuse, company appendix 모델은 pilot 이후 결정한다.
```

## 7. Source Pack과 웹검색의 역할

Industry Primer pilot은 Source Pack과 외부 웹검색을 모두 사용하는 실제 리서치 pilot으로 설계한다.

두 입력의 역할은 다르다.

| 입력 | 역할 |
|---|---|
| Source Pack | 회사 관점의 공식 1차 입력. 공시, IR, 실적자료, 회사 설명을 제공한다. |
| 웹검색 / 외부자료 | 산업 수준의 외부 검증, 시장 구조, 제도, 용어, 참여자, 최신 변화 확인에 사용한다. |

Source Pack만으로 Industry Primer를 작성하면 회사가 말하는 산업 설명에 갇힐 위험이 있다. 반대로 웹검색만 쓰면 회사 공식 입력과 연결되지 않는다.

따라서 원칙은 다음과 같다.

```text
Source Pack은 회사가 자신이 속한 산업을 어떻게 설명하는지 보여준다.
웹검색과 외부자료는 그 설명이 산업 현실과 맞는지 검증하고 보완한다.
```

웹검색이 필요한 대표 영역:

- 산업 정의와 하위시장
- 산업 참여자와 구매자 유형
- 성장 동인
- 제도 / 규제 / 표준
- 기술 변화
- 구조적 리스크
- 최신 시장 변화

웹검색이 불가능한 실행 환경에서는 해당 항목을 추측하지 않는다. 대신 `needs_external_research` 또는 `확인 필요`로 표시한다.

## 8. source register 원칙

Industry Primer는 웹검색과 Source Pack 입력을 함께 쓰므로 출처 추적성이 핵심이다.

Source Pack은 파일 경로와 catalog로 출처를 추적했다. Industry Primer는 웹 자료, 업로드 문서, Source Pack 파일을 함께 쓰기 때문에 별도 `source_register` 또는 이에 준하는 출처 표가 필요하다.

v0의 최소 필드는 다음과 같이 둔다.

| 구분 | 필드 | 설명 |
|---|---|---|
| required | `source_id` | 산출물 본문에서 참조할 짧은 ID |
| required | `source_type` | `source_pack`, `company_filing`, `company_ir`, `web`, `industry_report`, `regulator`, `news`, `other` 등 |
| required | `title` | 사람이 식별할 수 있는 제목 |
| required | `url_or_path` | 웹 URL 또는 Source Pack / local path |
| conditionally required | `accessed_at` | 웹 자료일 때 필수. 재현성 확인용 |
| optional | `publisher` | 발행자 |
| optional | `published_date` | 발행일 |
| optional | `used_for` | 해당 출처가 쓰인 섹션이나 판단 |

v0에서는 `reliability` 필드를 필수로 두지 않는다.

이유:

- 신뢰도 판단 기준을 먼저 정하지 않으면 실행자마다 점수가 달라진다.
- Source Pack Minimum Check의 자료 신뢰도 등급은 참고할 수 있지만 Industry Primer v0에 그대로 강제하기에는 무겁다.
- 첫 pilot에서는 출처 식별과 재현성을 우선한다.

따라서 `reliability`는 v1 승격 후보 또는 candidate-ledger 후보로 남긴다.

본문의 핵심 사실, 숫자, 규제, 시장 구조, 회사 주장에는 가능한 한 `source_ref`를 붙인다.

## 9. QA 설계 원칙

Industry Primer QA는 Source Pack QA와 성격이 다르다.

| 구분 | Source Pack QA | Industry Primer QA |
|---|---|---|
| 중심 | 파일 존재, catalog 등록, raw/index 무결성 | 내용 판단, 범위 통제, 출처 추적, 해석 품질 |
| 자동화 가능성 | 비교적 높음 | 낮음. 사람/모델 판단이 많이 필요 |
| 실패 유형 | 파일 없음, schema 불일치, catalog 누락 | 과도한 결론, 출처 없음, 확인 필요 누락, 후속 질문 부실 |

Industry Primer QA는 5층 구조로 둔다.

| QA 층 | 질문 |
|---|---|
| 1. 구조 QA | 필수 섹션과 표가 존재하는가? |
| 2. 출처 QA | 핵심 fact, 숫자, 규제, 시장 구조 주장에 `source_ref`가 있는가? |
| 3. 범위 QA | Value Chain, Business Model, Market / Share, Competition, Moat의 결론을 미리 내리지 않았는가? |
| 4. 판단 QA | fact, interpretation, `확인 필요`가 분리되어 있고 과장된 결론이 없는가? |
| 5. handoff QA | Section 13의 후속 질문이 실제 다음 하네스에서 사용할 수 있을 만큼 구체적인가? |

특히 handoff QA가 중요하다. Industry Primer의 마지막 산출물은 단순 요약이 아니라 다음 하네스로 넘길 질문이다.

좋은 handoff 질문:

- Value Chain에서 확인할 돈의 흐름 또는 병목 후보를 구체적으로 지목한다.
- Business Model에서 확인할 회사별 revenue/cost/profit engine 질문을 남긴다.
- Market / Share에서 확인할 시장 정의, 성장률, 점유율 질문을 남긴다.
- Competition / Moat에서 나중에 확인할 경쟁 구조 또는 지속성 질문을 남긴다.

나쁜 handoff 질문:

- “시장 규모를 더 확인한다”처럼 너무 막연하다.
- 이미 Industry Primer에서 결론을 내려버린다.
- 후속 하네스가 어떤 자료나 판단을 해야 하는지 알 수 없다.

## 10. type-schema 연결 원칙

`type-schema`는 Industry Primer에서 필요하다. 다만 역할은 좁게 둔다.

`type-schema`가 제공하지 않는 것:

- glossary 내용 자체
- 산업 리포트 문장
- 특정 산업의 정답
- 하위시장 최종 taxonomy

`type-schema`가 제공할 수 있는 것:

- 섹션별 출력 구조
- `fact`, `interpretation`, `확인 필요` 상태값
- `source_ref` 사용 규칙
- glossary 항목의 최소 필드
- growth driver category 후보
- risk category 후보
- handoff question type 후보

즉 `type-schema`는 내용을 쓰는 module이 아니라, 내용을 담을 그릇과 상태값 규칙을 정리하는 연결 항목이다.

Industry Primer v0에서 type-schema는 별도 전역 파일을 만들지 않고 아래에 연결한다.

| 연결 | 역할 |
|---|---|
| `design-preflight` | Industry Primer의 유형, 산출물 역할, 품질 축 결정 |
| `qa-scaffold` | 검증 기준과 상태값을 QA로 연결 |
| 각 하네스의 `harness/schemas/` | 실제 Industry Primer 산출물 schema 위치 |

## 11. module 선택 원칙

Industry Primer pilot에서 사용할 v5 module은 아래처럼 판단한다.

| module | 적용 수준 | 이유 |
|---|---|---|
| `design-preflight` | 필수 | Industry Primer 유형, 산출물 역할, 품질 축, 경계를 먼저 정해야 한다. |
| `type-schema` | 필요하지만 좁게 | glossary, source_ref, 확인 필요, handoff question 구조를 잡는 데 필요하다. |
| `security-baseline` | 기본 적용 | 웹자료, raw/source 파일, 비공개 대화, checkpoint를 다룰 수 있다. |
| `approval-gate` | 필수 | 논의/실행 분리, 범위 변경, 하네스 파일 생성 승인 경계가 필요하다. |
| `qa-scaffold` | 필수 | 판단 기반 QA를 5층 구조로 설계해야 한다. |
| `comparison` | 조건부 | Codex/Claude 결과 비교가 필요한 slice나 full pilot에서 사용한다. |
| `signal-routing` | 필수 | 확인 필요, blocked, source gap, 범위 초과, 보안 신호를 공통 방식으로 표현한다. |
| `observability` | 가볍게 적용 | pilot 중 막힌 지점, 반복 병목, 과잉/누락 규칙을 기록한다. |
| `candidate-ledger` | optional / lightweight | 반복되는 schema/source/status 후보가 보일 때만 가볍게 사용한다. |
| `pilot-first / testing` | 필수 원칙 | 처음부터 full run으로 가지 않고 preflight, slice, full pilot 순서로 간다. |
| `checkpoint` | 필요 시 적용 | 긴 논의와 세션 전환이 있으면 compact checkpoint를 쓴다. |
| `docs-organization` | 가볍게 적용 | 초기부터 복잡한 docs 구조를 강제하지 않고 문서가 늘면 정리한다. |

첫 Industry Primer pilot에서 candidate-ledger는 무겁게 쓰지 않는다.

이유:

- Industry Primer는 우선 run 단위 standalone 산출물이다.
- Source Pack처럼 반복 수집과 schema 진화가 이미 검증된 상태가 아니다.
- 첫 pilot에서 후보 원장을 과하게 쓰면 하네스보다 운영 체계가 먼저 무거워진다.

다만 아래 상황이 반복되면 candidate-ledger에 기록한다.

- 새 source type 후보가 반복해서 등장한다.
- glossary 항목 구조가 계속 바뀐다.
- `확인 필요` 상태값보다 세분화된 상태가 필요해진다.
- handoff question type이 반복 패턴으로 나타난다.
- 웹 출처 신뢰도나 source category 기준을 정식화할 필요가 생긴다.

## 12. pilot 단계 원칙

Industry Primer는 바로 full output으로 가지 않는다.

사용자와 Codex의 합의는 다음과 같다.

```text
최종적으로는 완성된 Industry Primer 문서까지 가야 한다.
하지만 첫 실행은 작은 preflight와 slice부터 시작한다.
```

권장 순서:

| 단계 | 목적 | 예상 산출물 |
|---|---|---|
| 1. pilot plan | 대상 산업/회사, 입력, 출력, module 선택 확정 | pilot plan note |
| 2. harness blueprint | 파일 구조, 계약, procedure, schema, QA 설계 | blueprint note |
| 3. cross-validation | Claude Code / Codex가 blueprint 검토 | review note |
| 4. harness build | 실제 Industry Primer 하네스 파일 작성 | `harness/` 후보 파일 |
| 5. preflight | 입력과 범위가 충분한지 확인 | preflight note |
| 6. slice pilot | 일부 섹션만 실행해 구조 검증 | slice output + QA |
| 7. full pilot | 완성형 Industry Primer 문서 생성 | final pilot output |
| 8. evaluation | v5 template의 누락, 과잉, drift 평가 | evaluation note |

이 순서는 Source Pack을 만들 때 효과가 있었던 조심스러운 접근을 Industry Primer에도 적용하는 것이다.

## 13. pilot 대상 후보

현재 논의에서 `APP / adtech / mobile advertising`이 첫 pilot 후보로 거론됐다.

이유:

- Source Pack 자료가 이미 존재한다.
- 산업이 너무 단순하지도, 너무 복잡하지도 않다.
- 광고 기술, 앱 생태계, 플랫폼, 데이터, privacy/regulation 등 Industry Primer에 적합한 구조적 질문이 많다.
- Source Pack과 웹검색을 함께 써야 하는 사례다.

다만 이 문서에서는 APP/adtech를 최종 확정하지 않는다.

pilot plan에서 사용자가 최종 결정해야 한다.

대안 후보를 볼 때 기준은 다음과 같다.

| 기준 | 질문 |
|---|---|
| Source Pack 입력 | 이미 Source Pack 또는 최소 원자료가 있는가? |
| 산업 난이도 | 너무 단순하거나 너무 난해하지 않은가? |
| 후속 단계 연결 | Value Chain, Business Model, Market / Share로 넘길 질문이 자연스럽게 나오는가? |
| 사용자의 판단 가능성 | 사용자가 산출물 품질을 어느 정도 판단할 수 있는 산업인가? |
| 웹검색 필요성 | 외부 산업 자료가 실제로 필요한가? |

## 14. 다음 논의에서 결정할 것

이 note가 Claude Code 교차검증을 통과하면, 다음은 Industry Primer pilot plan 논의다.

pilot plan 전에 결정해야 할 질문은 아래와 같다.

| 질문 | 현재 상태 | 권장 처리 |
|---|---|---|
| 첫 pilot 대상 회사/산업은 APP/adtech로 할까? | 미정 | 사용자 결정 + Codex/Claude 검토 |
| 첫 slice는 어느 섹션까지 실행할까? | 미정 | 구조 검증용 최소 slice를 먼저 정한다. |
| Source Pack 입력 범위는 어디까지 쓸까? | 미정 | company filing/IR/index/run-summary 등 사용 범위 결정 |
| 웹검색은 어떤 범위까지 허용할까? | 미정 | 최신 산업/규제/용어/참여자 확인 중심으로 제한 |
| source_register는 어디에 둘까? | 미정 | 산출물 내부 섹션 또는 별도 artifact 후보 검토 |
| Industry Primer harness 파일은 언제 만들까? | 미정 | pilot plan과 blueprint 교차검증 후 작성 |
| comparison mode를 첫 pilot에서 쓸까? | 미정 | 비용과 검증 가치 비교 후 결정 |

## 15. 현재 합의 요약

| 주제 | 합의 |
|---|---|
| Phase 7-B 대상 | Phase 2 - Step 4 `Industry Primer` |
| 하네스 성격 | 산업 이해 / 구조화 / 분석 준비형 |
| 실행 주체 | 도구 중립. Codex/Claude/ChatGPT 계열 모두 adapter로 실행 가능해야 함 |
| GPT 프롬프트 | reference로 흡수. 하네스 본문이 아님 |
| 산업/기업 단위 | 산업 중심 + target company context |
| v0 산출물 | run 단위 standalone / immutable |
| 재사용 모델 | master/update/company appendix 모델은 pilot 이후 보류 |
| 입력 | Source Pack + 웹검색 / 외부자료 |
| source_register | required 4필드 + 웹 source의 `accessed_at`; `reliability`는 v0 제외 |
| QA | 구조 / 출처 / 범위 / 판단 / handoff 5층 QA |
| type-schema | 내용이 아니라 구조, 상태값, source_ref, handoff question type 규칙 |
| security-baseline | 기본 적용. 웹자료, 외부 파일, Source Pack 자료, 비공개 대화/checkpoint를 다룰 때 최소 안전선을 적용 |
| candidate-ledger | optional / lightweight |
| pilot 방식 | plan → blueprint → cross-validation → build → preflight → slice → full pilot → evaluation |

## 16. 이 문서의 다음 단계

1. 사용자가 이 문서를 Claude Code에 전달한다.
2. Claude Code가 누락, 과잉, 이견을 검토한다.
3. 이견이 있으면 Codex와 다시 논의한다.
4. 합의가 되면 이 문서를 수정한다.
5. 이 문서를 기준으로 Industry Primer pilot plan을 논의한다.

현재 이 문서는 Claude Code 교차검증 PASS 상태이며, minor fix를 반영했다.
