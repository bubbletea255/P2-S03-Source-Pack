# SEC-IR 중복 처리와 raw 무결성 점검 설계 메모

- 작성일: 2026-06-05
- 대상 하네스: P2-S03 Source Pack
- 주제: SEC/IR 중복 처리, canonical source, raw 파일 무결성, 재확인 전략
- 상태: 설계 메모, 일부 최소 절차는 `harness/`에 반영됨
- 적용 전제: notes 기반 overlap/integrity 상태 기록은 `source-pack-ir-collector.md`에 최소 반영됐고, 별도 관계 원장과 정기 audit 자동화는 아직 반영하지 않음

이 문서는 SEC 자료와 Company IR 자료가 서로 중복될 때 어떻게 처리할지, 그리고 장기적으로 raw 파일이 삭제되거나 잘못 연결되는 위험을 어떻게 줄일지 정리하기 위한 설계 메모다.

이 문서는 투자 분석이 아니다.
이 문서는 Source Pack이 다음 하네스에 안정적인 원자료 입력을 넘기기 위한 운영 설계 기록이다.

## 1. 왜 이 메모를 남기는가

Source Pack은 SEC 공시, Company IR, transcript 같은 여러 출처의 원자료를 수집한다.
그중 SEC와 IR은 특히 중복 가능성이 높다.

예:

- 회사가 실적 발표 press release를 IR 사이트에 올리고, 같은 내용을 SEC 8-K EX-99.1로 제출한다.
- 회사가 financial supplement 또는 earnings presentation을 IR 사이트에 먼저 올리고, 나중에 SEC exhibit로 제출한다.
- SEC에는 공시가 있지만 IR 사이트에는 같은 자료가 없거나, 반대로 IR 사이트에만 investor conference deck이 있다.

중복 처리 기준이 없으면 아래 문제가 생긴다.

- 같은 파일을 SEC raw와 IR raw에 반복 저장한다.
- 같은 파일인지 아닌지 매 실행마다 다시 판단해 토큰과 시간이 낭비된다.
- 한 번 잘못 중복 판단하면 중요한 자료를 영구히 놓칠 수 있다.
- raw 파일이 삭제되거나 보안 제품에 격리되어도 catalog는 계속 있다고 믿을 수 있다.
- 200개 기업, 여러 해의 자료로 커졌을 때 사람이 일일이 열어보는 방식으로는 관리할 수 없다.

따라서 중복 제거와 무결성 점검은 Source Pack의 핵심 운영 설계다.

## 2. 쉬운 비유

이 하네스에서는 아래 네 가지를 구분해야 한다.

| 개념 | 쉬운 비유 | 현재 또는 후보 위치 |
|---|---|---|
| 논리 문서 | 어떤 책인가 | `documents.jsonl` |
| 실제 파일 | 책 파일 자체 | `files.jsonl` + `artifacts/raw/` |
| 출처 관찰 | 이 책을 어느 서점에서 봤는가 | 현재는 `notes`, 장기적으로 별도 source observation 후보 |
| 관계 기록 | 두 책이 같은가, 다른가, 아직 모르는가 | 현재는 `notes`, 장기적으로 별도 relationship 후보 |

핵심:

```text
파일이 같다는 것과 출처가 같다는 것은 다르다.
문서 의미가 같다는 것과 파일 hash가 같다는 것도 다르다.
```

## 3. 기본 원칙

### 3.1 SEC는 SEC 공시의 canonical source다

SEC form 자체는 SEC EDGAR를 canonical source로 둔다.

예:

- 10-K
- 10-Q
- DEF 14A
- 8-K
- 8-K exhibit

IR 사이트에 10-K PDF나 SEC filing 사본이 있어도, 운영 기준은 SEC 자료다.

### 3.2 IR-native 자료는 Company IR이 canonical source다

SEC에 보통 제출되지 않는 자료는 IR을 canonical source로 둔다.

예:

- investor conference deck
- investor day presentation
- product strategy deck
- shareholder letter
- scientific/clinical update deck
- governance 또는 ESG 자료 중 SEC filing이 아닌 회사 게시 자료

### 3.3 Earnings-related IR 자료는 중복 가능성이 높다

