# Source Pack 구조 논의 메모 1

- 작성일: 2026-06-04
- 대상 하네스: P2-S03 Source Pack
- 주제: SEC 안정성을 유지하면서 IR 수집 규칙을 추가하는 구조
- 상태: 구현 전 구조 합의 보관
- 적용 전제: 아직 `harness/` 운영 파일에는 반영하지 않음

이 문서는 IR 자료 수집 규칙을 기존 Source Pack 하네스에 어떻게 추가할지 논의한 내용을 보존하기 위한 구조 메모다.
대화 속 합의가 휘발되지 않도록 보관하며, 이 문서 자체는 하네스 실행 규칙을 변경하지 않는다.

관련 설계 메모:

```text
docs/ir-collection-design-notes-2026-06-04.md
docs/aapl-ir-preflight-2026-06-04.md
```

## 1. 문제의식

현재 Source Pack은 SEC 자료 수집을 중심으로 꽤 안정적인 구조를 갖고 있다.
SEC 수집은 이미 여러 테스트를 통과했고, 기존 `source-pack-collector.md`와 catalog 구조가 작동하는 상태다.

그러나 IR 자료 수집 규칙을 같은 문서에 계속 덧붙이면 다음 문제가 생길 수 있다.

- 기존 SEC 수집 절차가 불필요하게 무거워질 수 있다.
- 매번 모든 지침을 읽어야 해서 시간과 토큰 비용이 커질 수 있다.
- SEC와 IR의 성격이 달라 한 문서 안에서 예외 규칙이 누적될 수 있다.
- 나중에 transcript, industry source까지 추가되면 collector 문서가 누더기처럼 커질 수 있다.

따라서 IR 규칙을 어떻게 추가할지, 그리고 장기적으로 source별 모듈화를 어떻게 볼지 별도 구조 논의가 필요하다.

## 2. 검토한 구조 선택지

### 2.1 선택지 A: 기존 collector에 IR 규칙을 그대로 추가

구조:

```text
harness/procedures/source-pack-collector.md
```

장점:

- 파일 수가 늘지 않는다.
- 기존 실행 흐름을 거의 바꾸지 않아도 된다.
- 단기 구현이 가장 단순하다.

단점:

- SEC 문서가 IR 규칙까지 알아야 한다.
- collector 문서가 계속 두꺼워진다.
- SEC only 작업에서도 IR 지침을 읽게 될 가능성이 있다.
- 장기적으로 transcript, industry source까지 붙으면 문서가 복잡해진다.

판단:

- 지금 단계에서는 권장하지 않는다.
- SEC 안정성을 지키고 IR 규칙을 분리하려는 목적과 맞지 않는다.

### 2.2 선택지 B: `harness2/` 새 폴더 생성

구조 예시:

```text
harness/
harness2/
```

장점:

- SEC와 IR을 물리적으로 강하게 분리할 수 있다.
- IR만 실험하는 느낌을 줄 수 있다.

단점:

- Source Pack이라는 하나의 하네스가 둘로 갈라진다.
- catalog schema, run-summary, QA, raw 경로 원칙이 중복될 위험이 있다.
- Claude/Codex adapter와 AGENTS/CLAUDE 포인터가 더 복잡해진다.
- 장기적으로 drift가 생기기 쉽다.

판단:

- 현 시점에서는 권장하지 않는다.
- SEC와 IR은 다른 source지만, 둘 다 Source Pack의 같은 catalog와 output contract에 결과를 쌓는다.
- 따라서 완전히 별도 하네스로 나누는 것은 과한 분리일 가능성이 높다.

### 2.3 선택지 C: `core/ + sources/` 구조로 전면 개편

장기 목표 예시:

```text
harness/
  core/
    source-pack.contract.md
    source-pack-runbook.md
    source-pack-catalog.schema.md
    source-pack-run-summary.schema.md
  sources/
    sec/
      source-pack-sec-collector.md
    ir/
      source-pack-ir-collector.md
    transcripts/
      source-pack-transcript-collector.md
```

장점:

- 공통 계약과 source별 수집 규칙이 깔끔히 나뉜다.
- SEC only, IR only, transcript only 요청에서 필요한 지침만 읽도록 만들 수 있다.
- 장기적으로 가장 정돈된 구조가 될 가능성이 높다.

