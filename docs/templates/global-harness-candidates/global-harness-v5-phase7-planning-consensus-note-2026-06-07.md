# Global Harness v5 Phase 7 Planning Consensus Note

- 작성일: 2026-06-07
- 상태: Claude Code 교차검증 전 consensus draft
- 범위: Phase 7 진입 전 구조 합의, 남은 사용자 결정, 작업 지도 반영 전 기준 문서
- 기준 논의: 사용자, Codex, Claude Code가 Phase 6 완료 후 논의한 전역 배포, signal-routing, pilot 검증, 전역화 판단

## 1. 목적

이 문서는 Global Harness v5 후보 세트를 Phase 7로 넘기기 전에 합의된 운영 방향을 고정하기 위한 planning consensus note다.

Phase 6까지는 v5 core와 주요 module 후보를 만들고 교차검증했다. Phase 7부터는 이 후보 세트를 실제 하네스에 적용해보고, 어떤 부분을 전역 template bundle로 승격할지 판단해야 한다.

이 시점에서 바로 작업 지도만 수정하면 논의의 이유와 한계가 사라질 수 있다. 따라서 먼저 별도 합의 문서로 아래 내용을 정리한다.

- 전역 배포의 원칙과 시점
- signal-routing module을 둘지 여부와 범위
- Phase 7을 어떤 검증 단계로 나눌지
- Source Pack 소급 검증의 목적과 한계
- 다음 하네스 실전 검증 전에 사용자가 정해야 할 것
- Phase 7 작업 지도에 반영해야 할 항목

이 문서는 최종 template 본문이 아니다. 작업 지도에 반영하기 전, Phase 7 설계 결정을 추적하기 위한 합의 문서다.

## 2. 현재 상태

작업 지도 기준으로 Phase 1부터 Phase 6까지는 완료 상태다.

완료된 큰 작업은 다음과 같다.

| 영역 | 현재 상태 |
|---|---|
| v5 core scope | 전역 `harness-lab`과 v5 core의 역할 분리 완료 |
| v5 core v1 | `global-harness-core-structure-template-v1.md` 작성 및 registry 정렬 완료 |
| design-preflight | v4 설계 판단 장치를 core에 복사하지 않고 별도 v0 module로 분리 |
| hook cross-check | approval, QA, security hook과 module v0 사이 blocking conflict 없음 |
| module registry | 파일 기반 module, no-file 항목, registry 순서, 경로 표기 방식 검토 완료 |
| module v1 후보화 | observability, approval-gate, qa-scaffold, security-baseline, checkpoint, docs-organization, candidate-ledger v1 후보 작성 |
| no-file 항목 | type-schema, comparison, pilot-first/testing은 별도 파일 없이 registry와 연결 module로 처리 |
| Claude Code 교차검증 | Phase 6 module registry 정렬까지 PASS 확인 |

현재 작업 위치는 Phase 7 진입 전 확인 구간이다.

현재 작업 지도에는 Phase 7에 아래 항목들이 todo로 남아 있다.

- pilot 대상 하네스 선정
- v5 core 후보를 pilot 청사진에 적용
- 필요한 module만 선택 적용
- 적용 중 drift, 누락, 과잉 규칙 기록
- 공통 signal/notification module 후보 검토
- 전역 배포 시 registry 기준 경로 문구 portable화 검토
- 전역 `harness-lab` 수정 여부는 별도 논의로 보류

## 3. Phase 7 문서화 원칙

Phase 7에서는 바로 작업 지도부터 수정하지 않는다.

권장 순서는 다음과 같다.

1. Phase 7 planning consensus note 작성
2. Claude Code 교차검증
3. 이견이 있으면 consensus note 수정
4. 사용자가 정해야 할 항목을 답변
5. 답변을 Codex와 Claude Code가 다시 검토
6. 최종 consensus note 갱신
7. 그 문서를 근거로 작업 지도 업데이트
8. Phase 7 실행

이 분리의 이유는 다음과 같다.

- 작업 지도는 항해 지도이며, 모든 논거를 담기에는 적합하지 않다.
- consensus note는 항해 계획서이며, 결정 이유와 한계를 보존한다.
- Phase 7은 전역 배포와 다음 하네스 성능에 영향을 주므로, 단순 todo 이상의 설계 판단이 필요하다.

## 4. 전역 배포 원칙

### 4.1 사용자의 목표

사용자는 v5 template bundle을 Codex 전역 폴더와 Claude Code 전역 폴더에 각각 배치하고 싶어 한다.

의도는 `harness-lab`과 비슷하다. 어떤 AI 환경을 쓰든 같은 전역 템플릿에 접근할 수 있게 하려는 것이다.

이 목표는 타당하다.

사용자의 장기 목표는 Source Pack 하나를 완성하는 데 그치지 않는다.

- 가치투자 21단계 전체를 하네스 구조로 만든다.
- 21단계를 제어하는 상위 하네스도 만든다.
- 이후 반복 업무가 생길 때마다 하네스 구조를 적용한다.
- Source Pack에서 얻은 설계 지식과 운영 지침을 반복 가능한 template bundle로 축적한다.

