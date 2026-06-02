# Source Pack Contract

## 목적

미국 상장 주식 한 회사 또는 여러 회사의 신뢰 가능한 원자료를 수집하고, 가능한 원문 파일을 로컬에 저장한 뒤, 다음 가치투자 하네스가 반복해서 SEC/IR/transcript 사이트를 방문하지 않아도 되는 입력 패키지를 만든다.

Source Pack은 링크 북마크가 아니다.
Source Pack은 `raw/` 원자료 저장소와 `catalog/` 원장을 만드는 수집형 하네스다.

이 하네스는 투자 판단, 매수/매도 의견, 밸류에이션, 경쟁우위 결론, transcript 해석을 작성하지 않는다.

## 활성화 조건

- "source pack 만들어줘"
- "source pack 실행해줘"
- "{티커} 공시 자료 수집해줘"
- "{티커} 자료 정리해줘"
- "관심 종목 source pack 업데이트해줘"
- "source pack 업데이트해줘"
- "공시 링크 수집해줘"
- "공시 자료 다운로드해줘"
- "원자료 모아줘"
- "Phase 2 Step 3 원자료 모아줘"

## 입력

- 대상 티커 또는 `watchlist.md`
- `config.md`
- 기존 `artifacts/catalog/*.jsonl`
- 기존 `artifacts/companies/{TICKER}/index.md`
- 이전 `artifacts/runs/*/download-log.jsonl`
- SEC EDGAR
- 기업 IR 사이트
- 가능한 경우 transcript 원문 후보 소스

## 출력

필수:

- `artifacts/companies/{TICKER}/index.md`
- `artifacts/catalog/entities.jsonl`
- `artifacts/catalog/documents.jsonl`
- `artifacts/catalog/files.jsonl`
- `artifacts/catalog/runs.jsonl`
- `artifacts/runs/{run-id}/run-summary.md`
- `artifacts/runs/{run-id}/download-log.jsonl`
- `artifacts/runs/{run-id}/qa.md`

조건부:

- `artifacts/raw/sec-edgar/...`
- `artifacts/raw/company-ir/...`
- `artifacts/raw/transcripts/...`
- `artifacts/derived/text/...`
- `artifacts/README.md` 갱신
- `artifacts/improvement-log.md` 갱신
- 비교 모드 실행 리포트

`raw/` 파일은 수집 가능한 원자료가 있을 때 저장한다.
원자료 다운로드가 실패한 문서는 생략하지 않고 `catalog/documents.jsonl`과 해당 run의 `download-log.jsonl`에 실패 상태와 이유를 남긴다.

## 하네스 유형과 품질 축

하네스 유형: `수집형`

산출물 역할: `raw 원자료 저장소 / catalog 원장 / 다음 하네스 입력 패키지`

수준 선언: `수집 엄격도 - 표준 수집`

주요 품질 축: `출처 추적성`, `로컬 파일 존재성`, `catalog 일관성`, `누락 관리`, `다음 하네스 전달성`

허용되는 판단:

- CIK, ticker, company metadata 확인
- 자료 종류 분류
- SEC form type, accession, filing date, period_end 식별
- 출처 유효성 확인
- 로컬 파일 존재 여부와 hash/size 확인
- 누락, 실패, 수동 확인 필요 여부 표시
- 다음 하네스가 참고할 원자료 위치 안내

금지되는 판단:

- 원자료 내용 해석
- 투자 thesis 작성
- valuation 의견
- 매수/매도/보유 판단
- 경영진 또는 사업 품질에 대한 분석 결론
- transcript 번역, 요약, Q&A 구조화, 투자 관점 분석
- Cloudflare, bot 차단, 로그인, 유료벽 우회

이 하네스에는 분석형 리포트의 등급 체계를 그대로 적용하지 않는다.
Source Pack의 품질은 해석의 깊이가 아니라 원자료 확보, 출처 추적성, 로컬 파일 존재성, catalog 일관성, 후속 하네스 전달성의 엄격도로 판단한다.

## 원장 우선순위

| 원장 | 역할 | 우선순위 |
|---|---|---|
| `artifacts/catalog/documents.jsonl` | 문서 단위 단일 원장 | 문서 존재와 상태의 기준 |
| `artifacts/catalog/files.jsonl` | 실제 로컬 파일 단위 확정 원장 | 파일 존재와 경로의 기준 |
| `artifacts/catalog/entities.jsonl` | 회사/entity 원장 | ticker, CIK, 회사 메타데이터 기준 |
| `artifacts/catalog/runs.jsonl` | 실행 이력 기계용 인덱스 | run 요약 위치 기준 |
| `artifacts/companies/{TICKER}/index.md` | 사람용 회사별 지도 | catalog와 충돌하면 catalog 우선 |

