# Company-official Phase 3 Harness Change Proposal

작성일: 2026-06-14
대상: P2-S03 Source Pack / APP top-up Phase 3
상태: 수정안 제시, `harness/` 미수정
관련 실행 지도: `docs/design/app-source-pack-topup-2026-06/app-source-pack-topup-execution-map-2026-06-14.md`
관련 Phase 2 계획: `docs/design/app-source-pack-topup-2026-06/company-official-minimum-implementation-plan-2026-06-14.md`

---

## 0. 이 문서의 역할

이 문서는 `company-official` 자료를 Source Pack 운영 harness에 최소 반영하기 위한 Phase 3 수정안이다.

이 문서는 실제 운영 규칙을 변경하지 않는다. 사용자가 이 수정안을 검토하고 별도 승인하기 전에는 아래 `harness/` 파일을 편집하지 않는다.

대상 후보:

- `harness/schemas/source-pack-catalog.schema.md`
- `harness/procedures/source-pack-collector.md`
- `harness/procedures/source-pack-runbook.md`
- `harness/procedures/source-pack-qa.md`

비대상:

- `document_type` 신규값 추가
- `source_subtype` 정식 필드 추가
- APP product pages 실제 수집
- S04 파일 수정
- broad crawl 또는 자동 제품 페이지 발견

---

## 1. 핵심 결정

### 1.1 `company-official` document_id

`company-official` 스냅샷은 같은 공식 페이지의 여러 시점 캡처를 별도 document로 보존한다.

형식:

```text
company-official:{ticker-lower}:{page_key}:{captured_at_slug}
```

예:

```text
company-official:app:app-max:2026-06-13-09-15-00
```

규칙:

- `ticker-lower`는 티커를 소문자로 쓴다. 예: `app`
- `page_key`는 승인된 page_key를 그대로 쓴다.
- `captured_at`은 이번 run에서 실제 페이지를 캡처한 순간이다.
- `captured_at_slug`는 `captured_at`을 S03 기준 타임존 `+09:00`으로 렌더링한 값이다.
- `captured_at_slug` 형식은 `YYYY-MM-DD-hh-mm-ss`다.
- `captured_at_slug`는 별도 catalog 필드가 아니라 `document_id`와 raw path에 쓰는 결정적 파생값이다.
- `captured_at_slug`는 slug 규칙을 따른다: 소문자, 숫자, 하이픈만 사용한다.

예:

```text
captured_at: 2026-06-13T09:15:00+09:00
captured_at_slug: 2026-06-13-09-15-00
```

UTC `Z` 또는 대문자 `T`를 `captured_at_slug`에 넣지 않는다. Windows 경로와 기존 slug 규칙에서 대소문자 차이로 생길 수 있는 drift를 피하기 위해서다.

### 1.2 raw path

raw path는 `document_id`와 같은 시간 정밀도를 사용한다.

형식:

```text
artifacts/raw/company-official/{TICKER}/{captured_at_slug}_{page_key}/
```

예:

```text
artifacts/raw/company-official/APP/2026-06-13-09-15-00_app-max/
```

권장 파일명:

| 파일 | 역할 |
|---|---|
| `metadata.json` | page_key, canonical_url, source_url, captured_at, retrieved_at, 접근 상태 |
| `document.html` | 캡처한 HTML snapshot |
| `assets/` | 필요한 경우 이미지 또는 부속 파일 |

충돌 규칙:

- 같은 raw path가 이미 있으면 기존 파일을 덮어쓰지 않는다.
- 같은 `document_id`와 raw path가 이미 있고 fast path 조건을 통과하면 `skipped_existing` 후보가 될 수 있다.
- `skipped_existing`은 같은 page_key와 같은 captured_at_slug, 즉 동일 `document_id`에 대한 멱등 재실행 가드로만 허용한다.
- 기존 page_key가 있다는 이유만으로 fast path skip 또는 `skipped_existing` 처리하지 않는다.
- 같은 path가 있는데 catalog/file 관계가 맞지 않으면 `repair_required` 또는 QA 확인 필요로 표면화한다.
- 같은 초에 같은 page_key를 다시 캡처하는 경우는 드물지만, 발생하면 자동 덮어쓰기 대신 run-summary와 QA에 남긴다.

