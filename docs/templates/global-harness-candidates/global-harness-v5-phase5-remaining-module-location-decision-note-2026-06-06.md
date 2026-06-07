# Phase 5 Remaining Module Location Decision Note

- 작성일: 2026-06-06
- 대상: `comparison`, `file-template`, `type-schema`, `adapter-template`, `meta-orchestrator`
- 기준 문서: `global-harness-v5-design-preflight-extraction-note-2026-06-06.md`, `global-harness-core-structure-template-v1.md`, `global-harness-v5-work-map.md`
- 상태: Phase 5 module registry 검증의 남은 후보 위치 결정

## 1. 목적

v4 extraction note에서 발견된 추가 후보 5개를 v5 core registry, 별도 module, 기존 module 연결, backlog 중 어디에 둘지 결정한다.

핵심 원칙:

- `registry 등재 여부`와 `별도 파일 생성 여부`를 분리해서 판단한다.
- registry는 발견 가능성을 위한 목록이다. 별도 파일이 없어도 등재할 수 있다.
- 별도 파일은 반복 필요성과 독립 실행 절차가 확인될 때 만든다.
- v5 core가 파일 생성 template나 adapter 상세까지 관리하는 인상을 주지 않는다.

## 2. 판정 기준

| 판정 | 의미 |
|---|---|
| registry | 새 하네스 설계자가 "이 필요가 생기면 어디를 봐야 하는가"를 알아야 하므로 v1 registry에 노출한다. 별도 파일은 필수가 아니다. |
| backlog | 중요하지만 아직 v1 registry에 노출할 만큼 처리 위치나 반복성이 확정되지 않았다. work map에서 추적한다. |
| existing module link | 새 파일을 만들지 않고 기존 module의 일부 판단 또는 검증 단계에 연결한다. |
| reference-only | 전역 template가 아니라 역사적 baseline, 특정 도메인 예시, 또는 향후 판단 재료로만 둔다. |

각 후보는 아래 7개 필드로 판정한다.

| 필드 | 기록 방식 |
|---|---|
| 성격 | 이 후보가 실행 구조, schema, adapter, 파일 구조, orchestration 중 무엇에 가까운지 |
| registry 등재 여부 | v1 Section 12에 노출할지 여부 |
| 별도 파일 생성 여부 | 지금 `*-template-v0.md` 파일을 만들지 여부 |
| 기존 module 연결 | 연결해야 할 기존 module이나 실제 하네스 위치 |
| 최종 위치 | `registry yes/no, file yes/no, 연결: ...` 형식의 한 줄 요약 |
| 이유 | 그 위치가 적절한 이유 |
| 나중에 재검토할 조건 | 별도 module이나 registry 승격을 다시 볼 조건 |

## 3. comparison

| 필드 | 판정 |
|---|---|
| 성격 | 실행 구조와 운영 승인에 가까운 후보. 두 모델이나 두 실행자가 같은 입력을 병렬로 처리하고 결과를 비교한 뒤 운영 반영 여부를 판단한다. |
| registry 등재 여부 | yes |
| 별도 파일 생성 여부 | no |
| 기존 module 연결 | `design-preflight`는 comparison 필요 여부를 판단한다. `qa-scaffold`는 공통 rubric을 제공한다. `approval-gate`는 운영 반영 승인을 맡는다. `observability`는 비교 결과 기록을 맡는다. |
| 최종 위치 | registry yes, file no, 연결: `design-preflight` / `qa-scaffold` / `approval-gate` / `observability` |
| 이유 | comparison은 QA만의 문제가 아니라 실행 구조 문제다. 다만 지금 별도 실행 절차 파일을 만들 만큼 안정된 반복 패턴은 아직 부족하다. |
| 나중에 재검토할 조건 | 실제 pilot에서 Codex/Claude Code 비교 실행이 반복되고, 비교 결과 기록 형식과 승인 절차가 안정되면 별도 comparison module을 검토한다. |

주의:

- comparison을 `qa-scaffold`에 흡수한다고 단정하지 않는다.
- `qa-scaffold`의 역할은 두 실행 결과를 같은 기준으로 평가하게 하는 공통 rubric 연결이다.
- comparison의 실행 구조 판단은 `design-preflight`와 이후 pilot에서 다룬다.

## 4. file-template

| 필드 | 판정 |
|---|---|
| 성격 | 하네스별 파일 생성 template 또는 상세 scaffold 후보 |
| registry 등재 여부 | no |
| 별도 파일 생성 여부 | no |
| 기존 module 연결 | v1 core의 공통 원장 구조, `design-preflight`의 청사진, v4 reference |
| 최종 위치 | registry no, file no, 연결: v1 core / `design-preflight` / v4 reference |
| 이유 | 지금 registry에 올리면 v5 core가 파일 생성 template까지 관리한다는 인상을 줄 수 있다. 파일 생성 구조는 하네스 유형과 실제 pilot을 더 본 뒤 분리하는 편이 낫다. |
| 나중에 재검토할 조건 | 여러 하네스에서 같은 파일 생성 scaffold가 반복되고, core structure와 module template만으로는 매번 같은 빈칸이 생길 때 별도 module을 검토한다. |

## 5. type-schema

