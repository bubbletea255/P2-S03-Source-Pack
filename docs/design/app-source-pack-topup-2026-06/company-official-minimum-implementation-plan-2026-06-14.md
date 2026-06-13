# Company-official Minimum Implementation Plan

작성일: 2026-06-14
대상: P2-S03 Source Pack / APP top-up Phase 2
상태: 계획 수립, `harness/` 미수정
관련 실행 지도: `docs/design/app-source-pack-topup-2026-06/app-source-pack-topup-execution-map-2026-06-14.md`
관련 설계 메모: `docs/design/app-source-pack-topup-2026-06/source-pack-company-official-and-app-topup-work-map-2026-06-13.md`

---

## 0. 이 문서의 역할

이 문서는 APP product-level official pages pilot 전에 필요한 `company-official` 최소 반영 계획이다.

이 문서는 아직 운영 규칙을 변경하지 않는다. Phase 3의 첫 산출물은 실제 `harness/` 수정이 아니라 수정안 제시이며, 운영 규칙 편집은 그 수정안을 사용자가 검토하고 승인한 뒤에만 진행한다.

---

## 1. 현재 위치

완료:

- Phase 0: 설계 합의 보존, 실행 지도 작성, handoff SEC/sector item_id preflight
- Phase 1: APP latest 10-K, latest 10-Q, SEC entity/sector metadata top-up
- Phase 1 commit: `ddbeffa data: complete APP SEC sector top-up`

대기:

- Phase 3: `company-official` 최소 harness 반영 승인
- Phase 4: APP product official pages item_id/page_key/canonical_url 보강
- Phase 5: APP product official pages pilot

---

## 2. Phase 2 목표

목표는 product official pages를 운영 catalog에 넣기 전에 에이전트가 즉흥적으로 처리할 표면을 줄이는 것이다.

Phase 3에서 검토할 최소 반영 범위는 네 가지로 제한한다.

| 항목 | 목적 |
|---|---|
| catalog schema | `source_type: company-official`, raw path, company-official 전용 document 필드 정의 |
| collector 절차 | 승인된 URL snapshot 수집, page_key/canonical_url 처리, access_limited 기록 |
| no-inference rule | source_type/page_key/canonical_url을 fuzzy 추론으로 확정하지 않도록 중단 조건 명시 |
| QA 최소 규칙 | `captured_at`, raw/file 연결, access_limited/deferred, snapshot staleness 표면화 확인 |

---

## 3. 이번에 하지 않을 것

아래 항목은 APP product page pilot 이후 관찰 근거를 보고 판단한다.

| 보류 항목 | 보류 이유 |
|---|---|
| `document_type` 신규값 | 처음부터 product page 전용 taxonomy를 만들지 않고 `other` + candidate notes로 관찰 |
| `source_subtype` 정식 필드 | 현재는 `candidate_source_subtype`으로 notes에 기록하고 반복 필요를 확인 |
| run-summary schema 정식 item_id 원장 | APP pilot 결과를 본 뒤 형식을 굳힘 |
| company-official 전용 index schema | 실제 소비 패턴을 확인한 뒤 승격 판단 |
| 범용 요청서/이행보고 템플릿 | APP handoff를 첫 사례로 사용한 뒤 템플릿화 판단 |
| broad crawl/automatic discovery | 명시 또는 승인된 URL만 다루는 pilot-first 원칙 유지 |

---

## 4. Phase 3 최소 수정 후보

### 4.1 `harness/schemas/source-pack-catalog.schema.md`

반영 후보:

- `source_type` 허용값에 `company-official` 추가
- `artifacts/raw/company-official/{TICKER}/{YYYY-MM-DD}_{slug}/` raw path 규칙 추가
- raw path의 `{slug}`는 별도 추론 식별자가 아니라 승인된 `page_key`와 동일하게 둔다.
- company-official 전용 document 필드 추가:
  - `captured_at`
  - `page_key`
  - `canonical_url`
- 위 세 필드는 documents 공통 필수 필드로 올리지 않는다.
- 새 구조를 만들지 않고 기존 catalog schema의 source_type별 조건부 필드 그룹 패턴을 따른다.
- 기존 SEC/IR/transcript record가 새 규칙 때문에 schema 위반이 되면 안 된다.

보류:

- `document_type: company-product-page` 같은 신규값
- `source_subtype` 정식 필드

### 4.2 `harness/procedures/source-pack-collector.md`

반영 후보:

