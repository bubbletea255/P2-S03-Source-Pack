# Global Harness v5 Phase 5 Module Usage Note

- 작성일: 2026-06-06
- 상태: Phase 5 부분 검토 note. v5 core 또는 module template 자체가 아니다.
- 검토 범위:
  - `global-harness-core-structure-template-v1.md` Section 12 `Available Modules Registry`
  - registry에 연결된 module v0 후보 파일 8개
  - `pilot-first / testing` 후속 위치 결정

## 1. 한 줄 결론

v1 registry의 module 사용 조건은 전체 방향이 맞다.

다만 현재 v0 내용과 정확히 맞추기 위해 아래 세 줄은 다듬었다.

- `design-preflight`: 실제 v0가 실행 구조와 허용/금지 판단도 다루므로 사용 조건에 반영
- `qa-scaffold`: 현재 v0는 rubric/repair loop 상세가 아니라 QA 공통 질문과 상태값 후보 중심이므로 문구 축소
- `security-baseline`: `.env`, 민감 파일, 외부 공개 범위를 명시

## 2. module별 판정

| module | 검토 당시 registry 사용 조건 | 실제 v0 범위 | 판정 | 조치 |
|---|---|---|---|---|
| `design-preflight` | 하네스 유형, 산출물 역할, 품질 축, 수준 선언, 7요소, 도메인 프로그램 맥락을 정해야 할 때 | 유형, 산출물 역할, 품질 축, 수준 선언, 허용/금지 판단, 7요소, 단일/복수/meta/comparison 실행 구조, Phase 후보, 완료 기준, 도메인 맥락 | mostly aligned, 조금 좁음 | registry 한 줄 확장 |
| `approval-gate` | 논의/검토와 실제 수정/실행 승인 경계를 상세화할 때 | 논의와 실행 분리, 명시적 파일 수정 요청, 위험 작업 추가 승인, 실행 전 수정 범위 알림 | aligned | 유지 |
| `qa-scaffold` | QA 상태값, rubric, repair loop, 재검증 절차가 필요할 때 | 하네스별 QA 공통 질문, 권장 상태값 후보, Source Pack 전용 QA 비전역화, 향후 구체화 항목 | partly overstates current v0 | registry 한 줄 축소 |
| `security-baseline` | secret, credential, 격리 파일, 공개 범위를 점검할 때 | 비밀값 금지, `.env`/민감 파일 git 제외, 보안 격리 파일 처리, 외부 공개 승인 | aligned, 조금 좁음 | registry 한 줄 확장 |
| `observability` | run-summary에 운영 관찰 필드, 병목, trim 후보를 남길 때 | 선택 관찰 섹션, QA 실패 아님, 정확한 토큰 계측 아님, 기본 필드, 자동화 도입 조건 | aligned | 유지. Source Pack 원본 비교는 Phase 6에서 별도 수행 |
| `checkpoint` | 긴 작업을 compact checkpoint로 이어가야 할 때 | 원문 전체 저장 금지, compact checkpoint, 기본 저장 위치, 저장 모드 후보, 다음 세션 인계 | aligned | 유지 |
| `docs-organization` | docs가 많아져 역할별 정리와 참조 점검이 필요할 때 | 문서가 많아졌을 때 역할별 정리, 이동 전 참조 점검, 삭제 금지, handoff 과거 경로 보존 | aligned | 유지 |
| `candidate-ledger` | 새 분류, 상태값, 예외를 바로 schema에 넣지 않고 후보로 관찰할 때 | 새 분류/상태값/자료 유형/예외/branch 후보 누적, record 필드, 승인 요청 기준 후보 | aligned | 유지 |
| `pilot-first / testing` | 새 source, 새 자동화, 새 schema 변경을 운영 반영 전에 작게 검증할 때 | 전용 v0 파일 없음. 이후 `candidate-ledger`와 Phase 7 pilot validation에 묶어 처리하기로 결정 | aligned after decision | 별도 module v0 파일 없음 |

## 3. v1 registry 반영 내용

아래 세 줄을 v1 Section 12에 반영했다.

| module | 수정 방향 |
|---|---|
| `design-preflight` | `허용/금지 판단`, `실행 구조`를 포함하도록 확장 |
| `qa-scaffold` | `rubric`, `repair loop`, `재검증 절차`를 현재 v0 범위에 맞게 `QA 공통 질문`, `상태값 후보`, `실패/확인 필요 기록 기준`으로 축소 |
| `security-baseline` | `.env`, `민감 파일`, `외부 공개 범위`를 포함하도록 확장 |

## 4. 다음 단계

이 note의 반영까지 포함해 Phase 5의 "각 module의 사용 조건 한 줄이 실제 v0 내용과 맞는지 확인" 작업은 완료로 본다.

후속으로 module 경로 표기 방식 결정과 `pilot-first / testing` 위치 결정까지 완료됐다.
다음 작업은 `comparison mode`와 추가 module 후보의 위치를 결정하는 것이다.
