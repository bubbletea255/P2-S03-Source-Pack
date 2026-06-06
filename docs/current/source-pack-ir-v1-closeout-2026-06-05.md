# Source Pack IR v1 Closeout - 2026-06-05

- 대상 하네스: P2-S03 Source Pack
- 문서 목적: Company IR 수집 v1의 현재 완성 상태, 보류 항목, 다음 단계 기준을 고정한다.
- 문서 성격: 마감 메모 / handoff 기준 문서
- 최신 기준: 이 문서는 2026-06-05 기준 IR v1 논의와 pilot 결과를 반영한다.

이 문서는 투자 분석이 아니다.
이 문서는 Source Pack 하네스가 Company IR 자료 수집을 어디까지 처리할 수 있고, 무엇은 의도적으로 보류했는지 정리하기 위한 운영 메모다.

실제 실행 규칙의 최종 원본은 `harness/`다.
이 문서는 `harness/`에 반영된 것과 `docs/` 설계 메모에만 남아 있는 것을 구분해 다음 작업자가 방향을 잃지 않게 돕는다.

## 1. 한 줄 결론

```text
Source Pack IR v1은 최소 운영 가능 상태다.
이제 더 큰 구조 개편보다 실제 사용을 통해 반복 문제를 확인하는 단계로 넘어가도 된다.
```

단, 아래 의미로 이해해야 한다.

| 항목 | 현재 결론 |
|---|---|
| IR 수집 가능 여부 | 가능. AAPL, APP, TEM에서 수집 성공 |
| 운영 catalog 반영 가능 여부 | 가능. AAPL, APP에서 운영 반영 경험 있음 |
| 실패/보안 격리 처리 | 가능. NTRA 격리 사례를 절차에 반영 |
| taxonomy 확장 | 가능성 열림. 자동 schema 확장은 금지 |
| 중복/무결성 | notes 기반 최소 절차 반영 |
| 대규모 자동 운영 | 아직 아님. 관계 원장, 정기 audit, 자동 측정은 보류 |

따라서 v1의 상태는 아래와 같다.

```text
실사용에 들어가도 되는 최소 Source Pack IR 구조는 있다.
다만 수십~수백 기업 자동 운영용 완성 시스템은 아니다.
반복 문제가 실제로 쌓이면 그때 작은 보강으로 확장한다.
```

## 2. 참고 파일 지도

### 2.1 현재 상태 지도

| 파일 | 역할 |
|---|---|
| `docs/current/source-pack-ir-current-state-map-2026-06-05.md` | IR 수집, taxonomy, 관측 가능성, SEC-IR 중복/무결성의 현재 위치를 한 장으로 정리한 지도 |

### 2.2 설계와 의사결정 메모

| 파일 | 역할 |
|---|---|
| `docs/design/ir-collection-design-notes-2026-06-04.md` | IR document_id, taxonomy 원칙, source_label/handled_as, SEC overlap 기본 원칙 |
| `docs/design/source-pack-structure-decision-notes-2026-06-04.md` | `harness2/`를 만들지 않고 기존 `harness/` 안에서 source별 절차를 나누기로 한 결정 |
| `docs/current/ir-taxonomy-checkpoint-2026-06-05.md` | AAPL/NTRA/APP 이후 taxonomy 확장 여부 점검 |
| `docs/design/sec-ir-deduplication-and-integrity-design-2026-06-05.md` | SEC-IR 중복, canonical source, raw 무결성, recheck 전략 |

### 2.3 관측 가능성 템플릿

| 파일 | 역할 |
|---|---|
| `docs/templates/source-pack-observability-template.md` | run-summary 마지막에 선택적으로 붙이는 하네스 운영 관찰 템플릿 |

### 2.4 handoff / 대화 요약

| 파일 | 역할 |
|---|---|
| `docs/handoff/source-pack-ir-dedup-handoff-2026-06-05.md` | 중복 처리 논의 handoff. 단, APP recheck 이후 최신 상태와 일부 다를 수 있음 |

### 2.5 preflight 메모

