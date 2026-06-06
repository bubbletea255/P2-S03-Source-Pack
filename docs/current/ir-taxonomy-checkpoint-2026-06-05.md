# IR Taxonomy Checkpoint - 2026-06-05

- 대상 하네스: P2-S03 Source Pack
- 주제: AAPL/NTRA/APP IR pilot 이후 `document_type` 확장 여부 점검
- 상태: 사전 점검 메모
- 작업 범위: 문서 판단만 수행. `harness/`, `catalog/`, `raw/`, `index` 수정 없음

이 문서는 IR 자료 수집 구조를 더 키우기 전에, 지금까지의 AAPL/NTRA/APP 사례를 근거 등급별로 나누고 새 `document_type`을 추가할지 판단하기 위한 checkpoint다.

핵심 결론:

```text
지금은 schema를 확장하지 않는다.
ir-financial-update는 별도 document_type으로 올리지 않는다.
현재 active type인 ir-earnings-release, ir-financial-supplement, ir-deck을 유지한다.
새 유형은 계속 candidate로 관찰하되, 운영 반영된 강한 근거가 더 쌓인 뒤 승격한다.
```

## 1. 참고한 파일

설계 메모:

```text
docs/design/ir-collection-design-notes-2026-06-04.md
docs/design/source-pack-structure-decision-notes-2026-06-04.md
docs/design/sec-ir-deduplication-and-integrity-design-2026-06-05.md
```

Preflight 메모:

```text
docs/pilots/aapl-ir-preflight-2026-06-04.md
docs/pilots/ntra-ir-preflight-2026-06-04.md
docs/pilots/app-ir-preflight-2026-06-04.md
```

Run 결과:

```text
artifacts/runs/run-20260604-aapl-ir-pilot/run-summary.md
artifacts/runs/run-20260604-ntra-ir-pilot/run-summary.md
artifacts/runs/run-20260604-app-ir-pilot/run-summary.md
```

운영 index:

```text
artifacts/companies/AAPL/index.md
artifacts/companies/APP/index.md
```

## 2. 근거 등급 기준

IR taxonomy를 확장할 때 모든 관찰을 같은 무게로 보지 않는다.

| 근거 등급 | 의미 | taxonomy 판단에 쓰는 방식 |
|---|---|---|
| 강한 근거 | 운영 catalog/index에 반영됐고 QA가 통과 또는 제한 통과한 자료 | active type 유지 또는 새 type 검토의 주요 근거 |
| 중간 근거 | run-local raw 수집은 성공했지만 운영 반영은 안 됐거나 run 전체가 부분 실패인 자료 | 후보 관찰에는 사용하되 schema 승격 근거로는 부족 |
| 약한 근거 | preflight에서 공식 사이트 구조상 발견한 자료 유형 | 다음 pilot 후보 선정에 사용 |
| 사용 금지 근거 | 실패, 보안 격리, raw 검증 불가 자료 | taxonomy 확정 근거로 사용하지 않음 |

## 3. 회사별 근거 등급

### 3.1 AAPL

상태:

```text
운영 catalog/index 반영 완료
QA: pass
```

강한 근거:

| 자료 | 현재 document_type | 판단 |
|---|---|---|
| FY2026 Q2 earnings press release HTML | `ir-earnings-release` | active type 유지 근거 |
| FY2026 Q2 consolidated financial statements PDF | `ir-financial-supplement` | active type 유지 근거 |

AAPL이 주는 교훈:

- `ir-earnings-release`는 공식 실적 발표 자료에 적합하다.
- `ir-financial-supplement`는 실적 관련 재무 보충 PDF를 담는 넓은 type으로 쓸 수 있다.
- SEC 8-K/EX-99.1과 의미상 겹쳐도 hash가 다르면 company-ir raw를 별도 보관하고 notes에 overlap 관계를 남길 수 있다.

### 3.2 APP

주의:

```text
APP = AppLovin Corporation
AAPL = Apple Inc.
```

상태:

```text
운영 catalog/index 반영 완료
QA: partial_pass
```

`partial_pass` 이유는 raw 실패가 아니다.
APP SEC 자료가 아직 local catalog/raw에 없어 SEC-IR overlap을 확정할 수 없기 때문이다.

강한 근거:

| 자료 | 현재 document_type | notes/candidate | 판단 |
|---|---|---|---|
| Q1 2026 earnings press release HTML | `ir-earnings-release` | 없음 | active type 유지 근거 |
| Q1 2026 financial update PDF | `ir-financial-supplement` | `candidate_document_type: ir-financial-update` | 별도 type 승격 전 검토 대상 |

