# v4 Design Preflight Extraction Note

- 작성일: 2026-06-06
- 상태: Phase 3.5 판정 문서. 이후 `global-harness-design-preflight-template-v0.md` 생성과 v1 Module Registry 갱신까지 완료됐다.
- 입력 문서: `docs/templates/공용_하네스_템플릿_체크리스트_v4.md`
- 연결 문서:
  - `docs/templates/global-harness-candidates/global-harness-core-structure-template-v1.md`
  - `docs/templates/global-harness-candidates/global-harness-design-preflight-template-v0.md`
  - `docs/templates/global-harness-candidates/global-harness-v5-work-map.md`

## 1. 목적

v4에서 v5 `design-preflight`로 보존할 설계 판단 장치를 선별한다.

이 문서는 v4 내용을 v1 core에 복사하기 위한 문서가 아니다.
v4의 좋은 설계 판단을 잃지 않기 위해 어떤 항목을 core gate, `design-preflight` module, 다른 module, reference-only, domain reference, exclude로 보낼지 판정한다.

핵심 원칙:

```text
v1 core에는 상세 방법론을 넣지 않는다.
v1 core에는 설계 판단을 반드시 지나가게 하는 진입 조건만 둔다.
상세 설계 판단은 design-preflight reference/module에서 다룬다.
```

## 2. 판단 기준

- 모든 하네스 설계 전에 필요한가?
- 모델 중립 실행 core에 직접 들어갈 내용인가?
- 별도 module로 분리할 상세 방법론인가?
- 특정 도메인에만 필요한 내용인가?
- v5에서 중복 또는 과잉인 내용인가?

판정값:

| 판정 | 의미 |
|---|---|
| core gate | v1 core에 짧은 진입 조건이나 체크리스트로 남겨야 한다. |
| design-preflight module | 별도 설계 판단 module로 분리할 가치가 있다. |
| other module | approval, QA, security, docs, checkpoint, observability, comparison 등 다른 module이 맡는 것이 맞다. |
| core/file structure | v1 core의 공통 원장, adapter, artifact contract, 수정 경계와 직접 연결된다. |
| v4 reference-only | 지금은 원본 v4를 참조하면 충분하다. |
| domain reference | 가치투자 21단계, Source Pack 같은 특정 도메인 reference로 둔다. |
| exclude | v5에서는 중복, 과잉, 또는 현재 방향과 맞지 않아 제외한다. |

## 3. v4 항목별 판정