| 파일 | 역할 |
|---|---|
| `docs/pilots/aapl-ir-preflight-2026-06-04.md` | Apple IR 2파일 pilot 사전 조사 |
| `docs/pilots/ntra-ir-preflight-2026-06-04.md` | Natera IR pilot 사전 조사 |
| `docs/pilots/app-ir-preflight-2026-06-04.md` | AppLovin IR pilot 사전 조사 |
| `docs/pilots/tem-ir-preflight-2026-06-05.md` | Tempus AI IR pilot 사전 조사 |

### 2.6 pilot 실행 결과

| 파일 | 역할 |
|---|---|
| `artifacts/runs/run-20260604-aapl-ir-pilot/run-summary.md` | AAPL IR pilot 결과 |
| `artifacts/runs/run-20260604-ntra-ir-pilot/run-summary.md` | NTRA IR pilot 결과 |
| `artifacts/runs/run-20260604-app-ir-pilot/run-summary.md` | APP IR pilot 결과 |
| `artifacts/runs/run-20260605-tem-ir-pilot/run-summary.md` | TEM IR pilot 결과 |
| `artifacts/runs/run-20260605-tem-ir-pilot/qa.md` | TEM IR pilot QA |

### 2.7 harness 실제 반영 파일

| 파일 | 역할 |
|---|---|
| `harness/procedures/source-pack-runbook.md` | SEC/IR 라우팅, 후보 원장 확인, 관측 섹션 안내 |
| `harness/procedures/source-pack-ir-collector.md` | IR 전용 수집 절차, taxonomy 후보, SEC overlap, 보안 격리 처리 |
| `harness/schemas/source-pack-catalog.schema.md` | active IR document_type, 후보 원장 schema |
| `harness/schemas/source-pack-run-summary.schema.md` | run-summary 선택 관측 섹션 |
| `artifacts/catalog/ir-taxonomy-candidates.jsonl` | IR taxonomy 후보 관찰 원장 |

## 3. Source Pack IR v1의 목적

Source Pack은 가치투자 리서치 21단계 중 P2-S03 원자료 수집 하네스다.

Company IR 확장의 목적은 아래다.

```text
회사 공식 IR, Newsroom, investor page에 있는 원자료를
SEC raw/catalog 구조와 함께 다음 하네스가 읽을 수 있는 형태로 저장한다.
```

하지 않는 일:

- 투자 분석
- 리포트 작성
- 요약/번역
- valuation
- 매수/매도/보유 판단
- webcast/audio/video 저장
- transcript 생성
- 로그인, 유료벽, 봇 차단 우회
- archive-wide crawling

IR v1의 핵심 원칙:

```text
작게 수집한다.
공식 출처만 쓴다.
run_scope 밖 자료는 수집하지 않는다.
새 유형은 candidate로 기록하고 사용자 승인 전 schema를 바꾸지 않는다.
```

## 4. 파일럿 결과 요약

### 4.1 AAPL

| 항목 | 결과 |
|---|---|
| 상태 | 성공 |
| 운영 catalog/index 반영 | 완료 |
| 수집 자료 | FY2026 Q2 earnings release HTML, financial supplement PDF |
| 사용 type | `ir-earnings-release`, `ir-financial-supplement` |
| QA | pass |
| 의미 | IR 최소 수집 구조가 실제 운영 catalog에 들어갈 수 있음을 확인 |

AAPL은 v1의 기본 성공 사례다.

### 4.2 NTRA

| 항목 | 결과 |
|---|---|
| 상태 | 부분 실패 |
| 운영 catalog/index 반영 | 없음 |
| 수집 자료 | 목표 3건 중 1건 성공 |
| 보안 사건 | Q1 earnings presentation PDF가 Bitdefender에 의해 격리 |
| QA | fail |
| 의미 | taxonomy 확정보다 보안 격리/실패 처리 사례로 중요 |

NTRA의 격리 파일은 복구, 열람, 운영 승격하지 않는다.

### 4.3 APP

| 항목 | 결과 |
|---|---|
| 상태 | 부분 성공 |
| 운영 catalog/index 반영 | 완료 |
| 수집 자료 | Q1 2026 earnings release HTML, financial update PDF |
| 사용 type | `ir-earnings-release`, `ir-financial-supplement` |
| QA | partial_pass |
| 의미 | 회사별 자료명 차이를 `source_label` / `handled_as`로 흡수할 수 있음을 확인 |

중요:

```text
APP = AppLovin Corporation
AAPL = Apple Inc.
```

