# Global Harness Template v5 Discussion Handoff

- 작성일: 2026-06-06
- 목적: Source Pack 구축 과정에서 얻은 공통 하네스 원칙을 전역 템플릿으로 승격할지 논의하기 위한 새 채팅방 인계 문서
- 상태: 논의용 handoff. 아직 `harness-lab` 전역 스킬이나 공용 템플릿 v5에 반영한 문서가 아님

## 1. 이 문서를 만든 이유

P2-S03 Source Pack 하네스를 구축하면서 여러 공통 하네스 원칙이 반복해서 등장했다.

예:

- 공통 업무 규칙은 `harness/`에 두고 Codex/Claude Code adapter는 얇게 둔다.
- 사용자가 논의만 원할 때 AI가 임의로 파일을 수정하면 안 된다.
- 하네스가 커질수록 관측 가능성, 체크포인트, docs 정리, 보안 기본선이 필요해진다.
- 새 분류나 상태값은 바로 schema에 넣지 말고 candidate로 관찰하는 편이 안전하다.

이 원칙들은 Source Pack에만 필요한 것이 아니라, 앞으로 만들 다른 가치투자 하네스에도 반복 적용될 가능성이 크다.

다만 바로 전역 `harness-lab`을 수정하는 것은 위험하다.
따라서 먼저 전역화 후보 템플릿으로 분리하고, 다음 하네스에서 실제로 써본 뒤 안정된 것만 전역화하는 방향으로 논의하기로 했다.

## 2. 현재 참고해야 할 주요 파일

### 2.1 기존 공용 템플릿

```text
docs/templates/공용_하네스_템플릿_체크리스트_v4.md
```

역할:

- 현재 공용 하네스 템플릿의 기본 뼈대
- 하네스 유형, 품질 축, 공통 원장, adapter, schema, rubric, orchestrator, AGENTS/CLAUDE 템플릿 포함
- Source Pack 초기 구축의 주요 기준

현재 판단:

```text
v4는 틀린 템플릿이 아니다.
하지만 Source Pack 실전 구축 이후 생긴 운영 패턴을 반영하려면 v5 후보가 필요하다.
```

### 2.2 Source Pack 관측 가능성 템플릿

```text
docs/templates/source-pack-observability-template.md
```

역할:

- Source Pack에서 만든 하네스 운영 관찰 선택 섹션
- `instructions_files_consulted`, `instructions_lines_consulted_estimate`, `bottleneck_note`, `trim_candidate` 같은 필드 포함

현재 판단:

```text
주제 자체는 전역화 가치가 크다.
다만 Source Pack 이름을 제거하고, 모든 하네스에 적용 가능한 독립 module template로 다듬는 편이 좋다.
```

### 2.3 Source Pack 아키텍처 지도

```text
docs/current/source-pack-architecture-map-2026-06-06.md
```

역할:

- Source Pack 전체 구조를 비개발자도 이해할 수 있게 설명한 운영자용 아키텍처 지도
- 공통 원장, 얇은 adapter, raw/catalog/index/runs, IR taxonomy, SEC-IR overlap, 관측 가능성, QA, 보안 격리, 확장 지점 정리

현재 판단:

```text
전역 템플릿 논의의 실제 사례로 참고하기 좋다.
```

### 2.4 전역화 후보 템플릿 폴더

```text
docs/templates/global-harness-candidates/
```

현재 포함된 후보 파일:

```text
README.md
global-harness-core-structure-template-v0.md
global-harness-approval-gate-template-v0.md
global-harness-qa-scaffold-template-v0.md
global-harness-observability-template-v0.md
global-harness-security-baseline-template-v0.md
global-harness-checkpoint-template-v0.md
global-harness-docs-organization-template-v0.md
global-harness-candidate-ledger-template-v0.md
```

현재 판단:

```text
이 파일들은 완성 템플릿이 아니라 v0 후보 카드다.
다음 채팅방에서 하나씩 검토하고 v1 template 또는 v5 module로 다듬는다.
```

## 3. v4와 현재 Source Pack 구조의 차이

v4는 아래 핵심 원칙을 이미 잘 담고 있다.