다음 자료는 SEC 8-K Item 2.02 또는 EX-99.1과 겹칠 가능성이 크다.

현재 active type:

- `ir-earnings-release`
- `ir-financial-supplement`

후보 type:

- `ir-earnings-presentation`
- `ir-financial-update`

이 자료들은 production 단계에서 SEC overlap 확인 대상이다.

### 3.4 같은 파일은 hash로 판단한다

같은 파일인지의 가장 강한 증거는 SHA-256 hash다.

```text
hash 동일 → 파일 내용 동일
hash 다름 → 파일 내용이 다르거나 포맷/버전이 다름
```

다만 hash가 다르다고 해서 문서 의미가 완전히 다르다는 뜻은 아니다.
HTML/PDF 포맷 차이, 회사 IR 버전과 SEC exhibit 버전 차이, wrapper 페이지 차이 때문에 hash가 다를 수 있다.

## 4. 중복 판단 상태값 제안

현재는 schema 확장 없이 `notes`에 문자열로 남긴다.
나중에 필요하면 정식 필드 또는 별도 관계 원장으로 승격할 수 있다.

권장 상태값:

| 상태 | 의미 | 처리 |
|---|---|---|
| `same_hash_as_sec` | SEC 파일과 hash가 동일 | company-ir raw 중복 승격하지 않음 |
| `different_hash_from_sec_candidate` | 의미상 SEC 후보와 겹치지만 hash는 다름 | 별도 raw 보관 가능, notes에 관계 기록 |
| `sec_equivalent_not_found_in_scoped_8k` | 제한 확인 범위 안에서 해당 IR 유형에 대응하는 SEC exhibit가 없음 | IR raw를 company-ir canonical으로 보관, 확인 범위 기록 |
| `not_found_in_local_catalog` | 현재 로컬 catalog/raw 안에서는 SEC 후보 없음 | IR raw 보관 가능, 나중에 SEC 수집 시 재확인 |
| `sec_candidate_unverified` | 후보는 있으나 확인 불충분 | 사람 확인 또는 다음 QA 대상으로 남김 |
| `ir_native_no_sec_overlap_required` | IR-native 자료라 SEC overlap 기본 검사 불필요 | 바로 IR raw 보관 가능 |
| `security_quarantined` | 파일이 보안 제품에 의해 격리/삭제됨 | 운영 catalog/index 승격 금지 |
| `raw_missing_repair_required` | catalog에는 있으나 local_path 파일 없음 | repair queue로 보냄 |

## 5. 기록할 메타데이터 후보

현재는 `notes`에 넣고, 나중에 필요하면 정식 필드로 분리한다.

| 후보 필드 | 의미 |
|---|---|
| `canonical_source` | 대표 출처. 예: `sec-edgar`, `company-ir` |
| `overlap_status` | 중복 판단 상태 |
| `overlap_checked_at` | 중복 확인 날짜 |
| `overlap_check_scope` | 확인 범위. 예: `existing_local_catalog_files_raw_only` |
| `related_sec_document_id` | 관련 SEC document_id |
| `related_sec_file_id` | 관련 SEC file_id |
| `related_ir_document_id` | 관련 IR document_id |
| `exact_hash_match` | hash 동일 여부 |
| `confidence` | 판단 확신도. `high`, `medium`, `low` |
| `future_recheck_required` | 나중에 다시 확인해야 하는지 |
| `repair_required` | 파일/관계 복구가 필요한지 |

초기에는 아래처럼 `notes` 문자열로 충분하다.

```text
overlap_status: not_found_in_local_catalog; exact_hash_match: false; overlap_check_scope: existing_local_catalog_files_raw_only; confidence: low; future_recheck_required: true
```

## 6. 상황별 처리 원칙

### 6.1 SEC를 먼저 수집했고, IR에서 같은 파일을 나중에 발견한 경우

흐름:

```text
1. IR 후보 파일을 임시 위치에 다운로드한다.
2. hash와 size를 계산한다.
3. 동일 ticker의 기존 SEC files.jsonl record에서 같은 hash를 찾는다.
4. hash가 같으면 IR raw로 승격하지 않는다.
5. IR 출처를 발견했다는 기록 또는 cross-reference만 남긴다.
6. canonical_source는 sec-edgar로 둔다.
```

