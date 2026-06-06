# Source Pack IR/Dedup Handoff - 2026-06-05

## 1. 목적

이 문서는 P2-S03 Source Pack 하네스에서 IR 자료 수집과 SEC-IR 중복 처리 설계를 이어가기 위한 핸드오프 요약본이다.

새 채팅방, Claude Code, Codex가 이 문서만 읽어도 아래를 이해할 수 있도록 작성한다.

- 지금까지 무엇을 했는가
- 어떤 문제가 발견됐는가
- 어떤 판단을 했는가
- 어떤 문서를 참고해야 하는가
- 아직 무엇을 운영 catalog/index에 반영하지 않았는가
- 다음에 무엇을 요청하면 되는가

이 문서는 투자 분석이 아니다.
이 문서는 Source Pack 수집 하네스의 운영 설계와 실행 상태를 전달하기 위한 기록이다.

## 2. 전체 목표와 현재 위치

Source Pack의 목표는 가치투자 리서치 21단계 중 P2-S03 단계에서 원자료를 수집하는 것이다.

Source Pack은 분석 리포트를 만들지 않는다.
SEC 공시, Company IR 자료, 가능한 경우 transcript 원문을 raw 파일과 catalog 원장으로 정리해 다음 하네스가 읽을 수 있게 만든다.

현재 큰 구조:

```text
Source Pack
├─ SEC 수집: 기존 안정 경로
├─ IR 수집: 새로 붙이는 중
│  ├─ preflight
│  ├─ test_collection
│  ├─ run-local raw 저장
│  ├─ QA
│  ├─ SEC overlap 확인
│  └─ 사용자 승인 후 운영 catalog/index 반영
└─ transcript/audio: 아직 별도 설계 대상
```

현재 위치:

```text
AAPL IR pilot 성공 및 운영 반영 완료
NTRA IR pilot 실패 사례 보존 및 보안 격리 원칙 반영 완료
APP IR pilot run-local 성공
APP SEC overlap 확인 완료: 현재 로컬 보유 SEC 자료 기준 중복 후보 없음
SEC-IR 중복 처리와 raw 무결성 설계 메모 작성 및 Claude 피드백 반영 완료
다음 단계: APP IR raw 2건 운영 catalog/index 반영 여부 결정
```

## 3. 핵심 설계 원칙

### 3.1 SEC 안정성 보존

SEC 수집 절차는 기존 안정 경로로 유지한다.
IR 수집을 붙이면서 `harness/procedures/source-pack-collector.md`는 수정하지 않는 방향을 유지했다.

SEC 수집과 IR 수집은 같은 운영 catalog에 결과를 쌓지만, source별 절차는 분리한다.

### 3.2 IR은 작게 시작하고 확장한다

IR 사이트는 회사마다 구조가 다르다.
따라서 처음부터 큰 taxonomy를 만들지 않는다.

현재 active IR document_type:

```text
ir-deck
ir-earnings-release
ir-financial-supplement
```

관찰 중인 candidate_document_type:

```text
ir-earnings-presentation
ir-investor-conference-presentation
ir-financial-update
ir-shareholder-letter
ir-scientific-update
ir-non-gaap-reconciliation
```

현재 결론:

```text
아직 새 document_type 추가는 보류.
기존 schema로 pilot을 더 진행하고 반복 출현 여부를 본다.
```

### 3.3 Run-local → QA → Overlap 확인 → 승인 후 운영 반영

IR 자료는 바로 운영 catalog/index에 넣지 않는다.

기본 흐름:

```text
preflight
→ test_collection
→ run-local raw 저장
→ QA
→ SEC overlap 확인
→ 사용자 승인
→ 운영 documents/files/runs/index 반영
```

이 흐름 덕분에 NTRA에서 Bitdefender 격리된 PDF를 운영 catalog에 잘못 올리는 일을 막을 수 있었다.

## 4. 지금까지 실행한 IR pilot

## 4.1 AAPL IR Pilot

대상:

- Apple FY2026 Q2 earnings press release HTML
- Apple FY2026 Q2 consolidated financial statements PDF

run_id:

```text
run-20260604-aapl-ir-pilot
```

결과:

- raw 2건 수집 성공
- SEC overlap 확인 수행
- 사용자 승인 후 운영 catalog/index 반영 완료
- QA pass

운영 반영된 주요 파일:

```text
artifacts/catalog/documents.jsonl
artifacts/catalog/files.jsonl
artifacts/catalog/runs.jsonl
artifacts/companies/AAPL/index.md
artifacts/runs/run-20260604-aapl-ir-pilot/
artifacts/raw/company-ir/AAPL/
```

AAPL SEC overlap 판단:

- exact hash match: 없음
- 의미상 SEC 8-K / EX-99.1 overlap likely
- notes에 `sec_overlap: likely`, `canonical_source: sec-edgar`, `exact_hash_match: false` 기록

## 4.2 NTRA IR Pilot 2

대상:

- Natera Q1 2026 earnings press release HTML
- Natera Q1 2026 earnings presentation PDF
- 44th Annual J.P. Morgan Healthcare Conference presentation PDF

run_id:

```text
run-20260604-ntra-ir-pilot
```

결과:

| 자료 | 상태 | 의미 |
|---|---|---|
| Q1 2026 earnings press release HTML | HTTP 403 실패 | Natera IR HTML endpoint가 단순 요청을 거절 |
| Q1 2026 earnings presentation PDF | Bitdefender 격리 | download-log success 후 보안 제품이 파일 격리 |
| J.P. Morgan Healthcare Conference PDF | 성공 | raw 파일과 metadata 존재, hash 검증 성공 |

QA:

```text
overall_status: fail
```

중요한 의미:

- download-log success만 믿으면 위험하다는 사실이 확인됐다.
- 실제 local_path 존재와 hash 검증이 반드시 필요하다.
- 보안 제품이 격리한 파일은 운영 catalog/index에 승격하면 안 된다.

Bitdefender 정보:

```text
보안 제품: Bitdefender
탐지 계층: 지능형 위협 탐지(ATD)
탐지 ID: SuspiciousBehavior.182793FD17B38920
격리 파일: artifacts/raw/company-ir/NTRA/2026-05-07_fy2026-q1-earnings-presentation/document.pdf
```

NTRA 사후 메모:

```text
docs/ntra-ir-pilot-2-postmortem-2026-06-04.md
```

IR collector에 반영한 원칙:

```text
보안 제품이 다운로드된 IR 파일을 격리하거나 삭제한 경우 해당 파일을 복구하거나 열지 않는다.
이 파일은 운영 documents.jsonl, files.jsonl, 회사별 index.md에 승격하지 않는다.
download-log는 보존하고, run-summary/QA에는 security_quarantined 또는 이에 준하는 상태와 탐지 정보를 기록한다.
```

## 4.3 AppLovin(APP) IR Test Collection

주의:

```text
APP = AppLovin Corporation ticker
AAPL = Apple ticker
```

대상:

- AppLovin Q1 2026 earnings press release HTML
- AppLovin Q1 2026 financial update PDF

run_id:

```text
run-20260604-app-ir-pilot
```

결과:

| 자료 | document_type | 상태 |
|---|---|---|
| Q1 2026 earnings press release HTML | `ir-earnings-release` | 성공 |
| Q1 2026 financial update PDF | `ir-financial-supplement` | 성공 |

raw 파일:

```text
artifacts/raw/company-ir/APP/2026-05-06_fy2026-q1-earnings-release/document.html
artifacts/raw/company-ir/APP/2026-05-06_fy2026-q1-financial-update/document.pdf
```

hash:

```text
HTML sha256: 700b8775dac1e211ff10b27feaf4d5511df87badecffefcac330ebf2421f4e9e
PDF  sha256: bf35ed7b995a09bc999f251dcb2b885d8338435c2dc7e8ec5302379ece4f0ce8
```

QA:

```text
overall_status: partial_pass
```

왜 partial_pass인가:

- run-local raw 2건은 성공
- PDF 보안 격리 관찰 없음
- 하지만 운영 catalog/index에는 아직 사용자 승인 전이라 미반영
- SEC overlap 확인 후 운영 반영 여부를 결정해야 함

## 5. APP SEC overlap 확인 결과

APP IR raw 2건을 운영 catalog/index에 반영하기 전, 기존 로컬 자료만 대상으로 SEC overlap 여부를 확인했다.

조건:

- 새 SEC 다운로드 없음
- SEC filings page harvesting 없음
- `source-pack-collector.md` 수정 없음
- 운영 catalog/index 수정 없음
- 기존 `artifacts/catalog/files.jsonl`, `documents.jsonl`, raw 안에서만 비교