| v4 항목 | 핵심 내용 | v5 판정 | 이유 | 적용 위치 |
|---|---|---|---|---|
| 0.1 이 문서의 목적 | 양쪽 모델에서 같은 구조로 실행, 산출물 파일화, contract/procedure/schema/rubric 명시, 유형별 품질 기준 | core gate / design-preflight module | 모델 중립 실행은 core, 유형별 품질 판단은 preflight에 필요 | v1 Section 1, design-preflight |
| 0.2 v4의 핵심 변경 | 유형 -> 산출물 역할 -> 품질 축 -> 수준 선언 -> 허용/금지 판단 -> 파일 계약 순서 | design-preflight module | v4 설계 지능의 핵심 순서 | design-preflight |
| 0.3 읽는 방법 | 목적별로 Part 1~4를 읽는 안내 | v4 reference-only / docs-organization | v5에서는 문서 지도가 별도 필요하지만 core 내용은 아님 | 후보 폴더 README 또는 docs module |
| 0.4 LLM 행동 규칙 | 예시 복사 금지, 유형/역할/품질축/엄격도 확인, 21단계 거대 Phase화 금지, 애매하면 후보 제시 | core gate / design-preflight module | v1만 읽는 모델이 파일 구조부터 만들지 않도록 막는 안전핀 | v1 Section 1/14, design-preflight |
| 0.5 핵심 원칙 5줄 요약 | 공통 원장, 설계 순서, 유형별 수준, 수집형 경계, 21단계는 작게 갱신 | core gate / design-preflight / domain reference | 항목별 위치가 다르므로 분해 보존 필요 | v1, design-preflight, domain reference |
| 1.1 이 구조가 해결하는 문제 | 공통 원장과 adapter 분리, drift 방지, 품질 계약 | core/file structure | v1 core가 이미 대부분 흡수 | v1 core |
| 1.2 하네스 유형 먼저 정하기 | 수집형, 정규화형, 분석형, 모델링형, 판단형, 모니터링형, 실행 구조 구분 | design-preflight module | 모든 설계 전 판단의 첫 단계 | design-preflight |
| 1.3 품질 축은 유형별로 다르다 | 유형별 좋은 결과와 나쁜 결과 구분 | design-preflight module | QA와 schema 선택 전에 필요 | design-preflight, QA module |
| 1.4 산출물 역할과 유형별 수준 선언 | 유형, 산출물 역할, 품질 축, 수준 선언, 실행 구조 용어 구분 | design-preflight module | 청사진의 핵심 필드 | design-preflight |
| 1.5 유형별 수준 선언 | 수집 엄격도, 정규화 엄격도, 분석 깊이, 모델링 엄격도, 판단 보조 수준, 모니터링 엄격도 | design-preflight module | 유형별 검증 두께를 정하는 기준 | design-preflight |
| 1.6 Source Pack에서 얻은 교훈 | 수집형은 분석하지 않고 원자료 인덱스와 누락 관리에 집중 | domain reference / design-preflight example | 수집형 예시로 유용하지만 global core 본문에는 부적절 | Source Pack reference, design-preflight 예시 |
| 1.7 하네스 7요소 체크 | 목표, 컨텍스트, 도구, 중간 산출물, 검증, 승인, 기록/개선 | core gate / design-preflight module | `harness-lab` 핵심과 연결되는 설계 안전장치 | v1 Section 14, design-preflight |
| 1.8 단일 하네스와 복수 하네스 판단 기준 | 단일/복수/meta/comparison 구조 판단 | design-preflight module / other module | 설계 초기에 구조를 정해야 하지만 meta 상세는 별도 | design-preflight, meta-orchestrator module |
| 1.9 가치투자 21단계 설계 원칙 | 21개 전문 하네스를 하나로 합치지 않고, 사람 승인과 meta-orchestrator를 둔다 | domain reference / core gate | 원칙은 중요하지만 특정 도메인 내용 | domain reference, v1의 일반 도메인 맥락 gate |
| 1.10 Phase 설계 원칙 | Phase는 목적, 입력, 출력, 검증 단위가 분명할 때 나눈다 | design-preflight module | 청사진과 Phase 설계 전에 필요 | design-preflight |
| 1.11 최소 완료 기준과 우수 산출물 기준 분리 | 다음 단계 진행 기준과 품질 개선 기준 분리 | other module / design-preflight module | contract/QA와 직접 연결되지만 청사진에서도 필요 | QA scaffold, contract template, design-preflight |
| 1.12 유형별 우수 산출물 기준 | 유형별 우수 결과 기준 | other module / design-preflight module | rubric/QA 쪽이 주 담당, preflight에서는 품질축 선택에 사용 | QA scaffold, design-preflight |
| 1.13 중간 산출물 보존 규칙 | 최종 산출물이 핵심 근거, 원자료, 확인 필요, 승인 지점을 잃지 않게 함 | core gate / other module | 산출물 계약과 QA 양쪽에 필요 | v1 artifact contract, QA/observability |
| 1.14 유형 분류는 작게 시작한다 | 완벽한 분류보다 실제 하네스 실행과 개선으로 정교화 | design-preflight module / candidate-ledger | 설계 부담을 낮추고 후보 상태를 허용 | design-preflight, candidate-ledger |
| 2.1 권장 폴더 구조 | `harness/`, adapters, docs, artifacts 구조 | core/file structure | v1 core가 이미 담당 | v1 core |
| 2.2 run-id 생성 규칙 | run-id, 충돌 처리, 비교 모드 폴더 | other module / core hook | artifacts 운영과 comparison에 필요하지만 preflight 본문은 아님 | observability/checkpoint/comparison, runbook |
| 2.3 청사진 템플릿 | 목표, 유형, 산출물 역할, 품질 축, 수준, 허용/금지 판단, Phase, 승인, 테스트 | design-preflight module | 별도 module로 분리할 가장 강한 후보 | design-preflight |
| 2.4 Contract 템플릿 | Phase 목적, 유형/품질축, 입력/출력, 완료 기준, 금지사항, 실패 처리, 승인 | core/file structure / other module | core artifact contract와 QA에 연결됨. preflight에는 필드만 반영 | contract template module, QA scaffold |
| 2.5 Procedure 템플릿 | 입력 확인, contract/schema/rubric 읽기, 유형별 작업 원칙, 자체 점검 | other module / design-preflight module | 절차 상세는 file-template 쪽, 유형 확인은 preflight와 연결 | procedure template module, design-preflight |
| 2.6 Schema 템플릿 | 유형별 산출물 필수 섹션 | other module | schema 상세는 유형별 output module 영역 | type-schema module, QA scaffold |
| 2.7 Rubric 템플릿 | 자동 실패 조건, 평가 항목, 유형별 평가 | other module | QA/rubric module이 주 담당 | QA scaffold |
| 2.8 Orchestrator 템플릿 | 실행 모드, 비교 모드, run-id, Phase 목록, 중단 조건, 수정 경계 | core/file structure / other module | v1 core와 runbook/comparison/observability로 분해 필요 | v1 core, comparison, observability |
| 2.9 Meta-orchestrator 템플릿 | 여러 전문 하네스의 순서와 의존 관계 관리 | other module / design-preflight module | 필요 여부 판단은 preflight, 상세는 meta module | design-preflight, meta-orchestrator module |
| 2.10 MANIFEST.md 템플릿 | 파일 역할, 산출물 의존 관계, 수정 경계 | core/file structure | v1 core가 직접 담당 | v1 core |
| 2.11 AGENTS.md / CLAUDE.md 템플릿 | 구조 안내 동기화, 자연어 라우팅, 수정 원칙 | core/file structure | v1 core가 직접 담당 | v1 core |
| 2.12 Claude Adapter 템플릿 | Claude agent가 공통 contract/procedure/schema를 읽는 방식 | core/file structure / adapter module | thin adapter 원칙과 연결되지만 상세 템플릿은 별도 | adapter template/reference |
| 2.13 Codex Skill 템플릿 | Codex skill이 공통 원장을 읽고 결과 저장 | core/file structure / adapter module | thin adapter 원칙과 연결되지만 상세 템플릿은 별도 | adapter template/reference |
| 2.14 Codex Orchestrator Skill 템플릿 | Codex 전체 진입점 skill | core/file structure / adapter module | v1의 thin adapter 원칙과 연결 | adapter template/reference |
| 2.15 artifacts README 템플릿 | artifacts 지도, run-id, 실행 목록 | other module / core hook | 산출물 계약과 관측 가능성에 필요 | observability/artifacts module |
| 3.1 표기 규칙 | 확인 필요, 추정, 사람 승인, 도구 실패, 판단 보류, 미검증 표기 | other module | QA와 운영 표준에 필요 | QA scaffold, approval-gate |
| 3.2 교차검증 체크리스트 | Claude/Codex 구조, schema/rubric, adapter, comparison, MANIFEST 점검 | other module / core gate | 모델 중립 검증에 중요하지만 상세는 QA/comparison | QA scaffold, comparison module |
| 3.3 유형별 최종 검수 규칙 | 유형별 최종 산출물 검수 질문 | other module / design-preflight module | QA가 주 담당, preflight는 품질축 선택에 참조 | QA scaffold, design-preflight |
| 3.4 비교 모드 운영 규칙 | 입력 공유/각자 생성, 운영 반영 승인, 동일 rubric 사용 | other module | comparison 전용 module 후보 | comparison module 또는 QA scaffold |
| 3.5 개선 기록 규칙 | 실행 후 문제, 원인, 적용 개선, 수정 위치 기록 | other module | observability/evolution 영역 | observability, candidate-ledger |
| 3.6 템플릿 자체를 고치는 기준 | 반복 문제, 새 유형, 분할 기준, candidate 운영 | other module | candidate-ledger와 pilot-first에 연결 | candidate-ledger, testing/evolution |
| 4.1 새 하네스 청사진 요청 프롬프트 | 청사진 요청 시 필요한 입력과 금지사항 | design-preflight module | 실제 사용성이 높은 preflight prompt | design-preflight |
| 4.2 실행 하네스 구성 요청 프롬프트 | 청사진 승인 후 파일 구성 요청 | v4 reference-only / file-generation module | preflight 이후 구성 단계의 prompt | file-generation reference |
| 4.3 하네스 유형만 먼저 정하는 프롬프트 | 파일 생성 전 유형 후보, 품질축, 금지 판단만 판단 | design-preflight module | preflight 단독 실행 prompt로 중요 | design-preflight |
| 4.4 Source Pack 같은 수집형 하네스 요청 예시 | 수집형 하네스 점검 예시 | domain reference / design-preflight example | 수집형 예시로 보존하되 global core에는 넣지 않음 | Source Pack reference |
| 4.5 최종 체크리스트 | 청사진/파일 구성 완료 전 전체 점검 | design-preflight module / other module | 설계 항목과 구조/QA 항목이 섞여 있어 분해 필요 | design-preflight, QA, core |