### 1.3 entity_id

`company-official` page snapshot은 새 entity를 만들지 않는다.

규칙:

- 기존 회사 entity를 재사용한다.
- APP의 경우 `entity_id: sec-cik-0001751008`을 사용한다.
- product page, platform page, policy page가 각각 별도 entity가 되지 않는다.

### 1.4 `source_url`과 `canonical_url`

두 필드는 의미가 다르다.

| 필드 | 의미 |
|---|---|
| `canonical_url` | `page_key` 정체성을 묶는 기준 URL |
| `source_url` | 이번 run에서 실제 요청하거나 저장한 URL |

대부분 같을 수 있지만 redirect, locale, trailing slash, tracking parameter 때문에 다를 수 있다.

규칙:

- `canonical_url`은 page identity 판단에 쓴다.
- `source_url`은 이번 수집 attempt와 file provenance에 쓴다.
- 둘이 다르면 `notes`, `metadata.json`, `run-summary.md` 중 적어도 한 곳에 이유를 남긴다.
- URL fuzzy matching으로 canonical_url을 자동 확정하지 않는다.

### 1.5 filing_date와 captured_at

`company-official` product/platform page snapshot에서 권위 시각은 `captured_at`이다.

규칙:

- `filing_date`는 보통 `null`이다.
- `captured_at`을 `filing_date`에 대체 기입하지 않는다.
- 명시 게시일이 있는 별도 official document를 다루는 경우에도 APP pilot에서는 우선 `captured_at` 중심으로 처리하고, 게시일 처리 규칙은 pilot 이후 승격 판단한다.

---

## 2. `harness/schemas/source-pack-catalog.schema.md` 수정안

### 2.1 식별자 규칙

`document_id` 섹션에 아래 내용을 추가한다.

```text
Company official snapshot:

company-official:{ticker-lower}:{page_key}:{captured_at_slug}

예:

company-official:app:app-max:2026-06-13-09-15-00
```

추가 설명:

- `captured_at`은 이번 run에서 실제 페이지를 캡처한 순간이다.
- `captured_at_slug`는 `captured_at`을 `+09:00` 기준 `YYYY-MM-DD-hh-mm-ss`로 렌더링한 값이다.
- `captured_at_slug`는 slug 규칙을 따르며 대문자 `T`, `Z`, 콜론을 쓰지 않는다.
- 같은 `page_key`의 여러 스냅샷은 서로 다른 `captured_at_slug`를 가진 별도 document가 된다.
- 기존 page_key가 있다는 이유만으로 현재 페이지 수집을 fast path skip하지 않는다.

### 2.2 documents.jsonl 필드

`documents.jsonl`의 source_type 허용값에 `company-official`을 추가한다.

```text
`source_type` | `sec-edgar`, `company-ir`, `company-official`, `transcripts`, `industry-source`, `manual`
```

`document_type` 허용값은 변경하지 않는다.

`SEC 전용 필드`, `공통 선택 필드`, `IR/transcript 선택 필드` 패턴 옆에 아래 조건부 그룹을 추가한다.