단점:

- 지금 당장 적용하면 대규모 구조 개편이 된다.
- 아직 IR pilot을 실행하지 않아 실제 복잡도를 모른다.
- IR이 생각보다 단순하면 과설계가 될 수 있다.
- 반대로 IR이 회사별로 너무 다양하면 지금 만든 구조를 다시 바꿔야 할 수 있다.
- 이미 안정화한 SEC 경로를 불필요하게 흔들 수 있다.

판단:

- 장기 방향으로는 타당하다.
- 하지만 pilot 전 전면 개편은 아직 이르다.
- 실제 IR pilot 경험을 얻은 뒤 리팩터링 여부를 판단하는 것이 좋다.

### 2.4 선택지 D: IR 전용 procedure 파일만 추가

권장 구조:

```text
harness/procedures/source-pack-runbook.md
harness/procedures/source-pack-collector.md
harness/procedures/source-pack-ir-collector.md
harness/schemas/source-pack-catalog.schema.md
```

장점:

- 기존 SEC collector를 건드리지 않는다.
- IR 규칙을 별도 파일로 분리할 수 있다.
- 대규모 구조 개편 없이 source별 분리의 첫 발을 뗄 수 있다.
- pilot 후 `core/ + sources/` 구조로 옮기기도 쉽다.
- IR 규칙이 실제로 얼마나 복잡한지 관찰할 수 있다.

단점:

- 아직 완전한 source module 구조는 아니다.
- runbook에 라우팅 규칙을 추가해야 한다.
- catalog schema에는 최소 IR document_type 추가가 필요하다.

판단:

- 현재 단계의 권장안이다.
- 작고 되돌리기 쉬우며, SEC 안정성을 가장 잘 보존한다.

## 3. 현재 합의된 권장안

현재 가장 적절한 접근은 다음과 같다.

```text
큰 구조 개편은 하지 않는다.
SEC collector는 건드리지 않는다.
IR 전용 procedure 파일을 새로 만든다.
runbook이 SEC/IR 라우팅을 담당한다.
catalog schema에는 IR pilot에 필요한 최소 document_type만 추가한다.
AAPL IR 2파일 pilot 후 구조 개편 여부를 다시 판단한다.
```

핵심 원칙:

- 하네스는 하나로 유지한다.
- catalog 계약도 하나로 유지한다.
- source별 수집 규칙은 분리할 수 있게 한다.
- 잘 작동하는 SEC 경로는 가능한 한 건드리지 않는다.
- 구조 개편은 실제 pilot 경험 이후에 한다.

## 4. 최소 수정 목록

실제 반영 시 최소 수정 대상:

```text
1. harness/schemas/source-pack-catalog.schema.md
2. harness/procedures/source-pack-ir-collector.md
3. harness/procedures/source-pack-runbook.md
```

명시적으로 건드리지 않을 파일:

```text
harness/procedures/source-pack-collector.md
```

이유:

- `source-pack-collector.md`는 기존 SEC 수집 절차의 안정 경로다.
- IR 라우팅 책임은 collector가 아니라 runbook에 있어야 한다.
- SEC collector가 IR collector의 존재를 알 필요는 없다.

## 5. 파일별 역할

### 5.1 `source-pack-catalog.schema.md`

역할:

- Source Pack 전체 catalog 계약을 유지한다.
- SEC와 IR 모두 같은 `documents.jsonl`, `files.jsonl`, `runs.jsonl`에 기록되므로 schema는 공통으로 유지한다.

최소 추가 후보:

```text
ir-earnings-release
ir-financial-supplement
```

추가 원칙:

- IR document_type은 controlled but extensible vocabulary로 둔다.
- 새 IR type을 추가할 때 earnings-related 여부를 함께 판단한다.

### 5.2 `source-pack-ir-collector.md`

역할:

- IR 자료 수집 절차만 담당한다.
- SEC 수집 절차와 직접 얽히지 않는다.
- 첫 버전은 얇은 pilot용 절차서로 시작한다.

초기 포함 범위:

- IR 수집 전 preflight
- 승인된 `run_scope` 밖 수집 금지
- IR `document_id` 규칙
- `document_type` candidate 처리
- earnings-related SEC overlap 처리
- pilot과 production 중복 처리 차이
- webcast/audio out-of-scope 원칙