## 4. 우선 보존 후보

아래 항목은 v5에서 반드시 보존해야 한다.
다만 대부분은 v1 core 본문이 아니라 `design-preflight` module 또는 관련 module로 분리한다.

### 4.1 design-preflight 핵심 후보

`design-preflight` module이 생기면 최소한 아래를 포함해야 한다.

| 후보 | 출처 | 보존 이유 |
|---|---|---|
| 설계 순서 | 0.2, 0.5 | 유형 -> 산출물 역할 -> 품질 축 -> 수준 선언 -> 허용/금지 판단 -> 파일 계약 순서를 강제한다. |
| LLM 행동 규칙 | 0.4 | 예시 복사와 파일 구조 선진입을 막는다. |
| 하네스 유형 후보 | 1.2 | 수집/정규화/분석/모델링/판단/모니터링 경계를 정한다. |
| 실행 구조 후보 | 1.2, 1.8 | 단일/복수/meta/comparison을 업무 유형과 분리한다. |
| 산출물 역할 | 1.4, 2.3 | 다음 하네스 입력과 사람이 볼 산출물을 구분한다. |
| 품질 축 | 1.3, 1.4 | 좋은 결과의 기준을 유형별로 다르게 잡는다. |
| 유형별 수준 선언 | 1.5 | 하네스 두께와 QA 기준을 정한다. |
| 허용되는 판단 / 금지되는 판단 | 1.6, 2.3, 2.4 | 하네스가 넘지 말아야 할 판단 경계를 만든다. |
| 하네스 7요소 | 1.7 | 목표, 컨텍스트, 도구, 중간 산출물, 검증, 승인, 기록/개선을 빠뜨리지 않게 한다. |
| Phase 설계 원칙 | 1.10 | Phase를 예시 복사가 아니라 입력/출력/검증 단위로 나눈다. |
| 최소 완료 기준과 우수 산출물 기준 구분 | 1.11 | 다음 단계 진행 가능성과 품질 개선 기준을 분리한다. |
| 중간 산출물 보존 | 1.13 | 최종 요약 과정에서 원자료, 근거, 확인 필요, 승인 지점을 잃지 않게 한다. |
| 작게 시작하는 유형 분류 | 1.14 | 처음부터 완벽한 유형 체계를 만들려는 부담을 줄인다. |
| 청사진 템플릿 | 2.3 | 파일 생성 전에 설계 판단을 한 번에 확인하는 기본 산출물이다. |
| 유형만 먼저 정하는 프롬프트 | 4.3 | 애매한 요청을 파일 생성 전 판단 단계로 돌릴 수 있다. |
| 최종 체크리스트의 설계 항목 | 4.5 | preflight 결과가 빠지지 않았는지 확인한다. |