따라서 매번 Source Pack 구축 과정에서 했던 질문과 답변을 반복하는 것은 비효율적이다. 공통 운영 지식은 전역 template bundle로 올리는 것이 생산성 측면에서 의미가 있다.

### 4.2 합의된 원칙

전역 배포 방향은 맞다. 하지만 Phase 7 pilot 검증 전에 배포하지 않는다.

합의된 상태는 다음과 같다.

| 시점 | canonical source | Codex 전역 | Claude 전역 |
|---|---|---|---|
| 현재 | Source Pack repo의 `global-harness-candidates/` | 배포하지 않음 | 배포하지 않음 |
| Phase 7 검증 중 | Source Pack repo의 후보 폴더 | 배포하지 않음 | 배포하지 않음 |
| Phase 7 검증 후 | 검증된 template bundle 후보 | 배포 여부 판단 | 배포 여부 판단 |
| 21단계 상위 하네스 구축 후 | 상위 하네스 또는 별도 canonical bundle로 재검토 | 배포본 | 배포본 |

현재 Source Pack repo는 임시 canonical source다. 장기적으로 Source Pack이 모든 전역 template의 영구 원본이 되는 것은 어색할 수 있다. Source Pack은 가치투자 21단계 중 하나이기 때문이다.

21단계 전체를 묶는 상위 하네스가 생기면 canonical source 위치를 다시 판단한다.

### 4.3 지금 배포하지 않는 이유

Phase 7 전에 전역 폴더에 배포하면 이중 또는 삼중 관리가 시작된다.

예상되는 문제는 다음과 같다.

| 문제 | 설명 |
|---|---|
| drift | Source Pack 후보 폴더, Codex 전역, Claude 전역 파일이 서로 달라질 수 있다. |
| 수정 비용 증가 | pilot에서 수정이 나오면 여러 위치를 동시에 고쳐야 한다. |
| 과도한 확정 | 아직 실전 검증 전인 후보가 전역 표준처럼 굳어질 수 있다. |
| 경로 문제 | `docs/templates/global-harness-candidates/` 기준 문구가 전역 배포 위치와 맞지 않을 수 있다. |

따라서 현재 원칙은 다음과 같다.

```text
Phase 7 검증 전에는 전역 배포하지 않는다.
현재 후보 폴더를 임시 canonical source로 유지한다.
pilot 결과를 반영한 뒤 전역 배포 여부를 판단한다.
```

### 4.4 전역 template bundle의 성격

전역 template bundle은 모든 하네스에 강제되는 법전이 아니다.

역할은 다음에 가깝다.

- 하네스 설계의 기본값
- 모델 중립 실행 구조의 기준
- 공통 운영 module의 reference
- 새 하네스 생성 시 반복 질문을 줄이는 설계 출발점

각 하네스는 다음을 가질 수 있어야 한다.

- 필요한 module만 선택
- 하네스별 override
- 도메인별 schema/rubric
- adapter별 구현 차이
- 전역 template에 되먹임할 candidate 기록

즉 전역 template은 기본값을 제공하되, 하네스별 특수성을 지워서는 안 된다.

## 5. signal-routing 합의

### 5.1 문제의 출발점

`docs-organization` v1은 문서 구조가 복잡해졌을 때 정리가 필요하다는 성장 신호를 정의한다.

예를 들어 다음과 같은 상황이다.

- docs 루트에 서로 다른 성격의 문서가 많이 쌓인다.
- README, reference, handoff, review note가 섞인다.
- 사람이 파일 역할을 바로 구분하기 어렵다.

하지만 현재 구조만으로는 사용자가 이 신호를 어떻게 알게 되는지가 분명하지 않다.

사용자가 모든 하네스의 `docs/` 폴더를 직접 열어보며 확인하는 것은 비효율적이다. 가치투자 21단계 전체와 여러 반복 업무에 하네스를 적용할 경우, 수동 점검은 사실상 작동하지 않는다.

비슷한 요구는 다른 module에서도 이미 나타났다.

| module | 알림/경고 필요성 |
|---|---|
| security-baseline | secret, credential, 격리 파일, 외부 공개 위험은 즉시 사용자에게 보여야 한다. |
| qa-scaffold | `human_approval_needed`, `stop_required`, `security_review_needed` 같은 escalation을 표시해야 한다. |
| docs-organization | 문서 구조가 복잡해지면 정리 필요 신호를 사용자에게 알려야 한다. |
| observability | 반복 병목, trim 후보, 자동화 후보를 관찰하고 후속 판단으로 연결해야 한다. |
| candidate-ledger | 후보가 승격, 보류, 폐기 판단 지점에 도달하면 사용자 확인이 필요할 수 있다. |

따라서 알림/경고는 단순 후보가 아니라 v5 구조에서 빠진 공통 인터페이스일 가능성이 높다.

### 5.2 합의된 방향

공통 알림/경고 기능은 별도 module로 검토한다.