확인 결과:

```text
artifacts/catalog/entities.jsonl: APP entity 없음
artifacts/catalog/documents.jsonl: APP / CIK 0001751008 관련 SEC document 없음
artifacts/catalog/files.jsonl: APP / CIK 0001751008 관련 file 없음
artifacts/raw/sec-edgar/cik-0001751008: 없음
```

로컬 SEC raw 상태:

```text
artifacts/raw/sec-edgar/에는 현재 AAPL CIK(cik-0000320193)만 있음
```

exact hash match:

| APP IR raw | exact hash match |
|---|---|
| earnings release HTML | 없음 |
| financial update PDF | 없음 |

중요한 표현:

```text
"중복 없음"이 아니다.
"현재 로컬 보유 catalog/files/raw 기준으로 SEC 후보를 찾지 못했다"가 정확한 표현이다.
```

APP 운영 반영 시 사용할 notes 방향:

earnings release HTML:

```text
overlap_status: not_found_in_local_catalog; exact_hash_match: false; overlap_check_scope: existing_local_catalog_files_raw_only; confidence: low; future_recheck_required: true; earnings_related: true
```

financial update PDF:

```text
candidate_document_type: ir-financial-update; overlap_status: not_found_in_local_catalog; exact_hash_match: false; overlap_check_scope: existing_local_catalog_files_raw_only; confidence: low; future_recheck_required: true; earnings_related: true
```

## 6. SEC-IR 중복 처리 설계 논의

관련 설계 메모:

```text
docs/sec-ir-deduplication-and-integrity-design-2026-06-05.md
```

핵심 문제:

```text
SEC와 IR은 출처가 다르지만 같은 자료를 담을 수 있다.
어느 쪽을 먼저 수집하든 중복, 누락, 잘못된 판단, raw 삭제를 안전하게 관리해야 한다.
```

최종 방향:

### 6.1 SEC 먼저, IR 나중

흐름:

```text
IR 후보 파일을 임시 다운로드
→ hash 계산
→ 동일 ticker의 기존 SEC file hash와 비교
→ hash 같으면 company-ir raw로 승격하지 않음
→ IR 출처 발견 기록 또는 cross-reference만 남김
→ canonical_source: sec-edgar
```

중요:

```text
이 비교는 전체 catalog scan이 아니다.
방금 수집한 IR 파일 hash를 동일 ticker의 기존 SEC file record와만 비교한다.
```

### 6.2 IR 먼저, SEC 나중

흐름:

```text
IR raw는 기존 수집 record로 보존
→ 나중에 SEC 수집 시 SEC file hash 계산
→ runbook 또는 post-collection 단계가 방금 수집한 SEC file hash를 동일 ticker의 기존 IR records와 비교
→ hash 같으면 canonical_source를 sec-edgar로 업데이트
→ IR record에는 related_sec_document_id 또는 related_sec_file_id 기록
```

중요:

```text
SEC collector가 IR collector 세부 규칙을 직접 알 필요는 없다.
cross-source overlap check 호출 책임은 runbook 또는 별도 integrity/overlap check 단계에 둔다.
이 비교도 전체 catalog scan이 아니다.
```

### 6.3 hash 다름, 의미상 overlap

hash가 다르면 같은 파일로 취급하지 않는다.
다만 제목, 날짜, 실적 관련성 기준으로 의미상 overlap 후보일 수 있다.

권장 상태:

```text
overlap_status: different_hash_from_sec_candidate
exact_hash_match: false
confidence: medium
future_recheck_required: true
```

### 6.4 현재 로컬 SEC 후보 없음

AppLovin Corporation(APP) IR test_collection run `run-20260604-app-ir-pilot`가 이 경우다.

권장 상태:

```text
overlap_status: not_found_in_local_catalog
exact_hash_match: false
overlap_check_scope: existing_local_catalog_files_raw_only
confidence: low
future_recheck_required: true
```

### 6.5 보안 격리

NTRA가 이 경우다.

권장 상태:

```text
overlap_status: security_quarantined
collection_status: failed
repair_required: true
repair_reason: security_quarantined
```

운영 반영 금지:

```text
보안 제품이 격리한 파일은 documents.jsonl, files.jsonl, 회사별 index.md에 승격하지 않는다.
```

## 7. Claude 피드백과 반영 내용