APP이 주는 교훈:

- 회사가 자료 이름을 `Financial Update`라고 부르더라도, 내용상 분기 실적 관련 재무 보충자료라면 `ir-financial-supplement`로 처리 가능하다.
- `not_found_in_local_catalog`는 "중복 없음 확정"이 아니라 현재 로컬 APP SEC 자료가 없다는 뜻이다.
- APP SEC 수집 후 동일 ticker 범위에서 future recheck가 필요하다.

### 3.3 NTRA

상태:

```text
run-local pilot 부분 실패
QA: fail
운영 catalog/index 반영 없음
```

NTRA 결과는 근거 등급을 나눠 사용해야 한다.

중간 근거:

| 자료 | 현재 document_type | candidate | 판단 |
|---|---|---|---|
| 44th Annual J.P. Morgan Healthcare Conference presentation PDF | `ir-deck` | `ir-investor-conference-presentation` | run-local 성공 자료. candidate 관찰에는 사용 가능 |

약한 근거:

| preflight 관찰 자료 | candidate | 판단 |
|---|---|---|
| Q1 2026 earnings presentation | `ir-earnings-presentation` | 공식 사이트 구조상 후보. 성공 수집 근거는 부족 |
| Healthcare conference presentation | `ir-investor-conference-presentation` | 반복 관찰 후보 |
| Post-ESMO investor call presentation | `ir-scientific-update` | sector-specific 후보. 아직 schema 승격 금지 |
| Non-GAAP cash flow reconciliation | `ir-non-gaap-reconciliation` | `ir-financial-supplement`와 겹칠 수 있어 보류 |

사용 금지 근거:

| 자료 | 상태 | 이유 |
|---|---|---|
| Q1 2026 earnings press release HTML | failed | HTTP 403으로 raw 수집 실패 |
| Q1 2026 earnings presentation PDF | security_quarantined | Bitdefender가 `document.pdf`를 격리. 복구/열람/운영 승격 금지 |

NTRA가 주는 교훈:

- NTRA preflight는 자료 유형 탐색에는 유용하다.
- 그러나 실패/격리된 파일은 taxonomy 확정 근거로 쓰면 안 된다.
- NTRA는 "새 type 추가"보다 보안 격리 처리와 transport 실패 처리의 QA 사례로 더 가치가 크다.

## 4. Active document_type 현재 평가

현재 active IR type:

```text
ir-earnings-release
ir-financial-supplement
ir-deck
```

평가:

| document_type | 현재 근거 | 유지 여부 | 비고 |
|---|---|---|---|
| `ir-earnings-release` | AAPL/APP 운영 반영, NTRA preflight 관찰 | 유지 | 공식 실적 발표 HTML/PDF에 충분히 적합 |
| `ir-financial-supplement` | AAPL/APP 운영 반영 | 유지 | 회사별 명칭 차이를 흡수하는 넓은 type으로 적합 |
| `ir-deck` | 기존 schema, NTRA/APP preflight, NTRA JPM run-local 성공 | 유지 | presentation류를 당분간 넓게 수용 |

## 5. ir-financial-update vs ir-financial-supplement

판단 질문:

```text
APP의 "Financial Update"가 AAPL의 "Financial Supplement / Consolidated Financial Statements"와 개념적으로 다른가?
```

현재 답:

```text
별도 document_type으로 볼 만큼 다르다고 보기 어렵다.
```

근거:

- APP `Financial Update Q1 2026`는 분기 실적 발표와 함께 제공되는 재무 보충자료다.
- AAPL `Consolidated Financial Statements`도 분기 실적 발표와 함께 제공되는 재무 보충자료다.
- 회사마다 제목은 `Financial Update`, `Financial Statements`, `Supplement`, `Quarterly Results`처럼 다를 수 있다.
- 이 명칭 차이를 모두 별도 `document_type`으로 만들면 taxonomy가 빠르게 지저분해진다.

권고:

```text
ir-financial-update를 정식 document_type으로 추가하지 않는다.
APP financial update PDF는 ir-financial-supplement로 유지한다.
```

추가 정리 후보:

현재 APP 운영 notes에는 사용자의 이전 승인 범위에 따라 아래 문구가 남아 있다.

```text
candidate_document_type: ir-financial-update
```

checkpoint 이후에는 이 표현이 "곧 schema로 승격할 후보"처럼 보일 수 있다.
따라서 나중에 notes를 정리한다면 아래처럼 바꾸는 편이 더 정확하다.