권장 이름은 `signal-routing`이다.

이름을 `notification` 또는 `alerting`으로 좁히지 않는 이유는, 이 module의 핵심이 팝업이나 메시지 형식이 아니라 “신호를 어떻게 분류하고 어디로 보내는가”이기 때문이다.

파일명 후보:

```text
global-harness-signal-routing-template-v0.md
```

새 module이므로 v1이 아니라 v0로 시작한다.

### 5.3 signal-routing이 소유할 것

signal-routing은 언어와 문법을 소유한다.

구체적으로는 다음을 정의한다.

| 영역 | 설명 |
|---|---|
| severity 어휘 | 신호의 심각도와 처리 강도를 구분하는 공통 용어 |
| routing 규칙 | 신호가 approval, security, QA, observability, candidate-ledger 중 어디로 연결되는지 정하는 방식 |
| user-visible 기준 | 어떤 신호가 silent log에만 남으면 안 되고 사용자에게 보여야 하는지 |
| stop/approval 구분 | 즉시 멈춤, 승인 필요, 유지보수 알림, 개선 후보, 후보 기록을 구분 |
| adapter 책임 | 실제 UI, 대화 메시지, CLI 출력, 파일 기록은 adapter가 구현하도록 경계 설정 |
| safe notification 원칙 | secret 값 등 민감한 값은 알림에 노출하지 않는 원칙 |

### 5.4 signal-routing이 소유하지 않을 것

signal-routing은 모든 신호 목록을 중앙에서 관리하지 않는다.

이 원칙이 중요하다. signal-routing이 각 module의 모든 신호를 소유하면 다음 문제가 생긴다.

- security-baseline이 새 보안 신호를 추가할 때 signal-routing도 수정해야 한다.
- docs-organization이 새 정리 신호를 추가할 때 signal-routing도 수정해야 한다.
- observability가 새 병목 패턴을 발견할 때 signal-routing도 수정해야 한다.
- 결국 signal-routing이 모든 module 위에 있는 중앙 registry가 된다.

따라서 합의된 분리는 다음과 같다.

| 주체 | 소유하는 것 |
|---|---|
| signal-routing | severity, routing, 사용자 노출 기준, adapter 알림 원칙 |
| 각 module | 자신이 발생시키는 실제 signal 이름, 의미, evidence, trigger 조건 |
| adapter | 실제 알림 채널, 표시 방식, 사용자 확인 UI |

예시는 다음과 같다.

```text
security-baseline:
  "나는 secret_possible_detected라는 critical signal을 낸다."

docs-organization:
  "나는 docs_structure_complexity라는 maintenance signal을 낸다."

qa-scaffold:
  "나는 qa_escalation_required라는 action_required signal을 낸다."

signal-routing:
  "critical은 즉시 사용자에게 보여야 하고, 필요한 경우 stop_required로 연결한다.
   maintenance는 run-summary나 다음 행동 제안으로 노출한다.
   action_required는 approval-gate 또는 qa-scaffold escalation으로 연결한다."
```

### 5.5 초안 severity 후보

다음 severity는 아직 최종 확정이 아니라 Phase 7-0 scope note에서 검토할 초안이다.

| severity | 의미 | 일반 처리 |
|---|---|---|
| `critical` | 즉시 중단 또는 사용자 확인이 필요한 위험 | silent log 금지, 사용자에게 즉시 표시, 필요 시 stop |
| `action_required` | 사람이 승인하거나 조치해야 하는 상태 | approval-gate 또는 QA escalation 연결 |
| `maintenance` | 지금 당장 중단은 아니지만 정리나 관리가 필요한 상태 | run-summary, docs note, 다음 행동에 표시 |
| `improvement` | 반복 병목, trim 후보, 자동화 후보 | observability 또는 candidate-ledger 연결 |
| `candidate` | 새 schema, 상태값, source, 절차 후보 | candidate-ledger 기록, pilot-first 검증 |
| `info` | 참고용 정보 | 필요 시 기록, 사용자 즉시 알림은 기본 아님 |

이 표는 signal-routing v0 작성 시 재검토한다.
특히 `candidate`라는 severity 이름은 `candidate-ledger` module과 혼동될 수 있으므로, Phase 7-0 scope note에서 `tracking`, `review_candidate`, `candidate_signal` 같은 대체 이름을 검토한다.

### 5.6 타이밍 합의

signal-routing은 Phase 7의 첫 작업으로 다룬다.

권장 순서:

```text
Phase 7-0. signal-routing scope note 작성
Phase 7-0. global-harness-signal-routing-template-v0.md 작성
Phase 7-0. Claude Code 교차검증
Phase 7-0. v5 core registry에 반영할지 결정
Phase 7-A. Source Pack 소급 검증
```

signal-routing 없이 Source Pack 소급 검증을 하면 각 module의 경고와 알림이 제각각 표현될 수 있다. 따라서 Phase 7-A 전에 최소한의 signal-routing 계약을 먼저 잡는 것이 좋다.

## 6. Phase 7 구조 합의

