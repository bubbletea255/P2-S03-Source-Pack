# Global Harness v5 Core Gap Analysis

- 작성일: 2026-06-06
- 상태: Phase 2 분석 산출물. 아직 v5 core template 초안이 아니다.
- 목적: v5 core v1 초안을 쓰기 전에 `v0 후보`, `v4 baseline`, `v5 core scope consensus`의 차이를 정리한다.

## 1. 비교 대상

| 문서 | 역할 | 이번 분석에서의 취급 |
|---|---|---|
| `docs/templates/global-harness-candidates/global-harness-core-structure-template-v0.md` | 공통 원장 + 얇은 adapter 구조 v0 후보 | 후보 카드. 짧고 방향성 중심 |
| `docs/templates/공용_하네스_템플릿_체크리스트_v4.md` | 현재 공용 하네스 템플릿 baseline | 실제 기준 템플릿. 유지/추출 대상 |
| `docs/templates/global-harness-candidates/global-harness-v5-core-scope-consensus-2026-06-06.md` | 최신 합의 메모 | v5 core 범위 판단 기준 |
| `docs/templates/global-harness-candidates/global-harness-v5-work-map.md` | 작업 지도 | Phase 2 진행 상태 추적 |

## 2. 한 줄 결론

```text
v5 core v1은 v0를 단순 확장하는 문서가 아니라,
v4의 모델 중립 실행 구조를 추출하고,
consensus에서 합의한 core/module 경계와 최소 hook을 덧대는 새 후보 문서로 만드는 편이 가장 좋다.
```

이유:

- v0는 방향은 맞지만 너무 얇다.
- v4는 이미 공통 원장, 얇은 adapter, 산출물 계약, 수정 경계, AGENTS/CLAUDE 동기화, 비교 모드를 상당 부분 담고 있다.
- consensus는 v4 이후 새로 정리된 경계, 특히 `harness-lab` 비수정, module registry, Completion Contract 용어 미사용, 최소 hook 분리를 명확히 한다.

## 3. v0 요약

`global-harness-core-structure-template-v0.md`가 이미 담고 있는 것:

| 영역 | 내용 | 판단 |
|---|---|---|
| 목적 | 공통 업무 규칙과 모델별 실행 방식 분리 | 유지 |
| Source Pack 교훈 | 업무 의미는 `harness/`, Codex/Claude는 얇은 adapter | 유지 |
| 기본 구조 | `harness/`, `.agents/`, `.claude/`, `artifacts/`, `docs/` | 보강 필요 |
| 필수 질문 | 단일 원본, adapter 범위, 산출물 저장, 다음 하네스 입력 | 유지하되 core 문장으로 정리 |
| 아직 구체화할 것 | 폴더 구조, AGENTS/CLAUDE 포인터, 최소 세트, 단일/복수 기준 | v5 core v1에서 해결해야 함 |

v0의 성격:

```text
v0는 좋은 출발점이지만, v5 core template로 쓰기에는 후보 카드에 가깝다.
v5 core v1은 v0보다 훨씬 명시적인 파일 구조, 책임 경계, 최소 hook, module registry가 필요하다.
```

## 4. v4 baseline 요약

`공용_하네스_템플릿_체크리스트_v4.md`가 이미 담고 있는 core 요소:

| v5 core 후보 요소 | v4에 있는가 | 위치 또는 내용 | v5에서의 처리 |
|---|---:|---|---|
| 모델 중립 목적 | 예 | Claude Code와 Codex 양쪽에서 같은 구조로 실행 | 유지 |
| 공통 원장 | 예 | `harness/`를 공통 업무 원장으로 둠 | 유지 |
| 얇은 adapter | 예 | `.claude/agents/`, `.agents/skills/`, `.codex/` 분리 | 유지하되 adapter 현실 보강 |
| 공통 규칙 중복 금지 | 예 | adapter에 공통 업무 규칙을 길게 복사하지 않음 | 유지 |
| 권장 폴더 구조 | 예 | `AGENTS.md`, `CLAUDE.md`, `docs/`, `harness/`, adapters, `artifacts/` | 유지하되 v5 core용으로 압축 |
| `ORCHESTRATOR.md` | 예 | 전체 실행 흐름 템플릿 | 유지하되 runbook 분리 필요 |
| `MANIFEST.md` | 예 | 파일 역할, 의존 관계, 수정 경계 | 유지 |
| contract/procedure/schema/rubric | 예 | Part 2 템플릿 전반 | 유지하되 v5 core 본문에서는 최소 구조만 |
| 산출물 계약 | 예 | 중간/최종/비교/개선 기록을 파일로 남김 | v5 core 본문 핵심으로 유지 |
| 수정 경계 | 예 | 업무 의미, 절차, schema, rubric, adapter별 수정 위치 | 유지 |
| AGENTS/CLAUDE 동기화 | 예 | 같은 구조 안내, 한쪽 변경 시 다른 쪽 반영 | 유지 |
| 비교 모드 | 예 | 입력 공유/각자 생성, Judge 동일 rubric | v5 core 또는 선택 운영 규칙으로 위치 재검토 |
| 최소 완료/우수 기준 | 예 | contract와 rubric에 분리 | 유지하되 `Completion Contract` 용어는 쓰지 않음 |
| 사람 승인 지점 | 예 | 청사진, contract, orchestrator, 비교 모드, checklist | 유지하되 approval-gate 상세는 module로 분리 |
| QA/rubric | 예 | rubric 템플릿과 자동 실패 조건 | core에는 hook만, 상세는 module |
| 운영 개선 기록 | 예 | `artifacts/improvement-log.md` | 유지 |
| 템플릿 분할 가능성 | 예 | 운영 하네스가 늘면 분할 검토 | v5의 core + modules 방향과 연결 |