```text
source_label: Financial Update; handled_as: ir-financial-supplement
```

이때 `ir-financial-update`는 candidate vocabulary가 아니라 회사가 붙인 원래 자료명(source label)으로 취급한다.

단, 이 checkpoint 작업에서는 catalog/index를 수정하지 않는다.

## 6. Candidate document_type 평가

### 6.1 `ir-financial-update`

현재 근거:

- APP preflight 관찰
- APP 운영 반영된 financial update PDF

판단:

```text
정식 type으로 추가하지 않는다.
ir-financial-supplement에 흡수한다.
```

이유:

- 실적 관련 재무 보충자료라는 의미가 `ir-financial-supplement`와 겹친다.
- 별도 type을 만들면 회사별 명칭 차이를 taxonomy로 과도하게 반영하게 된다.

### 6.2 `ir-earnings-presentation`

현재 근거:

- NTRA preflight 관찰
- NTRA Q1 earnings presentation은 Bitdefender 격리로 raw 검증 불가
- APP preflight에서 earnings presentation 후보 발견
- APP pilot에서는 실제 수집하지 않음

판단:

```text
아직 정식 type으로 추가하지 않는다.
당분간 ir-deck + notes(earnings_related: true)로 처리한다.
```

승격 조건 후보:

- 2개 이상 회사에서 earnings presentation PDF가 정상 수집된다.
- presentation이 일반 investor deck과 다른 처리 규칙을 요구한다.
- SEC overlap hash 비교 대상이라는 점을 schema 수준에서 분리할 필요가 생긴다.

### 6.3 `ir-investor-conference-presentation`

현재 근거:

- NTRA preflight 관찰
- NTRA J.P. Morgan Healthcare Conference PDF run-local 성공
- 운영 catalog/index 반영은 없음

판단:

```text
아직 정식 type으로 추가하지 않는다.
당분간 ir-deck + event_context/investor_conference notes로 처리한다.
```

이유:

- IR-native 자료로 보이나, 현재는 `ir-deck`으로 충분하다.
- 반복 수집 전 별도 type을 만들면 taxonomy가 presentation subtype으로 과하게 쪼개진다.

### 6.4 `ir-shareholder-letter`

현재 근거:

- APP preflight에서 Shareholder Letter 구조 관찰
- 실제 pilot 수집 없음
- 운영 catalog 반영 없음

판단:

```text
정식 type으로 추가하지 않는다.
preflight 후보로만 유지한다.
```

승격 조건 후보:

- 실제 shareholder letter 원문을 raw로 수집한다.
- earnings release, financial supplement, deck 중 어느 쪽으로도 자연스럽게 분류되지 않는다.
- 다음 하네스가 shareholder letter만 별도로 필터링해야 할 필요가 생긴다.

### 6.5 `ir-scientific-update`

현재 근거:

- NTRA preflight에서 Post-ESMO Investor Call 관찰
- 실제 수집 없음
- sector-specific 성격

판단:

```text
정식 type으로 추가하지 않는다.
NTRA 같은 healthcare/biotech 회사군에서 반복 관찰될 때 다시 논의한다.
```

이유:

- Source Pack 공통 taxonomy에 바로 넣기에는 업종 특화성이 강하다.
- 지금 추가하면 일반 기업 IR taxonomy를 복잡하게 만든다.

### 6.6 `ir-non-gaap-reconciliation`

현재 근거:

- NTRA preflight 관찰
- 실제 수집 없음

판단:

```text
정식 type으로 추가하지 않는다.
실제 파일 확인 전에는 ir-financial-supplement와 겹칠 가능성이 높다.
```

## 7. Schema 확장 여부

권고:

```text
지금 schema 변경 없음.
```

유지할 active vocabulary:

```text
ir-earnings-release
ir-financial-supplement
ir-deck
```

보류할 candidate vocabulary:

```text
ir-earnings-presentation
ir-investor-conference-presentation
ir-shareholder-letter
ir-scientific-update
ir-non-gaap-reconciliation
```

`ir-financial-update`는 candidate vocabulary에서 제거한다.
현재로서는 별도 taxonomy type이 아니라 issuer/source label로 분류한다.

정리:

```text
ir-financial-update = Source Pack document_type 후보 아님
Financial Update = 회사가 붙인 source_label
handled_as = ir-financial-supplement
```

## 8. Notes 정리 권고

