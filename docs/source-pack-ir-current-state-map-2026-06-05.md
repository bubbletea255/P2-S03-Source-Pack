# Source Pack IR 현재 상태 지도

- 작성일: 2026-06-05
- 대상 하네스: P2-S03 Source Pack
- 목적: IR 수집 확장, taxonomy, 관측 가능성, SEC-IR 중복/무결성 논의의 현재 위치를 한 장의 지도처럼 정리한다.
- 문서 성격: 현재 상태 지도. 실행 절차의 최종 원본은 `harness/`를 따른다.

이 문서는 투자 분석이 아니다.
이 문서는 Source Pack 하네스가 지금 어디까지 구현됐고, 무엇이 설계 메모로만 남아 있으며, 다음에 무엇을 논의해야 하는지 정리하기 위한 작업 지도다.

## 1. 전체 목표

Source Pack은 가치투자 리서치 21단계 중 P2-S03 원자료 수집 하네스다.

목표:

```text
SEC 공시, Company IR 자료, 가능한 경우 transcript 원문을 raw 파일과 catalog 원장으로 정리해
다음 하네스가 외부 사이트를 다시 방문하지 않고 읽을 수 있게 만든다.
```

금지:

- 원자료 내용 해석
- 투자 thesis 작성
- valuation 의견
- 매수/매도/보유 판단
- transcript 번역, 요약, Q&A 구조화

## 2. 현재 한 줄 요약

현재 Source Pack은 SEC 수집 구조 위에 Company IR 자료를 제한 범위에서 수집하고, catalog에 반영하고, SEC와의 overlap을 notes로 관리할 수 있는 최소 운영 구조까지 왔다.

다만 아래 세 영역은 아직 완성된 자동화 시스템이 아니다.

```text
1. IR taxonomy 확장
2. 관측 가능성
3. SEC-IR 중복/무결성 자동화
```

이 세 영역은 일부는 `harness/`에 반영됐고, 일부는 `docs/` 설계 메모와 실제 pilot 사례로만 남아 있다.

## 3. 문서와 위치 구분

혼동을 줄이기 위해 아래 세 영역을 구분한다.

| 영역 | 위치 | 의미 |
|---|---|---|
| 실제 실행 규칙 | `harness/` | Codex/Claude가 실행할 때 따라야 하는 운영 원본 |
| 설계 메모/합의문 | `docs/` | 논의 결과, 판단 근거, 향후 확장 후보 |
| 실제 수집 결과 | `artifacts/` | raw 파일, catalog 원장, 회사별 index, run 기록 |

중요:

```text
docs에 문서가 있다는 것과 harness에 실제 규칙으로 반영됐다는 것은 다르다.
```

## 4. 지금 실제 하네스에 반영된 것

### 4.1 IR 라우팅

파일:

```text
harness/procedures/source-pack-runbook.md
```

반영 내용:

| 요청 범위 | 따를 절차 |
|---|---|
| SEC 수집 | `harness/procedures/source-pack-collector.md` |
| IR 수집 | `harness/procedures/source-pack-ir-collector.md` |
| SEC + IR 수집 | 승인된 `run_scope` 기준으로 SEC 절차 후 IR 절차 순차 실행 |

의미:

- `harness2/`를 새로 만들지 않았다.
- 기존 `harness/` 안에서 source별 절차를 나누는 방식을 선택했다.
- SEC collector가 IR 세부 규칙을 알 필요 없도록 했다.

### 4.2 IR 전용 collector 절차

파일:

```text
harness/procedures/source-pack-ir-collector.md
```

반영 내용:

- IR preflight
- IR `document_id` 규칙
- IR `document_type` 선택과 확장 원칙
- earnings-related SEC overlap 처리
- pilot 단계와 production 단계 구분
- Company IR raw 저장 경로
- 보안 제품 격리 파일 처리 원칙
- webcast/audio/transcript out-of-scope

의미:

- IR 수집을 위한 최소 절차서는 존재한다.
- 아직 모든 회사/모든 IR 자료 유형을 포괄하는 완성형 절차서는 아니다.
- pilot 경험을 반영해 점진적으로 얇게 확장하는 구조다.