APP의 `Financial Update`는 별도 `document_type`이 아니다.
현재 결론은 아래다.

```text
source_label: Financial Update
handled_as: ir-financial-supplement
```

### 4.4 APP SEC-IR overlap recheck

| 항목 | 결과 |
|---|---|
| 범위 | APP FY2026 Q1 실적 관련 8-K Item 2.02 / EX-99.1 후보만 제한 확인 |
| full SEC collection | 하지 않음 |
| earnings release | SEC EX-99.1과 의미상 후보이나 hash 다름 |
| financial update PDF | scoped 8-K 안에 대응 SEC financial update/supplement exhibit 없음 |
| 의미 | notes 기반 overlap 상태를 실제로 갱신하고 `future_recheck_required`를 닫은 사례 |

현재 APP 적용 상태:

| IR 자료 | 상태 |
|---|---|
| `ir-app-earnings-release-fy2026-q1` | `different_hash_from_sec_candidate` |
| `ir-app-financial-supplement-fy2026-q1` | `sec_equivalent_not_found_in_scoped_8k`, `canonical_source: company-ir` |

### 4.5 TEM

| 항목 | 결과 |
|---|---|
| 상태 | 부분 성공 |
| 운영 catalog/index 반영 | 없음 |
| raw 수집 | 3건 run-local 성공 |
| 수집 자료 | Q1 earnings release HTML, Q1 Overview PDF, Q1 Corporate Deck PDF |
| 사용 type | `ir-earnings-release`, `ir-financial-supplement`, `ir-deck` |
| QA | partial_pass |
| 보안 격리 | 없음 |
| 의미 | 다양한 IR 자료와 `ir-earnings-presentation` 후보 원장 기록을 실제로 검증 |

TEM은 `ir-taxonomy-candidates.jsonl`을 처음 실사용한 사례다.

기록된 후보:

```text
candidate_document_type: ir-earnings-presentation
handled_as: ir-deck
recommended_action: request_user_review
```

## 5. 현재 active document_type

현재 `harness/schemas/source-pack-catalog.schema.md`에 허용된 active IR type:

| document_type | 의미 | 현재 판단 |
|---|---|---|
| `ir-earnings-release` | 회사 IR 또는 Newsroom의 공식 실적 발표 자료 | 유지 |
| `ir-financial-supplement` | 실적 관련 재무 보충자료 | 유지 |
| `ir-deck` | IR presentation류를 넓게 수용 | 유지 |

이 세 type으로 v1 실사용을 시작할 수 있다.

## 6. candidate_document_type 상태

현재 schema에 추가하지 않은 후보:

| 후보 | 현재 상태 | 다음 판단 |
|---|---|---|
| `ir-earnings-presentation` | needs_review에 가까움. TEM에서 후보 원장 기록됨 | 다음 taxonomy checkpoint에서 정식 type 승격 논의 가능 |
| `ir-investor-conference-presentation` | 관찰 후보 | 아직 `ir-deck`으로 처리 |
| `ir-investor-day-presentation` | TEM preflight에서 관찰 | 실제 수집 성공 사례 추가 후 판단 |
| `ir-shareholder-letter` | APP/TEM에서 관찰 | 실제 raw 수집 후 판단 |
| `ir-scientific-update` | NTRA/TEM 맥락에서 관찰 | healthcare/biotech 반복 사례 필요 |
| `ir-non-gaap-reconciliation` | NTRA preflight 후보 | `ir-financial-supplement`와 중복 가능성 있어 보류 |

candidate가 아닌 것:

| 표현 | 처리 |
|---|---|
| `ir-financial-update` | 정식 후보에서 제거. 회사 자료명(source label)으로 취급 |

핵심 원칙:

```text
새 type은 발견 즉시 추가하지 않는다.
반복 관찰되고 기존 type으로 처리하면 의미가 왜곡될 때만 사용자 승인 후 추가한다.
```

## 7. ir-taxonomy-candidates.jsonl 의미와 현재 상태

파일:

```text
artifacts/catalog/ir-taxonomy-candidates.jsonl
```

역할:

- 새 IR document_type 후보를 한곳에 누적한다.
- 같은 후보가 여러 ticker에서 반복되는지 본다.
- 사용자에게 schema 확장 승인을 요청할 근거를 제공한다.

중요:

```text
이 원장은 schema를 자동 변경하지 않는다.
사용자 승인 전에는 `source-pack-catalog.schema.md`를 바꾸지 않는다.
```

현재 기록:

| candidate_id | 후보 | ticker | handled_as | 상태 |
|---|---|---|---|---|
| `candidate:ir-earnings-presentation:tem:2026-06-05:q1-corporate-deck` | `ir-earnings-presentation` | TEM | `ir-deck` | `needs_review` |

이 기록은 `ir-earnings-presentation`이 곧바로 active type이 됐다는 뜻이 아니다.
다음 taxonomy checkpoint에서 사용자와 논의할 만큼 근거가 쌓였다는 뜻이다.

## 8. SEC-IR overlap / integrity 처리 방식

현재 방식:

```text
notes 기반 최소 절차
```

사용 중인 주요 상태:

| 상태 | 의미 |
|---|---|
| `same_hash_as_sec` | SEC 파일과 hash가 같음 |
| `different_hash_from_sec_candidate` | 의미상 SEC 후보와 겹치지만 hash는 다름 |
| `sec_equivalent_not_found_in_scoped_8k` | 제한 확인 범위 안에서 대응 SEC exhibit가 없음 |
| `not_found_in_local_catalog` | 현재 로컬 catalog/raw 안에서는 SEC 후보 없음 |
| `security_quarantined` | 보안 제품 격리/삭제. 운영 승격 금지 |
| `raw_missing_repair_required` | catalog에는 있으나 파일 없음 |

중요 해석:

```text
not_found_in_local_catalog는 "중복 없음 확정"이 아니다.
현재 보유한 로컬 catalog/files/raw 안에서 찾지 못했다는 뜻이다.
```

현재 반영된 것:

- IR collector의 overlap 상태 기록 절차
- runbook의 IR 이후 overlap notes 기록 안내
- APP recheck 사례
- NTRA 격리 파일 처리 원칙

아직 보류한 것:

- `relationships.jsonl`
- `source-observations.jsonl`
- 정기 integrity audit 스크립트
- 자동 post-SEC / post-IR 전체 catalog scan

## 9. 보안 격리 파일 처리 원칙

NTRA pilot에서 실제 Bitdefender 격리 사건이 있었다.

현재 원칙:

```text
보안 제품이 격리한 파일은 복구하지 않는다.
열지 않는다.
운영 documents/files/index에 승격하지 않는다.
사용자 승인과 별도 보안 검토 전까지 재시도하지 않는다.
```

기록 상태:

```text
overlap_status: security_quarantined
collection_status: failed
repair_required: true
repair_reason: security_quarantined
```

이 원칙은 `harness/procedures/source-pack-ir-collector.md`에 반영됐다.

## 10. 관측 가능성 선택 섹션 적용 상태

현재 run-summary에는 선택적으로 아래 섹션을 붙일 수 있다.

```text
## 하네스 운영 관찰
- instructions_files_consulted
- instructions_lines_consulted_estimate
- bottleneck_note
- trim_candidate
```

반영 위치:

- `harness/schemas/source-pack-run-summary.schema.md`
- `harness/procedures/source-pack-runbook.md`

중요:

```text
선택 섹션이다.
비어 있거나 누락되어도 QA 실패 또는 run 실패가 아니다.
자동 측정 스크립트는 만들지 않았다.
```

현재 의미:

- 하네스가 커질 때 병목을 관찰하기 위한 가벼운 메모 구조다.
- Datadog 같은 자동 observability 시스템이 아니다.
- 2~3회 더 실사용하며 메모를 쌓은 뒤 자동화 여부를 판단한다.

## 11. harness에 실제 반영된 것

