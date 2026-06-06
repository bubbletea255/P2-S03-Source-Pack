# Global Harness Docs Organization Template v0

## 목적

하네스 구축 중 `docs/` 문서가 많아졌을 때 삭제 없이 역할별로 정리하기 위한 전역 후보 템플릿이다.

## Source Pack에서 얻은 교훈

Source Pack에서는 설계 메모, preflight, pilot 결과, handoff, PDF 참고자료가 섞이면서 사람이 보기 어려워졌다.
정리 전 참조 경로를 먼저 점검하고, 파일 삭제 없이 역할별 폴더로 이동했다.

## 전역화 후보 원칙

### 1. 처음부터 과도하게 나누지 않는다

문서가 적을 때는 루트 `docs/`에 둬도 된다.

### 2. 많아지면 역할별로 나눈다

후보 구조:

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

### 3. 이동 전 참조를 먼저 점검한다

아래를 먼저 확인한다.

- `harness/`에서 직접 참조하는 `docs/` 경로
- 현재 상태 지도와 closeout 문서의 참조
- handoff 내부의 역사적 참조

### 4. 삭제하지 않는다

정리 목적은 삭제가 아니라 사람이 찾기 쉽게 이동하는 것이다.

### 5. handoff 내부의 과거 경로는 유지할 수 있다

handoff는 당시 기록이므로 모든 경로를 현재 경로로 고치지 않아도 된다.

## 아직 구체화할 것

- 문서 수 기준
- 이동 전 점검 명령 예시
- `docs/README.md` 템플릿
- 링크/참조 업데이트 우선순위
