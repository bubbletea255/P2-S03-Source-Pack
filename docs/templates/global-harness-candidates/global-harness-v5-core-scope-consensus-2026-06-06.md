# Global Harness v5 Core Scope Consensus

- 작성일: 2026-06-06
- 상태: 논의 합의 메모. 아직 v5 core template, module template, 전역 `harness-lab`에 반영하지 않는다.
- 목적: Codex와 Claude Code가 논의한 v5 core 범위 합의를 보존하고, 이후 v5 core 초안 작성 시 기준점으로 사용한다.

## 1. 현재 결론

v5 core는 전역 `harness-lab`을 대체하거나 수정하는 문서가 아니다.

역할을 아래처럼 분리한다.

| 층위 | 역할 | 현재 처리 |
|---|---|---|
| 전역 `harness-lab` | 하네스 설계 철학, Phase 0-7, Agent/Skill/Orchestrator 방법론 | 원본 유지. 당장 수정하지 않음 |
| v5 core | Codex와 Claude Code가 같은 하네스를 실행하기 위한 모델 중립 실행 구조 | `docs/templates/` 안에서 후보 문서로 설계 |
| v5 modules | 승인, QA, 보안, 관측, 체크포인트, docs 정리 같은 선택 운영 장치 | 주제별 module template로 분리 |

핵심 정의:

```text
v5 core는 harness/ 공통 원장을 중심으로 업무 의미를 모델 중립적으로 고정하고,
Codex/Claude Code 같은 실행 환경은 얇은 adapter로 연결하며,
산출물 계약과 수정 경계를 통해 같은 하네스를 반복 실행 가능하게 만드는 최소 구조다.
```

## 2. `harness-lab`은 수정하지 않는다

현재 합의:

```text
전역 harness-lab은 기초 지반이다.
v5 core 논의가 충분히 검증되기 전에는 harness-lab을 수정하지 않는다.
```

v5 core에는 아래 정도의 포인터만 둔다.

```text
이 템플릿은 전역 harness-lab의 하네스 설계 방법론을 전제로 한다.
v5 core는 harness-lab을 대체하거나 수정하지 않고,
프로젝트별 모델 중립 실행 구조를 정의한다.
```

주의:

- `harness-lab` 내용을 v5 core에 길게 복사하지 않는다.
- v5 core 합의를 이유로 전역 `harness-lab`을 즉시 수정하지 않는다.
- 먼저 Source Pack과 다음 하네스에서 후보 템플릿을 검증한다.

## 3. Completion Contract 용어는 만들지 않는다

초기 논의에서 `Model-neutral completion criteria` 또는 `Completion Contract`라는 표현이 나왔지만, 최종 합의에서는 별도 용어로 승격하지 않는다.

이유:

- `Required outputs`와 `Output paths`는 기존의 산출물 계약과 겹친다.
- `Validation reference`는 QA hook과 겹친다.
- `Stop/approval conditions`는 승인/중단 조건과 겹친다.
- 새 용어가 늘어나면 v5 core 초안에서 혼란이 생길 수 있다.

대신 v5 core에서는 아래처럼 쓴다.

```text
완료 기준은 별도 adapter에 두지 않는다.
각 하네스는 harness/ 안의 산출물 계약, QA 기준, 승인/중단 조건을 통해 완료 여부를 정의한다.
Codex와 Claude Code adapter는 이 기준을 복사하지 않고 같은 원장을 참조한다.
```

## 4. v5 core 본문에 직접 둘 항목

아래 항목은 v5 core 본문에서 직접 정의한다.
별도 module로만 넘기면 모델 중립 실행 구조의 정체성이 흐려진다.

| 항목 | v5 core에 둘 내용 |
|---|---|
| v5 core 목적 | v5 core는 모델 중립 실행 구조 템플릿이다. 전역 `harness-lab`은 상위 설계 방법론으로 유지한다. |
| 공통 원장 | `harness/`가 업무 의미의 source of truth다. |
| 얇은 adapter | `.agents/`, `.claude/`, `.codex/` 등은 실행 표면이며, 공통 업무 규칙을 길게 복사하지 않는다. |
| runbook 분리 | `ORCHESTRATOR.md`는 전체 운영 원칙, runbook은 1회 실행 절차, procedure는 세부 작업 절차로 구분한다. |
| 산출물 계약 | 결과를 대화에만 남기지 않고 사람이 읽는 산출물과 기계가 읽는 산출물을 정해진 경로에 남긴다. |
| 수정 경계 | 업무 의미 변경은 `harness/`, 실행 방식 변경은 adapter, 산출물은 `artifacts/`, 논의/설계 자료는 `docs/`에 둔다. |
| AGENTS/CLAUDE 동기화 | `AGENTS.md`와 `CLAUDE.md`는 같은 구조 안내를 가져야 하며, 한쪽 변경 시 다른 쪽 drift를 점검한다. |

## 5. v5 core 본문에 최소 hook만 둘 항목

아래 항목은 모든 하네스에 중요하지만, 세부 절차는 하네스 유형과 위험도에 따라 달라진다.
따라서 v5 core 본문에는 2-3줄의 최소 원칙만 두고, 상세는 module template로 분리한다.