| 영역 | 반영 내용 | 위치 |
|---|---|---|
| IR 라우팅 | SEC는 기존 collector, IR은 IR collector로 라우팅 | `source-pack-runbook.md` |
| IR 절차 | preflight, document_id, document_type, raw 저장, SEC overlap, 보안 격리 처리 | `source-pack-ir-collector.md` |
| active IR type | `ir-earnings-release`, `ir-financial-supplement`, `ir-deck` | `source-pack-catalog.schema.md` |
| controlled/extensible 원칙 | 새 type 자동 추가 금지, 사용자 승인 필요 | `source-pack-catalog.schema.md`, `source-pack-ir-collector.md` |
| 후보 원장 | `ir-taxonomy-candidates.jsonl` schema와 확인/기록 절차 | `source-pack-catalog.schema.md`, `source-pack-ir-collector.md` |
| overlap notes | 상태값 기반 notes 기록 절차 | `source-pack-ir-collector.md`, `source-pack-runbook.md` |
| 보안 격리 | 격리 파일 복구/열람/승격 금지 | `source-pack-ir-collector.md` |
| 관측 가능성 | run-summary 선택 섹션 | `source-pack-run-summary.schema.md`, `source-pack-runbook.md` |

## 12. docs에만 남겨둔 것

아래는 아직 실행 규칙 또는 자동화로 완전히 올리지 않았다.

| 항목 | 위치 | 현재 상태 |
|---|---|---|
| relationship 원장 설계 | `docs/design/sec-ir-deduplication-and-integrity-design-2026-06-05.md` | 보류 |
| source observation 원장 설계 | 같은 문서 | 보류 |
| 정기 integrity audit 전략 | 같은 문서 | 보류 |
| high-risk recheck 전략 | 같은 문서 | 설계 메모 |
| source_label/handled_as 세부 설명 | `docs/design/ir-collection-design-notes-2026-06-04.md` | 원칙 문서 |
| taxonomy checkpoint 판단 | `docs/current/ir-taxonomy-checkpoint-2026-06-05.md` | checkpoint 메모 |
| 관측 가능성 템플릿 원안 | `docs/templates/source-pack-observability-template.md` | template |

## 13. 의도적으로 보류한 것과 이유

| 보류 항목 | 보류 이유 | 도입 조건 |
|---|---|---|
| `ir-earnings-presentation` schema 승격 | 아직 모든 케이스를 active type으로 쪼갤 만큼 확정하지 않음 | 사용자 승인 및 taxonomy checkpoint |
| `relationships.jsonl` | 운영 ticker가 적어 notes로 감당 가능 | overlap 관계 20건 초과 또는 notes 해석 오류 |
| `source-observations.jsonl` | provenance는 현재 notes로 충분 | 같은 문서의 다출처 관찰이 반복 |
| 정기 integrity audit 스크립트 | 지금 만들면 과설계 가능 | IR 운영 반영 ticker 5개 초과 또는 `future_recheck_required` 10건 초과 |
| 자동 관측 측정 스크립트 | 선택 메모로 충분히 시작 가능 | 반복 run에서 병목이 명확히 보일 때 |
| webcast/audio/video/transcript 수집 | 범위가 별도 하네스급으로 커짐 | transcript/audio 전용 설계 후 별도 진행 |
| archive-wide IR crawling | 사이트 정책/보안/노이즈 위험 | 특정 회사/범위 승인 후 제한적으로만 |

## 14. 나중에 도입할 조건

### 14.1 Taxonomy 확장

새 `document_type` 승격 조건:

- 같은 후보가 3개 이상 distinct ticker에서 관찰된다.
- 같은 후보가 5건 이상 관찰된다.
- 기존 type으로 처리하면 의미 왜곡이 QA/run-summary에서 2회 이상 기록된다.
- 사용자가 승인을 준다.

현재 가장 가까운 후보:

```text
ir-earnings-presentation
```

단, 이 closeout 문서는 승격을 실행하지 않는다.

### 14.2 중복/무결성 자동화

도입 조건:

- IR 운영 반영 ticker가 5개를 초과한다.
- `future_recheck_required` 항목이 10건을 초과한다.
- SEC-IR overlap 관계가 20건을 초과한다.
- notes 파싱/수동 검색을 2회 이상 반복한다.
- QA에서 notes 해석 오류가 발생한다.

첫 자동화 후보:

```text
files.jsonl local_path 존재 여부와 size/hash를 확인하는 작은 integrity checker
```

### 14.3 관측 가능성 자동화

도입 조건:

- run-summary의 `bottleneck_note`와 `trim_candidate`가 반복된다.
- 지침 파일 수/라인 수 계산이 매번 부담이 된다.
- Source Pack이 다른 하네스보다 과도하게 많은 토큰/시간을 요구한다.

첫 자동화 후보:

```text
읽은 지침 파일 수와 라인 수를 계산하는 작은 도구
```

