---
name: source-command
description: "Use when the user asks `/저장`, `저장`, `저장해줘`, or asks to save the current Codex conversation. 현재 대화를 원문 저장 없이 `docs/session-checkpoints/`에 compact checkpoint로 저장한다."
---

# Source Command 호환 어댑터

이 skill은 프로젝트 내부에 남아 있는 구버전 `source-command` 이름을 호환성 때문에 유지하는 Codex용 adapter다.
사용자가 `/저장`, `저장`, `저장해줘`, `현재 대화를 저장해줘`처럼 현재 대화 저장을 요청하면 아래의 compact session-checkpoint 계약을 따른다.

중요:

- 예전 방식인 대화 원문 전문 저장 계약을 사용하지 않는다.
- 전체 대화 원본을 저장하지 않는다.
- `docs/대화 원본/` 또는 `docs/대화 요약/`에 새 파일을 쓰지 않는다.
- 기존 `docs/대화 원본/`, `docs/대화 요약/` 파일을 읽거나 삭제하거나 이동하지 않는다.
- 전역 skill 파일 경로를 직접 참조하거나 요구하지 않는다.
- 이 Codex adapter에서 Claude Code 전역/프로젝트 skill 파일을 수정하지 않는다.

## 출력 계약

- 프로젝트 루트: 현재 workspace root를 사용한다. 필요하면 `git rev-parse --show-toplevel`을 먼저 시도하고, 실패하면 현재 작업 디렉터리를 사용한다.
- 체크포인트 폴더: `{PROJECT_ROOT}/docs/session-checkpoints/`
- 상태 파일: `{PROJECT_ROOT}/docs/session-checkpoints/_checkpoint.codex.md`
- 체크포인트 파일명: `YYYYMMDD_HHMMSS_codex_checkpoint.md`
- 작성 주체: `codex`
- 저장 모드: `initial`, `incremental`, `recovery`
- 저장 내용: 다음 세션 인계용 compact checkpoint만 저장한다.
- 핵심 이벤트는 1-5개 bullet 중심으로 쓴다.

## Gitignore 보호

`.gitignore`에 아래 항목이 있는지 확인한다.

```gitignore
# /저장 명령 - 세션 체크포인트 (GitHub 노출 방지)
docs/session-checkpoints/
```

항목이 없으면 추가한다.
기존 `docs/대화 원본/`, `docs/대화 요약/` ignore 항목은 제거하지 않는다.
해당 항목은 과거 방식으로 만들어진 historical file이 남아 있을 경우 GitHub 노출을 막기 위한 보호선이다.

## 저장 모드 결정

`docs/session-checkpoints/_checkpoint.codex.md`가 있는지 확인한다.

현재 `/저장` 요청 자체는 저장 대상과 anchor 대상에서 제외한다.
anchor는 저장 요청 직전의 마지막 실질 User 메시지와, 가능하면 그에 대한 Assistant 응답으로 잡는다.

저장 모드:

| 모드 | 조건 | 처리 |
|---|---|---|
| `initial` | 상태 파일이 없음 | 현재 접근 가능한 대화 맥락을 compact checkpoint로 요약 |
| `incremental` | 상태 파일이 있고 이전 anchor가 현재 대화에서 일치함 | 마지막 저장 이후의 핵심 변경만 요약 |
| `recovery` | 상태 파일은 있지만 anchor 매칭 실패 | 마지막 체크포인트와 현재 접근 가능한 맥락을 합쳐 복구 체크포인트 작성 |

`recovery`는 실패가 아니다.
다만 체크포인트에 복구 저장임을 명확히 표시한다.

## 체크포인트 작성 형식

체크포인트 파일은 아래 형식을 따른다.

```markdown
# 세션 체크포인트

저장일시: YYYY-MM-DD HH:MM
작성 주체: codex
저장 범위: initial 전체 범위 | incremental - 마지막 저장 이후 | recovery
저장 모드: initial | incremental | recovery
프로젝트: {PROJECT_NAME}
상태 파일: docs/session-checkpoints/_checkpoint.codex.md

## 현재 상태
- 현재 목표와 어디까지 왔는지

## 확정된 결정
- 무엇을 결정했는지
- 왜 그렇게 결정했는지

## 변경된 산출물
- 파일, 실행 기록, 문서, 커밋 또는 태그

## 남은 작업
- 다음에 바로 이어서 할 일

## 주의할 점
- 헷갈리기 쉬운 사실
- 금지된 방향
- 아직 미확정인 것

## 교차검증 메모
- Claude Code 또는 다른 도구와 교차검증이 있었을 때만 작성한다.

## 다음 세션 시작 메시지
2-3줄로 작성한다. 체크포인트 파일 경로 1개, 먼저 읽을 파일 1개, 다음 요청 1문장을 포함한다.
```

작성 원칙:

- 긴 붙여넣기 원문을 복사하지 않는다.
- 긴 Assistant 답변을 그대로 옮기지 않는다.
- 코드 블록이나 대화 전문을 저장하지 않는다.
- 다음 세션이 바로 이어받는 데 필요한 결정, 산출물, 주의점만 압축한다.

## 상태 파일 업데이트

체크포인트 파일을 쓴 뒤 `docs/session-checkpoints/_checkpoint.codex.md`를 아래 형식으로 저장한다.

```text
last_saved_time: YYYY-MM-DD HH:MM
last_saved_file: {checkpoint_filename}
last_saved_mode: {initial | incremental | recovery}
last_user_message_anchor: {저장 요청 직전 마지막 실질 User 메시지 앞 120자}
last_assistant_message_anchor: {해당 Assistant 응답 앞 120자}
previous_checkpoint_file: {이전 last_saved_file, initial이면 생략}
anchor_match: {both | user_only | none}
```

## 사용자 보고

저장이 끝나면 사용자에게 짧게 보고한다.

```text
세션 체크포인트를 저장했습니다.
- 파일: {checkpoint_filename}
- 저장 모드: {initial | incremental | recovery}
- 저장 위치: {PROJECT_ROOT}/docs/session-checkpoints/
```

`recovery` 모드이면 아래 한 줄을 추가한다.

```text
- 주의: 압축 후 복구 저장입니다. 일부 맥락이 누락될 수 있습니다.
```

## 경계

- 이 파일은 Codex 프로젝트 내부 `/저장` compatibility adapter다.
- Claude Code가 Codex 전역 파일을 읽도록 만들지 않는다.
- Codex 전역 `session-checkpoint` skill 파일을 직접 참조하거나 요구하지 않는다.
- Claude Code의 저장 명령은 별도 adapter 책임이다.
- 출력 계약을 바꾸면 Claude Code 쪽 adapter는 별도 환경에서 별도로 맞춘다.
