# APP Source Pack Top-up Execution Map

작성일: 2026-06-14  
대상: APP / AppLovin Corporation  
상태: Phase 1 APP SEC/sector top-up 완료, Phase 2 company-official 최소 반영 계획 대기  
관련 설계 메모: `docs/design/app-source-pack-topup-2026-06/source-pack-company-official-and-app-topup-work-map-2026-06-13.md`  
관련 요청서: `docs/handoff/s03-app-source-pack-topup-request-2026-06-13.md`

---

## 0. 이 문서의 역할

이 문서는 S04가 요청한 APP Source Pack top-up을 어떤 순서로 처리할지 추적하는 실행 지도다.

기존 설계 메모는 `company-official`, 계층2 스냅샷, 하네스 간 요청/이행 규칙을 왜 그렇게 정했는지 설명한다. 이 문서는 그 합의를 실제 작업 순서로 바꾼다.

이 문서 자체는 `harness/` 운영 규칙을 변경하지 않는다. 운영 규칙 변경은 Phase 3에서 별도 승인 후 진행한다.

---

## 1. 현재 결론

현재 작업은 아래 순서로 진행한다.

| Phase | 이름 | 상태 | 핵심 산출물 |
|---:|---|---|---|
| 0 | 합의 보존과 실행 지도 | done | 이 실행 지도, 설계 메모 보강, checkpoint/commit, SEC/sector item_id preflight |
| 1 | APP SEC 10-K/10-Q + sector top-up | done | APP SEC raw/catalog/index/run-summary/QA |
| 2 | `company-official` 최소 반영 계획 | pending | 최소 구현 계획 문서 |
| 3 | `company-official` 최소 harness 반영 | pending | schema/collector/QA 최소 수정 |
| 4 | APP product official pages `item_id`/page_key 보강 | pending | product official pages가 세분화된 APP 요청서 |
| 5 | APP product official pages pilot | pending | company-official snapshot pilot raw/catalog/run-summary/QA |
| 6 | S04 intake readiness 확인 | pending | S04가 읽을 입력 지도와 미해결 항목 |
| 7 | 회고와 다음 실행 기준 정리 | pending | improvement-log 또는 후속 설계 메모 |

다음 세션이나 다른 도구가 이어받을 때는 이 표에서 가장 앞의 `in_progress` 또는 `pending` Phase부터 시작한다.

---

## 2. 고정 원칙

아래 원칙은 이번 작업 전체에서 유지한다.

- Source Pack은 분석 리포트를 만들지 않는다. raw 파일, catalog, index, run-summary, QA를 남긴다.
- S04는 S03 산출물을 read-only로 읽는다. S04가 S03 catalog/raw를 직접 수정하지 않는다.
- APP Phase 1은 full SEC backfill이 아니다. latest 10-K, latest 10-Q, SEC entity/sector metadata만 다룬다.
- Phase 1 결과를 APP full collection 완료로 표시하지 않는다.
- `latest`는 실행 후 accession number와 filing date로 고정해 기록한다.
- S04 handoff의 Required item은 `item_id` 대조 대상이다. Phase 1 전에 SEC/sector 항목에는 먼저 `item_id`를 부여한다.
- `item_id`는 handoff 전체에서 고유해야 하며, Phase 0과 Phase 4가 같은 번호 공간을 공유한다. Phase 0 SEC/sector가 `-001`부터 쓰면 Phase 4 product pages는 다음 번호부터 이어간다.
- product-level official pages는 Phase 3 이전에 운영 catalog에 넣지 않는다.
- `company-official` 자료는 point-in-time snapshot으로 다룬다.
- `captured_at`, `page_key`, `canonical_url`은 향후 `company-official` 전용 document 필드로 다룬다. 전체 공통 필수 필드로 올리지 않는다.
- `retrieved_at`은 파일을 받은 시각이고, `captured_at`은 공식 페이지 스냅샷의 기준 시각이다.
- page_key는 URL fuzzy matching으로 추론하지 않는다. 에이전트가 제안할 수는 있지만 사람 승인 또는 기존 exact canonical URL 매칭이 필요하다.
- 오래된 스냅샷은 자동 삭제하지 않는다.

---

## 3. Phase 0: 합의 보존과 실행 지도

상태: done

목표:

이번 논의에서 합의한 내용을 잃지 않고, 실제 실행 전에 작업 순서를 고정한다.