| 필드 | 판정 |
|---|---|
| 성격 | 하네스 유형별 schema/rubric 구조를 정하기 위한 연결 후보 |
| registry 등재 여부 | yes |
| 별도 파일 생성 여부 | no |
| 기존 module 연결 | `design-preflight`는 유형과 품질 축을 판단한다. `qa-scaffold`는 검증 기준으로 구체화한다. 실제 schema 파일은 각 하네스의 `harness/schemas/`에 둔다. |
| 최종 위치 | registry yes, file no, 연결: `design-preflight` / `qa-scaffold` / 각 하네스의 `harness/schemas/` |
| 이유 | 새 하네스 작성자는 유형별 schema/rubric 구조가 필요할 때 어디를 봐야 하는지 알아야 한다. 하지만 전역 type-schema 파일은 아직 실제 반복 사례가 부족하다. |
| 나중에 재검토할 조건 | 수집형, 분석형, 판단형 등 여러 하네스에서 같은 schema/rubric 패턴이 반복되면 별도 type-schema module v0를 검토한다. |

중요한 구분:

- 여기서 `harness/schemas/`는 각 하네스 프로젝트 안의 개별 `harness/schemas/` 위치를 뜻한다.
- 전역 type-schema module 저장 위치를 뜻하지 않는다.
- 전역 type-schema module은 필요성이 검증된 뒤 별도 후보로 만든다.

## 6. adapter-template

| 필드 | 판정 |
|---|---|
| 성격 | Codex/Claude Code 같은 adapter 작성 방식의 상세 template 후보 |
| registry 등재 여부 | no |
| 별도 파일 생성 여부 | no |
| 기존 module 연결 | v1 core의 thin adapter 원칙과 adapter boundary |
| 최종 위치 | registry no, file no, 연결: v1 core thin adapter 원칙 |
| 이유 | v1 core는 adapter가 얇아야 한다는 원칙을 직접 다룬다. 지금 별도 adapter template을 registry에 올리면 core가 adapter 상세 구현까지 관리한다는 인상을 줄 수 있다. |
| 나중에 재검토할 조건 | Codex/Claude Code 외 추가 adapter가 생기고, adapter 생성 시 반복되는 frontmatter, routing, handoff 규칙이 안정되면 별도 adapter-template module을 검토한다. |

## 7. meta-orchestrator

| 필드 | 판정 |
|---|---|
| 성격 | 여러 하네스를 묶거나 여러 실행 구조를 조율하는 상위 orchestration 후보 |
| registry 등재 여부 | no |
| 별도 파일 생성 여부 | no |
| 기존 module 연결 | `design-preflight`의 실행 구조 선택지, Phase 7 pilot validation |
| 최종 위치 | registry no, file no, 연결: `design-preflight` / Phase 7 pilot validation |
| 이유 | meta-orchestrator는 단일 하네스 template보다 상위 운영 구조에 가깝다. 실제 multi-harness pilot 없이 전역 module로 올리면 범위가 과해진다. |
| 나중에 재검토할 조건 | 여러 하네스를 연결하는 실제 pilot에서 orchestration 계약, 승인 지점, 산출물 handoff가 반복되면 별도 meta-orchestrator module을 검토한다. |

## 8. v1 Registry 반영 결과

v1 registry에 추가할 항목:

| module | 현재 후보 파일 | 사용할 때 |
|---|---|---|
| comparison | 별도 파일 없음. `design-preflight`(필요 여부), `qa-scaffold`(공통 rubric), `approval-gate`(운영 반영 승인), `observability`(비교 결과 기록) 연결 | 두 모델 실행 결과를 같은 기준으로 비교하고 운영 반영 여부를 판단할 때 |
| type-schema | 별도 파일 없음. `design-preflight`(유형/품질 축 판단), `qa-scaffold`(검증 기준), 각 하네스의 `harness/schemas/` 연결 | 하네스 유형별 schema/rubric 구조를 정해야 할 때 |

v1 registry에 추가하지 않을 항목:

| 후보 | 처리 |
|---|---|
| file-template | work map backlog/reference에서 추적 |
| adapter-template | work map backlog/reference에서 추적 |
| meta-orchestrator | work map backlog/reference에서 추적 |

## 9. Work Map 반영 결과

| 항목 | 반영 |
|---|---|
| Phase 5 remaining module location task | done 처리 |
| `comparison mode` 열린 질문 | 결정됨으로 변경 |
| extraction note 추가 후보 열린 질문 | 결정됨으로 변경 |
| Backlog | `file-template`, `adapter-template`, `meta-orchestrator`를 후속 재검토 후보로 유지 |
| Phase 6 | `comparison`은 별도 module v1 후보가 아니라 registry no-file 연결 항목으로 정리 |

## 10. 결론

Phase 5의 remaining module location 판단은 아래처럼 확정한다.

| 후보 | registry | 별도 파일 | 최종 처리 |
|---|---|---|---|
| comparison | yes | no | existing modules 연결 |
| type-schema | yes | no | existing modules와 각 하네스 `harness/schemas/` 연결 |
| file-template | no | no | backlog/reference |
| adapter-template | no | no | backlog/reference |
| meta-orchestrator | no | no | backlog/reference |

이 결정으로 Phase 5 module registry 검증은 완료로 볼 수 있다.
다음 단계는 Phase 6 module v0 검토와 v1 후보화 범위 논의다.
