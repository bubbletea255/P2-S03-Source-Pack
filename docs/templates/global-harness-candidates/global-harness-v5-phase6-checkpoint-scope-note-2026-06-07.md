# Global Harness v5 Phase 6 Checkpoint Scope Note

- 작성일: 2026-06-07
- 상태: Phase 6 checkpoint v1 후보 작성 전 scope note. module template 본문이 아니다.
- 대상: `global-harness-checkpoint-template-v0.md`
- 기준 문서:
  - `global-harness-core-structure-template-v1.md`
  - `global-harness-security-baseline-template-v1.md`
  - 전역 `session-checkpoint` skill
  - 프로젝트 `source-command` compatibility adapter
  - `global-harness-v5-work-map.md`

## 1. 목적

이 note는 `checkpoint` v1 후보에 무엇을 직접 담고, 무엇을 전역 실행 skill, 프로젝트 adapter, 하네스별 저장 정책으로 둘지 정리한다.

핵심 질문:

```text
checkpoint v1은 이미 존재하는 /저장 실행 skill을 하나 더 만드는 것인가,
아니면 새 하네스가 checkpoint 기능을 설계할 때 따르는 공통 출력 계약인가?
```

현재 판단은 후자다.

`checkpoint` v1은 실행 skill을 대체하지 않는다.
새 하네스가 compact session checkpoint를 도입할 때 따라야 할 공통 출력 계약, 저장 원칙, adapter 경계, 보안 경계를 정리하는 module template가 되어야 한다.

## 2. 현재 존재하는 3층 구조

현재 checkpoint 관련 구조는 세 층으로 나뉜다.

| 층위 | 예시 | 역할 |
|---|---|---|
| 전역 실행 skill | 전역 `session-checkpoint` skill | `/저장` 요청을 실제 파일 저장으로 수행하는 실행 절차 |
| 프로젝트 adapter | Source Pack의 `source-command` compatibility adapter | 프로젝트 안의 기존 명령 이름과 전역 checkpoint 계약을 연결 |
| v5 후보 template | `global-harness-checkpoint-template-v0.md` 또는 이후 v1 | 새 하네스를 만들 때 checkpoint module을 어떻게 설계할지 알려주는 기준 |

따라서 `checkpoint` v1은 "두 번째 저장 명령"이 아니다.
실제 저장은 전역 skill이나 프로젝트 adapter가 수행하고, v5 checkpoint module은 그 실행들이 공유해야 할 출력 계약과 하네스 적용 기준을 설명한다.

## 3. checkpoint module의 역할

`checkpoint` module이 맡을 것:

| 역할 | 설명 |
|---|---|
| 공통 출력 계약 | checkpoint 파일이 어떤 최소 정보를 가져야 하는지 정한다. |
| compact 원칙 | 대화 원문을 저장하지 않고 다음 세션 인계에 필요한 핵심만 요약하게 한다. |
| 저장 모드 기준 | `initial`, `incremental`, `recovery`의 의미를 정한다. |
| anchor 개념 | 이전 저장 지점을 어떻게 식별하고, 실패 시 언제 recovery로 갈지 정한다. |
| 보안 경계 | 원문 저장 금지, 민감 정보 노출 금지, git 제외 후보를 `security-baseline`과 연결한다. |
| adapter 경계 | Codex, Claude Code, CLI 등 실제 실행 구현은 각 adapter가 맡게 한다. |

`checkpoint` module이 맡지 않을 것:

| 비범위 | 이유 |
|---|---|
| 전역 `session-checkpoint` skill 구현 | 실제 실행 skill은 별도 위치에서 관리된다. |
| Codex/Claude Code별 파일 쓰기 구현 | 도구별 권한, 파일 시스템, anchor 접근 방식이 다르다. |
| 모든 하네스의 checkpoint 저장 경로 고정 | docs 구조와 공개 범위가 하네스마다 다를 수 있다. |
| 대화 원문 archive | `security-baseline` 원칙과 충돌한다. |
| 구조적 handoff 문서 전체 대체 | session checkpoint와 handoff는 목적이 다르다. |

## 4. compact checkpoint 정의