권장 notes:

```text
overlap_status: same_hash_as_sec; canonical_source: sec-edgar; exact_hash_match: true; confidence: high
```

주의:

- hash 비교 전에는 "다운로드 안 함"이라고 말하면 안 된다.
- hash를 알려면 파일 bytes가 필요하므로, 적어도 임시 다운로드는 필요하다.
- 동일 hash 확인 후 임시 파일을 삭제하거나 company-ir raw로 승격하지 않는 것이 production 기본값이다.
- 이 비교는 전체 catalog scan이 아니다. IR 수집 직후, 방금 수집한 IR 파일 hash를 동일 ticker의 기존 SEC file record와만 비교한다.

### 6.2 IR을 먼저 수집했고, SEC에서 같은 파일을 나중에 발견한 경우

이 경우는 기존 설계 메모에서 상대적으로 덜 다뤄진 빈틈이다.

흐름:

```text
1. IR raw는 기존 수집 record로 보존한다.
2. 나중에 SEC 수집 시 SEC 파일 hash를 계산한다.
3. runbook 또는 별도 post-collection 단계가 방금 수집한 SEC file hash를 동일 ticker의 기존 IR files.jsonl record와 비교한다.
4. hash가 같으면 canonical_source를 sec-edgar로 업데이트한다.
5. IR record에는 related_sec_document_id 또는 related_sec_file_id를 남긴다.
6. 중복 raw를 즉시 삭제하지 않는다. 삭제 또는 대체는 별도 cleanup 정책이 있을 때만 한다.
```

권장 notes:

```text
overlap_status: same_hash_as_sec_after_sec_collection; canonical_source: sec-edgar; exact_hash_match: true; confidence: high
```

주의:

- IR을 먼저 받았다는 사실 자체는 오류가 아니다.
- SEC가 나중에 canonical source가 될 수 있다.
- 이미 수집한 IR raw를 조용히 없애면 provenance가 깨질 수 있으므로, 삭제는 별도 승인 또는 cleanup 정책 뒤에 한다.
- SEC collector가 IR collector의 세부 규칙을 직접 알 필요는 없다. cross-source overlap check 호출 책임은 runbook 또는 별도 integrity/overlap check 단계에 둔다.
- 이 비교도 전체 catalog scan이 아니다. SEC 수집 직후, 방금 수집한 해당 회사의 SEC file hash를 동일 ticker의 기존 IR records와만 비교한다.

### 6.3 SEC와 IR에 모두 있으나 hash가 다른 경우

가능한 이유:

- SEC는 HTML exhibit이고 IR은 PDF다.
- 같은 내용이지만 표지, 스크립트, wrapper, metadata가 다르다.
- 회사가 IR 사이트에 더 자세한 supplement를 올렸다.
- SEC exhibit에는 일부 내용만 포함됐다.

흐름:

```text
1. 둘 다 raw로 보관한다.
2. exact_hash_match는 false로 기록한다.
3. semantic overlap 후보로 notes에 남긴다.
4. 다음 하네스는 둘을 같은 파일로 보지 않는다.
```

권장 notes:

```text
overlap_status: different_hash_from_sec_candidate; canonical_source: sec-edgar_for_sec_filing_company-ir_for_ir_version; exact_hash_match: false; confidence: medium
```

주의:

- hash가 다르면 자동으로 중복 제거하지 않는다.
- 특히 실적 자료는 IR 버전과 SEC 버전의 차이가 중요할 수 있다.

### 6.4 SEC에만 있는 경우

SEC form 자체는 SEC collector가 canonical이다.

처리:

- SEC raw와 catalog만 유지한다.
- IR raw를 억지로 만들지 않는다.
- IR에서 발견되지 않았다고 실패로 보지 않는다.

권장 notes:

```text
canonical_source: sec-edgar; ir_counterpart: not_found_or_not_checked
```

### 6.5 IR에만 있는 경우

IR-native 자료는 정상적으로 IR raw로 보관한다.

처리:

- `source_type: company-ir`
- canonical_source는 `company-ir`
- SEC overlap 검사가 필요 없는 자료는 `ir_native_no_sec_overlap_required`로 둔다.

권장 notes:

```text
overlap_status: ir_native_no_sec_overlap_required; canonical_source: company-ir; confidence: high
```

### 6.6 현재 로컬에는 SEC 자료가 없어서 비교할 수 없는 경우

AppLovin Corporation(APP) IR test_collection run `run-20260604-app-ir-pilot`이 이 경우다.

상황:

- APP IR raw는 있다.
- 하지만 `artifacts/catalog/`와 `artifacts/raw/sec-edgar/` 안에 APP SEC 자료가 아직 없다.
- 따라서 "중복 없음"이 아니라 "현재 로컬 보유 자료 기준으로 중복 후보 없음"이라고 기록해야 한다.

권장 notes:

```text
overlap_status: not_found_in_local_catalog; exact_hash_match: false; overlap_check_scope: existing_local_catalog_files_raw_only; confidence: low; future_recheck_required: true
```

## 7. 판단 캐시 원칙

중복 판단은 매번 처음부터 다시 하면 낭비다.
하지만 한 번 판단했다고 영원히 믿으면 위험하다.

따라서 중복 판단에는 캐시와 만료 개념이 필요하다.

권장 기록:

```text
overlap_checked_at: 2026-06-05
overlap_check_scope: existing_local_catalog_files_raw_only
confidence: low|medium|high
future_recheck_required: true|false
next_recheck_after: 2026-09-05
```

중요:

```text
next_recheck_after는 기록만으로 작동하지 않는다.
이 날짜는 QA 또는 integrity check 단계가 읽고 실행해야 한다.
```

실행 주체:

| 상황 | 누가 읽는가 | 범위 |
|---|---|---|
| 단일 ticker run 직후 | runbook이 호출하는 run-local QA 또는 cross-source overlap check | 방금 실행한 ticker와 방금 생성/변경한 파일 |
| SEC 수집 직후 | runbook의 post-SEC cross-source overlap check | 방금 수집한 SEC file hash와 동일 ticker의 기존 IR records |
| IR 수집 직후 | runbook의 post-IR cross-source overlap check | 방금 수집한 IR file hash와 동일 ticker의 기존 SEC records |
| 정기 점검 | 별도 integrity check 단계 또는 향후 `source-pack-integrity-check` 절차 | 전체 catalog의 만료 recheck 항목, local_path, size, 선택적 hash |

현재 메모 단계에서는 절차 이름만 후보로 둔다.
나중에 `harness/`에 반영할 때 runbook 또는 별도 integrity check 절차에 "현재 날짜 기준으로 `next_recheck_after`가 지난 항목을 찾아 repair/recheck 후보로 보고한다"는 단계를 추가해야 한다.

기본 정책:

| 판단 | 재확인 주기 |
|---|---|
| exact hash match, confidence high | 자주 재확인하지 않음. 정기 integrity audit 때만 확인 |
| semantic overlap, hash 다름, confidence medium | 분기 또는 관련 SEC 수집 후 재확인 |
| local catalog에 SEC 없음, confidence low | 해당 회사 SEC 자료가 처음 수집될 때 재확인 |
| security_quarantined | 사용자 승인 또는 보안 검토 전까지 재시도/승격 금지 |
| high-risk 자료 | 더 짧은 주기로 재확인 |

## 8. High-risk 자료 기준

모든 자료를 같은 빈도로 재확인하면 비효율적이다.
중요도가 높은 자료는 더 엄격하게 본다.

다만 수집 시점에 어떤 자료가 high-risk인지 항상 알 수 있는 것은 아니다.
평범해 보이는 8-K나 실적 자료가 나중에 부채, 유동성, 구조조정, 파산 전조와 연결되는 경우가 있다.

따라서 high-risk 분류는 두 단계로 본다.

| 시점 | 처리 |
|---|---|
| 수집 시점 | 제목, form type, 8-K item, 명백한 키워드로 가능한 범위만 표시 |
| QA 또는 분석 하네스 이후 | 나중에 중요성이 드러나면 소급해서 `risk_class`, `future_recheck_required`, `repair_priority`를 갱신 |