Phase 7은 단일 pilot이 아니라 여러 검증 단계로 나눈다.

합의된 큰 구조는 다음과 같다.

```text
Phase 7-0. signal-routing scope note + v0 작성
Phase 7-A. Source Pack 소급 검증
Phase 7-B. 다음 하네스 실전 검증
Phase 7-C. 전역 배포 판단
```

각 단계의 의미는 다음과 같다.

| 단계 | 목적 | 산출물 |
|---|---|---|
| Phase 7-0 | 공통 signal-routing 계약을 먼저 정의 | signal-routing scope note, signal-routing v0 |
| Phase 7-A | Source Pack 경험과 v5 template 후보의 내부 일관성 확인 | Source Pack retrospective validation note |
| Phase 7-B | 다음 하네스에서 v5 template의 실제 사용성 검증 | pilot plan, pilot notes, evaluation note |
| Phase 7-C | 전역 배포, canonical source, packaging, portable path 판단 | packaging/deployment decision note |

핵심 표현은 다음과 같다.

```text
Phase 7-A는 필터다.
Phase 7-B가 진짜 pilot이다.
```

Phase 7-A는 빠른 내부 일관성 검사다. Phase 7-B는 새 하네스에서 실제로 적용해보는 독립 실전 검증이다.

## 7. Phase 7-A Source Pack 소급 검증

### 7.1 목적

Phase 7-A는 이미 만든 Source Pack에 v5 template 후보를 소급 적용해보는 검증이다.

목적은 다음과 같다.

- v5 module들이 Source Pack 구축 과정에서 실제로 발생한 문제를 설명할 수 있는지 확인한다.
- v5 core와 module 사이의 충돌이나 누락을 빠르게 발견한다.
- signal-routing v0가 실제 Source Pack 경험에서 필요한 신호를 포착할 수 있었는지 검토한다.
- 다음 하네스 실전 검증 전에 명백한 구조 오류를 줄인다.

### 7.2 한계와 확증 편향 경고

Phase 7-A는 독립 검증이 아니다.

Source Pack 경험을 바탕으로 v5 template를 만들었고, 다시 Source Pack에 대조하면 “잘 맞는다”는 결론이 나오기 쉽다.

이 검증은 내부 일관성 확인이다.

반드시 아래 한계를 기록한다.

```text
이 검증은 Source Pack 기반 내부 일관성 확인이다.
Source Pack 경험 자체가 잘못된 방향이었다면 이 검증은 그 오류를 잡지 못할 수 있다.
새 하네스와 다른 유형의 하네스에 대한 독립 검증은 Phase 7-B에서 수행한다.
```

Phase 7-A가 확인할 수 있는 것:

- Source Pack 현실과 v5 template가 충돌하지 않는가?
- 실제 발생했던 보안, QA, docs, 후보 관리 이슈를 module이 설명할 수 있는가?
- signal-routing이 필요한 사용자 노출 신호를 식별할 수 있는가?
- registry 경로, module 연결, docs 구조 같은 명백한 불일치가 있는가?

Phase 7-A가 확인할 수 없는 것:

- 새 하네스를 처음 만들 때 template가 사용하기 쉬운가?
- 수집형이 아닌 분석형, 판단형, 모니터링형 하네스에도 적합한가?
- Source Pack에서 잘못 처리한 방식이 template로 굳어진 것은 아닌가?
- 전역 배포 후 Codex와 Claude 전역 폴더 사이 drift를 실제로 막을 수 있는가?

### 7.3 Phase 7-A 산출물 명세

Phase 7-A의 산출물은 다음 파일로 둔다.

```text
global-harness-v5-phase7a-source-pack-retro-validation-note-2026-06-07.md
```

필수 내용은 다음과 같다.

| 항목 | 내용 |
|---|---|
| 검증 목적과 한계 | Source Pack 소급 검증의 목적, 확증 편향 경고, 독립 검증 아님을 명시 |
| 모듈별 소급 대조 결과표 | 각 module을 Source Pack에 대조하고 `applicable`, `needs adjustment`, `not applicable`, `unclear`로 판정 |
| signal-routing으로 포착했어야 할 신호 목록 | Source Pack 과정에서 사용자에게 보였어야 할 signal 후보 기록 |
| B 진입 전 수정 후보 목록 | 바로 수정할 것, B에서 관찰할 것, 보류할 것을 구분 |
| A에서 확인하지 못한 것 | 새 하네스 사용성, 다른 하네스 유형 적합성, 전역 배포 drift 등 |
| A 완료 판정 | `pass`, `pass with adjustments`, `blocked` 중 하나 |

### 7.4 모듈별 소급 대조 대상

Phase 7-A에서 최소한 아래 항목을 검토한다.