`compact checkpoint`는 대화 원문 저장이 아니다.

v1에 들어갈 compact 정의:

```text
compact checkpoint는 다음 세션이 이어받기 위해 필요한 핵심 이벤트, 확정 결정,
변경 산출물, 남은 작업, 주의점을 구조화해 요약한 파일이다.
대화 원문 전체, full context window 전체, 긴 사용자 붙여넣기,
긴 assistant 답변, 코드 전문을 복사하지 않는다.
```

compact checkpoint가 저장할 것:

- 현재 목표와 진행 위치
- 이번 범위의 핵심 이벤트
- 확정된 결정과 변경된 산출물
- 남은 작업
- 다음 세션이 조심해야 할 점
- 다음 세션 시작 메시지

compact checkpoint가 저장하지 않을 것:

- 대화 전문
- 긴 사용자 입력 전문
- 긴 assistant 답변 전문
- 전체 코드 블록
- secret, credential, private conversation 원문
- full context window dump

## 5. 원문 저장 금지와 security-baseline 연결

checkpoint는 `security-baseline`의 대화 원문/checkpoint 경계를 따른다.

연결 원칙:

| 항목 | checkpoint 처리 |
|---|---|
| 대화 원문 전체 | 저장하지 않는다. |
| 비공개 대화 원문 | 원문 그대로 저장하지 않는다. 필요하면 별도 승인과 제외 경로가 필요하다. |
| compact checkpoint | 허용 가능하되 민감 정보가 들어갈 수 있으면 git 제외 후보로 본다. |
| secret 값 | checkpoint에 포함하지 않는다. |
| 민감 파일 위치 | 필요한 경우 안전한 참조만 남기고 실제 값은 표시하지 않는다. |
| 보안 이슈 | 자동 해결하지 않고 `security-baseline`과 `approval-gate` 기준을 따른다. |

checkpoint는 다음 세션을 돕기 위한 요약이지, 증거 보존이나 원문 archive가 아니다.

## 6. 저장 위치 원칙

권장 기본 위치는 아래다.

```text
docs/session-checkpoints/
```

다만 이 경로는 권장 기본값이다.
하네스별 docs 구조가 다르면 별도 checkpoint 위치를 정할 수 있다.

전역 원칙:

- checkpoint는 주요 운영 산출물과 분리한다.
- 사람이 다음 세션에서 쉽게 찾을 수 있어야 한다.
- 민감 정보 가능성이 있으면 git 제외 후보로 본다.
- 저장 위치를 바꾸면 해당 하네스의 README, MANIFEST, runbook, adapter에서 같은 경로를 가리킨다.
- 기존 과거 대화 원본 폴더가 있더라도 새 checkpoint 흐름은 원문 저장 방식으로 돌아가지 않는다.

`docs/session-checkpoints/`를 쓰는 경우 `.gitignore` 후보:

```gitignore
docs/session-checkpoints/
```

이 항목의 최종 적용 여부는 하네스별 공개 범위와 저장소 정책에 따라 결정한다.

## 7. 저장 모드

checkpoint 저장 모드는 세 가지를 기본 후보로 둔다.

| 모드 | 의미 | 사용 상황 |
|---|---|---|
| `initial` | 첫 checkpoint 저장 | 이전 상태 파일이나 checkpoint가 없을 때 |
| `incremental` | 이전 저장 이후 핵심 변경만 저장 | 이전 anchor가 확인됐을 때 |
| `recovery` | 이전 저장 지점을 정확히 확인하지 못한 복구 저장 | anchor 불일치, ambiguous match, context 압축 등 |

`recovery`는 실패가 아니다.
다만 checkpoint 본문과 상태 파일에 recovery임을 명확히 남긴다.

저장 요청 자체는 저장 대상과 anchor 대상에서 제외한다.
anchor는 저장 요청 직전의 마지막 실질 user/assistant 메시지 또는 adapter가 제공하는 안전한 식별자를 기준으로 잡는다.

## 8. anchor 메커니즘과 matching 계약

`incremental` 저장을 지원하려면 이전 checkpoint가 어느 대화 지점까지 반영했는지 추적하는 anchor가 필요하다.

