# Global Harness Core Structure Template v0

## 목적

새 하네스를 만들 때 공통 업무 규칙과 모델별 실행 방식을 분리하기 위한 후보 템플릿이다.

## Source Pack에서 얻은 교훈

Source Pack은 공통 업무 규칙을 `harness/`에 두고, Codex와 Claude Code는 얇은 adapter로 붙였다.

```text
업무 의미는 harness/에 둔다.
Codex/Claude는 각자 adapter만 얇게 둔다.
공통 규칙을 adapter에 길게 복사하지 않는다.
```

## 전역화 후보 원칙

새 하네스는 가능하면 아래 구조를 기본 후보로 삼는다.

| 영역 | 역할 |
|---|---|
| `harness/` | 공통 업무 규칙, 계약, 절차, schema, QA 기준 |
| `.agents/` | Codex 실행 adapter |
| `.claude/` | Claude Code 실행 adapter |
| `artifacts/` | 실행 산출물 |
| `docs/` | 설계 메모, handoff, 현재 상태 지도 |

## 필수 질문

- 이 하네스의 공통 업무 의미는 어디에 둘 것인가?
- Codex와 Claude Code가 서로 다른 규칙을 보지 않게 하려면 어떤 파일을 단일 원본으로 둘 것인가?
- adapter에는 무엇만 남길 것인가?
- 산출물은 어디에 저장할 것인가?
- 다음 하네스가 무엇을 읽어야 하는가?

## 아직 구체화할 것

- 새 하네스 기본 폴더 구조
- `AGENTS.md` / `CLAUDE.md` 포인터 템플릿
- Orchestrator / procedure / schema / rubric 최소 세트
- 단일 하네스와 복수 하네스 분리 기준
