# 전역 하네스 후보 템플릿 모음

- 작성일: 2026-06-06
- 출처: P2-S03 Source Pack 구축 과정에서 반복적으로 필요성이 확인된 공통 하네스 원칙
- 목적: Source Pack 전용 규칙과 모든 하네스에 적용 가능한 공통 패턴을 분리하고, 나중에 전역 `harness-lab` 또는 공용 템플릿 v5로 승격할 후보를 보관한다.
- 상태: 후보 모음. 아직 전역 적용 확정 아님.

이 폴더는 하나의 거대한 템플릿 문서가 아니다.
주제별로 작은 후보 템플릿을 나눠 보관하고, 다음 하네스에 실제 적용해본 뒤 검증된 것만 전역화한다.

## 현재 길잡이

현재 v5 후보 작업의 기준 문서는 아래 순서로 읽는다.

1. `global-harness-v5-work-map.md` — 현재 작업 위치와 다음 순서
2. `global-harness-core-structure-template-v1.md` — v5 core 후보 본문
3. `global-harness-design-preflight-template-v0.md` — 새 하네스 청사진 전 설계 판단 module
4. `global-harness-v5-phase1-3_5-review-note-2026-06-06.md` — Phase 1~3.5 정합성 점검 결과
5. `global-harness-v5-phase4-hook-cross-check-2026-06-06.md` — approval/QA/security hook 검토 결과
6. `global-harness-v5-phase5-module-registry-comparison-2026-06-06.md` — registry와 실제 후보 파일 대조 결과
7. `global-harness-v5-phase5-module-usage-note-2026-06-06.md` — registry 사용 조건과 module v0 범위 대조 결과
8. `global-harness-v5-phase5-path-convention-note-2026-06-06.md` — registry 후보 파일 경로 표기 결정
9. `global-harness-v5-phase5-pilot-first-decision-note-2026-06-06.md` — pilot-first/testing 위치 결정
10. `global-harness-v5-phase5-remaining-module-location-decision-note-2026-06-06.md` — remaining module 위치 결정
11. `global-harness-v5-phase6-module-diagnostic-note-2026-06-06.md` — Phase 6 module v0 전체 진단
12. `global-harness-v5-phase6-observability-deep-review-note-2026-06-06.md` — observability 원본/v0 비교
13. `global-harness-observability-template-v1.md` — 현재 observability 후보
14. `global-harness-v5-phase6-approval-gate-scope-note-2026-06-07.md` — approval-gate v1 범위 합의
15. `global-harness-approval-gate-template-v1.md` — 현재 approval-gate 후보
16. `global-harness-v5-phase6-qa-scaffold-scope-note-2026-06-07.md` — qa-scaffold v1 범위 합의
17. `global-harness-qa-scaffold-template-v1.md` — 현재 qa-scaffold 후보
18. `global-harness-v5-phase6-security-baseline-scope-note-2026-06-07.md` — security-baseline v1 범위 합의 note
19. `global-harness-security-baseline-template-v1.md` — 현재 security-baseline 후보
20. `global-harness-v5-phase6-checkpoint-scope-note-2026-06-07.md` — checkpoint v1 범위 합의 note
21. `global-harness-checkpoint-template-v1.md` — 현재 checkpoint 후보
22. `global-harness-v5-phase6-docs-organization-scope-note-2026-06-07.md` — docs-organization v1 범위 합의 note
23. `global-harness-docs-organization-template-v1.md` — 현재 docs-organization 후보
24. `global-harness-v5-phase6-candidate-ledger-scope-note-2026-06-07.md` — candidate-ledger v1 범위 합의 note
25. `global-harness-candidate-ledger-template-v1.md` — 현재 candidate-ledger 후보
26. `global-harness-v5-phase7-planning-consensus-note-2026-06-07.md` — Phase 7 구조와 사용자 결정 Q1~Q5 합의 note
27. `global-harness-v5-phase7-signal-routing-scope-note-2026-06-08.md` — signal-routing v0 작성 전 범위 합의 note
28. `global-harness-signal-routing-template-v0.md` — 현재 signal-routing 후보
29. `global-harness-v5-phase7a-source-pack-retro-validation-note-2026-06-08.md` — Phase 7-A Source Pack 소급 검증 note
30. `global-harness-v5-phase7b-industry-primer-design-principles-note-2026-06-08.md` — Phase 7-B Industry Primer pilot plan 전 설계 원칙 note
31. `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v0-2026-06-08.md` — Phase 7-B Industry Primer pilot plan 질문/답변 누적 note v0
32. `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v1-2026-06-08.md` — Phase 7-B Industry Primer first slice pilot 실행 계획서 v1
33. `global-harness-v5-phase7b-industry-primer-blueprint-prep-note-2026-06-08.md` — Industry Primer blueprint 작성 전 rubric/QA/schema/handoff 합의 note
34. `global-harness-v5-phase7b-industry-primer-blueprint-prep-section11-consensus-note-2026-06-09.md` — blueprint-prep Section 11의 6개 질문 합의와 Claude Code PASS note
35. `global-harness-v5-phase7b-industry-primer-first-slice-rubric-calibration-note-2026-06-09.md` — Industry Primer first slice Section 1/3/5/13 rubric calibration과 Claude Code PASS note
36. `global-harness-v5-phase7b-industry-primer-blueprint-v0-2026-06-09.md` — Industry Primer first slice 하네스 blueprint v0 초안
37. `global-harness-v5-phase7b-industry-primer-pre-build-risk-review-note-2026-06-09.md` — Industry Primer 실제 하네스 파일 생성 전 첫 공식 challenge review와 blueprint v0 최소 수정 기준 note

