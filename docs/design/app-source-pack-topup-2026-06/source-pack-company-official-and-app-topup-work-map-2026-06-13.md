# Source Pack company-official and APP top-up work map

- 작성일: 2026-06-13
- 대상 하네스: P2-S03 Source Pack
- 발단: S04 full Industry Primer 준비를 위한 APP Source Pack top-up 요청
- 상태: 구조 논의 합의 보관 및 실행 작업 지도
- 적용 전제: 이 문서 자체는 아직 `harness/` 운영 규칙을 변경하지 않는다.

이 문서는 APP Source Pack top-up 요청을 검토하는 과정에서 Codex, Claude Code, 사용자가 교차 논의한 내용을 보존하기 위한 설계 메모다.
처음 목적은 APP/AppLovin의 S04 입력 보강이었으나, 논의 과정에서 S03이 SEC/IR 중심 원자료 저장소에서 회사 공식 웹 자료까지 어떻게 다룰지에 대한 구조 빈칸이 드러났다.

핵심 결론은 두 가지다.

1. APP의 최신 SEC 10-K, 10-Q, sector/entity metadata top-up은 기존 S03 구조로 안전하게 진행할 수 있다.
2. APP product-level official docs/pages는 `company-official` 계층2 스냅샷 규칙을 먼저 명문화한 뒤 수집하는 것이 안전하다.

## 0. 문서 역할과 실행 지도 관계

이 문서는 구조 논의의 근거와 합의를 보존하는 설계 메모다.
실제 실행 순서, 진행 상태, 중단/재개 기준은 아래 실행 지도가 소유한다.

```text
docs/design/app-source-pack-topup-2026-06/app-source-pack-topup-execution-map-2026-06-14.md
```

이 문서의 Phase A-E는 개념적 작업 묶음이고, 실행 지도의 Phase 0-7은 실제 작업 단위다.

| 이 문서의 개념 Phase | 실행 지도 기준 |
|---|---|
| Phase A. APP SEC top-up | Phase 1 |
| Phase B. company-official 계층2 규칙 명문화 | Phase 2 + Phase 3 |
| Phase C. APP product-level official docs/pages top-up | Phase 4 + Phase 5 |
| Phase D. S04 read-only intake check | Phase 6 |
| Phase E. S04 external-source expansion and official run | S04 소관, 실행 지도 범위 밖 |

## 1. 출발점

S04 handoff 요청:

```text
docs/handoff/s03-app-source-pack-topup-request-2026-06-13.md
```

요청의 필수 항목:

| 항목 | S03 현재 구조와의 관계 | 판단 |
|---|---|---|
| 최신 APP Form 10-K | SEC 수집 절차와 fast path/repair_required 규칙이 이미 있음 | 바로 진행 가능 |
| 최신 APP Form 10-Q | SEC 수집 절차와 fast path/repair_required 규칙이 이미 있음 | 바로 진행 가능 |
| APP product-level official docs/pages | 현재 `source_type`, raw 경로, 스냅샷 의미론이 없음 | 명문화 후 진행 |
| Sector/entity metadata | `entities.jsonl.sector` 등 기존 entity 원장으로 처리 가능 | 바로 진행 가능 |

S04 handoff는 full Source Pack backfill을 요청한 것이 아니라 S04 official run 준비용 top-up 요청이다.
따라서 APP SEC 작업은 `partial_recheck` 또는 제한된 top-up run으로 처리하고, 전체 10-K 10년/10-Q 12분기 수집 완료로 표시하지 않는다.

## 2. 확인된 기존 안전장치

SEC 10-K/10-Q에 대해서는 기존 S03 하네스 구조가 이미 안전장치를 갖고 있다.

기존 collector의 fast path:

| 판정 | 의미 |
|---|---|
| `new_document` | 같은 `document_id`가 없어 다운로드 시도 |
| `skipped_existing` | 기존 문서와 대표 파일이 검증 조건을 통과해 재다운로드하지 않음 |
| `repair_required` | 과거에 수집됐으나 catalog/file 관계가 깨져 사람 확인 필요 |
| `retry_eligible` | 실패, pending, 재포함된 skipped 문서를 다시 시도 가능 |

`skipped_existing`이 되려면 다음 조건이 필요하다.