### 4.2 v1 core gate로 이미 반영된 후보

아래 항목은 상세는 module/reference로 두되, v1 core에는 짧은 gate로 남겨야 한다.
현재 v1에는 Section 1과 Section 14에 최소 반영되어 있다.

| gate | 출처 | 현재 상태 |
|---|---|---|
| 하네스 유형 먼저 정하기 | 1.2 | v1 Section 14 체크리스트에 반영 |
| 산출물 역할과 품질 축 확인 | 1.3, 1.4 | v1 Section 1/14에 반영 |
| 유형별 수준 선언 | 1.5 | v1 Section 1/14에 반영 |
| 하네스 7요소 확인 | 1.7 | v1 Section 1/14에 반영 |
| 도메인 프로그램 맥락 확인 | 1.9 | v1 Section 1/14에 일반화해 반영 |
| 중간 산출물 보존 | 1.13 | v1 artifact contract와 다음 검증에서 추가 점검 필요 |

### 4.3 다른 module로 보내야 할 후보

| 후보 | 출처 | 권장 위치 |
|---|---|---|
| approval 표기와 사람 승인 지점 | 3.1, 3.4, 4.5 | approval-gate |
| QA 자동 실패 조건과 평가 rubric | 2.7, 3.2, 3.3 | qa-scaffold |
| run-id, 실행 목록, artifacts 지도 | 2.2, 2.15 | observability / checkpoint |
| comparison mode | 2.8, 3.4 | comparison module 또는 qa-scaffold 확장 |
| template 변경 기준과 후보 상태 운영 | 3.6 | candidate-ledger / testing |
| docs/folder 안내 | 0.3, 2.1 | docs-organization / v1 core |
| adapter 상세 템플릿 | 2.12, 2.13, 2.14 | adapter template/reference |
| schema/rubric 상세 | 2.6, 2.7 | type-schema / qa-scaffold |