| module / 항목 | 검토 질문 |
|---|---|
| `design-preflight` | Source Pack 유형, 품질 축, 수준 선언, 7요소를 충분히 설명하는가? |
| `type-schema` | Source Pack schema/rubric 구조와 연결이 자연스러운가? |
| `security-baseline` | 실제 보안, 비공개 대화, raw data, 외부 공개 위험을 충분히 다루는가? |
| `approval-gate` | 논의와 실행, 위험 작업 승인 경계를 잘 설명하는가? |
| `qa-scaffold` | Source Pack QA와 공통 QA 범주 사이의 경계가 적절한가? |
| `comparison` | no-file 항목이다. v1 registry와 `design-preflight`, `qa-scaffold`, `approval-gate`, `observability` 연결 기준으로 Claude/Codex 비교 작업을 실행 구조로 설명할 수 있는가? |
| `observability` | run-summary, 병목, trim 후보, 반복 관찰을 충분히 설명하는가? |
| `candidate-ledger` | 새 상태값, source, schema 후보, 개선 후보를 대기실로 관리할 수 있는가? |
| `pilot-first / testing` | Source Pack 변경을 바로 정식화하지 않고 작게 검증하는 원칙이 작동하는가? |
| `checkpoint` | 긴 논의와 세션 재개를 compact checkpoint로 이어갈 수 있는가? |
| `docs-organization` | docs 성장 신호, README 색인, 이동 전후 참조 점검이 Source Pack에 맞는가? |
| `signal-routing` | 보안, QA, docs, candidate, observability 신호를 공통 계약으로 표현할 수 있는가? |

### 7.5 Phase 7-A 완료 판정

Phase 7-A는 아래 중 하나로 종료한다.

| 판정 | 의미 | 다음 행동 |
|---|---|---|
| `pass` | 큰 수정 없이 Phase 7-B로 이동 가능 | 다음 하네스 실전 검증 계획 수립 |
| `pass with adjustments` | 작은 수정 후 Phase 7-B 가능 | 수정 후보 반영 후 B로 이동 |
| `blocked` | B 전에 core/module 구조 수정 필요 | 구조 수정 후 A 일부 재검토 |

`pass`가 나와도 전역 배포를 의미하지 않는다. 전역 배포 판단은 Phase 7-B 이후 Phase 7-C에서 다룬다.

## 8. Phase 7-B 다음 하네스 실전 검증

### 8.1 목적

Phase 7-B는 v5 template 후보를 새 하네스에 실제로 적용하는 검증이다.

Phase 7-A가 Source Pack 기반 내부 일관성 검사라면, Phase 7-B는 독립 실전 검증이다.

검증 목적은 다음과 같다.

- 새 하네스를 처음 만들 때 v5 core가 실제로 도움이 되는지 확인한다.
- module 선택 과정이 과하게 무겁거나 헷갈리지 않는지 본다.
- Source Pack 수집형 하네스에서 나온 template가 다른 유형에도 작동하는지 검증한다.
- signal-routing, approval, QA, docs, candidate-ledger가 실제 진행 중 자연스럽게 연결되는지 확인한다.
- 전역 배포 전에 누락, 과잉, drift 위험을 기록한다.

### 8.2 사용자가 정해야 할 것

Phase 7-B를 시작하려면 사용자가 먼저 아래 질문에 답해야 한다.

| 질문 | 이유 |
|---|---|
| 가치투자 21단계 중 Source Pack 다음으로 만들 하네스는 무엇인가? | B의 pilot 대상이 결정되어야 실제 계획을 세울 수 있다. |
| 그 하네스의 유형은 무엇인가? | 수집형, 정규화형, 분석형, 모델링형, 판단형, 모니터링형에 따라 필요한 module이 달라진다. |
| 다음 하네스가 Source Pack의 직접 후속 입력을 받는가? | 산출물 계약과 downstream readiness 검증 방식이 달라진다. |
| 해당 하네스의 최종 산출물은 무엇인가? | design-preflight와 v5 core 적용 방식이 달라진다. |
| 실전 pilot에서 어느 정도까지 파일을 만들 것인가? | 전체 구축인지, 청사진만인지, 작은 slice인지 정해야 한다. |

현재 사용자, Codex, Claude Code 논의 결과 질문 1~2는 아래처럼 정리한다.

| 항목 | 합의 내용 |
|---|---|
| Phase 7-B 첫 실전 pilot 대상 | Phase 2 - Step 4 `Industry Primer` |
| 선택 이유 | Source Pack 다음 순서로 자연스럽고, Source Pack과 충분히 다른 유형이며, `Moat`처럼 앞단 입력 의존성이 과하지 않다. |
| 하네스 유형 | 산업 이해 / 구조화 / 분석 준비형 |
| 보조 성격 | reference 정리, glossary/schema 후보 정리 |
| 기준 문서 | `docs/reference/가치투자 리서치 21단계 구조화 버전.md` |

`Earnings Call`은 Step 3 Source Pack에 포함된 원자료 범주이지만, Source Pack이 무거워졌기 때문에 `Phase 2 - Step 3-2 Earnings Call` 하위 하네스로 분화할 수 있다.

다만 `Industry Primer`의 핵심 질문은 산업이 왜 존재하는지, 핵심 용어가 무엇인지, 구매자가 누구인지, 산업 성장을 결정하는 요인이 무엇인지에 관한 산업 수준 이해다. 따라서 `Earnings Call` 하네스는 `Industry Primer`의 blocking dependency가 아니다.