High-risk 후보:

- 파산, 구조조정, going concern 관련 8-K
- 부채, 신용계약, covenant, liquidity 관련 8-K
- 증자, 전환사채, 주식 발행, warrant 관련 공시
- merger, asset sale, delisting, restatement 관련 공시
- 감사의견, 내부통제, 경영진 사임 관련 공시
- 실적 발표 8-K Item 2.02와 핵심 supplement

High-risk 자료는 notes에 아래처럼 남길 수 있다.

```text
risk_class: high; repair_priority: high; future_recheck_required: true
```

## 9. raw 무결성 점검 원칙

raw 파일을 매번 열어보거나 내용 분석하지 않는다.
무결성 점검은 가능한 한 기계적으로 한다.

기본 점검:

```text
1. files.jsonl의 local_path가 존재하는가?
2. size_bytes가 실제 파일 크기와 같은가?
3. sha256이 실제 파일 hash와 같은가?
4. file_status가 available인데 파일이 없지는 않은가?
5. documents.primary_file_id가 files.file_id와 연결되는가?
```

이 점검은 토큰을 거의 쓰지 않는다.
AI가 문서 내용을 읽는 것이 아니라, 프로그램이 파일 경로와 hash만 확인하면 된다.

## 10. 대규모 운영에서의 점검 전략

200개 기업, 5년치 자료로 커져도 모든 파일을 매번 열어보면 안 된다.

권장 레벨:

| 점검 레벨 | 범위 | 비용 | 목적 |
|---|---|---|---|
| Run-local QA | 방금 수집한 파일만 | 낮음 | 즉시 실패, 격리, hash mismatch 확인 |
| Fast integrity check | 전체 catalog의 local_path 존재와 size만 | 낮음 | 삭제/이동 탐지 |
| Hash audit | 중요 자료 또는 표본 파일 hash 재계산 | 중간 | 조용한 파일 변조/손상 탐지 |
| Deep repair review | 문제로 표시된 자료만 | 높음 | 사람 확인, 재수집, 관계 수정 |

권장 주기:

| 시점 | 할 일 |
|---|---|
| 매 run | 이번 run에서 생성/변경한 raw 파일 hash 확인 |
| 주간 또는 월간 | 전체 `files.jsonl` local_path 존재 여부 확인 |
| 분기별 | high-risk 또는 최근 변경 파일 hash audit |
| 반기 또는 연간 | 전체 catalog 관계 QA와 오래된 `future_recheck_required` 처리 |

## 11. repair_required 흐름

문제가 발견되면 즉시 조용히 고치지 않는다.
먼저 repair queue 성격으로 기록한다.

예:

```text
repair_required: true; repair_reason: raw_missing; detected_at: 2026-06-05
```

문제 유형:

| 문제 | 의미 | 처리 |
|---|---|---|
| `raw_missing` | catalog에는 있는데 파일 없음 | 재수집 또는 file_status 갱신 |
| `hash_mismatch` | 파일은 있으나 hash가 다름 | 보안/손상 가능성 확인, 재수집 후보 |
| `size_mismatch` | size_bytes와 실제 크기 불일치 | hash 재계산 후 판단 |
| `security_quarantined` | 보안 제품 격리 | 복구/열람 금지, 사용자 승인 필요 |
| `semantic_overlap_unverified` | 중복 후보 판단 미완료 | 다음 QA 또는 사람 확인 |
| `stale_overlap_cache` | 중복 판단이 오래됨 | 범위 제한 재확인 |

## 12. 일반 notes 템플릿

이 섹션은 특정 기업이 아니라 운영 catalog/index 반영 시 재사용할 일반 notes 템플릿이다.

### 12.1 현재 로컬 catalog에 SEC 후보가 없는 경우

```text
overlap_status: not_found_in_local_catalog; exact_hash_match: false; overlap_check_scope: existing_local_catalog_files_raw_only; confidence: low; future_recheck_required: true
```

의미:

- 현재 보유한 로컬 catalog/files/raw 안에서는 SEC 후보를 찾지 못했다.
- "중복 없음"을 확정한 것이 아니다.
- 나중에 해당 ticker의 SEC 자료가 수집되면 다시 확인해야 한다.

