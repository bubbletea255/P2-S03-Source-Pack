# Global Harness Candidate Ledger Template v0

## 목적

새 분류, 새 상태값, 새 자료 유형, 새 예외 규칙을 발견했을 때 즉시 정식 schema에 넣지 않고 후보로 누적하기 위한 전역 후보 템플릿이다.

## Source Pack에서 얻은 교훈

IR 자료는 회사마다 이름과 형식이 달랐다.
새 document_type을 바로 추가하면 schema가 빠르게 복잡해질 수 있었다.
그래서 `ir-taxonomy-candidates.jsonl` 후보 원장을 만들었다.

## 전역화 후보 원칙

```text
새 유형을 발견했다고 바로 정식 규칙으로 승격하지 않는다.
후보로 기록하고 반복 사례가 쌓이면 사용자 승인 후 정식화한다.
```

## 적용 가능한 영역

- 자료 유형 분류
- 상태값
- 실패 사유
- QA 예외
- 산출물 subtype
- workflow branch

## 후보 record에 들어갈 정보

| 필드 | 의미 |
|---|---|
| `candidate_id` | 후보 고유 ID |
| `candidate_type` | 후보 종류 |
| `observed_in` | 어떤 실행/파일/회사에서 관찰됐는지 |
| `handled_as` | 현재 임시로 어떻게 처리했는지 |
| `reason` | 왜 후보로 남기는지 |
| `evidence_count` | 관찰 횟수 |
| `status` | `observed`, `needs_review`, `approved`, `rejected` 등 |
| `recommended_action` | 다음 행동 |

## 사용자 승인 요청 기준 후보

- 같은 후보가 3개 이상 distinct context에서 관찰된다.
- 같은 후보가 5건 이상 누적된다.
- 기존 분류로 처리하면 의미 왜곡이 반복된다.
- QA에서 같은 애매함이 2회 이상 기록된다.

## 아직 구체화할 것

- JSONL schema 템플릿
- 하네스 유형별 candidate 예시
- 후보를 정식 schema로 승격하는 절차