현재 notes는 실험 단계의 흔적을 담고 있다.
따라서 바로 schema를 바꾸기보다 notes의 의미를 더 정확히 나누는 편이 좋다.

권고 원칙:

| 상황 | 권장 notes |
|---|---|
| 회사가 붙인 자료명만 다른 경우 | `source_label: {issuer label}; handled_as: {document_type}` |
| 실제 schema 후보인 경우 | `candidate_document_type: ir-...` |
| 실적 관련 SEC overlap 후보 | `earnings_related: true; overlap_status: ...` |
| IR-native 이벤트 자료 | `event_context: investor_conference` 등 |

APP financial update에 대한 권고:

```text
현재: candidate_document_type: ir-financial-update
권고: source_label: Financial Update; handled_as: ir-financial-supplement
```

단, 이 변경은 catalog/index 수정이 필요하므로 별도 사용자 승인 후 진행한다.

## 9. 다음 pilot 후보

지금 당장 schema를 키우기보다, 새 type이 정말 필요한지 확인하는 targeted pilot이 낫다.

우선순위:

| 목적 | 후보 |
|---|---|
| `ir-earnings-presentation` 반복성 확인 | APP Q1 2026 Earnings Presentation PDF 또는 다른 회사의 earnings presentation |
| `ir-shareholder-letter` 필요성 확인 | APP shareholder letter 원문 1건 |
| `ir-investor-conference-presentation` 필요성 확인 | NTRA 외 다른 회사의 investor conference deck |

주의:

- NTRA 격리 파일은 복구하거나 열지 않는다.
- NTRA 동일 run_scope를 무작정 재시도하지 않는다.
- 새 PDF pilot은 NTRA 보안 격리 사례를 고려해 local_path 존재, hash, 보안 격리 여부를 QA에 포함한다.

## 10. 최종 권고안

이번 checkpoint의 최종 권고:

1. `harness/schemas/source-pack-catalog.schema.md`는 지금 수정하지 않는다.
2. `ir-financial-update`는 정식 `document_type`으로 추가하지 않는다.
3. APP financial update PDF는 `ir-financial-supplement`로 유지한다.
4. `ir-earnings-presentation`, `ir-investor-conference-presentation`, `ir-shareholder-letter`는 candidate 관찰 상태로 유지한다.
5. NTRA 실패/격리 자료는 taxonomy 확정 근거로 사용하지 않는다.
6. 다음에는 schema 확장보다 notes cleanup 또는 targeted pilot을 먼저 한다.

## 11. 다음 요청 문구 후보

### 선택지 A: notes cleanup을 먼저 하는 경우

```text
IR taxonomy checkpoint 권고에 맞춰 APP financial update 관련 notes를 정리해줘.

참고 파일:
- docs/current/ir-taxonomy-checkpoint-2026-06-05.md
- artifacts/catalog/documents.jsonl
- artifacts/catalog/files.jsonl
- artifacts/companies/APP/index.md
- artifacts/runs/run-20260604-app-ir-pilot/run-summary.md
- artifacts/runs/run-20260604-app-ir-pilot/qa.md

작업 범위:
- APP financial update의 `candidate_document_type: ir-financial-update` 표현을 제거하거나 약화하고,
  `source_label: Financial Update; handled_as: ir-financial-supplement` 취지로 정리해줘.
- document_type 자체는 `ir-financial-supplement`로 유지해줘.
- QA도 notes 정리 결과와 맞춰줘.

금지:
- harness 파일 수정하지 말 것
- raw 파일 수정하지 말 것
- 새 다운로드 하지 말 것
- NTRA 격리 파일 열거나 복구하지 말 것
```

### 선택지 B: targeted pilot을 먼저 하는 경우

```text
다음 IR targeted pilot 후보를 설계해줘.

목표:
- 새 schema 추가 없이 `ir-earnings-presentation` 또는 `ir-shareholder-letter`가 정말 별도 type이 필요한지 확인
- 다운로드는 아직 하지 말 것
- catalog/index/raw 수정하지 말 것
- 공식 IR 사이트 기준으로 후보 1~2건만 제안

참고 파일:
- docs/current/ir-taxonomy-checkpoint-2026-06-05.md
- docs/pilots/app-ir-preflight-2026-06-04.md
- docs/pilots/ntra-ir-preflight-2026-06-04.md
```

현재 추천은 선택지 A다.
이미 APP catalog/index에 남은 candidate 표현이 checkpoint 결론과 약간 어긋날 수 있으므로, 작게 notes를 정리한 뒤 다음 pilot로 가는 편이 더 깔끔하다.