### 12.2 SEC 파일과 hash가 같은 경우

```text
overlap_status: same_hash_as_sec; canonical_source: sec-edgar; exact_hash_match: true; confidence: high
```

의미:

- 파일 bytes가 SEC 자료와 동일하다.
- production에서는 company-ir raw 중복 승격을 피한다.
- IR 출처 관찰은 notes 또는 향후 source observation 원장에 남긴다.

### 12.3 의미상 SEC 후보와 겹치지만 hash가 다른 경우

```text
overlap_status: different_hash_from_sec_candidate; exact_hash_match: false; confidence: medium; future_recheck_required: true
```

의미:

- 제목, 날짜, 실적 관련성으로 보면 SEC 후보와 겹칠 가능성이 있다.
- 하지만 hash가 다르므로 같은 파일로 취급하지 않는다.
- 둘 다 raw 보관 가능하며, 다음 QA 또는 사람 확인 대상으로 남긴다.

### 12.4 IR-native 자료라 SEC overlap 검사가 기본 불필요한 경우

```text
overlap_status: ir_native_no_sec_overlap_required; canonical_source: company-ir; confidence: high
```

의미:

- investor conference deck, investor day deck처럼 SEC 중복 가능성이 낮은 IR-native 자료다.
- 명백한 SEC 후보가 발견되지 않는 한 SEC hash 비교를 필수로 요구하지 않는다.

### 12.4.1 제한 확인 범위 안에 대응 SEC exhibit가 없는 경우

```text
overlap_status: sec_equivalent_not_found_in_scoped_8k; canonical_source: company-ir; exact_hash_match: false; confidence: low; future_recheck_required: false
```

의미:

- 같은 실적 8-K 안에 SEC EX-99.1 등은 있으나, 해당 IR 자료 유형에 대응하는 별도 SEC exhibit가 없다.
- 예: IR financial supplement PDF를 확인했지만 scoped 8-K 안에는 earnings press release EX-99.1만 있고 financial supplement/update exhibit는 없는 경우.
- 이 상태는 `different_hash_from_sec_candidate`와 다르다. SEC 후보가 있는데 hash가 다른 것이 아니라, 해당 유형의 SEC equivalent를 찾지 못한 것이다.
- 확인 범위는 `overlap_check_scope`에 반드시 남긴다.

### 12.5 보안 제품이 격리한 경우

```text
overlap_status: security_quarantined; collection_status: failed; repair_required: true; repair_reason: security_quarantined
```

의미:

- 보안 제품이 파일을 격리하거나 삭제했다.
- 운영 `documents.jsonl`, `files.jsonl`, 회사별 `index.md`에 승격하지 않는다.
- 복구/열람은 사용자 승인과 별도 보안 검토 전까지 하지 않는다.

## 13. AppLovin(APP) 운영 반영 적용 예시

이 섹션은 일반 규칙의 적용 예시다.
`APP`는 AppLovin Corporation의 ticker이며, Apple의 ticker `AAPL`과 다르다.
자료명이 회사별 label인지 새 `document_type` 후보인지 판단하는 기준은 `docs/design/ir-collection-design-notes-2026-06-04.md`의 `source_label` / `handled_as` 컨벤션을 따른다.

관련 run:

```text
run_id: run-20260604-app-ir-pilot
target: APP
scope: AppLovin Q1 2026 earnings press release HTML and Q1 2026 financial update PDF
```

AppLovin(APP) IR test_collection run 상태:

- APP IR raw 2건은 run-local 수집 성공
- 기존 로컬 catalog/raw에는 APP SEC 자료 없음
- 따라서 exact hash match 없음
- 의미상 earnings-related라 future SEC overlap 확인 필요

### 13.1 Earnings release HTML

권장 notes:

```text
overlap_status: not_found_in_local_catalog; exact_hash_match: false; overlap_check_scope: existing_local_catalog_files_raw_only; confidence: low; future_recheck_required: true; earnings_related: true
```

확장 notes가 필요하면:

```text
sec_overlap: not_found_in_local_catalog; exact_hash_match: false; overlap_check_scope: existing_local_catalog_files_raw_only; earnings_related: true; future_sec_overlap_check_required_if_app_sec_collected
```

### 13.2 Financial update PDF

권장 notes:

```text
source_label: Financial Update; handled_as: ir-financial-supplement; overlap_status: not_found_in_local_catalog; exact_hash_match: false; overlap_check_scope: existing_local_catalog_files_raw_only; confidence: low; future_recheck_required: true; earnings_related: true
```

확장 notes가 필요하면:

```text
source_label: Financial Update; handled_as: ir-financial-supplement; sec_overlap: not_found_in_local_catalog; exact_hash_match: false; overlap_check_scope: existing_local_catalog_files_raw_only; earnings_related: true; future_sec_overlap_check_required_if_app_sec_collected
```

### 13.3 APP SEC-IR recheck 이후 보정

후속 제한 재확인:

```text
run_id: run-20260605-app-sec-ir-overlap-recheck
scope: APP FY2026 Q1 8-K Item 2.02 / EX-99.1 candidate only
```

확인 결과:

- earnings release HTML은 SEC EX-99.1과 의미상 후보이나 hash가 다르다.
- financial update PDF는 scoped 8-K 안에 대응 SEC financial supplement/update exhibit가 없다.

따라서 financial update PDF에는 아래 상태가 더 정확하다.

```text
source_label: Financial Update; handled_as: ir-financial-supplement; overlap_status: sec_equivalent_not_found_in_scoped_8k; canonical_source: company-ir; exact_hash_match: false; sec_separate_financial_update_exhibit: not_found_in_scoped_8k; future_recheck_required: false
```

주의:

- EX-99.1 earnings press release HTML은 financial supplement PDF의 의미적 대응 후보가 아니다.
- 대응 SEC exhibit가 없으면 `different_hash_from_sec_candidate`가 아니라 `sec_equivalent_not_found_in_scoped_8k`를 사용한다.

## 14. 단기 적용안

지금 당장 큰 schema 개편은 하지 않는다.

단기 적용:

아래 원칙은 APP뿐 아니라 이후 모든 IR 운영 반영에 동일하게 적용한다.
단, 첫 적용 대상은 AppLovin Corporation(APP) IR test_collection run `run-20260604-app-ir-pilot`이다.

1. 운영 반영 시 notes에 overlap 상태와 future recheck 문구를 남긴다.
2. SEC overlap 확인은 `existing_local_catalog_files_raw_only`처럼 확인 범위를 명시한다.
3. hash match가 없을 때 "중복 없음"이라고 쓰지 않는다.
4. "현재 로컬 보유 자료 기준으로 찾지 못함"이라고 쓴다.
5. `security_quarantined` 파일은 운영 catalog/index에 승격하지 않는다.

## 15. 중기 개선 후보

반복 사례가 쌓이면 아래 개선을 검토한다.

notes 방식에서 구조화 원장으로 전환하는 기준:

| 전환 트리거 | 의미 |
|---|---|
| IR 운영 반영 ticker가 5개를 초과 | notes만으로 source 관계를 추적하기 어려워지기 시작 |
| `future_recheck_required` 항목이 10건을 초과 | 만료 항목 조회가 반복 작업이 됨 |
| SEC-IR overlap 관계가 20건을 초과 | 관계 자체가 별도 원장으로 관리될 가치가 생김 |
| 같은 notes 파싱 스크립트/수동 검색을 2회 이상 반복 | 자유형 notes가 사실상 구조화 데이터처럼 쓰이고 있다는 신호 |
| QA에서 notes 해석 오류가 1회라도 발생 | 운영 안정성을 위해 정식 필드 또는 별도 원장 검토 |

이 기준 중 하나라도 충족하면 `relationships.jsonl` 또는 `source-observations.jsonl` 도입을 검토한다.
다만 도입은 자동이 아니라 별도 설계/사용자 승인 후 진행한다.

### 15.1 별도 relationship 원장

후보 파일:

```text
artifacts/catalog/relationships.jsonl
```

역할:

- document 간 관계 기록
- SEC-IR overlap 관계 기록
- exact hash match, semantic overlap, superseded 관계 기록