`companies/{TICKER}/sources.jsonl`은 만들지 않는다.
`download_status: link_only` 또는 이와 동등한 링크-only 운영 상태는 사용하지 않는다.

## 수집 대상

| 계층 | 자료 | 기본 범위 | 처리 |
|---|---|---|---|
| Tier 1 | 10-K | 10년 | 필수 수집 대상, raw 저장 시도 |
| Tier 1 | 10-Q | 12분기 | 필수 수집 대상, raw 저장 시도 |
| Tier 1 | DEF 14A / Proxy | 5년 | 필수 수집 대상, raw 저장 시도 |
| Tier 1 | 8-K Item 2.02 실적 발표 자료 | 12분기 | 필수 수집 대상, raw 저장 시도 |
| Tier 2 | 주요 8-K 이벤트 | `config.md` 필터 기준 | 선택 수집 |
| Tier 2 | IR 투자자 프레젠테이션 | 발견 가능한 범위 | 선택 수집 |
| Optional | Earnings Transcript 원문 | 가능한 범위 | 실패해도 전체 오류 아님 |
| Optional | 산업/경쟁 자료 | 발견 가능한 범위 | 다음 하네스 보조 |

Transcript 원문 수집은 optional source다. 접근 차단, 유료벽, 로그인 요구, 미공개, 검색 실패가 있으면 우회하지 않고 실패를 기록한 뒤 다음 자료로 넘어간다.

Transcript 번역, 요약, 주제 구조화, 투자 관점 분석은 별도 Transcript 하네스에서 수행한다.

## catalog 계약

`catalog/*.jsonl`의 필드, 허용 값, ID 규칙은 `harness/schemas/source-pack-catalog.schema.md`를 따른다.

핵심 규칙:

- JSONL 한 줄은 하나의 JSON object다.
- 확인되지 않은 값은 필드를 생략하지 않고 `null`로 둔다.
- `document_type`, `source_type`, `collection_status`, `text_status`, `file_role`, `file_format`, `file_status`는 schema의 허용 값을 사용한다.
- `documents.jsonl`의 `primary_file_id`는 `collection_status: collected`일 때 null이면 안 된다.
- `primary_file_id`가 있으면 `files.jsonl`에 같은 `file_id` record가 있어야 한다.
- `files.jsonl`에는 실제 로컬 파일이 검증된 뒤 승격된 파일만 기록한다.
- run 중 임시 시도 기록은 `artifacts/runs/{run-id}/download-log.jsonl`에 남기고, 검증 완료된 파일만 `catalog/files.jsonl`로 승격한다.

## 경로 계약

회사별 사람용 지도:

```text
artifacts/companies/{TICKER}/index.md
```

SEC raw:

```text
artifacts/raw/sec-edgar/cik-{CIK}/accession-{ACCESSION}/
```

기업 IR raw:

```text
artifacts/raw/company-ir/{TICKER}/{YYYY-MM-DD}_{slug}/
```

Transcript raw:

```text
artifacts/raw/transcripts/{TICKER}/{YYYY-MM-DD}_{source_slug}_{slug}/
```

텍스트 추출물:

```text
artifacts/derived/text/{raw 하위 구조와 동일한 구조}/
```

run 산출물:

```text
artifacts/runs/{run-id}/
```

## 최소 완료 기준

- 회사 CIK 또는 CIK 확인 실패 사유가 기록되어 있다.
- `artifacts/companies/{TICKER}/index.md`가 존재한다.
- `artifacts/catalog/entities.jsonl`, `documents.jsonl`, `files.jsonl`, `runs.jsonl`이 schema에 맞는 JSONL 형식이다.
- `artifacts/runs/{run-id}/run-summary.md`, `download-log.jsonl`, `qa.md`가 존재한다.
- Tier 1 자료의 수집 성공, 실패, 누락, 보류 상태가 `documents.jsonl`과 `download-log.jsonl`에 기록되어 있다.
- 다운로드에 성공한 원자료 파일은 `raw/`에 저장되고 `files.jsonl`에 file record가 있다.
- `collection_status: collected`인 문서의 `primary_file_id`는 `files.jsonl`의 `file_id`와 연결된다.
- 수집 실패 항목은 실패 이유, 시도한 URL 또는 source, 다음 조치를 남긴다.
- `link_only` 상태를 사용하지 않는다.
- 회사별 `index.md`에는 다음 하네스가 읽을 수 있는 전달 요약이 있다.