- `harness/` 공통 원장
- Codex/Claude Code 얇은 adapter
- 하네스 유형과 품질 축
- 산출물 계약
- 비교 모드
- 사용자 승인 지점
- schema/rubric/procedure 구조
- `AGENTS.md` / `CLAUDE.md` 구조 안내

하지만 Source Pack 실전 구축 후 아래 패턴이 새로 중요해졌다.

| 새로 중요해진 패턴 | v4 대비 차이 |
|---|---|
| `runbook` 분리 | ORCHESTRATOR와 procedure 사이의 실제 1회 실행 절차가 더 중요해짐 |
| docs 역할별 정리 | `current/design/templates/pilots/handoff/reference` 구조가 생김 |
| 승인 게이트 강화 | 논의/검토와 실제 파일 수정/실행을 더 엄격히 분리해야 함 |
| 관측 가능성 | run-summary 선택 섹션으로 하네스 병목을 기록하는 패턴 생김 |
| 보안 기본선 | `.env`, API key, 대화 원문, 보안 격리 파일 처리 필요성 확인 |
| checkpoint | 원문 저장 대신 compact session checkpoint 필요성 확인 |
| candidate 원장 | 새 분류/상태값을 바로 schema에 넣지 않고 후보로 누적하는 패턴 생김 |
| adapter 현실 보강 | `.claude/skills/`, compatibility adapter 등 실제 구조가 v4보다 복잡해짐 |

따라서 결론:

```text
v4는 기본 뼈대로 유효하다.
하지만 v5 또는 v5+modules 구조를 만들 충분한 이유가 있다.
```

## 4. v5는 거대한 통합 문서가 아니라 core + modules가 되어야 한다

처음에는 각 주제별 후보를 다듬고 `공용_하네스_템플릿_체크리스트_v5.md`로 통합하자는 말이 나왔다.

하지만 이후 논의에서 아래 결론으로 수정됐다.

```text
v5는 모든 내용을 삼키는 거대한 문서가 아니라
공통 뼈대와 module 연결 규칙을 가진 core template이어야 한다.

관측 가능성, 보안, 체크포인트, docs 정리, candidate 원장 같은 굵직한 기능은
별도 module template로 유지하는 편이 낫다.
```

개발 비유:

- 완전한 거대 단일 문서: 나중에 너무 무거워짐
- 너무 잘게 쪼갠 문서: 찾기 어렵고 관리 부담이 커짐
- core + 선택 module: 지금 단계에 가장 적합

이 구조는 순수 마이크로서비스라기보다 `modular template architecture` 또는 `modular monolith`에 가깝다.

## 5. v5 core와 module 후보 분류

| 항목 | v5 core 포함 | 별도 module | 현재 판단 |
|---|---:|---:|---|
| 공통 원장 + 얇은 adapter | 예 | 보조 가능 | 하네스 기본 뼈대 |
| runbook 개념 | 예 | 아니오 | 실행 구조의 핵심 |
| 산출물 계약 | 예 | 아니오 | 모든 하네스의 기본 조건 |
| 승인 게이트 | 최소 규칙 포함 | 상세 module | 안전 핵심 |
| QA | QA 필요성 포함 | scaffold module | 하네스마다 QA 내용이 다름 |
| 관측 가능성 | hook만 포함 | 독립 module | 모든 하네스에 유용하지만 독립 기능 |
| 보안 기본선 | 최소 안전선 포함 | 상세 module | 하네스별 차이 있음 |
| 체크포인트 | 존재 언급 | 독립 module | 긴 작업에서 중요 |
| docs 구조 | 원칙만 포함 | 독립 module | 문서가 많아졌을 때 적용 |
| candidate 원장 | 패턴 언급 | 독립 module | 모든 하네스에 항상 필요한 것은 아님 |

## 6. 다음 채팅방에서 논의할 11개 항목

아래 11개 항목을 순서대로 검토한다.

### 1. 공통 원장 + 얇은 adapter

핵심 질문:

- `harness/` 공통 원장을 v5 core에서 어떻게 정의할 것인가?
- Codex/Claude Code adapter는 얼마나 얇아야 하는가?
- `.claude/agents/`, `.claude/skills/`, `.agents/skills/`의 실제 구조를 v5에 어떻게 반영할 것인가?
- 공통 업무 규칙을 adapter에 복사하지 않는다는 원칙을 어떻게 검증할 것인가?

현재 판단:

```text
v5 core에 반드시 포함.
```

### 2. 승인 게이트

핵심 질문:

- 논의/검토 요청과 실제 수정/실행 요청을 어떻게 구분할 것인가?
- 작은 수정도 사전 승인이 필요한가?
- 파일 이동, 삭제, schema 변경, 운영 catalog 반영 같은 위험 작업은 어떤 승인 문구가 필요한가?
- Codex와 Claude Code 양쪽에서 같은 승인 규칙을 보게 하려면 어디에 넣어야 하는가?

현재 판단:

```text
v5 core에 최소 규칙 포함.
상세 규칙은 별도 approval-gate module로 관리.
```

### 3. QA

핵심 질문:

- 모든 하네스에 공통 적용 가능한 QA 뼈대는 무엇인가?
- 하네스별로 달라져야 하는 QA 항목은 무엇인가?
- `pass`, `partial_pass`, `unverified`, `fail`, `stopped`, `repair_required` 같은 상태값을 공통 후보로 둘 것인가?
- QA를 너무 강제하면 하네스 성능을 떨어뜨리지 않는가?

현재 판단:

```text
v5 core에는 "QA를 설계해야 한다"는 원칙만 넣는다.
구체 QA scaffold는 별도 module로 둔다.
```

### 4. 관측 가능성

핵심 질문:

- 모든 하네스 run-summary에 선택 관측 섹션을 넣을 것인가?
- 관측 가능성을 QA 실패 조건과 분리하는 문구를 어떻게 고정할 것인가?
- 자동 측정 도구는 언제 도입할 것인가?
- Source Pack의 `source-pack-observability-template.md`를 전역용으로 어떻게 일반화할 것인가?

현재 판단:

```text
v5 core에는 hook만 둔다.
독립 observability module로 관리.
```

### 5. 보안 기본선

핵심 질문:

- 모든 하네스에 공통으로 필요한 최소 보안 체크는 무엇인가?
- `.env`, API key, token, credential, 대화 원문, checkpoint, raw 파일을 어떻게 다룰 것인가?
- 모든 docs를 비공개로 묶는 과도한 규칙은 피할 수 있는가?
- 보안 제품이 격리한 파일 처리 원칙을 전역화할 것인가?

현재 판단:

```text
v5 core에는 최소 안전선만 포함.
상세는 security-baseline module로 관리.
```

### 6. 체크포인트

핵심 질문:

- 긴 작업에서 compact checkpoint를 기본 기능으로 둘 것인가?
- 원문 전체 저장과 compact checkpoint를 어떻게 구분할 것인가?
- Codex/Claude Code 저장 adapter 경계를 어떻게 둘 것인가?
- `docs/session-checkpoints/`를 전역 기본 위치로 둘 것인가?

현재 판단:

```text
v5 core에는 checkpoint 필요성을 언급.
구체 저장 계약은 checkpoint module로 관리.
```

### 7. docs 구조

핵심 질문:

- 처음부터 `current/design/templates/pilots/handoff/reference` 구조를 강제할 것인가?
- 아니면 문서가 많아졌을 때 적용하는 정리 절차로 둘 것인가?
- 파일 이동 전 참조 점검을 전역 절차로 둘 것인가?
- handoff 내부 과거 경로는 역사 기록으로 유지한다는 원칙을 넣을 것인가?

현재 판단:

```text
v5 core에는 docs 정리 원칙만 둔다.
구체 폴더 구조와 정리 절차는 docs-organization module로 관리.
```

### 8. 산출물 계약

핵심 질문:

- 대화에만 결과를 남기지 않는다는 규칙을 v5 core에 어떻게 넣을 것인가?
- 중간 산출물, 최종 산출물, 다음 하네스 입력을 어떻게 구분할 것인가?
- `artifacts/` 구조를 모든 하네스에 기본으로 둘 것인가?
- 사용자에게 보여주는 산출물과 기계가 읽는 산출물을 어떻게 분리할 것인가?