`Earnings Call` 산출물 계약은 Step 6 `Business Model` 또는 이후 `Financial Quality`, `Monitoring` 계열 하네스를 설계하기 전에 정리한다. Industry Primer pilot에서는 Earnings Call 자료를 optional reference로만 볼 수 있다.

### 8.3 유형 선택에 따른 검증 가치

Source Pack은 수집형 하네스다.

다음 pilot이 또 수집형이면 빠르게 검증할 수 있지만, 범용성 검증 가치는 상대적으로 낮다. 반대로 분석형, 판단형, 모니터링형처럼 Source Pack과 다른 유형이면 v5 template가 진짜 전역 구조로 작동하는지 더 잘 검증할 수 있다.

| 다음 하네스 유형 | 검증 가치 | 주의점 |
|---|---|---|
| 수집형 | 빠르게 적용 가능 | Source Pack과 비슷해 확증 편향이 남을 수 있다. |
| 정규화형 | Source Pack 산출물을 다음 단계 입력으로 읽는 계약 검증에 좋다. | schema/type-schema 연결이 중요해진다. |
| 분석형 | v5 template의 범용성 검증 가치가 크다. | QA/rubric과 산출물 계약이 복잡해질 수 있다. |
| 판단형 | approval-gate와 human decision 경계 검증에 좋다. | 사람 승인과 책임 경계가 두꺼워진다. |
| 모니터링형 | observability, checkpoint, candidate-ledger 검증에 좋다. | 반복 실행과 상태 관리가 중요해진다. |

권장 원칙:

```text
실제 가치투자 workflow상 자연스러운 다음 단계가 우선이다.
다만 가능하면 Source Pack과 다른 유형의 하네스가 Phase 7-B 검증 가치가 높다.
```

### 8.4 Phase 7-A와 B의 순서 관계

기본 원칙은 A 먼저, B 다음이다.

다만 B의 계획 수립은 A와 병렬로 진행할 수 있다.

| 작업 | A와 병렬 가능 여부 | 이유 |
|---|---|---|
| 다음 하네스 후보 결정 | 가능 | 사용자가 이미 방향을 알고 있으면 미리 정할 수 있다. |
| 다음 하네스 유형 분류 | 가능 | design-preflight 수준의 사전 판단은 A와 병렬 가능하다. |
| B pilot 계획 초안 | 조건부 가능 | A 결과에 따라 수정될 수 있음을 명시해야 한다. |
| B 실제 실행 | A 완료 후 권장 | A에서 발견된 명백한 수정 후보를 반영한 뒤 실행하는 편이 안전하다. |

합의된 결론:

```text
Phase 7-B 계획 수립은 Phase 7-A와 병렬로 진행할 수 있다.
다만 Phase 7-B 실제 실행은 Phase 7-A 완료 후 진행한다.
```

병렬로 진행할 수 있는 작업은 Industry Primer pilot plan 초안, 하네스 유형 정리, 사용할 module 후보 선정, 입력/출력 산출물 후보 정리다.

실제 Industry Primer 하네스 파일 생성, v5 template 적용 결과 판정, 전역화 판단은 Phase 7-A에서 명백한 수정 후보를 확인한 뒤 진행한다.

### 8.5 Phase 7-A 소급 검증 범위

Phase 7-A는 Source Pack 전체를 대상으로 하되, 실제 검토는 대표 사건과 핵심 폴더 중심으로 진행한다.

전체 대상으로 보는 영역:

- `harness/` 구조
- `docs/templates/global-harness-candidates/`
- `artifacts/runs/`
- `artifacts/catalog/`
- `artifacts/companies/`
- `.agents` / `.claude` adapter 구조
- README / MANIFEST / runbook 계열

대표 사건 중심으로 보는 항목:

- Source Pack이 예상보다 무거워진 사건
- `Earnings Call`을 Source Pack에서 분리하는 판단
- v4 단일 template에서 v5 core + modules 구조로 분리한 사건
- security 알림 필요성이 드러난 사건
- docs-organization 알림 필요성이 드러난 사건
- candidate-ledger / pilot-first 필요성이 드러난 사건
- Claude Code / Codex 교차검증 workflow
- IR taxonomy overlap 충돌 사건: `sec_equivalent_not_found_in_scoped_8k`, `security_quarantined` 처리 과정

IR taxonomy overlap 충돌 사건은 candidate-ledger, security-baseline, signal-routing, type-schema, approval-gate가 실제 현장에서 어떻게 연결됐어야 하는지 검증하는 데 유용하다.

Phase 7-A는 exhaustive audit이 아니라 소급 검증이다. 따라서 전체 구조를 놓치지 않되, 실제 판단은 대표 사건과 핵심 폴더를 중심으로 수행한다.

## 9. Phase 7-C 전역 배포 판단

Phase 7-C는 전역 배포 여부를 판단하는 단계다.