- `documents.jsonl.primary_file_id`가 있다.
- `files.jsonl`에 대응 `file_id` record가 있다.
- `files.local_path`가 실제 존재한다.
- 실제 파일 크기가 0보다 크다.

따라서 APP 10-K 일부 연도를 먼저 수집한 뒤 나중에 full SEC collection을 실행하면, 이미 받은 SEC filing은 공식 raw/catalog 구조에서 재사용된다.
반대로 catalog에는 있는데 파일이 없거나 관계가 깨진 경우에는 조용히 넘어가지 않고 `repair_required`로 표시해야 한다.

## 3. Source Pack 소유 경계

논의에서 합의한 가장 중요한 원칙은 "공식자료"와 "외부 근거"를 구분하는 기준이다.

S03은 자료 내용의 진실성이나 투자 가치를 판단하지 않는다.
S03의 책임은 출처 충실도, 로컬 파일 존재성, catalog 일관성, 다음 하네스 전달성이다.

### 3.1 S03 소유 자료

회사가 직접 작성하거나 공식 배포한 자료는 S03 소유로 본다.

예시:

- SEC filings
- 회사 IR 자료
- 회사 뉴스룸 또는 공식 보도자료
- 회사 공식 웹의 제품, 플랫폼, 개발자, 가격, 거버넌스, 정책 페이지
- 회사가 직접 배포한 webcast/transcript 후보
- Business Wire, PR Newswire, Q4, Notified 등 제3자 호스팅을 쓰더라도 회사가 공식 배포한 자료

중요한 기준은 도메인이 아니라 저작자와 공식 배포 주체다.

### 3.2 S03 밖 자료

제3자가 회사나 산업에 대해 작성한 자료는 S03의 기본 수집 대상이 아니다.

예시:

- sell-side/buy-side analyst report
- fund manager commentary
- market research firm report
- 언론, 업계 기사
- 제3자가 작성한 산업/경쟁 분석

이 자료들은 S04 또는 다른 분석 하네스의 external-source expansion에서 다루는 것이 기본이다.

## 4. 두 축의 계층 구분

이번 논의에서 출처 축과 수집 시점 축을 분리했다.

| 계층 | 출처/소유 | 수집 시점 | 예시 | S03 처리 |
|---|---|---|---|---|
| Tier 1 | 회사 공식, 정기/filing형 | 기본/상시 또는 full scope 수집 | 10-K, 10-Q, DEF 14A, 8-K Item 2.02, 정기 IR 재무자료 | 기존 SEC/IR 구조 |
| Tier 2 | 회사 공식, 변동성 높거나 범위 넓음 | 요청 시 수집 | 제품 페이지, 플랫폼 페이지, 개발자 문서, 가격 페이지, 뉴스룸, 정책 페이지 | `company-official` 스냅샷 구조 필요 |
| Tier 3 | 제3자 자료 | S03 밖 | analyst report, market research, media | 분석 하네스의 외부 근거 |

Tier 2는 S03 소유지만 자동으로 전체 사이트를 긁지 않는다.
S04 같은 downstream 하네스가 특정 URL 또는 특정 공식 페이지 범위를 handoff로 요청하면, S03이 승인된 `run_scope` 안에서 날짜 스냅샷으로 수집한다.

## 5. company-official source_type 합의

`source_type`은 단순 라벨이 아니라 수집 절차, raw 경로, 접근 규칙을 끌고 다닌다.
따라서 애매한 공식 웹 자료를 `manual` 또는 `industry-source`에 임시로 욱여넣지 않는다.

합의한 방향:

```text
source_type: company-official
raw path: artifacts/raw/company-official/{TICKER}/{YYYY-MM-DD}_{slug}/
document_type: other
```

`document_type`은 당장 새로 만들지 않는다.
처음에는 `other`를 사용하고, `notes`에 실제 의미와 후보 분류를 남긴다.

반면 `captured_at`, `page_key`, `canonical_url`은 downstream의 최신 스냅샷 선택과 정체성 대조에 쓰인다.
따라서 Phase B 반영 시 이 값들은 자유 형식 `notes` 문자열에만 숨기지 않고 `company-official` 전용 document 필드로 정의한다.
이 필드들은 `company-official` record에는 필요하지만, SEC/IR/transcript record까지 깨뜨리지 않도록 `documents.jsonl` 공통 필수 필드로 올리지 않는다.

