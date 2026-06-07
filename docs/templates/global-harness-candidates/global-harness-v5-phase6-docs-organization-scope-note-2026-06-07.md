# Global Harness v5 Phase 6 Docs Organization Scope Note

- 작성일: 2026-06-07
- 상태: Phase 6 docs-organization v1 후보 작성 전 scope note. module template 본문이 아니다.
- 대상: `global-harness-docs-organization-template-v0.md`
- 기준 문서:
  - `global-harness-core-structure-template-v1.md`
  - `global-harness-v5-work-map.md`
  - `README.md`
  - `global-harness-checkpoint-template-v1.md`

## 1. 목적

이 note는 `docs-organization` v1 후보에 무엇을 직접 담고, 무엇을 하네스별 docs 정리 정책으로 둘지 정리한다.

핵심 질문:

```text
docs-organization은 새 하네스가 처음부터 따라야 하는 강제 폴더 구조인가,
아니면 문서가 많아졌을 때 안전하게 정리하는 기준인가?
```

현재 판단은 후자다.

`docs-organization`은 초기 구조 강제 문서가 아니다.
문서가 늘어나고 역할이 섞였을 때, 파일을 삭제하지 않고 참조를 깨지 않으면서 사람이 찾기 쉽게 정리하기 위한 module이다.

## 2. docs-organization의 역할

`docs-organization`이 맡을 것:

| 역할 | 설명 |
|---|---|
| 성장 후 정리 기준 | 문서가 많아졌을 때 역할별로 나누는 기준을 제공한다. |
| 참조 보호 | 파일 이동 전후에 깨질 수 있는 경로와 링크를 점검하게 한다. |
| docs 색인 | `docs/README.md`가 무엇을 안내해야 하는지 정한다. |
| 기록 보존 | 정리 과정에서 과거 handoff, reference, pilot 기록을 삭제하지 않게 한다. |
| module 경계 | checkpoint, handoff, reference의 개념을 재정의하지 않고 필요한 module을 가리키게 한다. |

`docs-organization`이 맡지 않을 것:

| 비범위 | 이유 |
|---|---|
| 모든 새 하네스에 복잡한 docs 폴더를 강제 | 작은 하네스가 불필요하게 무거워진다. |
| 전체 프로젝트 파일 지도 대체 | 전체 파일 지도는 `harness/MANIFEST.md` 같은 manifest가 맡는다. |
| 모든 파일명 규칙 고정 | 도메인과 산출물 성격에 따라 달라진다. |
| 과거 handoff 내부 경로 일괄 수정 | handoff는 당시 기록이므로 역사성을 보존할 수 있다. |
| archive/삭제 정책 전체 | 프로젝트별 보존 정책과 연결된다. |

## 3. 처음부터 강제하지 않는 원칙

작은 하네스는 단순하게 시작할 수 있다.

초기에는 아래 정도로 충분할 수 있다.

```text
docs/
  README.md
  {필요한 문서들}
```

처음부터 `current/`, `design/`, `templates/`, `pilots/`, `handoff/`, `reference/`를 모두 만들 필요는 없다.

원칙:

- 문서가 적을 때는 루트 `docs/`와 `docs/README.md`만으로 시작할 수 있다.
- 역할이 섞이기 전까지 폴더를 과하게 나누지 않는다.
- 새 폴더를 만들 때는 그 폴더가 어떤 탐색 문제를 해결하는지 설명할 수 있어야 한다.
- 폴더 구조 자체를 하네스 품질로 착각하지 않는다.

## 4. 적용이 필요한 성장 신호

아래 신호가 보이면 `docs-organization`을 적용할 수 있다.

| 성장 신호 | 의미 |
|---|---|
| docs 루트에 서로 다른 성격의 문서가 많이 쌓인다 | 사람이 무엇부터 읽어야 할지 알기 어렵다. |
| 현재 기준 문서와 과거 기록이 섞인다 | 최신 문서와 historical record가 혼동된다. |
| design note, pilot note, handoff, reference가 같은 위치에 있다 | 문서 역할별 탐색이 필요하다. |
| README나 work map에서 같은 파일 설명을 반복한다 | 색인 정리가 필요하다. |
| 파일 이동 후 링크 깨짐이 걱정된다 | 이동 전후 참조 점검 절차가 필요하다. |
| 여러 모델이나 다음 세션이 같은 폴더를 읽어야 한다 | 읽는 순서와 현재 활성 문서 목록이 필요하다. |

문서 수 기준은 전역에서 고정하지 않는다.
다만 사람이 파일 목록을 보고 역할을 바로 구분하기 어려워지는 순간이 적용 신호다.

## 5. 역할별 폴더 후보와 의미

문서가 많아졌을 때 사용할 수 있는 후보 구조:

```text
docs/
  README.md
  current/
  design/
  templates/
  pilots/
  handoff/
  reference/
  session-checkpoints/
```

각 폴더의 기본 의미:

| 폴더 | 역할 | 비고 |
|---|---|---|
| `current/` | 현재 운영 기준, 현재 구조 지도, 최신 상태 문서 | 최신 기준 문서만 둔다. |
| `design/` | 설계 노트, 의사결정 초안, 구조 논의 | 작업 진행 중인 설계 맥락 |
| `templates/` | 재사용 가능한 템플릿과 후보 템플릿 | 전역 후보와 하네스별 템플릿을 구분할 수 있다. |
| `pilots/` | pilot 실행 결과, 검증 노트, 평가 | 전역화 전 실제 적용 기록 |
| `handoff/` | 다른 세션, 다른 모델, 다른 팀에게 넘기는 구조적 인계 문서 | checkpoint와 다르다. |
| `reference/` | 외부 자료, PDF, 도메인 참고, 보존용 자료 | 현재 기준과 혼동하지 않는다. |
| `session-checkpoints/` | compact session checkpoint | checkpoint module과 security-baseline 기준을 따른다. |

이 구조는 후보일 뿐이다.
하네스별 docs 규모와 도메인에 따라 일부만 선택할 수 있다.

## 6. `docs/README.md`의 역할과 최소 내용 기준

`docs/README.md`는 `docs/` 폴더 내부 색인이다.
`harness/MANIFEST.md` 같은 전체 프로젝트 파일 지도를 대체하지 않는다.

역할 구분:

| 문서 | 역할 |
|---|---|
| `harness/MANIFEST.md` | 하네스 전체 파일 지도. contract, procedure, schema, adapter, artifacts까지 포함한다. |
| `docs/README.md` | docs 폴더 내부 색인. docs 안의 문서 역할과 읽는 순서를 안내한다. |
| 특정 후보 폴더 `README.md` | 해당 후보 모음이나 하위 폴더의 파일 지도와 사용 순서를 안내한다. |

`docs/README.md`의 최소 내용:

| 항목 | 설명 |
|---|---|
| 폴더/파일 역할 표 | 주요 하위 폴더나 주요 파일이 무엇을 위한 것인지 설명한다. |
| 현재 활성 문서 목록 | 지금 기준으로 먼저 읽어야 하는 문서와 현재 기준 문서를 표시한다. |
| 다음 작업 또는 관리 기준 | 문서를 추가하거나 이동할 때 무엇을 갱신해야 하는지 적는다. |

유지 시점:

- docs 안에 주요 파일을 새로 만들 때
- docs 파일을 이동할 때
- 현재 기준 문서가 바뀔 때
- handoff, reference, template, pilot 문서가 늘어나 읽는 순서가 바뀔 때

`docs/README.md`는 모든 문서의 긴 설명을 담지 않는다.
어디에 뭐가 있고 무엇부터 읽을지를 알려주는 색인이면 충분하다.

## 7. 이동 전후 참조 점검

문서를 이동하기 전에는 참조 경로를 먼저 확인한다.
문서를 이동한 뒤에는 현재 운영 참조와 색인을 갱신한다.

이동 전 점검:

| 점검 대상 | 확인할 것 |
|---|---|
| `harness/` | contract, runbook, MANIFEST, ORCHESTRATOR에서 docs 경로를 직접 참조하는가 |
| README류 | 프로젝트 README, docs README, 후보 폴더 README가 해당 경로를 가리키는가 |
| work map / current map | 현재 상태 지도나 작업판에서 해당 파일을 기준 문서로 쓰는가 |
| handoff | 과거 인계 문서 안의 historical reference인지 현재 운영 참조인지 |
| adapter/skill | Codex/Claude Code adapter가 해당 docs 경로를 읽도록 되어 있는가 |

이동 후 정리:

- 현재 운영 참조는 새 경로로 갱신한다.
- docs/README.md 또는 하위 README의 파일 지도를 갱신한다.
- 전체 파일 지도에 영향이 있으면 `harness/MANIFEST.md`도 함께 확인한다.
- 이동 기록이 중요하면 work map, closeout note, change log 중 적절한 곳에 남긴다.
- historical handoff 내부 과거 경로는 무리하게 고치지 않는다.

중요:

```text
이동은 삭제가 아니다.
참조를 깨지 않고 사람이 찾기 쉽게 만드는 정리 작업이다.
```

## 8. 삭제 금지와 historical reference 보존

docs-organization의 기본 정리 방식은 삭제가 아니라 이동과 색인 정리다.

원칙:

- 정리 중 파일을 삭제하지 않는다.
- 보존할 가치가 없는 임시 파일이라도 삭제 전에는 사용자 승인 또는 하네스별 cleanup 기준을 따른다.
- 과거 handoff, pilot note, decision note는 당시 판단의 기록으로 남길 수 있다.
- historical reference 안의 과거 경로는 현재 경로와 달라도 보존할 수 있다.
- 현재 운영 기준과 historical record가 헷갈리면 파일을 수정하기보다 위치와 README 설명으로 구분한다.

historical reference를 모두 최신 경로로 고치려고 하면 당시 맥락이 깨질 수 있다.
반대로 현재 운영 문서가 과거 경로를 계속 가리키면 drift가 된다.

따라서 이동 후에는 historical reference와 현재 운영 참조를 구분한다.

## 9. checkpoint / handoff / reference 경계 포인터

이 section은 checkpoint, handoff, reference를 재정의하지 않는다.
개념 정의는 해당 module이나 하네스별 문서를 따른다.

