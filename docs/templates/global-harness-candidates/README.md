# 전역 하네스 후보 템플릿 모음 v0

- 작성일: 2026-06-06
- 출처: P2-S03 Source Pack 구축 과정에서 반복적으로 필요성이 확인된 공통 하네스 원칙
- 목적: Source Pack 전용 규칙과 모든 하네스에 적용 가능한 공통 패턴을 분리하고, 나중에 전역 `harness-lab` 또는 공용 템플릿 v5로 승격할 후보를 보관한다.
- 상태: 후보 모음. 아직 전역 적용 확정 아님.

이 폴더는 하나의 거대한 템플릿 문서가 아니다.
주제별로 작은 후보 템플릿을 나눠 보관하고, 다음 하네스에 실제 적용해본 뒤 검증된 것만 전역화한다.

## 왜 나눠서 보관하는가

Source Pack에서 얻은 교훈은 여러 성격으로 나뉜다.

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

| 파일 | 역할 | 전역화 우선순위 |
|---|---|---|
| `global-harness-core-structure-template-v0.md` | 공통 원장과 얇은 adapter 구조 | 높음 |
| `global-harness-approval-gate-template-v0.md` | 논의/검토와 실행/수정의 승인 경계 | 매우 높음 |
| `global-harness-qa-scaffold-template-v0.md` | 하네스별 QA를 설계하기 위한 공통 뼈대 | 높음 |
| `global-harness-observability-template-v0.md` | 하네스 운영 관찰 선택 섹션 | 높음 |
| `global-harness-security-baseline-template-v0.md` | API key, `.env`, 민감 파일, 보안 격리 기본선 | 높음 |
| `global-harness-checkpoint-template-v0.md` | compact checkpoint와 새 세션 인계 규칙 | 높음 |
| `global-harness-docs-organization-template-v0.md` | docs가 많아졌을 때의 정리 원칙 | 중간 |
| `global-harness-candidate-ledger-template-v0.md` | 새 분류/상태/유형 후보를 바로 확정하지 않고 누적하는 패턴 | 중간 |

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

이 폴더의 각 v0 파일은 아직 초안이다.
다음 단계에서는 하나씩 골라 실제 템플릿으로 구체화한다.

추천 순서:

1. approval gate
2. observability
3. security baseline
4. checkpoint
5. core structure
6. QA scaffold
7. docs organization
8. candidate ledger