입력:

- `docs/design/app-source-pack-topup-2026-06/source-pack-company-official-and-app-topup-work-map-2026-06-13.md`
- `docs/handoff/s03-app-source-pack-topup-request-2026-06-13.md`
- Claude Code/Codex 교차 검토 대화

작업:

| 작업 | 상태 | 설명 |
|---|---|---|
| 실행 지도 생성 | done | 이 문서 생성 |
| 설계 메모 보강 | done | Phase B pilot-first 최소 4항목, 전용 필드, item_id 후행 주입, Phase 체계 관계 명시 |
| checkpoint/commit | done | 설계 문서와 README 변경을 git checkpoint로 보존 |
| SEC/sector item_id preflight | done | Phase 1 실행 전 handoff의 latest 10-K, latest 10-Q, sector/entity metadata 항목에 `item_id` 부여 |

완료 조건:

- 실행 지도가 존재한다.
- `docs/README.md`가 이 실행 지도를 가리킨다.
- 설계 메모에 Phase B 최소 범위가 반영된다.
- Phase 1 실행 전에 SEC/sector Required item에 `item_id`가 부여된다.
- git checkpoint 또는 commit이 만들어진다.

사람 승인 필요:

- Phase 1 외부 SEC 접근 및 운영 catalog/index 반영 승인

---

## 4. Phase 1: APP SEC 10-K/10-Q + sector top-up

상태: done

목표:

S04가 먼저 필요로 하는 APP 최신 SEC 핵심 자료와 SEC entity/sector metadata를 공식 S03 raw/catalog/index에 반영한다.

권장 run 설정:

```text
run_mode: partial_recheck
run_scope: APP S04 SEC top-up only - latest 10-K + latest 10-Q
           (accession/date pinned at retrieval), + SEC entity/sector metadata.
           No DEF 14A, no 8-K backlog, no transcript, no PDF table extraction.
           Not a full SEC backfill; do not mark as APP full collection.
운영 catalog/index 반영: approved
```

입력:

- `docs/handoff/s03-app-source-pack-topup-request-2026-06-13.md`
- Phase 1 전에 SEC/sector `item_id`가 부여된 APP handoff
- `config.md`
- `harness/procedures/source-pack-runbook.md`
- `harness/procedures/source-pack-collector.md`
- `artifacts/companies/APP/index.md`

작업:

| 작업 | 상태 | 설명 |
|---|---|---|
| SEC/sector item_id 확인 | done | latest 10-K, latest 10-Q, sector/entity metadata 요청 항목의 `item_id` 확인 |
| SEC submissions metadata 확인 | done | APP CIK, SIC/sector, fiscal year end 확인 |
| latest 10-K 식별 | done | accession number와 filing date 고정 |
| latest 10-Q 식별 | done | accession number와 filing date 고정 |
| raw SEC 파일 저장 | done | 공식 `artifacts/raw/sec-edgar/...` 구조 사용 |
| catalog 갱신 | done | `entities.jsonl`, `documents.jsonl`, `files.jsonl`, `runs.jsonl` 갱신 |
| APP index 갱신 | done | 10-K/10-Q와 sector 확인 필요 상태 해소 |
| run-summary/QA 작성 | done | SEC/sector `item_id`별 결과와 failed/repair/access_limited 여부 표면화 |

완료 조건:

- 최신 10-K와 최신 10-Q가 accession number, filing date, document_id로 식별된다.
- latest 10-K, latest 10-Q, sector/entity metadata 요청 `item_id`가 run-summary/QA 결과와 매핑된다.
- 각 collected document의 `primary_file_id`가 있고 `files.jsonl` record와 연결된다.
- 각 raw file의 `local_path`가 존재하고 size가 0보다 크다.
- APP `entities.jsonl`와 `artifacts/companies/APP/index.md`의 sector 확인 필요 상태가 해소되거나, 해소 실패 사유가 QA에 명시된다.
- run-summary와 QA에 이번 run이 full SEC backfill이 아님이 적힌다.
- `repair_required`, `failed`, `access_limited`가 있으면 최종 보고에서 사용자에게 명시된다.

사람 승인 필요:

- Phase 2 `company-official` 최소 반영 계획 착수 승인

---

## 5. Phase 2: `company-official` 최소 반영 계획

상태: pending

목표:

product-level official pages를 수집하기 전에, 최소한의 규칙 변경 범위만 계획한다.

원칙:

처음부터 큰 템플릿과 모든 schema를 정식화하지 않는다. APP product page pilot을 한 번 해보고 필요한 항목을 승격한다.

최소 계획 범위:

| 항목 | 설명 |
|---|---|
| catalog schema | `source_type: company-official`, raw path, company-official 전용 document 필드 |
| collector 절차 | 수집법, snapshot, page_key, access_limited, rate limit |
| no-inference rule | source_type/page_key를 추론으로 확정하지 않고 애매하면 멈춤 |
| QA 최소 규칙 | `captured_at` 존재, `access_limited` 기록, raw/file 연결 확인 |

명시적으로 뒤로 미룰 것:

- run-summary schema 정식 item_id 원장
- company-official 전용 index schema 정식화
- 범용 요청서/이행보고 템플릿
- document_type 신규값 선제 추가

완료 조건:

- 최소 구현 계획 문서가 존재한다.
- 어떤 파일을 수정할지와 어떤 파일은 아직 수정하지 않을지가 분리되어 있다.
- `captured_at`, `page_key`, `canonical_url`이 company-official 전용 필드로 들어간다는 점이 명확하다.

사람 승인 필요:

- Phase 3 harness 수정 착수 승인

---

## 6. Phase 3: `company-official` 최소 harness 반영

상태: pending

목표:

APP product official pages pilot이 즉흥 처리되지 않도록 최소 운영 규칙을 `harness/`에 반영한다.

예상 수정 후보:

| 파일 | 역할 | 우선순위 |
|---|---|---|
| `harness/schemas/source-pack-catalog.schema.md` | `company-official` source_type, raw path, 전용 필드 | 필수 |
| `harness/procedures/source-pack-collector.md` | company-official snapshot 수집 절차 | 필수 |
| `harness/procedures/source-pack-runbook.md` | 애매한 source_type/page_key 중단 조건 또는 포인터 | 필수 또는 collector에 통합 |
| `harness/procedures/source-pack-qa.md` | captured_at/access_limited/raw 연결 확인 | 가벼운 필수 |

주의:

- `captured_at`, `page_key`, `canonical_url`을 `documents.jsonl` 공통 필수 필드에 넣지 않는다.
- SEC, IR, transcript record는 기존 schema와 호환되어야 한다.
- `document_type` 신규값은 만들지 않는다. 처음에는 `other`와 notes/candidate 방식으로 관찰한다.

완료 조건:

- 기존 SEC/IR catalog record가 새 규칙 때문에 깨지지 않는다.
- company-official 수집 전 필요한 경로와 필드가 명시된다.
- broad crawl, 로그인/유료벽/봇 우회, URL fuzzy matching 금지가 명시된다.

사람 승인 필요:

- harness 운영 규칙 변경 승인
- 변경 후 checkpoint/commit 승인

---

## 7. Phase 4: APP product official pages `item_id`/page_key 보강

상태: pending

목표:

Phase 1 전에 먼저 부여한 SEC/sector `item_id`에 이어, product official pages를 수집 가능한 단위로 세분화하고 `item_id`, page_key, canonical_url 후보를 연결한다.

입력:

- `docs/handoff/s03-app-source-pack-topup-request-2026-06-13.md`

작업:

| 작업 | 상태 | 설명 |
|---|---|---|
| product 요청 항목 분해 | pending | product official pages를 item 단위로 나눔 |
| `item_id` 부여 | pending | 예: SEC/sector가 `-001`~`-003`을 쓰면 product pages는 `APP-S03-REQ-20260613-004`부터 이어감 |
| page_key/canonical_url 후보 표시 | pending | 명시 URL, 기존 exact match, 에이전트 제안 후보를 구분 |
| blocking 여부 표시 | pending | S04 진행 전 필수인지 구분 |
| snapshot 의미 표시 | pending | latest 또는 as-of 기준 명시 |

완료 조건:

- 각 product official pages 요청 항목에 stable `item_id`가 있다.
- S03 run-summary/QA가 나중에 해당 `item_id`로 결과를 매핑할 수 있다.
- 새 범용 템플릿을 만들기 전에 실제 APP 사례로 형식이 검증된다.

사람 승인 필요:

- 기존 handoff 문서 수정 승인

---