```text
company-official 전용 필드:

| 필드 | 타입 | 설명 | 예시 |
|---|---|---|---|
| `captured_at` | string or null | 공식 페이지 snapshot 기준 시각. ISO 8601 `+09:00` 기준 | `2026-06-13T09:15:00+09:00` |
| `page_key` | string or null | 같은 공식 페이지의 여러 snapshot을 묶는 승인 식별자 | `app-max` |
| `canonical_url` | string or null | page_key 정체성을 묶는 기준 URL | `https://www.applovin.com/max/` |
```

조건:

- 위 세 필드는 `source_type: company-official` record에는 필요하다.
- 위 세 필드를 documents 공통 필수 필드로 올리지 않는다.
- SEC, IR, transcript record에는 없어도 schema 위반이 아니다.
- product page는 `document_type: other`로 기록한다.
- `source_subtype`은 정식 필드로 추가하지 않는다.
- 필요한 경우 `notes`에 `handled_as: other`, `candidate_source_subtype: product_page`, `candidate_document_type: company-product-page`처럼 후보만 남긴다.

예시 record:

```json
{"document_id":"company-official:app:app-max:2026-06-13-09-15-00","entity_id":"sec-cik-0001751008","ticker":"APP","cik":"0001751008","source_type":"company-official","document_type":"other","title":"AppLovin MAX official page snapshot","filing_date":null,"period_end":null,"fiscal_year":null,"source_url":"https://www.applovin.com/max/","collection_status":"collected","primary_file_id":"sha256:example","raw_root":"artifacts/raw/company-official/APP/2026-06-13-09-15-00_app-max","text_root":null,"first_collected_run_id":"run-20260614-app-company-official-pilot","last_checked_run_id":"run-20260614-app-company-official-pilot","captured_at":"2026-06-13T09:15:00+09:00","page_key":"app-max","canonical_url":"https://www.applovin.com/max/","text_status":"not_applicable","notes":"handled_as: other; candidate_source_subtype: product_page"}
```

### 2.3 files.jsonl 필드

`files.jsonl`의 source_type 허용값에 `company-official`을 추가한다.

```text
`source_type` | `sec-edgar`, `company-ir`, `company-official`, `transcripts`, `industry-source`, `manual`
```

파일 역할은 기존 값을 재사용한다.

- snapshot HTML: `file_role: primary`, `file_format: html`
- raw support metadata: 기본적으로 `metadata.json`으로 저장하되, raw-only support file로 둔다.
- files catalog의 기본 승격 대상은 primary HTML snapshot인 `document.html`이다.
- 정식 `source_subtype`이나 신규 `file_role`은 만들지 않는다.

예시 file record:

```json
{"file_id":"sha256:example","document_id":"company-official:app:app-max:2026-06-13-09-15-00","entity_id":"sec-cik-0001751008","ticker":"APP","source_type":"company-official","file_role":"primary","file_format":"html","local_path":"artifacts/raw/company-official/APP/2026-06-13-09-15-00_app-max/document.html","source_url":"https://www.applovin.com/max/","sha256":"example","size_bytes":123456,"retrieved_at":"2026-06-13T09:15:03+09:00","run_id":"run-20260614-app-company-official-pilot","file_status":"available","content_type":"text/html"}
```

### 2.4 raw 경로 규칙

`raw 경로 규칙`에 `Company official` 하위 섹션을 추가한다.

```text
### Company official