전역 `harness-lab`은 이 폴더에서 직접 수정하지 않는다.
이 폴더의 문서는 후보이며, 실제 하네스 적용과 pilot 검증 후 전역화 여부를 별도로 결정한다.

## 왜 나눠서 보관하는가

Source Pack에서 얻은 교훈은 여러 성격으로 나뉜다.

- 설계 preflight
- 공통 구조 원칙
- 승인 게이트
- QA 뼈대
- 관측 가능성
- 보안 기본선
- 체크포인트
- docs 정리
- candidate 원장 패턴

이 주제들을 한 파일에 넣으면 처음에는 편하지만, 나중에 특정 항목만 수정하기 어렵다.
따라서 전역화 후보 단계부터 파일을 나눠 관리한다.

## 파일 지도

| 파일 | 역할 | 현재 사용법 |
|---|---|---|
| `global-harness-v5-work-map.md` | v5 후보 작업 지도 | 현재 위치, 결정 기록, 다음 작업 확인 |
| `global-harness-template-v5-discussion-handoff-2026-06-06.md` | 이전 논의 인계 문서 | v5 논의 배경 확인 |
| `global-harness-v5-core-scope-consensus-2026-06-06.md` | v5 core 범위 합의 메모 | core/module 경계 확인 |
| `global-harness-v5-core-gap-analysis-2026-06-06.md` | v0/v4/consensus 차이 분석 | v1 작성 근거 확인 |
| `global-harness-core-structure-template-v0.md` | 공통 원장과 얇은 adapter 구조 v0 후보 | 보존된 초기 후보 |
| `global-harness-core-structure-template-v1.md` | v5 core structure v1 후보 | 모델 중립 실행 core 본문 |
| `global-harness-v5-design-preflight-extraction-note-2026-06-06.md` | v4 설계 판단 장치 추출 note | v4에서 무엇을 보존했는지 확인 |
| `global-harness-design-preflight-template-v0.md` | design-preflight v0 후보 | 새 하네스 청사진 전 설계 판단 |
| `global-harness-v5-phase1-3_5-review-note-2026-06-06.md` | Phase 1~3.5 checkpoint review | Phase 4 진입 전 정합성 확인 |
| `global-harness-v5-phase4-hook-cross-check-2026-06-06.md` | Phase 4 hook cross-check | approval/QA/security hook과 module v0 충돌 여부 확인 |
| `global-harness-v5-phase5-module-registry-comparison-2026-06-06.md` | Phase 5 module registry comparison | v1 registry와 실제 후보 파일 목록 대조 |
| `global-harness-v5-phase5-module-usage-note-2026-06-06.md` | Phase 5 module usage note | registry 사용 조건과 module v0 범위 대조 |
| `global-harness-v5-phase5-path-convention-note-2026-06-06.md` | Phase 5 path convention note | registry 후보 파일 경로 표기 방식 결정 |
| `global-harness-v5-phase5-pilot-first-decision-note-2026-06-06.md` | Phase 5 pilot-first decision note | pilot-first/testing을 별도 module로 둘지 결정 |
| `global-harness-v5-phase5-remaining-module-location-decision-note-2026-06-06.md` | Phase 5 remaining module location decision note | comparison/type-schema registry 등재와 file-template/adapter-template/meta-orchestrator backlog 처리 결정 |
| `global-harness-v5-phase6-module-diagnostic-note-2026-06-06.md` | Phase 6 module diagnostic note | 개별 module v1 후보화 전 전체 v0 상태와 작업 순서 진단 |
| `global-harness-v5-phase6-observability-deep-review-note-2026-06-06.md` | Phase 6 observability deep review note | Source Pack observability 원본과 global observability v0 비교 |
| `global-harness-v5-phase6-approval-gate-scope-note-2026-06-07.md` | Phase 6 approval-gate scope note | approval-gate v1 후보 작성 전 승인 수준과 경계 정리 |
| `global-harness-v5-phase6-qa-scaffold-scope-note-2026-06-07.md` | Phase 6 qa-scaffold scope note | qa-scaffold v1 후보 작성 전 공통 QA 뼈대와 하네스별 QA/rubric/schema 경계 정리 |
| `global-harness-v5-phase6-security-baseline-scope-note-2026-06-07.md` | Phase 6 security-baseline scope note | security-baseline v1 후보 작성 전 최소 안전선과 하네스별 보안 절차 경계 정리 |
| `global-harness-approval-gate-template-v0.md` | 논의/검토와 실행/수정의 승인 경계 | Phase 4 hook 비교 대상 |
| `global-harness-approval-gate-template-v1.md` | 논의/검토와 실행/수정의 승인 경계 v1 | 현재 approval-gate 후보 |
| `global-harness-qa-scaffold-template-v0.md` | 하네스별 QA를 설계하기 위한 공통 뼈대 | Phase 4 hook 비교 대상 |
| `global-harness-qa-scaffold-template-v1.md` | 하네스별 QA를 설계하기 위한 공통 뼈대 v1 | 현재 qa-scaffold 후보 |
| `global-harness-security-baseline-template-v0.md` | API key, `.env`, 민감 파일, 보안 격리 기본선 | Phase 4 hook 비교 대상 |
| `global-harness-security-baseline-template-v1.md` | API key, `.env`, 비공개 대화 원문, 보안 격리, 외부 공개의 최소 안전선 v1 | 현재 security-baseline 후보 |
| `global-harness-v5-phase6-checkpoint-scope-note-2026-06-07.md` | Phase 6 checkpoint scope note | checkpoint v1 후보 작성 전 전역 skill, adapter, v5 template 경계 정리 |
| `global-harness-observability-template-v0.md` | 하네스 운영 관찰 선택 섹션 v0 | 보존된 초기 후보 |
| `global-harness-observability-template-v1.md` | 하네스 운영 관찰 선택 섹션 v1 | 현재 observability 후보 |
| `global-harness-checkpoint-template-v0.md` | compact checkpoint와 새 세션 인계 규칙 | Phase 5/6 module 검증 대상 |
| `global-harness-checkpoint-template-v1.md` | compact checkpoint와 새 세션 인계 규칙 v1 | 현재 checkpoint 후보 |
| `global-harness-v5-phase6-docs-organization-scope-note-2026-06-07.md` | Phase 6 docs-organization scope note | docs-organization v1 후보 작성 전 docs 구조 강제 여부, README 색인, 이동 전후 참조 점검 경계 정리 |
| `global-harness-docs-organization-template-v0.md` | docs가 많아졌을 때의 정리 원칙 | Phase 5/6 module 검증 대상 |
| `global-harness-docs-organization-template-v1.md` | docs가 많아졌을 때의 정리 원칙 v1 | 현재 docs-organization 후보 |
| `global-harness-candidate-ledger-template-v0.md` | 새 분류/상태/유형 후보를 바로 확정하지 않고 누적하는 패턴 | Phase 5/6 module 검증 대상 |
| `global-harness-v5-phase6-candidate-ledger-scope-note-2026-06-07.md` | Phase 6 candidate-ledger scope note | candidate-ledger v1 후보 작성 전 record/evidence, append-first, 승격/폐기/보류 경계 정리 |
| `global-harness-candidate-ledger-template-v1.md` | 새 분류/상태/유형 후보를 바로 확정하지 않고 누적하는 패턴 v1 | 현재 candidate-ledger 후보 |
| `global-harness-v5-phase7-planning-consensus-note-2026-06-07.md` | Phase 7 planning consensus note | Phase 7-0/A/B/C 구조, signal-routing, Source Pack 소급 검증, Industry Primer pilot, 전역 배포 판단 기준 |
| `global-harness-v5-phase7-signal-routing-scope-note-2026-06-08.md` | Phase 7-0 signal-routing scope note | signal-routing v0 후보 작성 전 severity, route, record, user visibility 경계 정리 |
| `global-harness-signal-routing-template-v0.md` | signal-routing v0 후보 | 알림/경고/에스컬레이션을 공통 signal envelope와 routing 계약으로 표현 |
| `global-harness-v5-phase7a-source-pack-retro-validation-note-2026-06-08.md` | Phase 7-A Source Pack 소급 검증 note | Source Pack 실제 구조와 대표 사건을 v5 core/module 후보에 read-only 방식으로 대조한 결과 |
| `global-harness-v5-phase7b-industry-primer-design-principles-note-2026-06-08.md` | Phase 7-B Industry Primer design principles note | pilot plan 작성 전 실행 주체, 산업/기업 단위, source tracking, QA, module 선택 원칙 정리 |
| `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v0-2026-06-08.md` | Phase 7-B Industry Primer pilot plan note v0 | pilot plan v1 작성 전 질문, 답변, 합의를 누적하는 살아있는 논의판 |
| `global-harness-v5-phase7b-industry-primer-pilot-plan-note-v1-2026-06-08.md` | Phase 7-B Industry Primer pilot plan note v1 | APP/adtech Industry Primer first slice pilot 실행 계획서 |
| `global-harness-v5-phase7b-industry-primer-blueprint-prep-note-2026-06-08.md` | Phase 7-B Industry Primer blueprint prep note | blueprint 작성 전 rubric, QA output, output schema, Section 13 handoff schema 합의 기준 |
| `global-harness-v5-phase7b-industry-primer-blueprint-prep-section11-consensus-note-2026-06-09.md` | Phase 7-B Industry Primer blueprint prep Section 11 consensus note | blueprint-prep Section 11의 6개 질문에 대한 하네스 구조, rubric, QA output, schema, handoff, module 연결 합의 기준 |
| `global-harness-v5-phase7b-industry-primer-first-slice-rubric-calibration-note-2026-06-09.md` | Phase 7-B Industry Primer first slice rubric calibration note | Section 1/3/5/13의 좋은 답, 보완 가능 답, blocked 기준과 full rubric/QA expansion 추적 기준 |
| `global-harness-v5-phase7b-industry-primer-blueprint-v0-2026-06-09.md` | Phase 7-B Industry Primer blueprint v0 | Industry Primer first slice 하네스의 contract, procedure, schema, rubric, output, adapter 경계와 module 연결 설계 초안 |
| `global-harness-v5-phase7b-industry-primer-pre-build-risk-review-note-2026-06-09.md` | Phase 7-B Industry Primer pre-build risk review note | 실제 파일 생성 전 challenge review, 즉시 blueprint 반영 항목과 backlog/trigger 구분 |