- `company-official` 수집은 명시 URL 또는 승인된 page_key/canonical_url만 대상으로 한다.
- 신규 page_key는 에이전트가 제안할 수 있지만, 사람 승인 전에는 운영 catalog에 확정 기록하지 않는다.
- 기존 page_key 재사용은 exact canonical_url match일 때만 허용한다.
- APP pilot 범위에서는 하나의 `page_key`가 여러 `canonical_url`에 대응하면 자동 병합하지 않고 QA 실패 또는 사람 확인으로 표면화한다.
- URL 이전처럼 같은 공식 페이지가 새 `canonical_url`로 옮겨간 가능성이 있으면 자동 병합하거나 자동 분리하지 않는다. 기존 `page_key`를 확장할지 새 `page_key`를 만들지는 사람 판단으로 보류한다.
- URL fuzzy matching, broad crawl, 자동 제품 페이지 발견을 금지한다.
- snapshot 저장 시 `captured_at`, `retrieved_at`, `page_key`, `canonical_url`, raw path, size, sha256을 남긴다.
- 접근 제한, 로그인, 유료벽, 봇 차단은 우회하지 않고 `access_limited` 또는 `deferred`로 기록한다.
- rate limit은 기존 IR 수집 간격을 재사용한다.

### 4.3 `harness/procedures/source-pack-runbook.md` 또는 collector 내부 포인터

반영 후보:

- source_type/page_key/canonical_url이 애매하면 수집을 멈추고 사람 확인으로 넘긴다.
- runbook에는 큰 절차를 복사하지 않고, 필요하면 collector의 company-official 절차로 연결한다.
- Phase 3에서 실제 편집 시 runbook 수정이 필요 없을 정도로 collector에 충분히 들어가면 runbook은 건드리지 않을 수 있다.

### 4.4 `harness/procedures/source-pack-qa.md`

반영 후보:

- company-official document에 `captured_at`, `page_key`, `canonical_url`이 있는지 확인
- APP pilot 범위에서 같은 ticker 안의 하나의 `page_key`가 둘 이상의 distinct `canonical_url`에 연결되면 QA 실패 또는 사람 확인 항목으로 표면화
- raw file path 존재, size > 0, sha256 기록 확인
- 같은 날짜에 같은 `page_key`를 재캡처해 raw path가 충돌하면 기존 파일을 덮어쓰지 않고 QA/run-summary에 표면화
- `access_limited`, `failed`, `deferred`가 조용히 누락되지 않았는지 확인
- 같은 page_key의 downstream 기본 선택 규칙이 최신 `captured_at`임을 확인 가능하게 표시
- 오래된 snapshot은 삭제 대상이 아니라 staleness 판단 대상으로 남긴다는 점 확인

---

## 5. APP Phase 4/5 적용 방향

현재 handoff item_id 상태:

| item_id | 항목 | Phase 1 결과 |
|---|---|---|
| `APP-S03-REQ-20260613-001` | latest APP Form 10-K | collected |
| `APP-S03-REQ-20260613-002` | latest APP Form 10-Q | collected |
| `APP-S03-REQ-20260613-003` | SEC entity/sector metadata | resolved |

Phase 4에서 product official pages는 같은 번호 공간을 이어서 사용한다. 즉 product item은 `APP-S03-REQ-20260613-004`부터 시작한다.

Phase 4에서 각 product item에 필요한 값:

- `item_id`
- 요청 대상 설명
- 명시 URL 또는 canonical_url 후보
- page_key 후보
- snapshot 의미론: latest at collection time 또는 명시 as-of
- blocking 여부
- 실패 시 S04 영향

---

## 6. Phase 3 승인 전 확인할 질문

Phase 3로 넘어가기 전에 사용자가 승인해야 할 질문은 아래 네 가지다.

1. `company-official` 최소 harness 반영을 위한 수정안을 먼저 제시해도 되는가?
2. Phase 3 수정안 범위를 catalog schema, collector, no-inference rule, QA 최소 규칙으로 제한해도 되는가?
3. `source_subtype`은 정식 필드가 아니라 `candidate_source_subtype` notes 관찰로 유지해도 되는가?
4. run-summary schema, company index schema, 범용 템플릿 정식화는 APP pilot 이후로 미뤄도 되는가?

주의:

- 위 질문에 대한 승인은 수정안 작성 승인이며, 실제 `harness/` 편집 승인이 아니다.
- 실제 운영 규칙 편집은 수정안 검토 후 별도 승인으로 진행한다.

---

## 7. Phase 2 완료 조건

완료 조건:

- 최소 수정 후보가 네 항목으로 제한되어 있다.
- Phase 3에서 건드릴 파일과 건드리지 않을 항목이 분리되어 있다.
- `captured_at`, `page_key`, `canonical_url`이 company-official 전용 필드이며 공통 필수 필드가 아니라는 점이 명시되어 있다.
- APP pilot 범위에서 `page_key`와 `canonical_url` 충돌, URL 이전 보류, 같은 날 raw path 충돌 처리 원칙이 명시되어 있다.
- product pages pilot 전까지 운영 catalog에 product page를 넣지 않는다는 순서가 유지된다.
- 이 문서 작성으로는 `harness/`가 수정되지 않는다.