## 8. Phase 5: APP product official pages pilot

상태: pending

목표:

Phase 3 규칙을 적용해 APP 공식 product/platform pages를 작은 범위로 수집하고, company-official 모델이 실제로 작동하는지 확인한다.

입력:

- item_id가 붙은 APP handoff
- Phase 3에서 반영된 company-official 규칙
- 사람이 승인한 page_key/canonical_url 목록

작업:

| 작업 | 상태 | 설명 |
|---|---|---|
| page_key/canonical_url 확인 | pending | 에이전트 제안 후 사람 승인 또는 기존 exact match 재사용 |
| snapshot 저장 | pending | `artifacts/raw/company-official/{TICKER}/{YYYY-MM-DD}_{slug}/` |
| catalog 갱신 | pending | `source_type: company-official`, `document_type: other` |
| run-summary/QA 작성 | pending | item_id별 collected/skipped/failed/access_limited 기록 |

완료 조건:

- 각 page snapshot에 `captured_at`, `page_key`, `canonical_url`이 있다.
- downstream default read rule이 적용 가능하다: 같은 page_key 중 최신 `captured_at` 사용.
- 실패하거나 접근 제한된 항목이 조용히 누락되지 않는다.

사람 승인 필요:

- 신규 page_key 승인
- access_limited/deferred 항목 처리 판단

---

## 9. Phase 6: S04 intake readiness 확인

상태: pending

목표:

S04가 APP 분석을 재개하기 전에 읽어야 할 S03 산출물과 미해결 항목을 명확히 한다.

작업:

| 작업 | 상태 | 설명 |
|---|---|---|
| required item 대조 | pending | handoff item_id와 run-summary/QA 결과 비교 |
| blocking issue 확인 | pending | S04 진행 금지 항목이 남았는지 확인 |
| read-only 경로 정리 | pending | S04가 읽을 catalog/index/raw 경로 정리 |
| stale snapshot 노출 | pending | company-official snapshot 나이와 재캡처 필요 여부 표시 |

완료 조건:

- S04가 읽어야 할 경로가 명확하다.
- blocking item이 모두 collected/skipped_existing 또는 명시적 deferred/failed 사유를 가진다.
- unresolved item은 사람 판단이 필요한 형태로 남는다.

사람 승인 필요:

- S04로 넘어가도 되는지 최종 승인

---

## 10. Phase 7: 회고와 다음 실행 기준 정리

상태: pending

목표:

APP top-up에서 배운 내용을 다음 회사와 다음 APP 업데이트에 재사용할 수 있게 남긴다.

작업:

| 작업 | 상태 | 설명 |
|---|---|---|
| improvement-log 갱신 | pending | 반복될 규칙, 예외, 개선 후보 기록 |
| template 승격 판단 | pending | item_id 요청/이행 형식을 정식 템플릿으로 만들지 판단 |
| document_type 관찰 집계 | pending | `other`로 둔 company-official 문서 유형이 반복되는지 확인 |
| 다음 S03 실행 기준 정리 | pending | 다른 회사에도 적용할 최소 절차 정리 |

완료 조건:

- 다음번 같은 요청에서 사람이 다시 같은 논의를 반복하지 않아도 된다.
- 새 taxonomy나 template이 필요하면 관찰 근거를 갖고 논의한다.
- 필요 없는 확장은 만들지 않는다.

사람 승인 필요:

- taxonomy 승격
- template 정식화
- 추가 harness 변경

---

## 11. 현재 중단/재개 규칙

작업을 멈췄다가 다시 시작하면 아래 순서로 확인한다.

1. 이 문서의 Phase 상태표를 읽는다.
2. 가장 앞의 `in_progress` 또는 `pending` Phase를 찾는다.
3. 해당 Phase의 입력, 작업, 완료 조건을 확인한다.
4. 관련 run-summary/QA 또는 git status를 확인한다.
5. 사람 승인 필요 항목이 있으면 실행 전에 사용자에게 확인한다.

사용자가 "우리 어디까지 했지?"라고 물으면 이 문서 기준으로 답한다.

---

## 12. 지금 바로 다음 행동

현재 기준 다음 행동은 아래 순서다.

1. Phase 2 `company-official` 최소 반영 계획을 작성한다.
2. Phase 3에서 실제 harness 수정에 들어가기 전 사용자 승인을 받는다.