## 우수 산출물 기준

- CIK 조회는 SEC ticker mapping을 우선 사용하고 fallback을 명시한다.
- SEC 문서는 CIK, accession, form type, filing date, period_end, source_url, raw_root가 안정적으로 연결된다.
- Item 2.02와 EX-99.1의 관계가 혼동되지 않는다.
- raw 파일은 가능한 경우 SHA-256 hash, byte size, retrieved_at, content_type을 기록한다.
- `documents.jsonl`과 `files.jsonl`의 관계가 일관된다.
- `derived/text/`가 생성된 경우 raw 하위 구조와 대응된다.
- Transcript는 optional source로 처리하며 실패해도 전체 run을 실패시키지 않는다.
- Transcript 수집 전 이전 `download-log.jsonl`을 확인해 90일 냉각 기간 규칙을 적용한다.
- 다음 하네스가 catalog를 ticker, CIK, document_type, fiscal_year, period_end 기준으로 필터링할 수 있다.

## 금지사항

- 투자 의견, 목표주가, 매수/매도 판단을 작성하지 않는다.
- 원자료 내용을 요약하거나 해석하지 않는다.
- 확인되지 않은 IR 사이트나 transcript 링크를 확정처럼 쓰지 않는다.
- SEC/IR 요청을 병렬로 폭주시키지 않는다.
- Cloudflare, bot 차단, 로그인, 유료벽을 우회하지 않는다.
- transcript 원문 수집 실패를 해결하려고 무한 탐색하지 않는다.
- 수집 실패를 조용히 생략하지 않는다.
- 공통 업무 규칙을 Claude/Codex adapter에 길게 복사하지 않는다.

## 실패 처리

실패 처리는 "실패했다"는 대화 보고로 끝내지 않고 파일에 남긴다.
완전한 raw 저장과 catalog 갱신을 끝내지 못하더라도 가능한 범위의 메타데이터, 수집 시도, 실패 사유, 다음 조치를 기록한다.

| 상황 | 파일에 남길 것 | 상태 |
|---|---|---|
| CIK 조회 실패 | 티커, 사용한 조회 경로, 실패 사유, 권장 수동 확인 방법 | 실패 |
| SEC 429 반복 | 마지막 성공 지점, 재시도 횟수, 재개 가능 섹션 | 부분 성공 |
| SEC 네트워크 오류 | 실패한 API/URL, 영향받은 자료 범위 | 부분 성공 또는 실패 |
| raw 다운로드 실패 | document_id, source_url, 실패 사유, 재시도 가능 여부 | 부분 성공 |
| hash/파일 검증 실패 | file_path, 기대 hash 또는 size, 실제 상태, 승격 여부 | 미검증 |
| catalog schema QA 실패 | 실패한 파일, 필드, 허용 값, 수정 필요 항목 | 미검증 |
| IR 사이트 접근 실패 | IR 후보 URL, 접근 실패 사유, 수동 확인 방법 | 부분 성공 |
| Transcript 차단/유료벽/로그인 | ticker, quarter, source_name, attempt_status, 냉각 기간 적용 여부 | 부분 성공 |
| Transcript 미공개/검색 실패 | ticker, quarter, 시도한 출처, 다음 재시도 조건 | 부분 성공 |
| 사용자 승인 중단 | 승인 전까지 확정된 대상과 설정, 미실행 사유 | 중단 |

실패 상태에서도 `수동 확인 필요 목록`과 `다음 하네스 전달 요약`을 가능한 범위에서 남긴다.
후속 하네스가 사용하면 위험한 산출물은 `미검증` 또는 `[확인 필요: {이유}]`로 표시한다.

## 사람 승인 필요

- 수집 대상 티커 확정
- `config.md` 속도 제한 또는 수집 범위 변경
- 유료/로그인 소스 사용
- Cloudflare, bot 차단, 로그인, 유료벽 우회 시도는 승인 대상이 아니라 금지 대상이다.
- 기존 산출물 삭제, archive, 또는 대규모 덮어쓰기
- 비교 모드 결과를 운영 catalog에 반영