artifacts/raw/company-official/{TICKER}/{captured_at_slug}_{page_key}/
```

권장 파일명:

| 파일 | 역할 |
|---|---|
| `metadata.json` | page_key, canonical_url, source_url, captured_at, retrieved_at, 접근 상태 |
| `document.html` | 캡처한 HTML snapshot |
| `assets/` | 필요한 경우 이미지 또는 부속 파일 |

주의:

- `{page_key}`는 승인된 page_key다. 경로용 slug를 따로 추론하지 않는다.
- `{captured_at_slug}`는 `captured_at`을 `+09:00` 기준 `YYYY-MM-DD-hh-mm-ss`로 렌더링한 값이다.
- 같은 raw path가 이미 있으면 덮어쓰지 않는다.
- `metadata.json`은 기본적으로 raw-only support file이며, files catalog 기본 승격 대상은 `document.html`이다.

---

## 3. `harness/procedures/source-pack-collector.md` 수정안

IR 수집 단계 다음, transcript 수집 전 위치에 `6.5단계: Company official page snapshot 수집`을 추가한다.

### 3.1 실행 조건

`company-official` 수집은 아래 조건을 만족할 때만 실행한다.

- 사용자 요청 또는 handoff item_id가 company-official 수집을 명시한다.
- run_scope에 company-official 범위가 명시되어 있다.
- 수집 대상 URL이 명시되어 있거나 사람이 승인한 canonical_url이 있다.
- page_key가 요청서에 명시되어 있거나, 기존 exact canonical_url match로 재사용 가능하거나, 사람이 신규 page_key를 승인했다.

아래 경우에는 수집하지 않고 `deferred` 또는 `[확인 필요:]`로 남긴다.

- page_key를 에이전트가 fuzzy URL matching으로 확정해야 하는 경우
- canonical_url이 불명확한 경우
- product page를 broad crawl로 찾아야 하는 경우
- URL 이전 여부를 자동 판단해야 하는 경우

### 3.2 접근 규칙

- Cloudflare, bot 차단, 로그인 요구, 유료벽, 등록벽은 우회하지 않는다.
- 접근 제한은 `access_limited`, `blocked`, `deferred`, `failed` 중 적절한 상태로 `download-log.jsonl`, `run-summary.md`, `qa.md`에 남긴다.
- 속도 제한은 company IR 수집 간격을 재사용한다.
- 요청 범위에 없는 product pages는 수집하지 않는다.

### 3.3 document_id와 raw path 생성

처리 순서:

1. 기존 회사 entity를 확인한다.
2. `entity_id`는 기존 회사 entity를 재사용한다. APP 예: `sec-cik-0001751008`
3. `captured_at`은 이번 run에서 실제 페이지를 캡처한 순간으로 기록한다.
4. `captured_at_slug`를 `YYYY-MM-DD-hh-mm-ss` 형식으로 만든다.
5. `document_id`를 만든다.

```text
company-official:{ticker-lower}:{page_key}:{captured_at_slug}
```

6. raw_root를 만든다.

```text
artifacts/raw/company-official/{TICKER}/{captured_at_slug}_{page_key}/
```

7. 같은 raw_root가 이미 있으면 덮어쓰지 않는다.
8. 동일 `document_id`의 catalog/file/raw 관계가 완전하면 멱등 재실행으로 보고 `skipped_existing` 후보로 기록할 수 있다.
9. 기존 raw_root가 있는데 catalog/file 관계가 불완전하면 `repair_required` 또는 QA 확인 필요로 남긴다.

fast path 제한:

- 새 수집 요청은 새 `captured_at`과 새 `captured_at_slug`를 만들며, 따라서 새 `document_id`를 만든다.
- 기존 page_key가 있다는 이유만으로 fast path skip 또는 `skipped_existing` 처리하지 않는다.
- `skipped_existing`은 같은 page_key와 같은 captured_at_slug, 즉 동일 `document_id`가 이미 있고 catalog/file/raw 관계가 온전한 경우에만 허용한다.

### 3.4 URL 처리

- `canonical_url`은 page_key 정체성 기준 URL이다.
- `source_url`은 이번 run에서 실제 요청한 URL이다.
- 둘이 다르면 redirect 또는 접근 경로 차이를 `metadata.json`과 notes에 남긴다.
- exact canonical_url match가 있으면 기존 page_key를 재사용할 수 있다.
- 같은 page_key가 APP pilot 범위에서 둘 이상의 distinct canonical_url에 연결되면 자동 병합하지 않고 사람 확인으로 넘긴다.
- URL 이전 가능성이 있으면 기존 page_key 확장 또는 새 page_key 생성 여부를 사람 판단으로 보류한다.

### 3.5 성공 처리

성공 시:

- `documents.jsonl`
  - `source_type: company-official`
  - `document_type: other`
  - `captured_at`, `page_key`, `canonical_url` 기록
  - `filing_date: null` 기본
  - `source_url`은 실제 요청 URL
  - `entity_id`는 기존 회사 entity
- `files.jsonl`
  - HTML snapshot은 `file_role: primary`, `file_format: html`
  - `retrieved_at`은 파일을 받은 시각
  - `local_path`는 `document.html`
- raw folder
  - `metadata.json`
  - `document.html`
  - 필요 시 `assets/`
- `metadata.json`
  - 기본적으로 raw-only support file로 둔다.
  - files catalog 기본 승격 대상은 아니다.
- `download-log.jsonl`
  - 성공 또는 실패 attempt 기록
- `run-summary.md`와 `qa.md`
  - item_id별 collected, skipped_existing, deferred, failed, access_limited 기록

### 3.6 실패와 보류 처리

- 실패한 다운로드는 기본적으로 `files.jsonl`에 승격하지 않는다.
- 접근 제한은 조용히 누락하지 않는다.
- page_key/canonical_url 승인이 없으면 `deferred`로 남긴다.
- URL 이전 의심은 자동 처리하지 않고 `[확인 필요: company-official URL 이전 판단]`으로 남긴다.

---

## 4. `harness/procedures/source-pack-runbook.md` 수정안

runbook에는 세부 절차를 길게 복사하지 않고 collector 절차로 연결한다.

### 4.1 요청 범위 표

`티커별 실행`의 요청 범위 표에 아래 행을 추가한다.

| 요청 범위 | 따를 절차 |
|---|---|
| company-official page snapshot | `harness/procedures/source-pack-collector.md`의 company-official 단계 |

### 4.2 승인 조건

설정 확인과 승인 또는 티커별 실행 직전에 아래 조건을 추가한다.

- company-official 수집은 run_scope에 명시되어야 한다.
- 명시 URL 또는 승인된 canonical_url이 있어야 한다.
- 승인된 page_key 또는 exact canonical_url match 기반 기존 page_key가 있어야 한다.
- 신규 page_key는 사용자 승인 전 운영 catalog에 확정 기록하지 않는다.
- 요청 범위에 없는 product pages는 수집하지 않는다.

### 4.3 사용자 보고

사용자 보고에는 아래 항목을 포함한다.

- item_id별 collected, skipped_existing, deferred, failed, access_limited
- 신규 page_key 승인 필요 항목
- page_key/canonical_url 충돌
- URL 이전 판단 보류
- stale snapshot 주의
- S04가 읽을 때 같은 page_key 중 최신 captured_at snapshot을 기본으로 본다는 안내

---

## 5. `harness/procedures/source-pack-qa.md` 수정안

표준 14단계 제목은 유지한다. company-official은 조건부 체크로 추가한다.

### 5.1 3단계: schema 허용값 검증

추가 확인:

- `documents.jsonl.source_type`과 `files.jsonl.source_type`에서 `company-official`은 허용값이다.
- `document_type` 신규값이 있으면 fail 또는 확인 필요로 둔다.
- `source_subtype` 정식 필드가 등장하면 확인 필요로 둔다.
- product page snapshot은 `document_type: other`로 처리됐는지 확인한다.

### 5.2 5단계: document 원장 검증

`source_type: company-official` record에 아래 조건을 추가한다.

- `document_id`가 아래 형식인지 확인한다.

```text
company-official:{ticker-lower}:{page_key}:{captured_at_slug}
```

- `captured_at_slug`가 `captured_at`을 `+09:00` 기준 `YYYY-MM-DD-hh-mm-ss`로 렌더링한 값과 일치하는지 확인한다.
- `captured_at`, `page_key`, `canonical_url`이 존재하는지 확인한다.
- `captured_at`이 이번 run의 실제 캡처 순간으로 기록됐는지 확인한다.
- `entity_id`가 기존 회사 entity에 연결되는지 확인한다.
- `filing_date`에 captured_at을 대체 기입하지 않았는지 확인한다.
- `source_url`과 `canonical_url`의 의미가 구분되어 있는지 확인한다.
- APP pilot 범위에서 하나의 page_key가 둘 이상의 distinct canonical_url에 연결되면 QA 실패 또는 사람 확인 항목으로 표면화한다.
- URL 이전 가능성은 자동 통과시키지 않고 사람 확인 항목으로 남긴다.
- 기존 page_key가 있다는 이유만으로 fast path skip 또는 `skipped_existing` 처리되지 않았는지 확인한다.

### 5.3 6단계: file 원장과 raw 파일 검증

추가 확인:

- raw_root가 아래 형식인지 확인한다.

```text
artifacts/raw/company-official/{TICKER}/{captured_at_slug}_{page_key}/
```

- `files.jsonl.local_path`가 raw_root 하위의 실제 파일을 가리키는지 확인한다.
- primary HTML snapshot은 `file_role: primary`, `file_format: html`인지 확인한다.
- `metadata.json`은 기본적으로 raw-only support file이고, files catalog 기본 승격 대상이 `document.html`인지 확인한다.
- company-official record의 `captured_at`과 primary file의 `retrieved_at`이 같은 run의 실제 캡처 흐름 안에서 근접한지 확인한다.
- 두 시각이 설명 없이 크게 벌어지면 `[확인 필요: captured_at vs retrieved_at 불일치]`로 표면화한다.
- `captured_at`과 `retrieved_at`은 의미가 다른 필드이므로 완전 동일성을 요구하지 않는다.
- 같은 raw path 충돌이 있었는지 확인하고, 덮어쓰기가 의심되면 `unverified` 또는 `fail`로 둔다.
- `size_bytes`, `sha256`, `file_status: available` 기존 검증을 적용한다.

### 5.4 7단계: download-log와 승격 관계 검증

추가 확인:

- 접근 제한, 로그인 요구, 유료벽, bot 차단이 조용히 누락되지 않았는지 확인한다.
- 실패 attempt가 `files.jsonl`에 잘못 승격되지 않았는지 확인한다.
- `deferred`, `access_limited`, `failed` 사유가 run-summary 또는 QA에 남아 있는지 확인한다.

### 5.5 다음 하네스 사용 가능 여부

`다음 하네스 사용 가능 여부` 섹션에 company-official이 포함된 경우 아래를 남긴다.

- downstream 기본 선택 규칙: 같은 `page_key` 중 최신 `captured_at` snapshot을 사용한다.
- stale snapshot이면 최신성 주의를 남긴다.
- `access_limited`, `deferred`, URL 이전 보류 항목이 있으면 S04 사용 가능 여부에 반영한다.

---

## 6. 기존 SEC/IR/transcript 호환성 검토

이 수정안은 기존 record를 깨지 않도록 아래 원칙을 둔다.

| 항목 | 호환성 판단 |
|---|---|
| `source_type` | 허용값 추가만 한다. 기존 값은 유지된다. |
| `document_type` | 신규값을 만들지 않는다. 기존 enum 그대로다. |
| `captured_at`, `page_key`, `canonical_url` | company-official 조건부 필드다. 공통 필수 필드가 아니다. |
| SEC document_id | 기존 `sec:{cik}:{accession_no}:{document_type_slug}` 유지 |
| IR document_id | 기존 `ir-{ticker}-{document_type_slug}-{period_or_date}` 유지 |
| transcript document_id | 기존 `transcript:{ticker}:{date}:{source_slug}:{slug}` 유지 |
| SEC fast path | 변경하지 않는다. |
| company-official fast path | 동일 `document_id` 멱등 재실행 가드로만 사용한다. page_key만으로 skip하지 않는다. |
| IR taxonomy candidate 방식 | 유지한다. `document_type` 승격은 사용자 승인 후 별도 처리다. |

따라서 기존 SEC/IR/transcript records는 `captured_at`, `page_key`, `canonical_url`이 없어도 schema 위반이 되면 안 된다.

---

## 7. Phase 3 실제 편집 순서 제안

실제 반영을 승인받으면 아래 순서로 진행한다.

1. `source-pack-catalog.schema.md`
   - `document_id` 형식 추가
   - documents/files `source_type` enum 추가
   - company-official 전용 필드 추가
   - raw path 규칙 추가
2. `source-pack-collector.md`
   - 6.5단계 추가
   - no-inference, access_limited, document_id/raw path 생성 규칙 추가
3. `source-pack-runbook.md`
   - 요청 범위 표와 승인 조건에 포인터 추가
   - 세부 절차는 collector로 연결
4. `source-pack-qa.md`
   - 3/5/6/7단계 조건부 체크 추가
   - 다음 하네스 사용 가능 여부에 최신 captured_at/stale 표시 추가
5. 검증
   - `git diff --name-only -- harness`
   - `rg -n "company-official|captured_at_slug|page_key|canonical_url" harness`
   - 기존 catalog JSONL parse 확인
   - 기존 SEC/IR/transcript record가 새 company-official 필드 부재로 실패하지 않는지 확인

---

## 8. 승인 전 확인 질문

실제 `harness/` 편집 전 확인할 질문:

1. `company-official` document_id를 `company-official:{ticker-lower}:{page_key}:{captured_at_slug}`로 확정해도 되는가?
2. `captured_at_slug`를 `+09:00` 기준 `YYYY-MM-DD-hh-mm-ss`로 확정해도 되는가?
3. raw path를 `artifacts/raw/company-official/{TICKER}/{captured_at_slug}_{page_key}/`로 확정해도 되는가?
4. `source_url`과 `canonical_url`의 의미 구분, `filing_date: null` 기본 원칙을 schema/collector/QA에 넣어도 되는가?
5. Phase 3 실제 편집 범위를 이 문서의 4개 harness 파일로 제한해도 되는가?