### 4.3 catalog schema의 IR 최소 확장

파일:

```text
harness/schemas/source-pack-catalog.schema.md
```

현재 active IR `document_type`:

```text
ir-deck
ir-earnings-release
ir-financial-supplement
```

반영된 원칙:

```text
IR document_type은 controlled but extensible vocabulary다.
```

의미:

- 현재 허용값은 작게 유지한다.
- 새 IR type을 자동으로 늘리지 않는다.
- 새 공식 IR 자료 유형이 반복 발견되면 preflight 또는 run-summary에 `candidate_document_type`으로 기록한다.
- 사용자 승인 후 schema에 추가한다.
- 새 type을 추가할 때 의미, 제외 기준, 기본 `file_role`, earnings-related 여부를 함께 정한다.

## 5. 아직 harness에 완전히 반영되지 않은 것

### 5.1 관측 가능성

기준 문서:

```text
docs/source-pack-observability-template.md
```

현재 상태:

- 선택 템플릿이다.
- QA 필수 항목이 아니다.
- 누락돼도 run 실패나 QA 실패로 보지 않는다.
- 일부 run-summary에서 운영 관찰 메모를 남길 수 있다.

아직 없는 것:

- 자동 측정 도구
- 지침 라인 수 자동 계산기
- 토큰/시간 추적 원장
- 관측 필드 필수화
- 관측 결과를 읽어 자동으로 감량 권고를 만드는 절차

현 단계에서의 의미:

```text
관측 가능성은 "작게 메모할 수 있는 구조"만 있다.
아직 자동화된 observability 시스템은 아니다.
```

### 5.2 SEC-IR 중복/무결성 자동화

기준 문서:

```text
docs/sec-ir-deduplication-and-integrity-design-2026-06-05.md
```

현재 적용된 것:

- AAPL/APP catalog notes에 SEC-IR overlap 상태 기록
- APP SEC-IR overlap recheck 수행
- APP `future_recheck_required` 해소
- NTRA 보안 격리 파일은 운영 catalog 승격 금지로 처리

아직 없는 것:

- `relationships.jsonl`
- `source-observations.jsonl`
- 정기 integrity check 스크립트
- `next_recheck_after` 만료 항목 자동 탐지
- 전체 catalog local_path/size/hash 자동 audit
- SEC 수집 직후 자동 post-SEC overlap check
- IR 수집 직후 자동 post-IR overlap check

현 단계에서의 의미:

```text
중복/무결성 기준은 설계와 실제 사례 적용까지는 했다.
하지만 별도 자동화 원장이나 integrity checker는 아직 만들지 않았다.
```

## 6. 주요 설계 문서 지도

### 6.1 IR 수집 설계 출발점

파일:

```text
docs/ir-collection-design-notes-2026-06-04.md
```

역할:

- IR taxonomy 설계 메모
- IR `document_id` 규칙
- `ir-financial-supplement` 명칭 선택 이유
- controlled but extensible vocabulary
- `source_label` / `handled_as` / `candidate_document_type` 구분
- earnings-related SEC overlap 원칙

현재 중요 결론:

```text
Financial Update처럼 회사가 붙인 이름이 기존 type으로 흡수 가능하면
candidate_document_type이 아니라 source_label/handled_as로 처리한다.
```

### 6.2 구조 결정 메모

파일:

```text
docs/source-pack-structure-decision-notes-2026-06-04.md
```

역할:

- `harness2/`를 만들지 않기로 한 결정 기록
- 하나의 catalog와 하나의 `harness/`를 유지하되 source별 procedure를 분리하는 방향
- pilot 전 대규모 구조 개편을 피하고, IR collector만 얇게 추가하는 방식

현재 중요 결론:

```text
harness2를 만들지 않는다.
기존 harness 안에서 runbook이 SEC/IR 절차를 라우팅한다.
```

### 6.3 IR taxonomy checkpoint

파일:

```text
docs/ir-taxonomy-checkpoint-2026-06-05.md
```

역할:

- AAPL/NTRA/APP pilot 결과를 근거 등급별로 정리
- active type 유지 여부 판단
- `ir-financial-update`를 새 type으로 승격하지 않기로 한 판단
- candidate vocabulary 후보 정리

현재 중요 결론:

```text
현재 active type은 유지한다.
ir-financial-update는 document_type 후보가 아니라 source_label로 처리한다.
새 type 추가는 아직 하지 않는다.
```

현재 active type:

```text
ir-earnings-release
ir-financial-supplement
ir-deck
```

관찰 중인 후보:

```text
ir-earnings-presentation
ir-investor-conference-presentation
ir-shareholder-letter
ir-scientific-update
ir-non-gaap-reconciliation
```

주의:

- NTRA는 보안 격리 사건 때문에 taxonomy 확정 근거로 강하게 쓰지 않는다.
- NTRA JPM conference deck처럼 run-local 성공한 일부 자료는 약한 관찰 근거로만 쓴다.

### 6.4 관측 가능성 템플릿

파일:

```text
docs/source-pack-observability-template.md
```

역할:

- run-summary에 선택적으로 붙일 수 있는 운영 관찰 섹션
- 지침 파일 수, 지침 라인 수 추정, 병목 메모, 감량 후보 기록

중요 원칙:

```text
선택 섹션이다.
비어 있거나 누락되어도 QA 실패 또는 run 실패로 보지 않는다.
```

### 6.5 SEC-IR 중복/무결성 설계 메모

파일:

```text
docs/sec-ir-deduplication-and-integrity-design-2026-06-05.md
```

역할:

- SEC와 IR 중복 처리 기준
- canonical source 원칙
- hash 비교 기준
- `future_recheck_required`
- raw 무결성 점검
- 장기적으로 `relationships.jsonl`, `source-observations.jsonl`, integrity audit 도입 기준

현재 중요 결론:

```text
같은 파일인지는 SHA-256 hash로 판단한다.
hash가 다르면 같은 의미 후보라도 별도 raw로 보관 가능하다.
SEC form 자체는 SEC EDGAR가 canonical이다.
IR-native 자료는 Company IR이 canonical이다.
notes 방식이 복잡해지면 별도 관계 원장을 검토한다.
```

### 6.6 핸드오프 문서

파일:

```text
docs/source-pack-ir-dedup-handoff-2026-06-05.md
```

역할:

- 새 채팅방이나 Claude Code가 이어받기 위한 요약본

주의:

- 이 문서는 APP SEC-IR recheck와 그 후속 cleanup 이전의 내용이 일부 섞여 있을 수 있다.
- 최신 APP overlap 상태는 `artifacts/companies/APP/index.md`, `artifacts/runs/run-20260605-app-sec-ir-overlap-recheck/`, `artifacts/catalog/documents.jsonl`, `artifacts/catalog/files.jsonl`을 우선한다.

## 7. 실제 pilot 결과 지도

### 7.1 AAPL IR pilot

관련 파일:

```text
docs/aapl-ir-preflight-2026-06-04.md
artifacts/runs/run-20260604-aapl-ir-pilot/
artifacts/companies/AAPL/index.md
```

결과:

- IR raw 2건 수집 성공
- 운영 catalog/index 반영 완료
- `ir-earnings-release`, `ir-financial-supplement` 사용
- SEC 8-K/EX-99.1과 의미상 overlap likely
- exact hash match는 false

의미:

```text
AAPL은 IR 최소 구조가 실제 운영 catalog에 들어갈 수 있음을 보여준 강한 성공 사례다.
```

### 7.2 NTRA IR pilot

관련 파일:

```text
docs/ntra-ir-preflight-2026-06-04.md
docs/ntra-ir-pilot-2-postmortem-2026-06-04.md
artifacts/runs/run-20260604-ntra-ir-pilot/
```

결과:

- 목표 3건 중 일부 실패
- earnings presentation PDF가 Bitdefender에 의해 격리/삭제됨
- 해당 파일은 복구/열람/운영 승격 금지
- JPM conference presentation은 run-local 성공

의미:

```text
NTRA는 taxonomy 확정 근거라기보다 보안 격리와 실패 처리 QA 사례다.
```

### 7.3 APP IR pilot

관련 파일:

```text
docs/app-ir-preflight-2026-06-04.md
artifacts/runs/run-20260604-app-ir-pilot/
artifacts/companies/APP/index.md
```

결과:

- IR raw 2건 수집 성공
- 운영 catalog/index 반영 완료
- `ir-earnings-release`, `ir-financial-supplement` 사용
- `Financial Update`는 새 `document_type`이 아니라 `source_label`로 정리

의미:

```text
APP는 회사별 자료명이 달라도 기존 taxonomy로 흡수할 수 있음을 보여준 사례다.
```

### 7.4 APP SEC-IR overlap recheck

관련 파일:

```text
artifacts/runs/run-20260605-app-sec-ir-overlap-recheck/
artifacts/raw/sec-edgar/cik-0001751008/accession-0001751008-26-000042/
artifacts/companies/APP/index.md
```

결과:

- APP FY2026 Q1 8-K Item 2.02 / EX-99.1 후보만 제한 수집
- APP 전체 SEC full collection은 수행하지 않음
- earnings release HTML은 SEC EX-99.1과 의미상 후보이나 hash 다름
- financial update PDF는 scoped 8-K 안에 대응 SEC financial supplement/update exhibit가 없음
- APP IR 2건의 `future_recheck_required`는 false로 해소

현재 정확한 상태:

| IR 자료 | 상태 |
|---|---|
| `ir-app-earnings-release-fy2026-q1` | `different_hash_from_sec_candidate` |
| `ir-app-financial-supplement-fy2026-q1` | `sec_equivalent_not_found_in_scoped_8k`, `canonical_source: company-ir` |

의미:

```text
APP는 SEC-IR overlap notes를 실제로 갱신하고 future recheck를 닫은 첫 구체 사례다.
```

## 8. 현재 적용된 taxonomy 상태

현재 schema에 반영된 active type:

| document_type | 상태 | 의미 |
|---|---|---|
| `ir-earnings-release` | active | 공식 실적 발표 자료 |
| `ir-financial-supplement` | active | 실적 관련 재무 보충자료 |
| `ir-deck` | active | IR presentation류를 넓게 수용 |

현재 승격하지 않은 후보:

| 후보 | 현재 처리 |
|---|---|
| `ir-financial-update` | 새 type 아님. `source_label: Financial Update; handled_as: ir-financial-supplement` |
| `ir-earnings-presentation` | 후보. 아직 schema 추가 안 함 |
| `ir-investor-conference-presentation` | 후보. 아직 schema 추가 안 함 |
| `ir-shareholder-letter` | 후보. 실제 사례 필요 |
| `ir-scientific-update` | 후보. 바이오/헬스케어 사례 추가 필요 |
| `ir-non-gaap-reconciliation` | 후보. `ir-financial-supplement`와 중복 가능성 있어 보류 |

현재 원칙:

```text
새 type은 발견 즉시 추가하지 않는다.
반복 관찰되고 기존 type으로 처리하면 의미가 왜곡될 때만 사용자 승인 후 추가한다.
```

## 9. 현재 적용된 중복 처리 상태

현재 catalog notes에서 실제로 사용한 상태:

| 상태 | 사용 여부 | 의미 |
|---|---|---|
| `different_hash_from_sec_candidate` | 사용 | SEC 후보와 의미상 관련 있으나 hash 다름 |
| `sec_equivalent_not_found_in_scoped_8k` | 사용 | 제한 확인 범위 안에서 대응 SEC exhibit 없음 |
| `not_found_in_local_catalog` | 사용했으나 APP는 recheck 후 해소 | 당시 로컬 catalog 기준 SEC 후보 없음 |
| `future_recheck_required` | 사용 | 나중에 다시 확인 필요 여부 |
| `security_quarantined` | 설계/QA에서 사용 | 보안 제품 격리 파일은 운영 승격 금지 |

아직 구조화하지 않은 것:

- overlap 관계 원장
- source observation 원장
- canonical source 정식 필드
- overlap status 정식 필드

현재는 모두 `notes` 문자열에 기록한다.

## 10. 나중에 할 수 있는 자동화 후보

### 10.1 Taxonomy 확장

다음 논의 질문:

- `ir-earnings-presentation`을 `ir-deck`에서 분리할 필요가 있는가?
- `ir-investor-conference-presentation`을 별도 type으로 둘 필요가 있는가?
- 바이오/헬스케어 기업의 scientific/clinical update는 어떤 type으로 둘 것인가?
- shareholder letter가 반복 관찰되면 별도 type으로 둘 것인가?

추천 다음 행동:

```text
새 회사 1~2개를 더 preflight해서 실제 IR 자료 유형을 본 뒤 결정한다.
```

### 10.2 관측 가능성

다음 논의 질문:

- run-summary 선택 섹션을 계속 수동 메모로 둘 것인가?
- 지침 파일 수/라인 수를 자동 계산하는 작은 스크립트를 만들 것인가?
- 관측 결과를 언제 감량 의사결정에 사용할 것인가?

추천 다음 행동:

```text
아직 자동화하지 말고, 2~3회 더 pilot/run-summary에 선택 메모로 쌓아본다.
```

### 10.3 중복/무결성 자동화

다음 논의 질문:

- `notes` 방식이 언제 한계에 도달하는가?
- `relationships.jsonl`을 언제 도입할 것인가?
- 전체 `files.jsonl` local_path/size/hash integrity check 도구를 언제 만들 것인가?
- `future_recheck_required` 또는 `next_recheck_after`를 누가 언제 스캔할 것인가?

설계 메모의 전환 기준:

| 전환 트리거 | 의미 |
|---|---|
| IR 운영 반영 ticker가 5개를 초과 | notes만으로 source 관계 추적이 어려워짐 |
| `future_recheck_required` 항목이 10건을 초과 | 만료 항목 조회가 반복 작업이 됨 |
| SEC-IR overlap 관계가 20건을 초과 | 관계 원장 도입 가치가 생김 |
| 같은 notes 파싱/수동 검색을 2회 이상 반복 | 자유형 notes가 사실상 구조화 데이터처럼 쓰이는 신호 |
| QA에서 notes 해석 오류가 1회라도 발생 | 운영 안정성을 위해 정식 필드 검토 |

추천 다음 행동:

```text
지금은 notes 기반 유지.
다만 작은 integrity check 도구는 다음 감량 후보로 좋다.
```

## 11. 현재 주의할 점

### 11.1 APP는 Apple이 아니다

```text
APP = AppLovin Corporation
AAPL = Apple Inc.
```

APP 관련 문서와 run은 AppLovin을 뜻한다.

### 11.2 NTRA 격리 파일은 건드리지 않는다

NTRA pilot 중 Bitdefender가 격리한 파일은 복구, 열람, 운영 승격하지 않는다.

관련 상태:

```text
security_quarantined
```

### 11.3 source-pack-ir-dedup-handoff는 최신 상태와 다를 수 있다

`docs/source-pack-ir-dedup-handoff-2026-06-05.md`는 새 채팅방 핸드오프용으로 유용하지만, APP SEC-IR recheck 이후의 최신 cleanup까지 모두 반영하지 않았을 수 있다.

최신 APP 상태는 아래를 우선한다.

```text
artifacts/companies/APP/index.md
artifacts/runs/run-20260605-app-sec-ir-overlap-recheck/
artifacts/catalog/documents.jsonl
artifacts/catalog/files.jsonl
```

## 12. 다음 논의 추천 순서

이 문서를 기준으로 다음 논의는 아래 순서가 자연스럽다.

### 12.1 먼저 taxonomy 확장

이유:

- IR 수집의 핵심은 자료 유형 분류다.
- active type이 너무 적으면 억지 분류가 생긴다.
- type이 너무 많으면 초보 운영자가 헷갈리고 schema가 무거워진다.

추천 방식:

```text
새 회사 1개 IR preflight → candidate 관찰 → pilot 여부 결정 → taxonomy checkpoint 업데이트
```