초기 제외 범위:

- 전체 IR taxonomy 대량 추가
- 모든 회사 IR 페이지 자동 수집
- webcast/audio 저장
- transcript 생성
- per-company 복잡 규칙

### 5.3 `source-pack-runbook.md`

역할:

- 요청 범위를 판단하고 적절한 collector로 라우팅한다.
- SEC collector와 IR collector 사이의 교통정리를 담당한다.

추가할 라우팅 원칙:

```text
SEC 수집이면 source-pack-collector.md를 따른다.
IR 수집이면 source-pack-ir-collector.md를 따른다.
SEC + IR 요청이면 승인된 run_scope 기준으로 순차 실행한다.
```

중요:

- IR 라우팅 문구는 runbook에 둔다.
- SEC collector에 "IR은 다른 파일을 보라"는 포인터를 넣지 않는다.

## 6. 책임 분리 원칙

쉬운 비유:

```text
runbook = 접수 데스크 / 교통정리
source-pack-collector.md = SEC 담당자
source-pack-ir-collector.md = IR 담당자
catalog schema = 공통 장부 양식
```

따라서:

- 접수 데스크가 요청을 분류한다.
- SEC 담당자는 SEC만 처리한다.
- IR 담당자는 IR만 처리한다.
- 공통 장부는 하나로 유지한다.

이 구조가 SEC와 IR 사이의 불필요한 의존성을 줄인다.

## 7. Pilot 전 구조 개편을 하지 않는 이유

IR pilot을 아직 실행하지 않았기 때문에 실제 복잡도를 모른다.

지금 `core/ + sources/`로 전면 개편하면 다음 위험이 있다.

- IR이 단순한데 구조만 복잡해질 수 있다.
- IR이 예상보다 복잡하면 다시 구조를 바꿔야 할 수 있다.
- 안정화된 SEC 경로를 불필요하게 건드릴 수 있다.
- 구조 개편 자체가 새로운 오류 원인이 될 수 있다.

따라서 지금은 작은 확장 지점을 만들고, 실제 pilot 후 구조를 판단한다.

## 8. Pilot 후 다시 볼 구조 질문

AAPL IR 2파일 pilot 후 아래를 관찰한다.

- IR collector 문서가 실제로 얼마나 길어지는가?
- SEC collector와 중복되는 규칙이 얼마나 있는가?
- runbook 라우팅이 충분히 명확한가?
- catalog schema에 IR vocabulary가 너무 많이 섞이는가?
- QA가 source별로 분리될 필요가 있는가?
- SEC only 작업에서 IR 지침을 읽지 않아도 되는가?
- IR only 작업에서 SEC 지침을 얼마나 읽어야 하는가?
- transcript를 붙일 때 같은 방식으로 확장 가능한가?

이 관찰을 바탕으로 아래 중 하나를 선택한다.

```text
1. 현재 구조 유지
2. procedure 파일만 source별로 더 분리
3. harness/core + harness/sources 구조로 리팩터링
4. 별도 하네스로 분리
```

현재 예상으로는 3번이 장기적으로 가장 깔끔할 수 있지만, pilot 전에는 확정하지 않는다.

## 9. 예상 실행 순서

권장 순서:

```text
1. 구조 메모 보관
2. catalog schema 최소 수정
3. source-pack-ir-collector.md 신규 생성
4. runbook 라우팅 문구 추가
5. aapl-ir-preflight 메모 정리
6. AAPL IR 2파일 pilot 실행
7. pilot 결과와 관측 메모 확인
8. 구조 개편 여부 재논의
```

주의:

- 2~5번은 아직 실행하지 않았다.
- 이 문서는 1번 구조 메모 보관에 해당한다.

## 10. 현재 결론

한 줄 결론:

```text
지금은 harness2도, core/sources 전면 개편도 하지 않는다.
SEC collector를 보존한 채 IR 전용 procedure 파일과 runbook 라우팅으로 작게 시작한다.
```

이 결론은 최종 영구 구조가 아니라 pilot 전 임시 구조 전략이다.
Pilot 결과가 쌓이면 구조는 다시 논의하고 바꿀 수 있다.
