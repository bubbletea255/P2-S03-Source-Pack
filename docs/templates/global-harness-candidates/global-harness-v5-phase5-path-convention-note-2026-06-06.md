# Global Harness v5 Phase 5 Path Convention Note

- 작성일: 2026-06-06
- 상태: Phase 5 부분 결정 note. v5 core 또는 module template 자체가 아니다.
- 검토 범위:
  - `global-harness-core-structure-template-v1.md` Section 12 `Available Modules Registry`
  - 후보 폴더 README와 work map의 경로 표기 방식

## 1. 결정

v1 core Section 12의 `Available Modules Registry`에서는 후보 파일을 파일명만으로 표기한다.

경로 기준은 아래 폴더로 고정한다.

```text
docs/templates/global-harness-candidates/
```

즉 registry의 아래 표기는:

```md
global-harness-design-preflight-template-v0.md
```

아래 전체 상대 경로를 뜻한다.

```text
docs/templates/global-harness-candidates/global-harness-design-preflight-template-v0.md
```

## 2. 이유

| 선택지 | 판단 |
|---|---|
| registry에 전체 상대 경로를 반복한다 | 표가 길어지고 core 본문 가독성이 떨어진다. |
| registry에는 파일명만 둔다 | 같은 후보 폴더 안의 module 목록이라는 뜻이 분명하면 가장 읽기 쉽다. |
| README/work map에는 전체 상대 경로를 쓴다 | 폴더 밖에서 길을 찾는 안내 문서이므로 더 명확하다. |

따라서 v1 core registry는 짧게 유지하고, 기준 폴더를 명시하는 방식이 가장 균형이 좋다.

## 3. 적용 규칙

- v1 core Section 12 registry의 `현재 후보 파일` 열은 파일명만 쓴다.
- registry 바로 위에 기준 폴더를 명시한다.
- work map과 README처럼 안내/항법 문서에서는 필요하면 전체 상대 경로를 쓴다.
- module 파일이 후보 폴더 밖으로 이동하면 registry 기준 폴더를 다시 결정한다.
- `pilot-first / testing`처럼 별도 파일을 만들지 않기로 결정한 정책 항목은 `별도 파일 없음`으로 표시하고 처리 위치를 함께 적는다.

## 4. 다음 단계

이 note를 반영한 뒤 Phase 5의 "실제 파일 경로 표기 방식 결정" 작업은 완료로 본다.

다음 작업은 `comparison mode`와 추가 module 후보의 위치를 결정하는 것이다.
