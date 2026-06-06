# Global Harness Observability Template v0

## 목적

하네스가 어디서 무거워지고, 어떤 지침이 반복 병목이 되는지 가볍게 기록하기 위한 전역 후보 템플릿이다.

## Source Pack에서 얻은 교훈

Source Pack에서는 지침 파일이 늘어나면서 "어디서 시간이 오래 걸렸는지"를 기억하기 어려워졌다.
그래서 run-summary 마지막에 선택 관찰 섹션을 붙이는 방식이 생겼다.

## 전역화 후보 원칙

관측 가능성은 처음부터 자동화하지 않는다.
우선 선택 메모로 시작한다.

```text
QA 실패 조건이 아니다.
누락되어도 run 실패가 아니다.
정확한 토큰 계측이 아니다.
```

## 기본 섹션 후보

```markdown
## 하네스 운영 관찰

- instructions_files_consulted:
- instructions_lines_consulted_estimate:
- bottleneck_note:
- trim_candidate:
```

## 필드 뜻

| 필드 | 뜻 |
|---|---|
| `instructions_files_consulted` | 실행 중 참고한 주요 지침 파일 수 |
| `instructions_lines_consulted_estimate` | 읽은 지침 라인 수의 거친 추정 |
| `bottleneck_note` | 시간이 오래 걸린 지점 |
| `trim_candidate` | 나중에 줄이거나 도구화할 후보 |

## 자동화 도입 조건

- 같은 bottleneck이 3회 이상 반복된다.
- 읽는 지침 파일 수가 계속 증가한다.
- 하네스 실행 시간이 실제 업무보다 지침 해석에 더 많이 쓰인다.
- 사용자가 감량 또는 성능 개선을 논의하기 시작한다.

## 아직 구체화할 것

- Source Pack 이름 제거한 완성 템플릿
- run-summary schema에 넣을 공통 문구
- 자동 line-count 도구 도입 기준