docs-organization이 다루는 것은 위치와 색인이다.

| 항목 | 정의 위치 | docs-organization에서 다루는 것 |
|---|---|---|
| session checkpoint | `checkpoint` module | `docs/session-checkpoints/` 같은 위치와 docs 색인 여부 |
| 구조적 handoff | 하네스별 handoff 문서 또는 checkpoint module의 handoff 구분 | `docs/handoff/` 같은 위치와 historical/current 구분 |
| reference | 하네스별 docs 정책 | `docs/reference/` 같은 위치와 현재 기준 문서와의 구분 |
| template | 하네스별 또는 전역 template 정책 | `docs/templates/` 위치와 후보/현재 template 구분 |

원칙:

- checkpoint와 handoff의 정의를 docs-organization에서 복사하지 않는다.
- docs-organization은 각 문서 유형이 docs 안에서 어디에 놓이고 어떻게 찾히는지만 다룬다.
- 개념 정의를 복사해야 한다면 짧은 포인터만 둔다.

## 10. v1에 직접 담을 것

`docs-organization` v1에 직접 담을 항목:

| 항목 | 이유 |
|---|---|
| 목적: 삭제가 아니라 찾기 쉽게 정리 | module의 핵심 역할 |
| 처음부터 과도한 구조 강제 금지 | 작은 하네스가 무거워지는 것 방지 |
| 적용이 필요한 성장 신호 | 언제 module을 적용할지 판단 기준 |
| 역할별 폴더 후보 | 문서가 많아졌을 때 사용할 기본 선택지 |
| 폴더별 역할 정의 | current/design/templates/pilots/handoff/reference/checkpoint 혼동 방지 |
| `docs/README.md` 최소 내용 기준 | docs 내부 색인 유지 |
| 이동 전후 참조 점검 | 링크 깨짐과 drift 방지 |
| 삭제 금지와 historical reference 보존 | 기록 손실 방지 |
| checkpoint/handoff/reference 포인터 | 개념 중복 정의와 drift 방지 |
| 적용 전 체크리스트 | 과잉 정리와 참조 누락 방지 |

## 11. 하네스별로 둘 것

아래 항목은 `docs-organization` v1에 직접 고정하지 않는다.

| 항목 | 이유 |
|---|---|
| 정확한 폴더명 | 프로젝트마다 용어와 규모가 다를 수 있다. |
| 문서 수 기준 | 작은 하네스와 큰 하네스의 기준이 다르다. |
| 도메인별 폴더 | IR, SEC, report, image, client 같은 도메인 구조는 하네스별로 다르다. |
| 공개/비공개 문서 분리 | `security-baseline`과 하네스별 공개 정책을 따른다. |
| archive/cleanup 정책 | 보존 기간, 삭제 기준, 승인자가 프로젝트마다 다르다. |
| 파일명 naming convention | 날짜, 버전, 티커, 고객명 등 도메인별 차이가 있다. |
| 어떤 파일을 current로 볼지 | 운영 방식과 산출물 계약에 따라 다르다. |
| 이동 자동화 여부 | 아직 전역에서 강제하면 과하다. |
| `harness/MANIFEST.md`와의 세부 동기화 방식 | 전체 파일 지도 구조가 하네스마다 다를 수 있다. |

하네스별 docs 정책은 README, MANIFEST, runbook, security-baseline, approval-gate와 함께 정한다.

## 12. v1 권장 목차

`global-harness-docs-organization-template-v1.md`를 만든다면 아래 목차를 권장한다.

1. 목적
2. 범위와 비범위
3. 처음부터 강제하지 않는 원칙
4. 적용이 필요한 성장 신호
5. 역할별 폴더 후보와 의미
6. `docs/README.md`의 역할과 최소 내용 기준
7. 이동 전후 참조 점검
8. 삭제 금지와 historical reference 보존
9. checkpoint / handoff / reference 경계 포인터
10. 하네스별로 정할 것
11. 적용 전 체크리스트

v1 파일에는 아래 판단을 반영한다.

- docs-organization v1은 초기 강제 구조가 아니라 성장 후 정리 기준이다.
- 작은 하네스는 단순한 docs 구조로 시작할 수 있다.
- `docs/README.md`는 docs 내부 색인이고 전체 MANIFEST를 대체하지 않는다.
- 파일 이동은 삭제가 아니며, 이동 전후 참조 점검이 필요하다.
- checkpoint, handoff, reference 개념은 재정의하지 않고 포인터로 둔다.

다음 결정:

```text
이 scope note를 Claude Code와 교차검증한 뒤,
global-harness-docs-organization-template-v1.md 후보 파일을 만들지 결정한다.
```

현재 권장안은 "교차검증 후 만든다"이다.

이유:

- v0는 좋은 방향을 갖고 있지만 `docs/README.md`, 이동 전후 점검, MANIFEST 경계가 얇다.
- v5 후보 문서가 이미 많아져 docs 정리 원칙의 필요성이 실제로 확인됐다.
- docs-organization은 작게 쓰더라도 문서 이동과 reference 보존 기준을 제공해야 한다.
