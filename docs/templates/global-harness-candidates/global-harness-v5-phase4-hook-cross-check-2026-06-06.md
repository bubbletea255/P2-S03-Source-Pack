# Global Harness v5 Phase 4 Hook Cross-Check

- 작성일: 2026-06-06
- 상태: Phase 4 검토 note. v5 core 또는 module template 자체가 아니다.
- 검토 범위:
  - `global-harness-core-structure-template-v1.md` Section 11
  - `global-harness-approval-gate-template-v0.md`
  - `global-harness-qa-scaffold-template-v0.md`
  - `global-harness-security-baseline-template-v0.md`

## 1. 한 줄 결론

blocking conflict는 없다.

v1 core의 approval, QA, security hook은 모두 "모든 하네스가 반드시 정의해야 하는 최소 의무" 수준으로 작성되어 있고, 각 module v0는 그 의무를 실제 문구, 상태값, 체크리스트, 예외 처리로 상세화한다.

따라서 Phase 4 기준에서는 v1 core 본문을 즉시 수정할 필요가 없다.

## 2. 판단 기준

이번 cross-check에서는 아래 기준으로 보았다.

| 기준 | 의미 |
|---|---|
| core가 너무 많이 복사했는가 | module 상세 규칙을 v1 core가 과도하게 가져오지 않았는가 |
| core가 너무 얇은가 | 모든 하네스에 필요한 최소 안전 의무가 빠지지 않았는가 |
| module과 충돌하는가 | 같은 항목을 서로 다른 기준으로 설명하지 않는가 |
| 다음 Phase로 넘길 항목이 있는가 | 지금 고정하지 말고 registry/module v1 검토 때 다룰 후보가 있는가 |

## 3. Approval Hook 검토

### v1 core hook

v1 core는 각 하네스가 실행 전에 아래를 정해야 한다고 요구한다.

- 자동으로 수행 가능한 작업
- 멈추고 사용자 승인을 받아야 하는 작업
- 보호해야 할 파일과 경로

또한 삭제, 이동, schema 변경, 운영 catalog 반영, 외부 제출은 명시 승인 없이 수행하지 않는다고 둔다.

### approval-gate v0

approval-gate v0는 아래를 상세화한다.

- 논의/평가/검토 요청은 실행 승인이 아니다.
- 파일 수정은 명시적 요청이 필요하다.
- 삭제, 대량 이동, 운영 catalog/index 반영, schema 변경, 외부 업로드/공개/배포, 보안 격리 파일 복구/열람, 기존 하네스 실행 절차 변경은 특히 조심한다.
- 실행 전 수정 범위와 수정하지 않는 범위를 짧게 알린다.

### 판정

aligned.

v1 core는 승인 경계의 존재를 강제하고, approval-gate v0는 사용자의 실제 문장과 위험 작업 분류를 다룬다. 역할이 겹치지 않는다.

### Phase 5/6로 넘길 후보

approval-gate v1 후보를 만들 때 아래 항목을 core 예시로 올릴지, module 상세로만 둘지 다시 볼 수 있다.

- 기존 하네스 실행 절차 변경
- 보안 격리 파일 복구/열람

현재 v1 core에서는 각각 수정 경계와 security hook으로 포괄되므로 Phase 4 blocking 이슈는 아니다.

## 4. QA Hook 검토

### v1 core hook

v1 core는 모든 하네스가 최소한 아래를 정의해야 한다고 요구한다.

- 무엇을 검증할지
- 어떤 파일을 확인할지
- 실패 시 어디서 멈출지

세부 상태값, rubric, 재검증 절차는 qa-scaffold module로 넘긴다.

### qa-scaffold v0

qa-scaffold v0는 아래 공통 질문을 둔다.

- 최종 산출물은 어디에 있는가?
- 입력과 출력이 연결되는가?
- 실패와 확인 필요가 기록됐는가?
- 다음 하네스가 읽을 수 있는가?
- 사람이 승인해야 할 지점이 표시됐는가?

또한 `pass`, `partial_pass`, `unverified`, `fail`, `stopped`, `repair_required` 같은 상태값 후보를 둔다.

### 판정

aligned.

v1 core는 QA 정의의 존재와 중단 지점을 요구하고, qa-scaffold v0는 각 하네스가 자기 산출물에 맞는 QA를 만들 수 있는 질문과 상태값을 제공한다.

### Phase 5/6로 넘길 후보

하네스 유형별 QA 질문 세트와 schema/rubric 선택 기준은 Phase 5의 `type-schema` 또는 `qa-scaffold` 경계 검토에서 다시 다룬다.

## 5. Security Hook 검토

### v1 core hook

v1 core는 새 하네스가 아래 기본선을 가져야 한다고 요구한다.

- secret
- credential
- `.env`
- 민감 파일
- 공개 범위

상세 보안 점검과 공개 범위 판단은 security-baseline module로 넘긴다.

### security-baseline v0

security-baseline v0는 아래를 상세화한다.

- 비밀값을 코드와 문서에 직접 쓰지 않는다.
- `.env`와 민감 파일은 git에 올리지 않는다.
- 보안 제품이 격리한 파일은 열거나 복구하지 않는다.
- 외부 공개는 별도 승인한다.
- 하네스별 공개 여부를 전역에서 단정하지 않는다.

### 판정

aligned.

v1 core는 보안 기본선의 존재를 강제하고, security-baseline v0는 흔한 실수와 공개 범위 판단을 구체화한다. core가 상세 절차를 과도하게 복사하지 않았고, module도 core 원칙과 충돌하지 않는다.

### Phase 5/6로 넘길 후보

security-baseline v1 후보를 만들 때 아래를 정리한다.

- `.gitignore` 최소 후보 세트
- 공개 저장소와 비공개 저장소별 권고 차이
- 보안 격리 파일 처리 문구를 approval-gate와 어떻게 연결할지

## 6. 종합 판정

| hook | 판정 | v1 core 수정 필요 | 다음 처리 |
|---|---|---|---|
| approval | aligned | 없음 | Phase 6 approval-gate v1 후보에서 위험 작업 분류 정리 |
| QA | aligned | 없음 | Phase 5/6에서 `qa-scaffold`와 `type-schema` 경계 검토 |
| security | aligned | 없음 | Phase 6 security-baseline v1 후보에서 공개/격리/`.gitignore` 상세 정리 |

## 7. 다음 단계

Phase 4는 완료로 본다.

다음은 Phase 5 `Available Modules Registry` 검증이다. 특히 아래를 확인한다.

- v1 module registry와 실제 후보 파일 목록이 맞는가
- 각 module의 사용 조건 한 줄이 실제 v0 내용과 맞는가
- module 경로 표기 방식을 어떻게 고정할 것인가
- `comparison mode`를 별도 module로 둘지 `qa-scaffold`에 포함할 것인가
- `file-template`, `type-schema`, `adapter-template`, `meta-orchestrator`를 registry 또는 backlog에 둘 것인가
