# Global Harness Docs Organization Template v1

- 작성일: 2026-06-07
- 기반: `global-harness-docs-organization-template-v0.md`와 Phase 6 docs-organization scope note
- 기준 문서: `global-harness-v5-phase6-docs-organization-scope-note-2026-06-07.md`
- 상태: 전역 하네스 후보 module. 아직 전역 `harness-lab` 반영 아님.

## 1. 목적

이 template는 하네스의 `docs/` 문서가 많아졌을 때 삭제 없이 역할별로 정리하고, 참조 경로를 깨뜨리지 않기 위한 전역 후보 module이다.

docs-organization은 새 하네스가 처음부터 복잡한 폴더 구조를 따라야 한다는 뜻이 아니다.
작은 하네스는 단순하게 시작하고, 문서가 늘어나 역할과 읽는 순서가 헷갈릴 때 이 module을 적용한다.

핵심 문장:

```text
docs-organization은 초기 구조 강제 문서가 아니라,
문서가 많아진 뒤 안전하게 정리하기 위한 성장 후 정리 기준이다.
```

## 2. 범위와 비범위

이 module이 다루는 것:

- 문서가 많아졌을 때 역할별로 나누는 기준
- `docs/README.md`가 안내해야 할 최소 내용
- 문서 이동 전후 참조 점검
- 파일 삭제 없이 historical reference를 보존하는 원칙
- checkpoint, handoff, reference, template 문서의 위치와 색인 원칙

이 module이 다루지 않는 것:

- 모든 새 하네스에 복잡한 docs 폴더 구조 강제
- 전체 프로젝트 파일 지도 대체
- 모든 하네스의 정확한 파일명과 폴더명 고정
- 과거 handoff 내부 경로 일괄 수정
- archive, cleanup, 삭제 정책 전체
- checkpoint, handoff, reference 개념 재정의

전체 프로젝트 파일 지도는 `harness/MANIFEST.md` 같은 manifest가 맡는다.
docs-organization은 `docs/` 안에서 사람이 무엇을 먼저 읽고, 어떤 문서가 어떤 역할인지 찾게 하는 데 집중한다.

## 3. 처음부터 강제하지 않는 원칙

작은 하네스는 단순하게 시작할 수 있다.

초기에는 아래 정도로 충분할 수 있다.

```text
docs/
  README.md
  {필요한 문서들}
```

처음부터 아래 폴더를 모두 만들 필요는 없다.

```text
docs/
  current/
  design/
  templates/
  pilots/
  handoff/
  reference/
  session-checkpoints/
```

원칙:

- 문서가 적을 때는 루트 `docs/`와 `docs/README.md`만으로 시작할 수 있다.
- 역할이 섞이기 전까지 폴더를 과하게 나누지 않는다.
- 새 폴더를 만들 때는 그 폴더가 어떤 탐색 문제를 해결하는지 설명할 수 있어야 한다.
- 폴더 구조 자체를 하네스 품질로 착각하지 않는다.

## 4. 적용이 필요한 성장 신호

아래 신호가 보이면 docs-organization을 적용할 수 있다.

| 성장 신호 | 의미 |
|---|---|
| docs 루트에 서로 다른 성격의 문서가 많이 쌓인다 | 사람이 무엇부터 읽어야 할지 알기 어렵다. |
| 현재 기준 문서와 과거 기록이 섞인다 | 최신 운영 기준과 historical record가 혼동된다. |
| design note, pilot note, handoff, reference가 같은 위치에 있다 | 문서 역할별 탐색이 필요하다. |
| README나 work map에서 같은 파일 설명을 반복한다 | 색인 정리가 필요하다. |
| 파일 이동 후 링크 깨짐이 걱정된다 | 이동 전후 참조 점검 절차가 필요하다. |
| 여러 모델이나 다음 세션이 같은 폴더를 읽어야 한다 | 읽는 순서와 현재 활성 문서 목록이 필요하다. |

문서 수 기준은 전역에서 고정하지 않는다.
사람이 파일 목록을 보고 역할을 바로 구분하기 어려워지는 순간을 적용 신호로 본다.

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
| `design/` | 설계 노트, 의사결정 초안, 구조 논의 | 작업 진행 중인 설계 맥락을 둔다. |
| `templates/` | 재사용 가능한 템플릿과 후보 템플릿 | 전역 후보와 하네스별 템플릿을 구분할 수 있다. |
| `pilots/` | pilot 실행 결과, 검증 노트, 평가 | 전역화 전 실제 적용 기록을 둔다. |
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
| 하위 폴더 `README.md` | 해당 후보 모음이나 하위 폴더의 파일 지도와 사용 순서를 안내한다. |

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
- `docs/README.md` 또는 하위 README의 파일 지도를 갱신한다.
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

## 10. 하네스별로 정할 것

아래 항목은 하네스별 docs 정책으로 정한다.

| 항목 | 이유 |
|---|---|
| 정확한 폴더명 | 프로젝트마다 용어와 규모가 다를 수 있다. |
| 문서 수 기준 | 작은 하네스와 큰 하네스의 기준이 다르다. |
| 도메인별 폴더 | research, legal, report, image, client 같은 도메인 구조는 하네스별로 다르다. |
| 공개/비공개 문서 분리 | `security-baseline`과 하네스별 공개 정책을 따른다. |
| archive/cleanup 정책 | 보존 기간, 삭제 기준, 승인자가 프로젝트마다 다르다. |
| 파일명 naming convention | 날짜, 버전, 대상명, 고객명 등 도메인별 차이가 있다. |
| 어떤 파일을 current로 볼지 | 운영 방식과 산출물 계약에 따라 다르다. |
| 이동 자동화 여부 | 전역에서 강제하면 과하다. |
| `harness/MANIFEST.md`와의 세부 동기화 방식 | 전체 파일 지도 구조가 하네스마다 다를 수 있다. |

하네스별 docs 정책은 README, MANIFEST, runbook, security-baseline, approval-gate와 함께 정한다.

## 11. 적용 전 체크리스트

새 하네스에 이 module을 적용하기 전에 확인한다.

- [ ] docs-organization을 초기 폴더 구조 강제가 아니라 성장 후 정리 기준으로 적용한다.
- [ ] 작은 하네스라면 단순한 `docs/` 구조로 시작해도 된다고 판단했다.
- [ ] 새 폴더가 어떤 탐색 문제를 해결하는지 설명할 수 있다.
- [ ] 현재 기준 문서와 historical record가 섞여 있는지 확인했다.
- [ ] `docs/README.md`가 docs 내부 색인이고 `harness/MANIFEST.md`를 대체하지 않는다고 명시했다.
- [ ] `docs/README.md`에 폴더/파일 역할 표, 현재 활성 문서 목록, 관리 기준을 두었다.
- [ ] 문서 이동 전 `harness/`, README류, work map/current map, handoff, adapter/skill 참조를 확인했다.
- [ ] 문서 이동 후 현재 운영 참조와 docs 색인을 갱신했다.
- [ ] 전체 파일 지도에 영향이 있으면 `harness/MANIFEST.md`도 확인했다.
- [ ] 정리 중 파일을 삭제하지 않았다.
- [ ] historical handoff 내부 과거 경로를 무리하게 고치지 않았다.
- [ ] checkpoint, handoff, reference 개념을 docs-organization에서 재정의하지 않았다.
- [ ] checkpoint, handoff, reference, template 문서는 위치와 색인 원칙만 다뤘다.
- [ ] 공개/비공개 문서 분리와 archive/cleanup 정책은 하네스별 정책으로 남겼다.