Claude Code가 설계 메모에 대해 주요 피드백을 제공했다.

### 7.1 next_recheck_after 실행 주체 없음

피드백:

```text
next_recheck_after를 기록해도 누가 읽는지 없으면 작동하지 않는다.
```

반영:

- QA 또는 향후 `source-pack-integrity-check` 단계가 읽어야 한다고 명시
- run-local QA, post-SEC overlap check, post-IR overlap check, 정기 integrity check로 실행 주체를 나눔

### 7.2 IR 먼저 수집 후 SEC 나중 수집 시 cross-lookup 책임자 없음

피드백:

```text
SEC collector가 IR collector를 알아서는 안 되므로 runbook 또는 별도 단계가 호출해야 한다.
```

반영:

- runbook 또는 post-collection integrity/overlap check 단계가 호출 책임을 갖는다고 명시
- 전체 catalog scan이 아니라 동일 ticker 범위 제한 비교라고 명시

### 7.3 notes 방식의 한계

피드백:

```text
notes 문자열은 3~5개 기업을 넘으면 조회가 어려워진다.
structured relationship/source observation 원장으로 넘어가는 기준이 필요하다.
```

반영:

전환 트리거 추가:

```text
IR 운영 반영 ticker가 5개 초과
future_recheck_required 항목 10건 초과
SEC-IR overlap 관계 20건 초과
같은 notes 파싱/수동 검색 2회 이상 반복
QA에서 notes 해석 오류 1회라도 발생
```

### 7.4 high-risk 분류 시점 문제

피드백:

```text
수집 시점에는 어떤 자료가 high-risk인지 모를 수 있다.
```

반영:

- 수집 시점 분류와 QA/분석 하네스 이후 소급 분류를 구분
- 나중에 중요성이 드러나면 `risk_class`, `future_recheck_required`, `repair_priority`를 갱신할 수 있다고 명시

### 7.5 APP vs AAPL 혼동

피드백:

```text
APP가 Apple 오타인지 AppLovin인지 문서 안에 명시해야 한다.
```

반영:

- `APP = AppLovin Corporation ticker`
- `AAPL = Apple ticker`
- 관련 run_id `run-20260604-app-ir-pilot`
- 섹션 제목을 `AppLovin(APP) 운영 반영 적용 예시`로 변경

## 8. 생성/수정된 주요 문서 목록

### 설계/결정 문서

```text
docs/ir-collection-design-notes-2026-06-04.md
docs/source-pack-structure-decision-notes-2026-06-04.md
docs/sec-ir-deduplication-and-integrity-design-2026-06-05.md
```

### Preflight 문서

```text
docs/aapl-ir-preflight-2026-06-04.md
docs/ntra-ir-preflight-2026-06-04.md
docs/app-ir-preflight-2026-06-04.md
```

### Postmortem 문서

```text
docs/ntra-ir-pilot-2-postmortem-2026-06-04.md
```

### Run 산출물

```text
artifacts/runs/run-20260604-aapl-ir-pilot/
artifacts/runs/run-20260604-ntra-ir-pilot/
artifacts/runs/run-20260604-app-ir-pilot/
```

### Harness 변경

```text
harness/schemas/source-pack-catalog.schema.md
harness/procedures/source-pack-ir-collector.md
harness/procedures/source-pack-runbook.md
```

주의:

```text
harness/procedures/source-pack-collector.md는 수정하지 않는 원칙을 유지했다.
```

## 9. 아직 운영 catalog/index에 반영하지 않은 것

### APP

APP IR raw 2건은 run-local 수집 성공했지만 아직 운영 catalog/index에 반영하지 않았다.

미반영 대상:

```text
artifacts/raw/company-ir/APP/2026-05-06_fy2026-q1-earnings-release/document.html
artifacts/raw/company-ir/APP/2026-05-06_fy2026-q1-financial-update/document.pdf
```

반영 시 필요한 작업:

```text
1. APP entity가 없으면 entities.jsonl에 sec-cik-0001751008 추가
2. documents.jsonl에 APP IR document 2건 추가
3. files.jsonl에 APP IR file 2건 추가
4. runs.jsonl에 run-20260604-app-ir-pilot record 추가 또는 갱신
5. artifacts/companies/APP/index.md 생성
6. QA 실행
```

사용할 notes:

```text
overlap_status: not_found_in_local_catalog; exact_hash_match: false; overlap_check_scope: existing_local_catalog_files_raw_only; confidence: low; future_recheck_required: true; earnings_related: true
```