전역 배포는 Phase 7-A만으로 결정하지 않는다. Phase 7-B 실전 검증 결과를 본 뒤 판단한다.

Phase 7-C에서 다룰 항목은 다음과 같다.

| 항목 | 판단 질문 |
|---|---|
| Codex 전역 배포 | Codex 전역 폴더에 template bundle을 둘 준비가 되었는가? |
| Claude 전역 배포 | Claude Code 전역 폴더에 같은 bundle을 둘 준비가 되었는가? |
| canonical source | 원본은 Source Pack repo에 둘 것인가, 별도 bundle repo 또는 21단계 상위 하네스에 둘 것인가? |
| sync 방식 | Codex 전역과 Claude 전역 배포본의 drift를 어떻게 막을 것인가? |
| registry 경로 문구 | `docs/templates/global-harness-candidates/` 기준 문구를 portable한 bundle 기준 문구로 바꿀 것인가? |
| README/package guide | 전역 설치, 하네스별 적용, override 규칙을 별도 안내할 것인가? |
| version/changelog | template bundle 버전을 어떻게 추적할 것인가? |
| `harness-lab` 관계 | 전역 `harness-lab`은 계속 원본 유지할 것인가, 포인터만 둘 것인가? |

Phase 7-C에서 특히 중요한 원칙은 다음과 같다.

```text
원본은 하나여야 한다.
Codex 전역과 Claude 전역은 배포본이어야 한다.
두 전역 폴더를 손으로 각각 고치는 방식은 장기적으로 drift를 만든다.
```

## 10. 아직 결정되지 않은 질문

다음 질문은 아직 결정되지 않았다.

Phase 7-B의 다음 하네스와 유형은 Section 8.2에서 `Industry Primer`, 산업 이해 / 구조화 / 분석 준비형으로 정리했다.

| 질문 | 왜 중요한가 | 권장 처리 |
|---|---|---|
| signal-routing을 v1 core registry에 언제 올릴 것인가? | 새 module 발견 가능성과 과잉 registry 노출 사이 균형이 필요하다. | v0 작성 후 registry 반영 여부 결정 |
| signal-routing severity 어휘는 무엇으로 확정할 것인가? | 모든 module의 알림 강도 해석에 영향을 준다. | Phase 7-0 scope note에서 확정 |
| 전역 template bundle의 최종 canonical source는 어디인가? | Codex/Claude 전역 drift 관리에 영향을 준다. | Phase 7-C 또는 21단계 상위 하네스 설계 시 결정 |
| registry의 후보 폴더 기준 경로 문구를 어떻게 portable하게 바꿀 것인가? | 전역 배포 시 현재 경로가 틀어질 수 있다. | Phase 7-C packaging note에서 결정 |
| Phase 7-A 결과가 `pass with adjustments`이면 B 전에 어디까지 수정할 것인가? | 작은 수정과 구조 수정의 경계가 필요하다. | A validation note에서 수정 후보를 분류 |
| Source Pack 사례를 전역 bundle에 어느 정도 reference로 남길 것인가? | 도메인 예시가 전역 template로 역유입될 수 있다. | reference/example로만 제한 |

## 11. 작업 지도 반영 원칙

이 문서가 Claude Code 교차검증을 통과하면 작업 지도는 아래 방향으로 갱신한다.

### 11.1 Phase 7 구조 재편

현재 Phase 7을 다음 하위 단계로 나눈다.

```text
Phase 7-0. signal-routing scope note + v0
Phase 7-A. Source Pack 소급 검증
Phase 7-B. 다음 하네스 실전 검증
Phase 7-C. 전역 배포 판단
```

### 11.2 Phase 7-0 작업 후보

작업 지도에 다음 항목을 추가한다.

| 상태 | 작업 | 산출물 |
|---|---|---|
| todo | signal-routing scope note 작성 | `global-harness-v5-phase7-signal-routing-scope-note-2026-06-07.md` |
| todo | signal-routing v0 후보 파일 작성 | `global-harness-signal-routing-template-v0.md` |
| todo | signal-routing Claude Code 교차검증 PASS 확인 | validation note |
| todo | signal-routing을 v1 core registry에 반영할지 결정 | registry decision |

### 11.3 Phase 7-A 작업 후보

작업 지도에 다음 항목을 추가한다.

| 상태 | 작업 | 산출물 |
|---|---|---|
| todo | Source Pack 소급 검증 note 작성 | `global-harness-v5-phase7a-source-pack-retro-validation-note-2026-06-07.md` |
| todo | module별 소급 대조 결과 작성 | validation note |
| todo | signal-routing으로 포착했어야 할 신호 목록 작성 | validation note |
| todo | B 진입 전 수정 후보 목록 작성 | validation note |
| todo | A 완료 판정 기록 | pass / pass with adjustments / blocked |

### 11.4 Phase 7-B 작업 후보

작업 지도에 다음 항목을 추가한다.