v4의 성격:

```text
v4는 v5 core의 중요한 baseline이다.
다만 v4는 공용 템플릿 전체 문서라서, v5 core로 가져올 때는 모델 중립 실행 구조만 추출해야 한다.
하네스 유형별 schema/rubric, 가치투자 21단계 예시, 세부 프롬프트는 v5 core 본문에 그대로 가져오지 않는다.
```

## 5. consensus 기준 요약

`global-harness-v5-core-scope-consensus-2026-06-06.md`의 핵심 합의:

| 항목 | 합의 |
|---|---|
| `harness-lab` | 전역 기초 지반. 수정하지 않고 포인터만 둔다. |
| v5 core | 모델 중립 실행 구조 템플릿이다. |
| v5 modules | 선택 운영 장치다. core 본문에 세부 규칙을 복사하지 않는다. |
| Completion Contract | 별도 용어로 만들지 않는다. 완료 기준은 산출물 계약 + QA 기준 + 승인/중단 조건으로 설명한다. |
| core 본문 핵심 | 목적, `harness-lab` 포인터, 공통 원장, 얇은 adapter, runbook 분리, 산출물 계약, 수정 경계, AGENTS/CLAUDE 동기화 |
| core 최소 hook | 승인 게이트, QA, 보안 |
| module registry | 관측 가능성, 체크포인트, docs organization, candidate ledger, pilot-first/testing, 상세 QA, 상세 보안 |
| core 밖 | 전역 `harness-lab` 수정, Agent Team 상세 런타임, 특정 도메인 예시, 자동화 스크립트, HTML/dashboard 템플릿 |

## 6. v0와 consensus 차이

| 영역 | v0 상태 | consensus 기준 | gap |
|---|---|---|---|
| v5 core 목적 | 공통 규칙과 모델별 실행 방식 분리 | 모델 중립 실행 구조 템플릿으로 명확히 정의 | 목적 문구 보강 필요 |
| `harness-lab` 관계 | 없음 | 전역 방법론으로 포인터만 둠 | 신규 추가 필요 |
| core/module 구분 | 없음 | core 본문, 최소 hook, module registry, core 밖으로 구분 | 신규 추가 필요 |
| 공통 원장 | 있음 | `harness/`가 업무 의미의 source of truth | 유지하되 문장 강화 |
| adapter | 있음 | `.agents/`, `.claude/`, `.codex/`는 실행 표면 | 유지하되 실제 경로 세분화 |
| runbook 분리 | 없음 | `ORCHESTRATOR.md`, runbook, procedure 구분 | 신규 추가 필요 |
| 산출물 계약 | 질문 형태로 있음 | core 본문 핵심 | 명시적 계약으로 승격 필요 |
| 수정 경계 | 없음 | `harness/`, adapter, `artifacts/`, `docs/` 책임 분리 | 신규 추가 필요 |
| AGENTS/CLAUDE 동기화 | 구체화 예정으로만 있음 | core 본문 핵심 | 신규 추가 필요 |
| 완료 기준 | 없음 | 별도 Completion Contract 없이 산출물/QA/승인 조합 | 신규 추가 필요 |
| 승인/QA/보안 hook | 없음 | core에 2-3줄 최소 hook | 신규 추가 필요 |
| module registry | 없음 | 선택 module 발견 가능성 확보 | 신규 추가 필요 |
| core 밖 항목 | 없음 | 다루지 않을 항목 명시 | 신규 추가 필요 |

판단:

```text
v0는 폐기하지 않는다.
하지만 v0를 직접 수정해 v1로 만드는 것보다,
v0를 참고 입력으로 삼아 별도 v1 후보를 만드는 편이 더 안전하다.
```

## 7. v4와 consensus 차이

| 영역 | v4 상태 | consensus 기준 | v5 처리 |
|---|---|---|---|
| `harness-lab` 관계 | 없음 | 전역 방법론 포인터 필요 | 신규 추가 |
| v5 core 목적 | 공용 설계 매뉴얼 전체 | 모델 중립 실행 구조에 집중 | 범위 축소 |
| core/module 구분 | 하나의 큰 문서 안에 대부분 포함 | core + modules 구조 | 구조 재편 |
| runbook 분리 | `ORCHESTRATOR.md`가 전체 실행 흐름을 포함 | `ORCHESTRATOR.md` / runbook / procedure 구분 | 신규 보강 |
| 산출물 계약 | 강함 | core 본문 핵심 | 유지 |
| 완료 기준 | 최소 완료 기준과 우수 산출물 기준 분리 | 새 용어 없이 산출물/QA/승인 조합 | 유지하되 용어 정리 |
| 승인 게이트 | 여러 위치에 있음 | core에는 최소 hook, 상세는 module | 분리 |
| QA/rubric | 상세함 | core에는 필요성과 최소 hook, 상세는 module | 분리 |
| 보안 기본선 | 거의 없음 | core에는 최소 보안 hook, 상세는 module | 신규 추가 |
| 관측 가능성 | 없음 | module registry에서 노출 | 신규 추가 |
| 체크포인트 | 없음 | module registry에서 노출 | 신규 추가 |
| docs organization | 단순 docs 폴더만 있음 | module registry에서 노출 | 신규 추가 |
| candidate ledger | 유형 분류를 작게 시작한다는 원칙은 있음 | candidate ledger module로 일반화 | 보강 |
| pilot-first | 하네스 하나씩 만들며 갱신하는 원칙은 있음 | module registry 또는 testing 계열로 분리 | 위치 재검토 |
| Source Pack 사례 | 본문 예시로 있음 | 특정 도메인 예시는 reference/example로 둠 | 본문에서는 줄이기 |
| Agent Team 상세 런타임 | 거의 없음 | core 밖 | 그대로 core 밖 |
| HTML/dashboard 템플릿 | 없음 | core 밖 | 유지 |

판단:

```text
v4는 v5 core의 baseline이지만, v5 core는 v4 전체를 압축한 문서가 아니다.
v5 core는 v4에서 모델 중립 실행 구조만 추출하고,
운영 장치와 세부 QA/schema/rubric은 module 또는 reference로 분리해야 한다.
```

## 8. 유지 / 수정 / 신규 추가 항목

### 8.1 유지할 항목

v5 core에 그대로 또는 압축해 유지할 것:

| 항목 | 근거 |
|---|---|
| `harness/` 공통 원장 | v0, v4, consensus가 모두 동의 |
| 얇은 adapter 원칙 | v0, v4, consensus가 모두 동의 |
| 공통 규칙 중복 금지 | 모델 중립성의 핵심 |
| `harness/ORCHESTRATOR.md` | 전체 흐름의 공통 원장 |
| `harness/MANIFEST.md` | 파일 역할과 의존 관계 지도 |
| `contracts/`, `procedures/`, `schemas/`, `rubrics/` | 좋은 결과의 구조를 외부화하는 기본 세트 |
| `artifacts/` 산출물 저장 | 대화에만 남기지 않는 핵심 계약 |
| `AGENTS.md` / `CLAUDE.md` 구조 안내 동기화 | adapter drift 방지 |
| 수정 경계 | v4에 이미 강하게 있음 |

### 8.2 수정해서 가져올 항목

v4에서 가져오되 v5 core에 맞게 줄이거나 위치를 바꿀 것:

| 항목 | 수정 방향 |
|---|---|
| 하네스 유형/품질 축 | v5 core 본문에서는 “하네스별 contract가 정의한다” 정도로 축소. 상세 유형표는 v4/reference에 둔다. |
| schema/rubric 템플릿 | v5 core에는 존재와 역할만. 상세 템플릿은 module 또는 v4 reference로 둔다. |
| 비교 모드 | 모델 중립 실행 구조와 관련은 있으나 상세 운영은 별도 section 또는 module 후보로 분리 가능. |
| approval 관련 문구 | core에는 최소 hook, 상세 위험도와 문구는 approval-gate module로 이동. |
| QA/rubric | core에는 무엇을 검증할지/어떤 파일을 확인할지/실패 시 어디서 멈출지 정도만. |
| Source Pack 사례 | v5 core 본문 예시로 길게 넣지 말고 reference/example로 연결. |
| 가치투자 21단계 예시 | v5 core 본문 밖으로 둔다. |