현재 판단:

```text
v5 core에 반드시 포함.
별도 module보다는 core의 핵심 계약으로 둔다.
```

### 9. 수정 경계

핵심 질문:

- 업무 의미 변경, 실행 방식 변경, 산출물 변경, 설계 메모 변경을 어떻게 구분할 것인가?
- `harness/`, `.agents/`, `.claude/`, `artifacts/`, `docs/`의 책임 경계를 v5에 어떻게 쓸 것인가?
- `AGENTS.md`와 `CLAUDE.md`를 어떻게 동기화할 것인가?
- drift를 검출하는 최소 체크리스트는 무엇인가?

현재 판단:

```text
v5 core에 반드시 포함.
```

### 10. Pilot-first 원칙

핵심 질문:

- 새 source, 새 분류, 새 자동화는 언제 pilot을 먼저 해야 하는가?
- pilot 결과를 운영 산출물에 반영하려면 어떤 승인 절차가 필요한가?
- pilot과 production의 차이를 어떻게 기록할 것인가?
- 작은 테스트를 하지 않고 대규모 구조 개편을 하는 위험을 어떻게 줄일 것인가?

현재 판단:

```text
v5 core에 원칙 포함.
필요하면 별도 pilot/testing module로 확장 가능.
```

### 11. Candidate 원장 패턴

핵심 질문:

- 새 분류/상태/유형/예외를 바로 정식화하지 않고 후보로 남기는 패턴을 어떻게 일반화할 것인가?
- candidate ledger가 필요한 하네스와 필요 없는 하네스를 어떻게 구분할 것인가?
- 사용자 승인 요청 기준을 어떻게 둘 것인가?
- 후보가 쌓이면 schema로 승격하는 절차를 어떻게 정할 것인가?

현재 판단:

```text
v5 core에는 패턴만 언급.
구체 구조는 candidate-ledger module로 관리.
```

## 7. 현재 추천 논의 순서

다음 채팅방에서는 아래 순서를 추천한다.

```text
1. v5 core의 범위 확정
2. approval gate module 구체화
3. observability module 구체화
4. security baseline module 구체화
5. checkpoint module 구체화
6. core structure / runbook / 산출물 계약 정리
7. QA scaffold module 구체화
8. docs organization module 구체화
9. candidate ledger module 구체화
10. 공용_하네스_템플릿_체크리스트_v5 초안 작성 여부 결정
```

이 순서를 추천하는 이유:

- 승인 게이트는 AI가 임의로 수정하는 문제를 막는 가장 직접적인 안전장치다.
- 관측 가능성과 보안 기본선은 거의 모든 하네스에 재사용 가능하다.
- 체크포인트는 긴 작업을 이어가기 위한 인프라다.
- core structure는 이미 v4가 어느 정도 있으므로, 앞선 module 논의 후 보강하는 편이 낫다.

## 8. 다음 채팅방 시작 요청 예시

```text
아래 handoff 문서를 먼저 읽고, 공용 하네스 템플릿 v5와 전역 module template 구조를 논의하자.

참고 파일:
- docs/templates/global-harness-candidates/global-harness-template-v5-discussion-handoff-2026-06-06.md
- docs/templates/공용_하네스_템플릿_체크리스트_v4.md
- docs/templates/global-harness-candidates/README.md

목표:
- v5 core에 들어갈 것과 별도 module로 둘 것을 확정
- 11개 항목을 순서대로 검토
- 바로 전역 harness-lab을 수정하지 말고, 먼저 docs/templates 안에서 v1 후보를 만든다
```

## 9. 현재 결론

현재 합의된 방향은 아래다.

```text
v4는 폐기하지 않는다.
v4는 core template의 기반으로 유지한다.
Source Pack 실전 경험에서 나온 새 원칙은 v5 core와 별도 module template로 나눈다.
관측 가능성, 보안, 체크포인트, docs 정리, candidate 원장은 하나의 거대 v5 문서에 흡수하지 않는다.
먼저 docs/templates/global-harness-candidates/에서 주제별 후보를 다듬고, 검증된 것만 전역화한다.
```