| 상태 | 작업 | 산출물 |
|---|---|---|
| done | 다음 하네스 후보와 유형 결정 | user decision: Phase 2 - Step 4 `Industry Primer`, 산업 이해 / 구조화 / 분석 준비형 |
| todo | 다음 하네스 pilot plan 작성 | pilot plan |
| todo | v5 core와 필요한 module만 선택 적용 | pilot notes |
| todo | 적용 중 누락, 과잉 규칙, drift 기록 | evaluation note |
| todo | B 완료 판정 기록 | pilot decision |

### 11.5 Phase 7-C 작업 후보

작업 지도에 다음 항목을 추가한다.

| 상태 | 작업 | 산출물 |
|---|---|---|
| todo | 전역 배포 여부 결정 | deployment decision note |
| todo | Codex/Claude 전역 배포 구조 결정 | packaging note |
| todo | canonical source 위치 결정 | packaging note |
| todo | registry 경로 문구 portable화 여부 결정 | packaging note |
| todo | version/changelog 정책 결정 | packaging note |
| todo | 전역 `harness-lab` 수정 여부는 계속 보류 또는 별도 논의 | deferred decision |

## 12. 사용자 결정 요청 목록

이 문서가 교차검증을 통과한 뒤, 사용자는 아래 질문에 답해야 한다.

질문 1~2는 사용자, Codex, Claude Code 논의를 거쳐 아래처럼 답변 완료 상태로 둔다.

| 우선순위 | 질문 | 현재 답변 |
|---|---|---|
| 1 | Source Pack 다음으로 만들 가치투자 21단계 하네스는 무엇인가? | Phase 2 - Step 4 `Industry Primer` |
| 2 | 그 하네스는 어떤 유형인가? | 산업 이해 / 구조화 / 분석 준비형. 보조적으로 reference 정리와 glossary/schema 후보 정리 성격을 가진다. |

관련 보정:

| 항목 | 합의 내용 |
|---|---|
| `Earnings Call` 위치 | Step 3 Source Pack의 하위 분화 후보로 본다. |
| `Earnings Call`과 Industry Primer 관계 | Industry Primer의 blocking dependency가 아니다. |
| `Earnings Call` 계약 정리 시점 | Step 6 `Business Model` 또는 이후 Financial Quality/Monitoring 계열 하네스 전까지 산출물 계약을 정리한다. |

질문 3~5도 사용자, Codex, Claude Code 논의를 거쳐 아래처럼 답변 완료 상태로 둔다.

| 우선순위 | 질문 | 현재 답변 |
|---|---|---|
| 3 | B 계획 수립을 A와 병렬로 할까, A 완료 후 할까? | B 계획 수립은 A와 병렬로 진행할 수 있다. B 실제 실행은 A 완료 후 한다. |
| 4 | signal-routing은 Phase 7-0에서 v0까지 만들까, scope note만 먼저 만들까? | Phase 7-0에서 scope note와 v0까지 작성한다. 단, v0는 severity/routing/user-visible 원칙 중심으로 작게 유지한다. |
| 5 | Phase 7-A에서 Source Pack 전체를 볼까, 대표 사건/폴더만 볼까? | Source Pack 전체를 대상으로 하되, 실제 검토는 대표 사건과 핵심 폴더 중심으로 한다. |

Q5의 대표 사건에는 IR taxonomy overlap 충돌 사건(`sec_equivalent_not_found_in_scoped_8k`, `security_quarantined`)을 반드시 포함한다.

아래는 새로 결정할 질문이 아니라, 이미 합의된 원칙을 Phase 7 시작 전에 다시 확인할 항목이다.

| 확인 항목 | 확인 내용 |
|---|---|
| 전역 배포 시점 | 전역 배포는 Phase 7-B 실전 검증 이후에만 판단한다. |

현재 합의 기준의 권장 답은 다음과 같다.

| 질문 | 권장 답 |
|---|---|
| B 계획 수립 시점 | A와 병렬 계획 가능, B 실제 실행은 A 완료 후 |
| signal-routing 처리 | Phase 7-0에서 scope note와 v0까지 작성 |
| Phase 7-A 범위 | Source Pack 전체를 보되, 대표 사건과 현재 구조를 중심으로 검토 |
| 전역 배포 시점 | Phase 7-B 실전 검증 이후 판단 |

## 13. 이 문서의 다음 단계

이 문서 작성 후 다음 순서로 진행한다.

1. 사용자가 이 문서를 Claude Code에 전달한다.
2. Claude Code가 누락, 과잉, 이견을 검토한다.
3. 이견이 있으면 Codex와 다시 논의한다.
4. 합의가 되면 이 문서를 수정한다.
5. 사용자가 결정해야 할 질문에 답한다.
6. Codex와 Claude Code가 사용자 결정을 검토한다.
7. 최종 consensus note를 기준으로 작업 지도를 갱신한다.
8. Phase 7-0부터 실행한다.

현재 이 문서는 Claude Code 교차검증 PASS 상태이며, 사용자 결정 Q1~Q5 완료 상태다.
다음 단계는 이 문서를 기준으로 작업 지도를 업데이트한 뒤 Phase 7-0을 실행하는 것이다.