financial update PDF에는 추가로:

```text
candidate_document_type: ir-financial-update
```

### NTRA

NTRA는 운영 catalog/index에 반영하지 않는다.

이유:

```text
3건 중 1건만 완전 수집
1건 HTTP 403
1건 Bitdefender 격리
QA fail
```

NTRA는 실패 사례와 보안 격리 사례로 보존한다.

## 10. 다음 단계

가장 자연스러운 다음 단계:

```text
APP IR pilot raw 2건을 운영 catalog/index에 반영한다.
```

단, 아래 조건을 지킨다.

- SEC 다운로드 하지 않음
- SEC filings page harvesting 하지 않음
- `source-pack-collector.md` 수정하지 않음
- APP raw 2건만 운영 반영
- APP entity가 없으면 추가
- notes에는 `not_found_in_local_catalog`, `future_recheck_required`를 남김
- 반영 후 QA 실행

## 11. 다음 요청 문구

다음 채팅방 또는 이어지는 작업에서 사용할 요청 문구:

```text
APP IR pilot raw 2건을 운영 catalog/index에 반영해줘.

참고 파일:
- docs/app-ir-preflight-2026-06-04.md
- docs/sec-ir-deduplication-and-integrity-design-2026-06-05.md
- artifacts/runs/run-20260604-app-ir-pilot/run-summary.md
- artifacts/runs/run-20260604-app-ir-pilot/qa.md
- harness/procedures/source-pack-ir-collector.md
- harness/schemas/source-pack-catalog.schema.md

조건:
- SEC 다운로드는 하지 말 것
- SEC filings page harvesting 하지 말 것
- source-pack-collector.md는 수정하지 말 것
- APP entity가 없으면 entities.jsonl에 sec-cik-0001751008로 추가할 것
- APP IR raw 2건만 documents.jsonl, files.jsonl, runs.jsonl, APP index.md에 반영할 것
- notes에는 sec-ir-deduplication 설계 메모의 not_found_in_local_catalog / future_recheck_required 문구를 남길 것
- financial update PDF에는 candidate_document_type: ir-financial-update를 남길 것
- 반영 후 QA를 실행하고 결과를 요약할 것

대상 APP IR raw:
- artifacts/raw/company-ir/APP/2026-05-06_fy2026-q1-earnings-release/document.html
- artifacts/raw/company-ir/APP/2026-05-06_fy2026-q1-financial-update/document.pdf
```

## 12. 새 세션에서 먼저 읽을 파일

새 채팅방에서 이어갈 때 권장 읽기 순서:

```text
1. docs/source-pack-ir-dedup-handoff-2026-06-05.md
2. docs/sec-ir-deduplication-and-integrity-design-2026-06-05.md
3. docs/app-ir-preflight-2026-06-04.md
4. artifacts/runs/run-20260604-app-ir-pilot/run-summary.md
5. artifacts/runs/run-20260604-app-ir-pilot/qa.md
6. harness/procedures/source-pack-ir-collector.md
7. harness/schemas/source-pack-catalog.schema.md
```

## 13. 주의할 점

1. `APP`는 AppLovin Corporation이고, `AAPL`은 Apple이다.
2. `not_found_in_local_catalog`는 "중복 없음"이 아니다.
3. NTRA의 Bitdefender 격리 파일은 복구하거나 열지 않는다.
4. 보안 제품이 격리한 파일은 운영 catalog/index에 승격하지 않는다.
5. SEC/IR 중복 비교는 전체 catalog scan이 아니라 동일 ticker 범위 제한 비교가 기본이다.
6. `next_recheck_after` 같은 재확인 정보는 기록만으로 작동하지 않는다. 나중에 QA 또는 integrity check 절차가 읽어야 한다.
7. 지금은 notes 기반으로 시작하되, 기준을 넘으면 `relationships.jsonl` 또는 `source-observations.jsonl` 도입을 검토한다.

## 14. 한 줄 결론

현재 Source Pack은 IR 수집을 안전하게 붙이는 과정에서 AAPL 성공, NTRA 실패/보안 격리, APP 성공 사례를 확보했다.
다음 단계는 APP run-local raw 2건을 설계 메모의 SEC-IR dedup notes 원칙에 따라 운영 catalog/index에 반영하는 것이다.