## 15. 다음 하네스 또는 실사용 단계 주의사항

다음 가치투자 하네스는 원칙적으로 외부 사이트를 다시 방문하지 않는다.

읽기 순서:

1. `artifacts/companies/{TICKER}/index.md`
2. `artifacts/catalog/entities.jsonl`
3. `artifacts/catalog/documents.jsonl`
4. `artifacts/catalog/files.jsonl`
5. `artifacts/derived/text/`
6. `artifacts/raw/`

주의:

- AAPL/APP는 운영 catalog/index에 IR 자료가 반영돼 있다.
- TEM은 raw 3건이 run-local로만 존재하고 운영 catalog/index에는 아직 반영하지 않았다.
- NTRA는 보안 격리/실패 사례이므로 다음 하네스의 확정 입력으로 쓰지 않는다.
- `ir-taxonomy-candidates.jsonl`은 분석 입력이 아니라 schema 확장 검토용 보조 원장이다.
- `not_found_in_local_catalog`는 중복 없음 확정이 아니다.

## 16. Source Pack을 수정해야 하는 트리거

앞으로 실사용 중 아래가 발생하면 Source Pack을 보강한다.

| 트리거 | 권장 조치 |
|---|---|
| 같은 IR 후보 type이 반복된다 | taxonomy checkpoint 열기 |
| `ir-deck`에 너무 많은 하위 유형이 섞인다 | presentation subtype 분리 논의 |
| 같은 overlap notes 해석을 반복한다 | 관계 원장 또는 정식 필드 검토 |
| raw 파일 missing/hash mismatch가 발견된다 | integrity checker 도입 |
| 보안 격리 사례가 반복된다 | 보안 격리 runbook 보강 |
| shell transport 실패가 반복된다 | IR downloader fallback 절차 또는 도구화 |
| run-summary 관측 메모가 같은 병목을 반복 지적한다 | 감량/자동화 작업으로 전환 |

## 17. 다음 추천 단계

이제 Source Pack IR 자체를 더 오래 붙잡기보다, 다음 가치투자 단계 하네스로 넘어가는 편이 좋다.

권장 순서:

```text
1. Source Pack IR v1은 잠정 고정한다.
2. 실제 관심 기업 자료 수집에 사용한다.
3. TEM raw 3건을 운영 catalog/index에 반영할지는 별도 승인으로 결정한다.
4. `ir-earnings-presentation` 승격 여부는 다음 taxonomy checkpoint에서 짧게 논의한다.
5. 다음 가치투자 하네스 설계로 넘어간다.
```

다음 하네스 후보:

| 후보 | 설명 |
|---|---|
| 원자료 선별 하네스 | Source Pack catalog에서 다음 분석에 필요한 핵심 원자료를 고르는 단계 |
| 기업 개요/비즈니스 모델 하네스 | 수집된 원자료를 바탕으로 회사가 무엇을 하는지 구조화 |
| 재무제표/실적 패키지 하네스 | SEC/IR 실적자료를 기반으로 숫자 중심 입력을 정리 |

Source Pack 관점의 추천:

```text
다음에는 "원자료를 더 모으는 하네스"가 아니라
"모인 원자료 중 무엇을 읽을지 고르는 하네스"로 넘어가는 것이 자연스럽다.
```

## 18. 최종 마감 판단

Source Pack IR v1은 아래 조건을 충족했다.

| 조건 | 상태 |
|---|---|
| 공식 IR 자료 수집 절차 | 충족 |
| SEC collector와 IR collector 분리 | 충족 |
| 운영 catalog 반영 사례 | 충족 |
| run-local pilot 사례 | 충족 |
| 실패/보안 격리 사례 | 충족 |
| taxonomy 확장 원칙 | 충족 |
| 후보 원장 | 충족 |
| 관측 가능성 선택 메모 | 충족 |
| SEC-IR overlap 최소 notes 절차 | 충족 |
| 대규모 자동 무결성 시스템 | 의도적으로 보류 |

따라서 마감 결론:

```text
Source Pack IR v1은 실사용 가능한 최소 버전으로 마감한다.
앞으로는 실제 사용 중 반복되는 문제를 근거로만 보강한다.
다음 단계는 가치투자 21단계의 다음 하네스 구축이다.
```