### 12.2 그다음 관측 가능성

이유:

- 지금은 하네스가 커지는 초기 단계다.
- 자동화보다 가벼운 관찰을 몇 번 더 쌓는 편이 낫다.

추천 방식:

```text
run-summary 선택 섹션을 2~3회 더 사용한 뒤
자동 라인 수 측정 도구가 필요한지 판단한다.
```

### 12.3 마지막으로 중복/무결성 자동화

이유:

- 중요하지만 아직 운영 ticker 수가 적다.
- notes 방식으로 몇 건은 감당 가능하다.
- 다만 APP 사례처럼 해석 오류가 생겼으므로, 추후 정식 상태값/관계 원장 논의는 필요하다.

추천 방식:

```text
IR 운영 반영 ticker가 5개에 가까워지거나 future_recheck_required가 쌓이면
relationships/source-observations 또는 integrity check 도구를 논의한다.
```

## 13. 다음 요청 문구 예시

### 13.1 taxonomy 확장을 위한 새 preflight

```text
{TICKER} 공식 IR 사이트만 대상으로 IR pilot 4 preflight 메모를 작성해줘.

목표:
- 다운로드는 하지 말 것
- catalog/index/raw는 수정하지 말 것
- 공식 IR 사이트에 어떤 자료 유형이 있는지 확인할 것
- 기존 document_type으로 분류 가능한 자료와 candidate_document_type이 필요한 자료를 구분할 것
- SEC overlap 가능성이 있는 earnings-related 자료와 IR-native 자료를 구분할 것
- pilot으로 수집할 후보 1~3건을 제안할 것

참고 파일:
- docs/source-pack-ir-current-state-map-2026-06-05.md
- docs/ir-collection-design-notes-2026-06-04.md
- docs/ir-taxonomy-checkpoint-2026-06-05.md
- harness/procedures/source-pack-ir-collector.md
- harness/schemas/source-pack-catalog.schema.md

산출물:
- docs/{ticker-lower}-ir-preflight-2026-06-05.md
```

### 13.2 관측 가능성 논의

```text
docs/source-pack-ir-current-state-map-2026-06-05.md와
docs/source-pack-observability-template.md를 기준으로,
Source Pack 관측 가능성을 지금 어느 정도까지 적용할지 논의해줘.

특히:
- 지금 구현된 것과 문서만 있는 것을 구분
- 자동화하면 좋은 것과 아직 수동 메모로 충분한 것 구분
- 하네스 안정성을 해치지 않는 최소 적용안을 제안
```

### 13.3 중복/무결성 자동화 논의

```text
docs/source-pack-ir-current-state-map-2026-06-05.md와
docs/sec-ir-deduplication-and-integrity-design-2026-06-05.md를 기준으로,
SEC-IR 중복/무결성 자동화를 언제 어떻게 도입할지 논의해줘.

특히:
- notes 방식 유지 가능 범위
- relationships/source-observations 원장 도입 기준
- integrity check 도구를 만들 경우 가장 작은 1단계
- 기존 SEC/IR 수집 안정성을 해치지 않는 적용 순서
```

## 14. 결론

현재 Source Pack IR 확장은 아래 상태다.

```text
작동 가능한 최소 IR 수집 구조: 있음
실제 pilot 검증: AAPL/APP 성공, NTRA 보안 격리 사례 확보
taxonomy 확장성: harness에 원칙 반영, 자동 확장 아님
관측 가능성: docs 템플릿과 선택 메모 수준
SEC-IR 중복 처리: notes 기반 사례 적용, 자동화는 아직 없음
무결성 점검: 기본 QA는 있음, 정기 integrity checker는 아직 없음
```

따라서 다음 단계는 대규모 구조 개편이 아니라, 이 현재 상태 지도를 기준으로 작은 논의를 하나씩 진행하는 것이다.

추천 순서:

```text
1. taxonomy 확장을 위한 새 IR preflight
2. 관측 가능성 최소 적용 여부 논의
3. 중복/무결성 자동화 도입 시점 논의
```