## 적용 단계

권장 순서:

1. 이 후보 모음을 읽고 다음 하네스에 필요한 항목을 고른다.
2. 새 하네스 청사진에 선택한 후보를 반영한다.
3. 실제 pilot 또는 초기 실행에서 잘 작동하는지 본다.
4. 반복적으로 유효하면 공용 템플릿 v5 후보로 승격한다.
5. 충분히 안정되면 전역 `harness-lab` 또는 전역 템플릿에 반영한다.

## 전역화 금지 원칙

아래는 전역 템플릿에 그대로 넣지 않는다.

- SEC EDGAR 전용 규칙
- IR taxonomy의 구체 document_type
- SEC-IR overlap의 세부 상태값
- 특정 회사 pilot 결과
- Source Pack raw/catalog 세부 필드

다만 아래 패턴은 전역화 후보가 될 수 있다.

- 새 분류는 candidate로 먼저 기록한다.
- 중복/무결성은 hash, notes, QA로 추적한다.
- 운영 반영과 schema 변경에는 사용자 승인이 필요하다.
- 파일 이동 전 참조 경로를 먼저 점검한다.

## 다음 작업

현재 작업 순서는 `global-harness-v5-work-map.md`를 따른다.

현재 다음 큰 단계:

1. 실제 Industry Primer first slice 하네스 파일 생성 여부를 사용자 승인 게이트에서 결정한다.
2. 승인 전에는 `harness/`, `.agents/`, `.claude/`, `artifacts/` 파일을 만들지 않는다.