예시 catalog 방향:

```text
source_type: company-official;
document_type: other;
page_key: app-max;
canonical_url: https://www.applovin.com/max/;
captured_at: 2026-06-13T00:00:00+09:00;
notes: handled_as: other; candidate_source_subtype: product_page; candidate_document_type: company-product-page; snapshot_semantics: point_in_time
```

`document_type` 후보는 반복 관찰 뒤 사용자 승인으로 schema 승격 여부를 결정한다.
이는 기존 IR taxonomy 원칙과 같다.
`source_subtype` 역시 확정 schema 필드가 아니라 pilot 관찰 후보다.
product_page, newsroom, developer_docs, pricing_page 같은 세부 구분이 반복적으로 필요하다고 확인되면 APP pilot 이후 정식 필드 여부를 판단한다.

## 6. company-official 스냅샷 의미론

SEC filing은 accession number가 있어 고정 문서로 다룰 수 있다.
회사 공식 웹페이지는 같은 URL이라도 시간이 지나며 조용히 바뀔 수 있다.

따라서 `company-official`은 "현재 페이지"가 아니라 "특정 시점의 스냅샷"으로 다룬다.

합의한 규칙:

- `captured_at`은 필수다.
- 같은 URL이라도 현재본이 필요하면 새 `captured_at`으로 다시 수집한다.
- 기존 스냅샷이 있다는 이유만으로 현재 페이지 수집을 fast path skip하지 않는다.
- downstream은 기본적으로 같은 `page_key`의 최신 `captured_at` 스냅샷을 읽는다.
- 과거 시점 분석이 필요하면 `as_of`를 명시한다.
- 오래된 스냅샷은 자동 삭제하지 않는다.
- 스냅샷 나이와 stale risk는 S04 intake 또는 다음 하네스 전달 요약에 드러낸다.

`retrieved_at`과 `captured_at`은 구분한다.
대부분 같을 수 있지만, `retrieved_at`은 파일을 받은 시각이고 `captured_at`은 스냅샷 기준 시각이다.

## 7. page_key 원칙

같은 페이지의 여러 스냅샷을 묶으려면 `page_key`가 필요하다.
그러나 URL 자동 정규화나 퍼지 매칭으로 page_key를 추론하면 안 된다.

합의한 규칙:

- `page_key`는 에이전트가 확정 추론하지 않는다.
- handoff 요청서가 명시하면 그 값을 사용한다.
- 기존 catalog에서 같은 `canonical_url`이 정확히 일치하면 기존 `page_key`를 재사용할 수 있다.
- 처음 보는 페이지면 에이전트는 `page_key` 후보를 제안할 수 있지만, 운영 catalog 반영 전 사용자 승인 또는 요청서 명시가 필요하다.
- trailing slash, www, http/https, query parameter, locale, redirect 기반의 퍼지 매칭은 하지 않는다.

`item_id`는 특정 요청 항목 식별자이고, `page_key`는 요청을 가로질러 같은 공식 페이지를 묶는 영속 식별자다.

## 8. company-official 접근 규칙

`company-official`은 기존 S03 안전 규칙을 상속한다.

- 로그인, 유료벽, 봇 차단, Cloudflare 우회 금지
- robots.txt 또는 사이트 정책상 명백한 제한 존중
- 접근 제한은 조용히 누락하지 않고 `access_limited`, `blocked`, `failed`, `deferred` 등으로 기록
- broad crawl 금지
- 승인된 `run_scope`의 특정 URL 또는 명시된 공식 페이지 후보만 수집
- 요청 간격은 우선 IR 사이트 간격 2.0초를 재사용
- HTML/PDF 원문, metadata, URL, access status, size, SHA-256, content type을 가능한 범위에서 기록
- 원문 내용 요약, 번역, 투자 판단 금지

## 9. 하네스 간 통신 원칙

S04가 S03을 실시간 함수처럼 호출하지 않는다.
하네스 간 통신은 "수동적 요청 문서 + 사람 승인 + 수신 하네스 실행 + read-only intake" 모델을 따른다.

합의한 흐름:

```text
S04-A: 자료 구멍 발견 -> S03 handoff 요청서 작성 -> S04 해당 단계 중단
사용자: 요청서 검토 및 S03 실행 승인
S03: 자기 runbook, schema, QA 기준으로 수집 -> raw/catalog/index/run-summary/QA 갱신
S04-B: 갱신된 S03 Source Pack을 read-only intake check -> 통과 후 full Industry Primer 진행
```

금지:

- 한 하네스가 다른 하네스의 raw/catalog/index를 직접 수정
- S04가 S03 수집을 몰래 실행
- S03 raw를 S04로 복사
- 실시간 호출 또는 쓰기 권한이 있는 cross-harness coupling

## 10. 요청서와 이행보고 닫힌 고리

요청서와 이행보고는 `item_id`로 연결한다.
이 구조가 "요청한 줄 알았는데 누락됨"을 줄이는 핵심 장치다.
handoff의 Required item은 SEC/sector와 product official pages 모두 `item_id` 대조 대상이다.
따라서 Phase 1 SEC/sector top-up을 실행하기 전에 latest 10-K, latest 10-Q, sector/entity metadata 항목에는 먼저 `item_id`를 부여한다.
product official pages는 Phase B 최소 규칙과 page_key/canonical_url 후보가 준비된 뒤 Phase C 직전에 더 세분화한다.
`item_id`는 handoff 전체에서 고유해야 하며, SEC/sector preflight와 product pages 세분화가 같은 번호 공간을 공유한다.
예를 들어 SEC/sector 항목이 `APP-S03-REQ-20260613-001`부터 `-003`까지 사용하면, product pages는 `APP-S03-REQ-20260613-004`부터 이어간다.

### 10.1 S04 -> S03 요청서 권장 필드

| 필드 | 의미 |
|---|---|
| `item_id` | 요청 항목 고유번호 |
| `tier` | 1 또는 2 |
| `source_class` | sec, company-ir, company-official 등 |
| `requested_identifier` | SEC는 form/period, Tier 2는 URL 또는 공식 페이지 대상 |
| `page_key` | Tier 2에서 known이면 명시 |
| `canonical_url` | Tier 2에서 known이면 명시 |
| `snapshot_semantics` | latest_at_collection 또는 as_of |
| `blocking` | 미수집 시 downstream 진행 차단 여부 |
| `reason` | 수집 사유 |
| `affected_sections` | downstream 영향 섹션 |
| `explicit_deferred_items` | 이번에 수집하지 않을 것 |

### 10.2 S03 -> S04 이행보고 권장 필드

S03 이행보고는 별도 독립 원장보다 run-summary/QA 안에 포함하는 방식을 우선한다.

| 필드 | 의미 |
|---|---|
| `item_id` | 요청서 항목과 1:1 연결 |
| `status` | collected, skipped, failed, access_limited, deferred |
| `document_id` | catalog 문서 ID |
| `captured_at` | Tier 2 스냅샷 기준 시각 |
| `raw_path` | raw 파일 또는 raw root |
| `catalog_record` | documents/files catalog 참조 |
| `notes` | 실패, 보류, staleness, 재캡처 필요 사유 |

S04 intake check는 요청서와 이행보고를 대조한다.
required/blocking item이 모두 collected인지, 아니라면 실패/보류 사유가 명시됐는지 확인한 뒤 full Industry Primer 진행 여부를 결정한다.

## 11. 남은 작업 지도

원래 목적은 APP Source Pack top-up이다.
그러나 지금까지 논의로 인해 작업 순서는 두 갈래로 나뉜다.

### Phase A. 지금 바로 가능한 APP SEC top-up

목표:

- 최신 APP 10-K 수집
- 최신 APP 10-Q 수집
- sector/entity metadata 확인 또는 unresolved reason 기록

실행 성격:

- `partial_recheck` 또는 APP S04 SEC top-up
- 실행 전 handoff의 latest 10-K, latest 10-Q, sector/entity metadata 항목에 `item_id`를 부여
- 운영 catalog/index 반영은 사용자 승인 후 진행
- APP full SEC collection 완료로 표시하지 않음
- DEF 14A, 8-K backlog, transcript, PDF table extraction은 범위 밖

완료 산출물:

- `artifacts/companies/APP/index.md`
- `artifacts/catalog/entities.jsonl`
- `artifacts/catalog/documents.jsonl`
- `artifacts/catalog/files.jsonl`
- `artifacts/catalog/runs.jsonl`
- `artifacts/runs/{run-id}/run-summary.md`
- `artifacts/runs/{run-id}/qa.md`