anchor는 adapter별로 다르게 구현할 수 있다.

가능한 구현 예:

- 마지막 실질 user 메시지의 짧은 문자열
- 마지막 assistant 응답의 짧은 문자열
- message id
- timestamp
- 도구가 제공하는 conversation cursor

필드명은 adapter별로 달라도 되지만, matching 결과와 모드 전환 계약은 공통으로 둔다.

| anchor 상태 | 의미 | 저장 모드 |
|---|---|---|
| `both` | user anchor와 assistant anchor가 둘 다 일치 | `incremental` |
| `user_only` | user anchor만 일치 | `incremental`, 단 부분 일치로 기록 |
| `none` | 이전 저장 지점을 확인할 수 없음 | `recovery` |
| `ambiguous` | 동일 anchor 문자열이 대화 내 2개 이상 위치에서 매칭됨 | `recovery` |

원칙:

- `both`와 `user_only`는 incremental을 허용한다.
- `user_only`는 상태 파일이나 checkpoint metadata에 반드시 남긴다.
- `none`은 recovery로 전환한다.
- `ambiguous`는 최근 위치를 추측하지 않고 recovery로 전환한다.
- 동일 anchor 문자열이 2개 이상 위치에서 매칭되면 ambiguous로 본다.
- checkpoint는 똑똑한 추정보다 보수적 복구를 우선한다.

상태 파일 예시는 adapter별로 달라질 수 있지만, 최소한 아래 정보가 필요하다.

```text
last_saved_time:
last_saved_file:
last_saved_mode:
anchor_match: both | user_only | none | ambiguous
```

## 9. 기본 checkpoint 섹션

checkpoint v1은 기본 skeleton을 제공하되, 모든 섹션을 같은 강도로 요구하지 않는다.

| 구분 | 섹션 | 처리 |
|---|---|---|
| 필수 | 현재 상태 | 항상 작성 |
| 필수 | 남은 작업 | 항상 작성 |
| 필수 | 다음 세션 시작 메시지 | 항상 작성 |
| 조건부 필수 | 확정된 결정 | 결정이 없으면 `없음` |
| 조건부 필수 | 변경된 산출물 | 변경이 없으면 `없음` |
| 조건부 필수 | 주의할 점 | 주의점이 없으면 `없음` |
| 선택 | 교차검증 메모 | 교차검증이 있었을 때만 작성 |

권장 skeleton:

```markdown
# 세션 체크포인트

저장일시:
작성 주체:
저장 범위:
저장 모드:
프로젝트:
상태 파일:

## 현재 상태

## 확정된 결정

## 변경된 산출물

## 남은 작업

## 주의할 점

## 교차검증 메모

## 다음 세션 시작 메시지
```

`없음` 기준:

| 섹션 | `없음`을 쓸 수 있는 조건 |
|---|---|
| 확정된 결정 | 이 세션에서 새 방향 결정이나 기존 결정 변경이 없을 때 |
| 변경된 산출물 | 파일 수정, 새 파일 생성, 커밋, 태그, 실행 기록이 없을 때 |
| 주의할 점 | 금지 방향, 혼동 가능성, 미확정 쟁점, 다음 작업자가 조심해야 할 내용이 없을 때 |

`교차검증 메모`는 교차검증이 있었을 때만 작성한다.
없다면 섹션 자체를 생략할 수 있다.

## 10. session checkpoint vs handoff 구분

session checkpoint와 handoff는 목적이 다르다.

| 구분 | 목적 | 내용 | 기본 위치 후보 |
|---|---|---|---|
| session checkpoint | 같은 작업을 다음 세션에서 이어가기 위한 시간적 스냅샷 | 현재 상태, 결정, 변경 산출물, 남은 작업, 다음 시작 메시지 | `docs/session-checkpoints/` |
| 구조적 handoff | 다른 사람, 다른 모델, 다른 팀이 작업 맥락을 이해하도록 넘기는 문서 | 배경, 의사결정 맥락, 파일 지도, 판단 근거, 운영 규칙 | `docs/handoff/` 또는 하네스별 docs |
| artifacts handoff | 하네스 실행 중 phase/team 전환을 위한 산출물 인계 | 이전 phase 결과, 다음 phase 입력, 책임 경계 | `artifacts/handoff.md` 또는 하네스별 artifacts |