### 8.3 신규 추가할 항목

v5 core v1에 새로 들어가야 할 것:

| 항목 | 이유 |
|---|---|
| `harness-lab` 포인터 | v5 core가 전역 방법론을 대체하지 않음을 명시 |
| core/module 구분 | v5를 거대한 단일 문서로 만들지 않기 위함 |
| runbook 분리 | Source Pack 실전에서 ORCHESTRATOR와 1회 실행 절차 분리가 중요해짐 |
| 완료 기준 위치 설명 | `Completion Contract` 용어 없이 산출물 계약, QA 기준, 승인/중단 조건으로 설명 |
| 최소 보안 hook | v4에는 부족함 |
| module registry | 선택 module 발견 가능성 확보 |
| core 밖 항목 | 전역 `harness-lab`, Agent Team 런타임, 도메인 예시, 자동화 스크립트를 core에서 분리 |
| 적용 전 체크리스트 | v5 core가 과하게 적용되는 것을 방지 |

## 9. v5 core v1 목차 권장안

아래 목차는 consensus의 목차를 v4 baseline 반영 후 약간 구체화한 것이다.

```text
1. 목적: 모델 중립 실행 구조
2. 전역 harness-lab과 v5 core의 관계
3. core와 module의 구분
4. 공통 원장: harness/
5. 얇은 adapter: Claude Code / Codex 실행 표면
6. 권장 최소 파일 구조
7. ORCHESTRATOR / runbook / procedure 구분
8. 산출물 계약
9. 완료 기준의 위치: 산출물 계약 + QA 기준 + 승인/중단 조건
10. 수정 경계와 AGENTS/CLAUDE 동기화
11. 최소 hook: 승인 게이트, QA, 보안
12. Available Modules Registry
13. v5 core 밖에 둘 항목
14. 적용 전 체크리스트
```

## 10. v5 core v1 파일 생성 방식 권장

권장:

```text
새 파일로 `global-harness-core-structure-template-v1.md`를 만든다.
```

이유:

- v0는 후보 카드로 보존 가치가 있다.
- v4는 현재 baseline이므로 직접 덮어쓰지 않는다.
- v1은 v4와 consensus를 반영한 새 후보 문서로 두는 편이 변경 이력이 분명하다.
- 전역 `harness-lab`은 수정하지 않는다.

권장 경로:

```text
docs/templates/global-harness-candidates/global-harness-core-structure-template-v1.md
```

## 11. v5 core v1 작성 시 주의할 점

- v4 전체를 요약하려고 하지 않는다.
- 하네스 유형별 상세 schema/rubric을 core 본문에 넣지 않는다.
- Source Pack 예시는 길게 넣지 않는다.
- approval, QA, security 상세 절차를 core 본문에 복사하지 않는다.
- module registry는 발견 가능성을 위한 목록으로만 둔다.
- `Completion Contract`라는 새 용어를 만들지 않는다.
- `harness-lab`은 참조만 하고 수정하지 않는다.
- v1은 아직 전역화 확정 문서가 아니라 후보 template로 둔다.

## 12. 남은 판단

아래 질문은 v5 core v1 작성 전에 사용자와 확인하면 좋다.

| 질문 | 현재 권장 |
|---|---|
| v1을 새 파일로 만들까? | 예. `global-harness-core-structure-template-v1.md` |
| v4 원본을 수정할까? | 아니오. baseline으로 유지 |
| v0를 수정할까? | 아니오. 후보 카드로 유지 |
| README를 바로 갱신할까? | v1 생성 후 갱신 권장 |
| comparison mode를 core 본문에 둘까? | 최소 언급은 가능. 상세 운영은 module 또는 reference 검토 |
| pilot-first는 어디에 둘까? | core 본문보다 module registry에 두는 편이 현재 합의와 맞음 |

## 13. 다음 작업

이 분석을 기준으로 다음 단계는 아래 둘 중 하나다.

### 권장 경로

```text
global-harness-core-structure-template-v1.md 초안을 새로 작성한다.
```

초안 작성 후:

1. work map의 Phase 2를 완료 처리한다.
2. Phase 3 일부를 진행 처리한다.
3. README에 v1과 gap analysis 링크를 추가할지 결정한다.

### 보수적 경로

```text
이 gap analysis를 Claude Code 또는 사용자가 검토한 뒤 v1 초안 작성 여부를 확정한다.
```

이 경우 work map의 Phase 2는 `v5 core v1 후보 파일명 결정`만 미완료로 둔다.