### 5.1 승인 게이트 hook

v5 core에 둘 최소 문구:

```text
논의, 검토, 설계와 실제 파일 수정, 실행, 삭제, 운영 반영을 구분한다.
삭제, 이동, schema 변경, 운영 catalog 반영, 외부 제출은 명시 승인 없이는 수행하지 않는다.
상세 승인 문구와 위험도 분류는 approval-gate module을 따른다.
```

### 5.2 QA hook

v5 core에 둘 최소 문구:

```text
모든 하네스는 최소한 무엇을 검증할지, 어떤 파일을 확인할지, 실패 시 어디서 멈출지 정의해야 한다.
세부 상태값, rubric, 재검증 절차는 qa-scaffold module을 따른다.
```

### 5.3 최소 보안 hook

v5 core에 둘 최소 문구:

```text
secret, credential, API key, .env, 대화 원문, 격리된 파일은 raw 산출물이나 docs에 무심코 저장하지 않는다.
상세 보안 점검과 공개 범위 판단은 security-baseline module을 따른다.
```

## 6. v5 core 본문에는 두지 않고 module registry에만 둘 항목

아래 항목은 유용하지만 v5 core 본문 규칙으로 넣으면 문서가 무거워진다.
따라서 v5 core 끝의 `Available Modules Registry`에서 이름, 사용 조건, 경로만 노출한다.

| module | 사용할 때 |
|---|---|
| observability | run-summary에 운영 관찰 필드, 병목, trim 후보를 남기고 싶을 때 |
| checkpoint | 긴 작업을 compact checkpoint로 이어가야 할 때 |
| docs-organization | docs가 많아져 역할별 정리와 참조 점검이 필요할 때 |
| candidate-ledger | 새 분류, 상태값, 예외를 바로 schema에 넣지 않고 후보로 관찰할 때 |
| pilot-first / testing | 새 source, 새 자동화, 새 schema 변경을 운영 반영 전에 작게 검증할 때 |
| detailed security-baseline | secret scan, 격리 파일 처리, 공개/비공개 판단이 필요할 때 |
| detailed qa-scaffold | 상태값, rubric, repair loop, 재검증 절차가 필요할 때 |

주의:

```text
module registry는 발견 가능성을 위한 목록이다.
module 세부 규칙을 v5 core 본문에 복사하지 않는다.
```

## 7. v5 core 밖에 둘 항목

아래 항목은 v5 core가 다루지 않는다.

| 항목 | 이유 |
|---|---|
| 전역 `harness-lab` 수정 | 아직 충분한 검증 전이며, 모든 하네스에 영향을 줄 수 있다. |
| Agent Team 상세 런타임 | `harness-lab` 또는 별도 agent-team 설계 문서의 영역이다. |
| 특정 도메인 예시 | Source Pack, SEC, IR, transcript 같은 사례는 reference/example로 둔다. |
| 자동화 스크립트 | 특정 도구나 런타임에 묶일 수 있으므로 core에 넣지 않는다. |
| HTML 리포트/대시보드 출력 템플릿 | 산출물 유형별 템플릿이지 모델 중립 실행 구조가 아니다. |

## 8. 현재 합의된 v5 core 목차 후보

아직 초안이 아니며, 이후 작성 시 출발점으로 사용한다.

```text
1. 목적: 모델 중립 실행 구조
2. harness-lab과 v5 core의 관계
3. core와 module의 구분
4. 공통 원장: harness/
5. 얇은 adapter: Codex / Claude Code 실행 표면
6. runbook / procedure / orchestrator 구분
7. 산출물 계약
8. 완료 기준의 위치: 산출물 계약 + QA 기준 + 승인/중단 조건
9. 수정 경계와 AGENTS/CLAUDE 동기화
10. 최소 hook: 승인 게이트, QA, 보안
11. Available Modules Registry
12. 적용 전 체크리스트
```

## 9. 다음 작업 후보

이 문서는 바로 적용하지 않는다.
다음 작업은 아래 순서가 자연스럽다.

1. 기존 `global-harness-core-structure-template-v0.md`와 이 합의 메모를 비교한다.
2. v5 core 초안에 포함할 A항목 본문을 작성한다.
3. 승인 게이트, QA, 보안 hook의 최소 문구를 확정한다.
4. `Available Modules Registry`를 만든다.
5. module template v0들을 v1 후보로 다듬을지 결정한다.
6. 최소 한 개의 다음 하네스에서 v5 후보 구조를 시험한다.
7. 충분히 검증된 뒤에만 전역 `harness-lab` 수정 여부를 별도로 논의한다.

## 10. 보류된 질문

아래 질문은 아직 확정하지 않았다.

- v5 core 초안을 별도 파일로 만들지, 기존 v4 템플릿의 다음 버전으로 만들지
- module registry의 실제 경로 표기를 어떤 형식으로 고정할지
- `AGENTS.md`와 `CLAUDE.md` drift 검사를 수동 체크리스트로 둘지, 나중에 자동화할지
- pilot-first를 `candidate-ledger` module에 포함할지, 별도 `pilot-testing` module로 분리할지
- Source Pack 사례를 v5 core 본문 예시로 둘지, reference 문서로만 둘지