경계 원칙:

- session checkpoint는 handoff 전체를 흡수하지 않는다.
- handoff는 매 저장마다 누적되는 checkpoint가 아니다.
- artifacts handoff는 하네스 실행 산출물이며, 대화 재개용 checkpoint와 분리한다.
- 긴 설명, 배경, 판단 근거가 필요하면 별도 handoff 문서로 분리하고 checkpoint에서는 링크만 남긴다.

## 11. Codex/Claude Code adapter 경계

checkpoint module은 공통 출력 계약을 제공한다.
실제 실행 구현은 adapter가 맡는다.

공통으로 맞출 것:

- compact checkpoint 원칙
- 원문 저장 금지
- 저장 모드 의미
- anchor matching 상태와 recovery 전환 원칙
- 기본 checkpoint 섹션
- security-baseline 연결
- 저장 후 사용자 보고 기준

adapter별로 둘 것:

- `/저장`, `저장해줘`, `세션 체크포인트 저장해줘` 같은 트리거 문구
- 실제 skill 또는 command 파일 위치
- 파일 쓰기 권한 처리
- 상태 파일명과 작성 주체 표기
- message id, timestamp, anchor 문자열 등 anchor 구현 방식
- Claude Code와 Codex의 tool permission 처리
- 여러 채팅창 또는 병렬 session lane 처리

adapter는 checkpoint module의 원칙을 읽되, 공통 업무 규칙을 길게 복사하지 않는다.

## 12. 하네스별로 정할 것

각 하네스가 직접 정해야 하는 항목:

- checkpoint 기능을 사용할지 여부
- 기본 저장 위치
- checkpoint 폴더의 git 제외 여부
- 상태 파일명과 작성 주체 표기
- 저장 요청 trigger를 adapter에 둘지 여부
- checkpoint에 추가할 하네스별 섹션
- session checkpoint와 handoff 문서의 위치
- private checkpoint, private raw/input의 보안 경계
- checkpoint를 어느 시점에 권장할지
- 여러 모델이나 여러 채팅방이 동시에 저장할 때 충돌을 어떻게 다룰지

전역 template는 이 항목을 단정하지 않는다.
각 하네스의 README, MANIFEST, runbook, adapter, security-baseline 연결에서 구체화한다.

## 13. v1 권장 목차

`global-harness-checkpoint-template-v1.md`를 만든다면 아래 목차를 권장한다.

1. 목적
2. 범위와 비범위
3. checkpoint module의 역할
4. compact checkpoint 정의
5. 원문 저장 금지와 security-baseline 연결
6. 저장 위치 원칙
7. 저장 모드
8. anchor 메커니즘과 matching 계약
9. 기본 checkpoint skeleton
10. session checkpoint vs handoff 구분
11. Codex/Claude Code adapter 경계
12. 하네스별로 정할 것
13. 적용 전 체크리스트

v1 파일에는 아래 판단을 반영한다.

- checkpoint v1은 실행 skill이 아니라 module template다.
- 전역 `session-checkpoint` skill을 대체하지 않는다.
- compact checkpoint 정의는 v1 본문에 직접 넣는다.
- anchor matching 계약은 공통 출력 계약으로 둔다.
- `docs/session-checkpoints/`는 권장 기본 위치이지만 하네스별 override를 허용한다.
- session checkpoint와 handoff는 독립 섹션으로 구분한다.

다음 결정:

```text
이 scope note를 Claude Code와 교차검증한 뒤,
global-harness-checkpoint-template-v1.md 후보 파일을 만들지 결정한다.
```

현재 권장안은 "교차검증 후 만든다"이다.

이유:

- v0는 compact checkpoint 방향은 맞지만 이미 존재하는 전역 실행 skill과 v5 template의 관계가 얇다.
- security-baseline v1에서 checkpoint 보안 경계가 정리됐으므로 checkpoint v1도 이에 맞춰야 한다.
- Codex와 Claude Code adapter가 같은 출력 계약을 보려면 checkpoint module의 공통 기준이 필요하다.