## 5. 제외 또는 reference-only 후보

아래 항목은 보존하되 v5 core나 `design-preflight` 본문에 그대로 넣지 않는다.

| 항목 | 처리 | 이유 |
|---|---|---|
| v4 전체 문장과 긴 예시 | v4 reference-only | 그대로 옮기면 v5가 다시 단일 거대 템플릿이 된다. |
| Source Pack 상세 예시 | domain reference | 수집형 예시로 유용하지만 global module의 본문 기본값은 아니다. |
| 가치투자 21단계 그룹 예시 전체 | domain reference | Investment Research OS에는 중요하지만 global core에는 과도하다. |
| 유형별 schema 전문 | other module | design-preflight는 schema를 직접 만들기보다 어떤 schema가 필요한지 결정한다. |
| adapter 파일 전문 | adapter reference | 실행 표면 상세이지 설계 preflight의 본문은 아니다. |
| 실행 하네스 구성 요청 프롬프트 전문 | file-generation reference | 청사진 승인 후 단계라 preflight와 구분한다. |

현재 기준으로 완전한 `exclude` 항목은 많지 않다.
대부분은 v5에서 버릴 내용이라기보다 위치를 바꿔 보존할 내용이다.

## 6. 권장 `design-preflight` module 범위

별도 `global-harness-design-preflight-template-v0.md`를 만든다면 아래 범위가 적절하다.

### 포함할 것

- 설계 전 행동 규칙
- 설계 판단 순서
- 하네스 유형 후보와 선택 이유
- 산출물 역할
- 품질 축
- 유형별 수준 선언
- 허용되는 판단과 금지되는 판단
- 하네스 7요소
- 단일/복수/meta/comparison 구조 판단
- Phase 설계 원칙
- 최소 완료 기준과 우수 산출물 기준의 구분
- 중간 산출물 보존 질문
- 도메인 프로그램 맥락 확인
- 청사진 템플릿
- 유형만 먼저 정하는 프롬프트
- design-preflight 완료 체크리스트

### 포함하지 않을 것

- `harness/` 폴더 구조 전문
- contract/procedure/schema/rubric 상세 템플릿 전문
- Claude/Codex adapter 상세 템플릿 전문
- comparison mode 상세 운영 규칙
- improvement-log 상세 운영 규칙
- Source Pack 또는 가치투자 21단계 상세 예시 전문

## 7. 후속 결정 상태

이 note 이후의 처리 상태는 아래와 같다.

| 결정 항목 | 상태 | 메모 |
|---|---|---|
| 별도 `global-harness-design-preflight-template-v0.md` 후보 파일 생성 | 완료 | v4 reference-only로 두면 새 하네스 작성자가 설계 판단을 건너뛸 위험이 있어 별도 module v0로 분리했다. |
| v1 core Module Registry의 `design-preflight` 후보 파일 경로 갱신 | 완료 | `global-harness-core-structure-template-v1.md` Section 12가 새 v0 파일을 가리킨다. |
| `file-template`, `type-schema`, `adapter-template`, `meta-orchestrator` 추가 후보 처리 | 미정 | Phase 5에서 registry/backlog에 둘지 검토한다. 이 항목은 module 확정이 아니라 Phase 5 작업자가 extraction note 후보를 놓치지 않기 위한 추적이다. |
| `comparison mode` 위치 | 미정 | 별도 comparison module로 둘지 `qa-scaffold`에 포함할지 Phase 5에서 결정한다. |
| 가치투자 21단계 상세 원칙 위치 | 결정됨 | global core 본문이 아니라 domain reference로 유지한다. |