### Phase B. company-official 계층2 규칙 명문화

이 Phase B는 개념적 묶음이다.
실제 실행에서는 실행 지도 Phase 2에서 최소 반영 계획을 만들고, Phase 3에서 승인된 최소 범위만 먼저 반영한다.

목표:

- S03 소유 경계와 계층 구분 반영
- `company-official` source_type 추가
- company-official raw path, document_id, file rules 정의
- 스냅샷 의미론, `captured_at`, `page_key`, stale risk, automatic cleanup 금지 명시
- company-official QA 기준 추가
- 하네스 간 handoff 규칙과 item_id 닫힌 고리 명문화

pilot-first 원칙:

- APP product page pilot을 한 번 수행할 수 있는 최소 규칙을 먼저 반영한다.
- run-summary schema, index schema, 범용 템플릿은 pilot 결과를 본 뒤 정식화한다.
- document_type 신규값은 선제 추가하지 않는다.

우선 반영할 최소 범위:

| 항목 | 최소 반영 내용 |
|---|---|
| catalog schema | `company-official` source_type, raw path, company-official 전용 document 필드 |
| collector 절차 | 수집법, snapshot, page_key, access_limited, rate limit |
| no-inference rule | source_type/page_key를 추론으로 확정하지 않고 애매하면 멈춤 |
| QA 최소 규칙 | `captured_at` 존재, access_limited 기록, raw/file 연결 확인 |

전체 수정 후보:

아래 표는 장기적으로 닿을 수 있는 후보 목록이다. 한 번에 전부 수정한다는 뜻이 아니다.

| 파일 | 수정 내용 |
|---|---|
| `harness/contracts/source-pack.contract.md` | S03 소유 경계, Tier 2 정의, 사람 승인/중단 조건 |
| `harness/schemas/source-pack-catalog.schema.md` | `company-official` source_type, company-official 필드/notes 관례, raw path |
| `harness/procedures/source-pack-runbook.md` | handoff 요청서 처리, item_id 대조, 계층2 요청 시 절차 분기 |
| `harness/procedures/source-pack-collector.md` | company-official 수집 절차 또는 별도 절차 파일 포인터 |
| `harness/procedures/source-pack-qa.md` | company-official 스냅샷, page_key, captured_at, access_limited 검증 |
| `harness/schemas/source-pack-index.schema.md` | 회사별 index의 company-official 섹션 또는 전달 요약 규칙 |
| `harness/schemas/source-pack-run-summary.schema.md` | 요청 item_id별 이행보고 섹션 |
| `docs/templates/` | S04 -> S03 요청서 템플릿, S03 이행보고 템플릿 |
| `docs/README.md` | 새 설계 메모 포인터 |
| `artifacts/improvement-log.md` | 운영 규칙 반영 시 변경 기록 |

주의:

- `document_type`은 지금 새로 늘리지 않는다.
- company-official 수집은 broad crawl이 아니다.
- 스냅샷 재사용은 "최신 선택"을 위한 읽기 규칙이지, 오래된 자료 삭제나 현재성 보증이 아니다.

### Phase C. APP product-level official docs/pages top-up

전제:

- Phase B의 최소 규칙이 반영됐거나, 최소한 이번 run에서 승인된 company-official 임시 절차가 명시돼 있어야 한다.

대상:

- MAX
- AXON
- AppDiscovery
- mediation
- advertising
- measurement
- 기타 handoff에서 요구한 official product/platform pages

실행 성격:

- S04 요청 기반 Tier 2 company-official top-up
- URL 또는 page_key가 명확하지 않으면 candidate로 기록하고 사용자 확인
- 수집 시점의 스냅샷으로 raw/catalog에 저장

완료 산출물:

- APP index의 company-official/Tier 2 공식자료 섹션
- `documents.jsonl`의 `source_type: company-official` records
- `files.jsonl`의 raw snapshot file records
- run-summary의 item_id별 이행보고
- QA의 stale risk, access_limited, page_key 검증

### Phase D. S04 read-only intake check

S03 작업 완료 후 S04에서 수행한다.

확인:

- APP index 최신 run 반영
- 최신 10-K/10-Q catalog 및 raw 파일 존재
- sector/entity metadata resolved 또는 unresolved reason 기록
- product-level official docs/pages item_id별 collected/failed/deferred 정산
- captured_at과 staleness 확인
- S04 raw copy 없음
- S03 산출물에 투자 판단, 시장점유율, 경쟁/해자/valuation 결론 없음

### Phase E. S04 external-source expansion and official run

S03 Source Pack intake가 통과하면 S04가 이어서 수행한다.

S04 소관:

- submarket taxonomy
- growth drivers
- regulation and platform policy
- technology change
- structural risk
- third-party analyst, market research, media, industry sources

S04는 S03 raw/catalog를 read-only로 참조하고, 외부 근거는 S04 자신의 source register와 QA 기준으로 관리한다.

## 12. 권장 실행 순서

아래 순서는 이 설계 메모의 개념적 흐름이다.
실제 진행 상태와 다음 행동은 실행 지도 `docs/design/app-source-pack-topup-2026-06/app-source-pack-topup-execution-map-2026-06-14.md`를 기준으로 갱신한다.

가장 안전한 순서:

1. 이 문서를 사용자와 Claude Code/Codex가 확인한다.
2. APP SEC/sector 항목에 먼저 `item_id`를 부여한다.
3. APP SEC 10-K/10-Q + sector/entity metadata top-up을 실행한다.
4. company-official 계층2 규칙 반영 계획을 만든다.
5. 사용자 승인 후 `harness/`와 templates에 최소 규칙을 반영한다.
6. APP product-level official docs/pages 요청 항목을 `item_id`, page_key, canonical_url 후보로 세분화한다.
7. APP product-level official docs/pages를 company-official 스냅샷으로 수집한다.
8. S03 run-summary/QA에서 S04 handoff item_id별 이행보고를 완성한다.
9. S04가 updated APP Source Pack을 read-only intake check한다.
10. S04가 external-source expansion 여부를 판단하고 full Industry Primer official run으로 넘어간다.

속도를 우선할 경우:

1. APP SEC top-up만 먼저 진행한다.
2. 단, SEC/sector item_id는 먼저 부여하고 run-summary/QA에 매핑한다.
3. S04에 SEC/metadata 부분 사용 가능 상태를 알려준다.
4. product-level docs는 company-official 규칙 반영 뒤 별도 top-up으로 진행한다.

피해야 할 순서:

- company-official 규칙 없이 product page를 `manual/other`로 바로 운영 catalog에 밀어 넣기
- S04가 S03 수집을 실시간 호출하거나 S03 raw/catalog를 직접 수정하기
- 오래된 product page 스냅샷을 현재 공식 페이지처럼 표시하기
- APP top-up 결과를 APP full Source Pack collection 완료로 표시하기

## 13. 현재 합의 상태

합의된 내용:

- S03은 회사 공식 작성/배포 자료의 보관소다.
- 제3자 분석 자료는 S03이 아니라 downstream external-source expansion의 대상이다.
- Tier 1과 Tier 2는 모두 S03 소유지만 수집 시점이 다르다.
- Tier 2는 요청 시 스냅샷으로 수집한다.
- `company-official` source_type은 필요하다.
- `document_type`은 지금 선제 신설하지 않고 `other + notes/candidate`로 관찰한다.
- `captured_at`, `page_key`, stale risk를 드러낸다.
- `page_key`는 추론이 아니라 요청서 명시, 정확 일치 재사용, 또는 사용자 승인으로 확정한다.
- 오래된 스냅샷은 자동 정리하지 않는다.
- handoff는 요청서와 이행보고를 `item_id`로 연결한다.
- S04는 S03을 실시간 호출하지 않고, S03 완료 후 read-only intake를 수행한다.

아직 운영 규칙에 반영해야 할 내용:

- `harness/` 계약, schema, 절차, QA에 company-official 최소 규칙 반영
- Phase 1 전 기존 APP handoff의 SEC/sector 항목에 `item_id`를 먼저 부여
- product official pages 항목은 Phase 5 전 `item_id`, page_key, canonical_url 후보로 세분화
- `source_subtype` 정식 필드 도입 여부는 APP pilot 이후 판단
- 요청서/이행보고 템플릿 정식화 여부는 APP pilot 이후 판단