예시 필드:

```json
{"relationship_id":"rel:...","left_document_id":"ir-app-earnings-release-fy2026-q1","right_document_id":"sec:...:8-k","relationship_type":"semantic_overlap","exact_hash_match":false,"confidence":"medium","checked_at":"2026-06-05","notes":"..."}
```

### 15.2 별도 source observation 원장

후보 파일:

```text
artifacts/catalog/source-observations.jsonl
```

역할:

- 같은 논리 문서를 여러 출처에서 발견했다는 사실 기록
- 다운로드하지 않은 IR 출처도 추적 가능
- canonical file은 SEC에 두되 IR 페이지에서 발견했다는 provenance 보존

예시 필드:

```json
{"observation_id":"obs:...","document_id":"sec:...:8-k","source_type":"company-ir","source_url":"https://...","observed_at":"2026-06-05","action":"not_downloaded_duplicate_hash","notes":"same hash as SEC file"}
```

주의:

- 지금 바로 만들지는 않는다.
- notes가 복잡해지고 같은 문제가 반복되면 도입한다.

### 15.3 Integrity audit 스크립트

후보 기능:

```text
files.jsonl 읽기
→ local_path 존재 확인
→ size 비교
→ 선택적으로 sha256 재계산
→ repair_required 후보 목록 출력
```

이 기능은 AI가 문서를 읽는 작업이 아니라 작은 검증 도구로 빼는 것이 좋다.
토큰 절약 효과가 크다.

## 16. 판단 기준 요약

| 질문 | 답 |
|---|---|
| SEC와 IR 양쪽에 같은 파일이 있으면 둘 다 저장하는가? | production에서는 보통 저장하지 않는다. SEC를 canonical으로 두고 IR 출처 기록만 남긴다. |
| IR이 먼저 받고 SEC가 나중이면? | IR raw를 보존하고, SEC 수집 후 hash가 같으면 canonical을 SEC로 업데이트한다. |
| hash가 다르면? | 같은 의미 후보라도 별도 raw로 보관 가능하다. notes에 semantic overlap을 남긴다. |
| SEC에 없고 IR에만 있으면? | IR-native면 company-ir canonical으로 보관한다. |
| 한 번 overlap 판단하면 다시 안 보는가? | 아니다. confidence와 recheck 조건을 둔다. |
| raw가 삭제되면? | integrity check가 local_path/size/hash 불일치를 찾아 repair_required로 보낸다. |
| 대규모 자료를 다 열어보는가? | 아니다. 경로/크기/hash 기반의 싼 검증을 먼저 한다. |

## 17. 다음 실행에 대한 제안

APP raw 2건을 운영 catalog/index에 반영하기 전에 이 메모의 12섹션 일반 notes 템플릿과 13섹션 APP 적용 예시를 사용한다.

그 다음 APP SEC 자료를 나중에 수집하게 되면:

1. APP SEC 8-K/EX-99.1 hash를 계산한다.
2. APP IR raw 2건의 hash와 비교한다.
3. 같으면 overlap_status를 `same_hash_as_sec`로 업데이트한다.
4. 다르면 `different_hash_from_sec_candidate`로 남긴다.
5. 판단 결과와 확인 범위를 QA에 기록한다.

## 18. 결론

SEC-IR 중복 처리는 단순히 파일을 덜 저장하는 문제가 아니다.

진짜 목표는 아래 네 가지다.

```text
1. 같은 파일을 불필요하게 반복 저장하지 않는다.
2. 서로 다른 출처에서 발견했다는 provenance는 잃지 않는다.
3. 잘못된 중복 판단으로 중요한 자료를 놓치지 않는다.
4. raw 파일 삭제, 격리, 손상을 가볍게 감지하고 repair 흐름으로 보낸다.
```

현재 Source Pack은 `documents.jsonl`, `files.jsonl`, `download-log.jsonl`, `qa.md`를 통해 이 방향으로 갈 기반을 이미 갖고 있다.
당장은 notes 기반으로 시작하고, 반복되는 복잡도가 확인되면 relationship/source observation 원장과 integrity audit 스크립트를 추가하는 것이 적절하다.
