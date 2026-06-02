# Source Pack Artifacts Raw Structure v1

이 문서는 Source Pack 하네스의 `artifacts/` 구조를 raw 원자료 저장소와 catalog 원장 중심으로 재설계하기 위한 청사진이다.

Source Pack은 더 이상 단순 링크 북마크가 아니다.  
이 하네스의 목적은 가치투자 21단계의 다른 하네스들이 반복해서 SEC, IR, transcript 사이트를 다시 방문하지 않아도 되도록, 원자료를 한 번 수집하고 로컬에 저장한 뒤 재사용 가능한 입력 패키지로 만드는 것이다.

## 1. 목표

Source Pack artifacts 구조의 목표는 네 가지다.

1. 사람이 특정 회사의 원자료 수집 상태를 빠르게 확인할 수 있다.
2. 다른 가치투자 하네스가 안정적인 catalog를 읽어 필요한 원자료를 찾을 수 있다.
3. SEC/IR 원문 파일과, 가능한 경우 transcript 원문을 로컬에 저장해 반복 다운로드와 토큰 낭비를 줄인다.
4. 나중에 데이터베이스, RAG, 텍스트 추출, embedding 구조로 확장할 수 있다.

핵심 원칙:

```text
Source Pack catalog는 링크 북마크가 아니다.
Source Pack catalog는 로컬에 저장된 원자료와 그 메타데이터의 원장이다.
```

## 2. 설계 원칙

### 2.1 사람용 구조와 기계용 구조 분리

`companies/`는 사람이 읽는 회사별 지도다.  
`catalog/`는 다른 하네스와 나중의 데이터베이스가 읽는 기계용 원장이다.

같은 정보를 두 곳에 원본처럼 저장하지 않는다.

| 영역 | 역할 | 원본 여부 |
|---|---|---|
| `companies/{TICKER}/index.md` | 사람용 회사별 요약 지도 | 원본 아님 |
| `catalog/documents.jsonl` | 문서 단위 원장 | 원본 |
| `catalog/files.jsonl` | 로컬 파일 단위 원장 | 원본 |
| `catalog/entities.jsonl` | 회사/entity 원장 | 원본 |
| `catalog/runs.jsonl` | 실행 이력 인덱스 | 원본 |

`companies/{TICKER}/sources.jsonl`은 만들지 않는다.  
회사별 기계용 문서 목록은 `catalog/documents.jsonl`에서 `ticker` 또는 `cik`로 필터링해 얻는다.

### 2.2 폴더는 사람을 위해, catalog는 검색을 위해

연도별 폴더나 공시자료별 폴더를 주 구조로 삼지 않는다.

예를 들어 아래 질문들은 폴더보다 catalog가 더 잘 처리한다.

- AAPL의 최근 10-K는 무엇인가?
- 2024년에 제출된 모든 8-K Item 2.02는 무엇인가?
- 모든 회사의 DEF 14A는 어디에 있는가?
- raw는 있지만 text 추출이 아직 안 된 문서는 무엇인가?
- 특정 하네스가 먼저 읽어야 할 원자료 묶음은 무엇인가?

따라서 연도, form type, filing date, fiscal period, source는 폴더가 아니라 `catalog/*.jsonl`의 필드로 관리한다.

### 2.3 링크 전용 모드는 운영 구조에 포함하지 않음

초기 실습에서 만든 링크-only artifacts는 운영 자산으로 보지 않는다.  
새 구조는 raw 다운로드를 기본 전제로 한다.

링크는 여전히 필요하지만, 의미가 다르다.

- 이전 의미: 링크만 수집한 북마크
- 새 의미: 로컬 raw 파일의 원출처 URL

따라서 `download_status: link_only` 같은 상태는 두지 않는다.

### 2.4 raw와 derived는 git에 넣지 않음

`raw/`와 `derived/`는 파일 크기가 커질 수 있으므로 git 추적 대상에서 제외한다.  
대신 catalog와 사람용 index는 git에 남긴다.

권장 `.gitignore`:

```gitignore
artifacts/raw/
artifacts/derived/
```

필요하면 나중에 raw 파일은 별도 object storage, NAS, S3, DVC, Git LFS, 데이터베이스 저장소로 옮길 수 있다.  
하지만 현재 단계에서는 로컬 폴더와 catalog 원장을 기준으로 시작한다.

## 3. 최종 폴더 구조

권장 구조:

```text
artifacts/
├── README.md
├── improvement-log.md
│
├── companies/
│   └── AAPL/
│       └── index.md
│
├── catalog/
│   ├── entities.jsonl
│   ├── documents.jsonl
│   ├── files.jsonl
│   └── runs.jsonl
│
├── raw/
│   ├── sec-edgar/
│   │   └── cik-0000320193/
│   │       └── accession-0000320193-24-000123/
│   │           ├── metadata.json
│   │           ├── primary.html
│   │           └── exhibits/
│   ├── company-ir/
│   │   └── AAPL/
│   │       └── 2025-09-09_iphone-event-presentation/
│   └── transcripts/
│       └── AAPL/
│           └── 2025-01-30_motley-fool-q1-earnings-call/
│
├── derived/
│   └── text/
│       ├── sec-edgar/
│       ├── company-ir/
│       └── transcripts/
│
└── runs/
    └── run-20260602-aapl/
        ├── run-summary.md
        ├── download-log.jsonl
        └── qa.md
```

## 4. 폴더별 역할

### 4.1 `companies/`

사람이 읽는 회사별 요약 지도다.

예:

```text
artifacts/companies/AAPL/index.md
```

`index.md`는 다음 정보를 담는다.

- 회사 메타데이터
- 수집 범위
- SEC/IR/transcript 수집 현황 요약
- 핵심 원자료 목록
- 누락 및 실패 요약
- 다음 하네스 전달 요약

주의:

- `index.md`는 사람이 보기 위한 문서다.
- 기계용 원본 장부는 `catalog/*.jsonl`이다.
- `index.md`와 catalog가 충돌하면 catalog를 우선한다.

### 4.2 `catalog/`

다른 하네스와 나중의 데이터베이스가 읽는 원장이다.

| 파일 | 단위 | 역할 |
|---|---|---|
| `entities.jsonl` | 회사/entity | 티커, CIK, 회사명, 거래소 등 |
| `documents.jsonl` | 문서 | SEC filing, IR deck, transcript 같은 논리적 문서 |
| `files.jsonl` | 파일 | 실제 로컬에 저장된 HTML/PDF/TXT/JSON 파일 |
| `runs.jsonl` | 실행 | 각 수집 run의 기계용 인덱스 |

`catalog/`는 Source Pack의 핵심 산출물이다.  
다른 하네스는 가능한 한 `companies/index.md`보다 `catalog/*.jsonl`을 우선해 필요한 원자료를 찾는다.

### 4.3 `raw/`

원자료 원본 저장소다.

raw 파일은 가능한 한 원본에 가깝게 저장한다.

- SEC HTML, XML, TXT, exhibit 파일
- 회사 IR PDF, HTML
- transcript PDF, HTML, TXT
- 원문과 함께 내려받은 metadata

raw 파일은 해석하거나 요약하지 않는다.

Transcript도 Source Pack에서는 원자료로만 다룬다.

허용:

- transcript 원문 후보 발견
- transcript 원문 파일 다운로드
- transcript metadata, source_url, raw_path, text_path catalog 등록

금지:

- transcript 번역
- transcript 요약
- Q&A 주제 구조화
- 투자 관점 해석
- 경영진 발언 평가
- 발언의 신뢰도 또는 중요도 판단

transcript 번역, 요약, 주제 구조화, 투자 관점 분석은 별도 Transcript 하네스에서 수행한다.
`raw/transcripts/`는 Transcript 하네스 산출물 저장소가 아니라 Transcript 하네스가 읽을 원문 저장소다.

Transcript는 optional source다.  
접근 차단, 유료벽, 로그인 요구, 미공개, 검색 실패가 흔하므로 transcript 수집 실패는 Source Pack 전체 실패로 보지 않는다.

Transcript 수집 중단 규칙:

- 티커당 transcript 탐색에 사용할 `source_name` 또는 `source_type` 종류는 최대 3개로 제한한다. 예: Company IR, AlphaStreet, Motley Fool.
- 각 분기에서는 위 후보 출처 중 최대 2개 출처만 시도한다.
- 출처당 자동 접근은 최대 1회로 제한한다.
- Cloudflare, bot 차단, 로그인 요구, 유료벽이 감지되면 우회하지 않고 즉시 중단한다.
- 동일 티커, 동일 분기, 동일 출처에서 실패 기록이 있으면 기본적으로 90일 동안 자동 재시도하지 않는다.
- 사용자가 명시적으로 재시도를 요청한 경우에만 냉각 기간을 무시할 수 있다.
- transcript 실패는 `download-log.jsonl`, `run-summary.md`, `qa.md`, 회사별 `index.md` 확인 필요 목록에 남기고 다음 자료 수집으로 넘어간다.

Transcript 접근 금지:

- 유료/로그인 소스 자동 우회
- Cloudflare 또는 bot 차단 회피
- 세션, 쿠키, 계정 정보를 임의로 사용
- 차단된 URL 반복 호출
- transcript 확보를 위해 전체 run을 멈추는 행동

### 4.4 `derived/text/`

raw 파일에서 추출한 텍스트 저장소다.

이 단계는 해석이 아니라 비해석 정제다.

허용:

- HTML에서 visible text 추출
- PDF에서 text 추출
- 줄바꿈 정리
- boilerplate 제거 후보 표시
- 원문 위치와 page/section 메타데이터 보존

금지:

- 투자 thesis 작성
- 실적 해석
- 경영진 평가
- valuation 의견
- 매수/매도/보유 판단

chunking, embedding, vector DB 적재는 Source Pack의 다음 확장 단계로 미룬다.  
v1 구조에서는 `derived/text/`까지만 설계한다.

### 4.5 `runs/`

실행 단위 기록이다.

예:

```text
artifacts/runs/run-20260602-aapl/
```

각 run 폴더에는 사람이 읽는 실행 리포트와 실행 중 로그를 남긴다.

| 파일 | 역할 |
|---|---|
| `run-summary.md` | 사람이 읽는 실행 요약 |
| `download-log.jsonl` | 실행 중 다운로드 시도 로그 |
| `qa.md` | 수집 및 파일 검증 결과 |

`runs/`는 상세 실행 기록이고, `catalog/runs.jsonl`은 그 실행을 찾기 위한 기계용 인덱스다.

## 5. catalog 스키마

모든 `.jsonl` 파일은 한 줄이 하나의 JSON object다.  
필드명은 snake_case를 사용한다.  
날짜는 `YYYY-MM-DD`, 시각은 ISO 8601 형식을 사용한다.

### 5.1 `entities.jsonl`

회사 또는 분석 대상 entity의 원장이다.

필수 필드:

| 필드 | 설명 | 예시 |
|---|---|---|
| `entity_id` | entity 고유 ID | `sec-cik-0000320193` |
| `ticker` | 현재 티커 | `AAPL` |
| `cik` | 10자리 SEC CIK | `0000320193` |
| `company_name` | 회사명 | `Apple Inc.` |
| `exchange` | 거래소 | `Nasdaq` |
| `country` | 국가 | `US` |
| `sector` | SEC SIC description 또는 수집된 sector | `Electronic Computers` |
| `fiscal_year_end` | 회계연도 종료 월일 | `09-28` |
| `entity_status` | 상태 | `active` |
| `last_updated` | 마지막 갱신일 | `2026-06-02` |

선택 필드:

| 필드 | 설명 |
|---|---|
| `former_tickers` | 과거 티커 배열 |
| `ir_site` | IR 사이트 URL |
| `notes` | 확인 필요 또는 수동 메모 |

예시:

```json
{"entity_id":"sec-cik-0000320193","ticker":"AAPL","cik":"0000320193","company_name":"Apple Inc.","exchange":"Nasdaq","country":"US","sector":"Electronic Computers","fiscal_year_end":"09-28","entity_status":"active","last_updated":"2026-06-02","ir_site":"https://investor.apple.com/"}
```

### 5.2 `documents.jsonl`

논리적 문서의 원장이다.

문서란 SEC filing, IR presentation, earnings transcript처럼 다른 하네스가 분석 단위로 인식하는 자료를 뜻한다.

필수 필드:

필수 필드는 record마다 key가 있어야 한다.  
아직 값이 확정되지 않은 경우에는 필드를 생략하지 말고 `null`을 사용한다.

| 필드 | 설명 | 예시 |
|---|---|---|
| `document_id` | 문서 고유 ID | `sec:0000320193:0000320193-24-000123:10-k` |
| `entity_id` | 연결 entity | `sec-cik-0000320193` |
| `ticker` | 티커 | `AAPL` |
| `cik` | CIK | `0000320193` |
| `source_type` | 자료 출처 유형 | `sec-edgar` |
| `document_type` | 문서 유형 | `10-K` |
| `title` | 문서 제목 | `Form 10-K` |
| `filing_date` | 제출일 또는 게시일 | `2024-11-01` |
| `period_end` | 대상 기간 종료일 | `2024-09-28` |
| `fiscal_year` | 회계연도 | `2024` |
| `source_url` | 원출처 URL | `https://www.sec.gov/...` |
| `collection_status` | 문서 수집 상태 | `collected` |
| `primary_file_id` | 대표 파일 ID. `collection_status: collected`일 때 필수이고, `pending`, `failed`, `skipped`에서는 `null` 가능 | `sha256:...` |
| `raw_root` | raw 문서 폴더 | `artifacts/raw/sec-edgar/...` |
| `text_root` | derived text 폴더 | `artifacts/derived/text/sec-edgar/...` |
| `first_collected_run_id` | 최초 수집 run | `run-20260602-aapl` |
| `last_checked_run_id` | 마지막 확인 run | `run-20260602-aapl` |

SEC 전용 필드:

| 필드 | 설명 | 예시 |
|---|---|---|
| `accession_no` | SEC accession number | `0000320193-24-000123` |
| `form_type` | SEC form type | `10-K` |
| `items` | 8-K item 목록 | `["2.02","9.01"]` |
| `primary_doc` | SEC primary document 이름 | `aapl-20240928.htm` |

공통 선택 필드:

| 필드 | 설명 | 예시 |
|---|---|---|
| `text_status` | 텍스트 추출 상태. v1에서는 선택 필드이며 `pending`, `extracted`, `failed`, `not_applicable` 중 하나를 사용 | `pending` |
| `notes` | 확인 필요, 수동 메모, 예외 사항 | `[확인 필요: exhibit 누락 여부 확인]` |

IR/transcript 선택 필드:

| 필드 | 설명 |
|---|---|
| `event_date` | 발표 또는 이벤트 날짜 |
| `source_name` | 회사 IR, Quartr, AlphaStreet 등 |
| `slug` | 경로에 사용하는 제목 slug |
| `language` | 문서 언어 |

`collection_status` 값:

| 값 | 의미 |
|---|---|
| `collected` | 문서 메타데이터와 하나 이상의 raw 파일이 catalog에 등록됨 |
| `pending` | 수집 대상이나 아직 완료되지 않음 |
| `failed` | 수집 시도 실패 |
| `skipped` | 정책상 수집하지 않음 |

운영 원칙:

- `link_only` 상태는 사용하지 않는다.
- `documents.jsonl`에는 수집 대상 문서와 수집된 문서의 상태를 남긴다.
- 실제 로컬 파일 존재 여부는 `files.jsonl`에서 확인한다.

예시:

```json
{"document_id":"sec:0000320193:0000320193-24-000123:10-k","entity_id":"sec-cik-0000320193","ticker":"AAPL","cik":"0000320193","source_type":"sec-edgar","document_type":"10-K","title":"Form 10-K","filing_date":"2024-11-01","period_end":"2024-09-28","fiscal_year":"2024","source_url":"https://www.sec.gov/Archives/edgar/data/320193/000032019324000123/","collection_status":"collected","primary_file_id":"sha256:example","raw_root":"artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-24-000123","text_root":"artifacts/derived/text/sec-edgar/cik-0000320193/accession-0000320193-24-000123","first_collected_run_id":"run-20260602-aapl","last_checked_run_id":"run-20260602-aapl","accession_no":"0000320193-24-000123","form_type":"10-K","items":[],"primary_doc":"aapl-20240928.htm","text_status":"pending"}
```

### 5.3 `files.jsonl`

실제 로컬에 존재하는 파일의 확정 원장이다.

`files.jsonl`에는 검증 후 로컬에 존재한다고 확인된 파일만 기록한다.  
실패한 다운로드 시도는 `runs/{run-id}/download-log.jsonl`과 `qa.md`에 남긴다.

필수 필드:

| 필드 | 설명 | 예시 |
|---|---|---|
| `file_id` | 파일 고유 ID | `sha256:...` |
| `document_id` | 연결 문서 ID | `sec:0000320193:...` |
| `entity_id` | 연결 entity | `sec-cik-0000320193` |
| `ticker` | 티커 | `AAPL` |
| `source_type` | 출처 유형 | `sec-edgar` |
| `file_role` | 파일 역할 | `primary` |
| `file_format` | 파일 형식 | `html` |
| `local_path` | 로컬 파일 경로 | `artifacts/raw/sec-edgar/.../primary.html` |
| `source_url` | 원출처 URL | `https://www.sec.gov/...` |
| `sha256` | 파일 해시 | `...` |
| `size_bytes` | 파일 크기 | `123456` |
| `retrieved_at` | 다운로드 시각 | `2026-06-02T10:00:00+09:00` |
| `run_id` | 생성 run | `run-20260602-aapl` |
| `file_status` | 파일 상태 | `available` |

`file_role` 값:

| 값 | 의미 |
|---|---|
| `metadata` | metadata JSON |
| `primary` | SEC primary document 또는 대표 문서 |
| `exhibit` | SEC exhibit |
| `ir_deck` | IR presentation |
| `transcript` | transcript 원문 |
| `text` | derived text 파일 |
| `other` | 기타 |

`file_status` 값:

| 값 | 의미 |
|---|---|
| `available` | 로컬 파일 존재 확인 |
| `missing` | catalog에는 있으나 파일이 사라짐 |
| `replaced` | 새 파일로 대체됨 |

예시:

```json
{"file_id":"sha256:example","document_id":"sec:0000320193:0000320193-24-000123:10-k","entity_id":"sec-cik-0000320193","ticker":"AAPL","source_type":"sec-edgar","file_role":"primary","file_format":"html","local_path":"artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-24-000123/primary.html","source_url":"https://www.sec.gov/Archives/edgar/data/320193/000032019324000123/aapl-20240928.htm","sha256":"example","size_bytes":123456,"retrieved_at":"2026-06-02T10:00:00+09:00","run_id":"run-20260602-aapl","file_status":"available"}
```

### 5.4 `runs.jsonl`

기계용 실행 이력 인덱스다.

상세 실행 기록은 `artifacts/runs/{run-id}/`에 둔다.  
`catalog/runs.jsonl`은 그 상세 기록으로 가는 짧은 포인터다.

필수 필드:

| 필드 | 설명 | 예시 |
|---|---|---|
| `run_id` | 실행 ID | `run-20260602-aapl` |
| `target` | 실행 대상 | `AAPL` |
| `run_mode` | 실행 모드 | `new_collection` |
| `started_at` | 시작 시각 | `2026-06-02T10:00:00+09:00` |
| `ended_at` | 종료 시각 | `2026-06-02T10:15:00+09:00` |
| `status` | 실행 상태 | `success` |
| `documents_attempted` | 시도 문서 수 | `42` |
| `documents_collected` | 수집 성공 문서 수 | `40` |
| `files_available` | 사용 가능 파일 수 | `55` |
| `run_summary_path` | 사람용 실행 요약 | `artifacts/runs/run-20260602-aapl/run-summary.md` |
| `qa_path` | QA 파일 | `artifacts/runs/run-20260602-aapl/qa.md` |

`status` 값:

| 값 | 의미 |
|---|---|
| `success` | 필수 수집 범위 성공 |
| `partial_success` | 일부 실패가 있으나 usable |
| `failed` | 핵심 수집 실패 |
| `stopped` | 사용자 승인 또는 중단 조건으로 중단 |

예시:

```json
{"run_id":"run-20260602-aapl","target":"AAPL","run_mode":"new_collection","started_at":"2026-06-02T10:00:00+09:00","ended_at":"2026-06-02T10:15:00+09:00","status":"partial_success","documents_attempted":42,"documents_collected":40,"files_available":55,"run_summary_path":"artifacts/runs/run-20260602-aapl/run-summary.md","qa_path":"artifacts/runs/run-20260602-aapl/qa.md"}
```

## 6. 식별자 규칙

식별자는 나중에 데이터베이스와 RAG로 옮겨도 유지될 수 있어야 한다.

### 6.1 `entity_id`

SEC 회사:

```text
sec-cik-{10-digit-cik}
```

예:

```text
sec-cik-0000320193
```

### 6.2 `document_id`

SEC 문서:

```text
sec:{cik}:{accession_no}:{document_type_slug}
```

예:

```text
sec:0000320193:0000320193-24-000123:10-k
```

IR 문서:

```text
ir:{ticker}:{date}:{slug}
```

예:

```text
ir:AAPL:2025-09-09:iphone-event-presentation
```

Transcript:

```text
transcript:{ticker}:{date}:{source_slug}:{slug}
```

예:

```text
transcript:AAPL:2025-01-30:motley-fool:q1-earnings-call
```

### 6.3 `file_id`

파일 ID는 파일 내용의 SHA-256 hash를 사용한다.

```text
sha256:{hash}
```

같은 파일이 다른 경로에서 발견되어도 같은 `file_id`를 가질 수 있다.

### 6.4 slug 규칙

slug는 폴더명과 ID 일부에 쓰는 안전한 문자열이다.

규칙:

- 소문자 사용
- 공백은 `-`로 변경
- 알파벳, 숫자, 하이픈만 사용
- 연속 하이픈은 하나로 축소
- 80자 이하 권장
- 제목이 없으면 `document`, `presentation`, `transcript` 같은 일반 이름 사용

## 7. raw 경로 규칙

### 7.1 SEC EDGAR

SEC는 CIK와 accession number를 기준으로 저장한다.

```text
artifacts/raw/sec-edgar/cik-{CIK}/accession-{ACCESSION}/
```

예:

```text
artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-24-000123/
```

권장 파일명:

| 파일 | 역할 |
|---|---|
| `metadata.json` | filing metadata |
| `primary.html` | primary document |
| `primary.txt` | SEC 원문 txt가 있는 경우 |
| `exhibits/{sequence}_{filename}` | exhibit 파일 |

원본 파일명이 중요한 경우 `metadata.json`에 원래 파일명을 보존한다.

### 7.2 Company IR

IR 자료는 안정적인 공통 ID가 없으므로 티커, 날짜, slug를 사용한다.

```text
artifacts/raw/company-ir/{TICKER}/{YYYY-MM-DD}_{slug}/
```

예:

```text
artifacts/raw/company-ir/AAPL/2025-09-09_iphone-event-presentation/
```

권장 파일명:

| 파일 | 역할 |
|---|---|
| `metadata.json` | 출처, 제목, URL, 접근 시각 |
| `document.pdf` | IR PDF |
| `document.html` | IR HTML |
| `assets/` | 필요 시 이미지 또는 부속 파일 |

### 7.3 Transcripts

Transcript는 티커, 날짜, 출처, slug를 사용한다.

```text
artifacts/raw/transcripts/{TICKER}/{YYYY-MM-DD}_{source_slug}_{slug}/
```

예:

```text
artifacts/raw/transcripts/AAPL/2025-01-30_motley-fool_q1-earnings-call/
```

권장 파일명:

| 파일 | 역할 |
|---|---|
| `metadata.json` | 출처, 제목, URL, 접근 시각 |
| `transcript.html` | HTML 원문 |
| `transcript.pdf` | PDF 원문 |
| `transcript.txt` | 원출처가 TXT를 제공하는 경우 |

## 8. derived text 경로 규칙

`derived/text/`는 `raw/`의 하위 구조를 가능한 한 그대로 mirror한다.

SEC:

```text
artifacts/derived/text/sec-edgar/cik-0000320193/accession-0000320193-24-000123/
```

Company IR:

```text
artifacts/derived/text/company-ir/AAPL/2025-09-09_iphone-event-presentation/
```

Transcript:

```text
artifacts/derived/text/transcripts/AAPL/2025-01-30_motley-fool_q1-earnings-call/
```

권장 파일명:

| 파일 | 역할 |
|---|---|
| `primary.txt` | SEC primary document text |
| `exhibit-{sequence}.txt` | SEC exhibit text |
| `document.txt` | IR document text |
| `transcript.txt` | transcript text |
| `text-metadata.json` | 추출 방식, 원본 파일, page/section 정보 |

원칙:

- raw 경로를 보면 derived text 경로를 예측할 수 있어야 한다.
- derived text는 원문을 해석하지 않는다.
- 원문과의 연결을 `files.jsonl`에 `document_id`와 `file_role: text`로 기록한다.

## 9. download log와 catalog 승격 규칙

### 9.1 역할 구분

`runs/{run-id}/download-log.jsonl`과 `catalog/files.jsonl`은 역할이 다르다.

| 파일 | 역할 | 성격 |
|---|---|---|
| `runs/{run-id}/download-log.jsonl` | 실행 중 다운로드 시도 기록 | 작업 일지 |
| `catalog/files.jsonl` | 검증된 로컬 파일 원장 | 확정 등록부 |

### 9.2 download-log 기록 대상

`download-log.jsonl`에는 모든 시도를 기록한다.

- 성공
- 실패
- 재시도
- skipped
- 임시 경로
- HTTP status
- 오류 메시지
- 파일 크기
- hash 계산 결과

예시:

```json
{"run_id":"run-20260602-aapl","attempt_id":"attempt-001","document_id":"sec:0000320193:0000320193-24-000123:10-k","source_url":"https://www.sec.gov/...","target_path":"artifacts/raw/sec-edgar/cik-0000320193/accession-0000320193-24-000123/primary.html","attempt_status":"success","http_status":200,"size_bytes":123456,"sha256":"example","started_at":"2026-06-02T10:00:00+09:00","ended_at":"2026-06-02T10:00:03+09:00","error":null}
```

### 9.3 catalog 승격 조건

run 완료 후 아래 조건을 만족한 파일만 `catalog/files.jsonl`에 승격한다.

- 로컬 파일이 실제 존재한다.
- 파일 크기가 0보다 크다.
- SHA-256 hash가 계산됐다.
- `document_id`가 존재한다.
- `file_role`이 정해졌다.
- source URL 또는 원출처 metadata가 남아 있다.

실패한 다운로드는 기본적으로 `catalog/files.jsonl`에 올리지 않는다.  
실패는 `download-log.jsonl`, `run-summary.md`, `qa.md`에 남긴다.

### 9.4 documents와 files의 관계

`documents.jsonl`은 논리적 문서의 상태를 기록한다.  
`files.jsonl`은 실제 로컬 파일의 상태를 기록한다.

하나의 document는 여러 file을 가질 수 있다.

예:

```text
document: 8-K Item 2.02 filing
├── file: metadata.json
├── file: primary.html
└── file: exhibit EX-99.1 PDF
```

문서 수집이 부분 성공인 경우:

- `documents.jsonl`에는 `collection_status: collected` 또는 `partial` 사용 여부를 향후 contract에서 결정한다.
- v1 권장값은 `collected`, `pending`, `failed`, `skipped`만 사용한다.
- exhibit 일부 누락은 `qa.md`와 `companies/{TICKER}/index.md`의 확인 필요 목록에 남긴다.

## 10. 상태값 정의

### 10.1 `collection_status`

`documents.jsonl`의 문서 상태다.

| 값 | 의미 |
|---|---|
| `pending` | 수집 대상이나 아직 완료되지 않음 |
| `collected` | 하나 이상의 raw 파일이 로컬에 저장되고 catalog에 등록됨 |
| `failed` | 문서 수집 시도 실패 |
| `skipped` | 정책상 수집하지 않음 |

`link_only`는 사용하지 않는다.

### 10.2 `file_status`

`files.jsonl`의 파일 상태다.

| 값 | 의미 |
|---|---|
| `available` | 로컬 파일 존재 확인 |
| `missing` | catalog에는 있으나 파일이 사라짐 |
| `replaced` | 새 파일로 대체됨 |

### 10.3 `attempt_status`

`download-log.jsonl`의 다운로드 시도 상태다.

| 값 | 의미 |
|---|---|
| `success` | 다운로드 성공 |
| `failed` | 다운로드 실패 |
| `retry` | 재시도 예정 또는 재시도 기록 |
| `skipped` | 정책상 다운로드하지 않음 |
| `blocked` | bot 차단, Cloudflare, 접근 차단 감지 |
| `paywalled` | 유료벽 또는 로그인 요구 감지 |
| `not_found` | 원문 후보를 찾지 못함 |
| `skipped_budget` | 시도 예산 또는 냉각 기간 때문에 건너뜀 |

Transcript 시도는 위 상태값을 적극적으로 사용한다.  
`blocked`, `paywalled`, `not_found`, `skipped_budget`은 Source Pack 전체 실패가 아니라 optional source 실패 또는 보류로 처리한다.

### 10.4 `text_status`

텍스트 추출 상태다.  
v1에서는 선택 필드로 둔다.

| 값 | 의미 |
|---|---|
| `pending` | 추출 예정 |
| `extracted` | 추출 완료 |
| `failed` | 추출 실패 |
| `not_applicable` | 추출 대상이 아님 |

## 11. 다음 하네스가 Source Pack을 읽는 방식

다른 가치투자 하네스는 원칙적으로 외부 사이트를 다시 방문하지 않는다.

권장 읽기 순서:

1. `artifacts/companies/{TICKER}/index.md`를 읽어 회사별 수집 상태와 확인 필요 항목을 파악한다.
2. `artifacts/catalog/entities.jsonl`에서 `ticker`와 `cik`를 확인한다.
3. `artifacts/catalog/documents.jsonl`에서 필요한 문서를 찾는다.
4. `artifacts/catalog/files.jsonl`에서 실제 로컬 파일 경로를 찾는다.
5. `derived/text/`가 있으면 텍스트를 먼저 읽고, 없으면 `raw/`를 읽는다.
6. raw도 없으면 Source Pack 재실행 또는 사람 확인을 요청한다.

예:

Industry Primer 하네스가 AAPL 자료를 읽는 경우:

```text
1. companies/AAPL/index.md
2. catalog/documents.jsonl에서 ticker=AAPL, document_type in [10-K, 10-Q, IR]
3. catalog/files.jsonl에서 file_role in [primary, ir_deck, text]
4. derived/text 우선, raw fallback
```

Business Model 하네스:

```text
1. 최신 10-K primary text
2. 최근 IR presentation
3. 최근 10-Q
4. 확인 필요 항목
```

Management/Proxy 하네스:

```text
1. DEF 14A
2. 10-K governance 관련 섹션
3. 필요한 경우 IR 자료
```

Transcript 하네스:

```text
1. catalog/documents.jsonl에서 document_type=transcript 또는 source_type=transcripts 문서 확인
2. catalog/files.jsonl에서 raw transcript와 derived text 경로 확인
3. Source Pack의 raw/transcripts/ 또는 derived/text/transcripts/를 입력으로 사용
4. 번역, 요약, Q&A 구조화, 투자 관점 해석은 Transcript 하네스 산출물로 별도 저장
```

다른 분석 하네스는 목적에 따라 두 종류의 입력을 선택해 읽을 수 있다.

| 입력 | 의미 | 사용 예 |
|---|---|---|
| Source Pack transcript raw/text | 원문 또는 비해석 텍스트 | 발언 원문 확인, 직접 인용 검증 |
| Transcript 하네스 산출물 | 번역, 요약, 주제 구조화, 분석 | 경영진 발언 추적, Q&A 쟁점 분석 |

Source Pack은 Transcript 하네스의 분석 결과를 만들지 않는다.

## 12. 기존 링크-only artifacts 처리 원칙

현재 존재하는 아래 구조는 초기 실습 결과로 본다.

```text
artifacts/AAPL/phase2/step3-source-pack/index.md
artifacts/U/phase2/step3-source-pack/index.md
```

새 raw-enabled 구조로 억지 이전하지 않는다.

권장 처리:

1. 새 구조 적용 전 사용자 승인을 받는다.
2. 기존 링크-only artifacts는 삭제하거나 archive한다.
3. 운영 catalog에는 링크-only 항목을 넣지 않는다.
4. AAPL, U가 필요하면 새 구조로 다시 수집한다.

archive를 선택하는 경우:

```text
artifacts/archive/link-only-legacy/AAPL/index.md
artifacts/archive/link-only-legacy/U/index.md
```

단, archive는 운영 입력으로 사용하지 않는다.

## 13. Source Pack 하네스 수정 범위

이 청사진을 적용하려면 다음 파일을 수정해야 한다.

| 목적 | 위치 | 작업 |
|---|---|---|
| 전체 실행 흐름 | `harness/ORCHESTRATOR.md` | 수정 |
| 산출물 계약 | `harness/contracts/source-pack.contract.md` | 수정 |
| 수집 절차 | `harness/procedures/source-pack-collector.md` | 수정 |
| QA 절차 | `harness/procedures/source-pack-qa.md` | 없으면 신규 작성 |
| 회사별 index schema | `harness/schemas/source-pack-index.schema.md` | 수정 |
| catalog schema | `harness/schemas/source-pack-catalog.schema.md` | 신규 작성 |
| run summary schema | `harness/schemas/source-pack-run-summary.schema.md` | 수정 |
| QA rubric | `harness/rubrics/source-pack-qa.rubric.md` | 수정 |
| artifacts 안내 | `artifacts/README.md` | 수정 |
| git 제외 | `.gitignore` | 수정 |

새로 추가할 가능성이 높은 파일:

```text
harness/procedures/source-pack-qa.md
harness/schemas/source-pack-catalog.schema.md
docs/source-pack-artifacts-raw-structure-v1.md
```

## 14. 단계별 적용 계획

### Phase 1. 청사진 확정

- 이 문서를 사용자와 검토한다.
- 폴더 구조, catalog 원장, raw/derived 정책을 확정한다.
- 기존 링크-only artifacts를 삭제할지 archive할지 결정한다.

### Phase 2. 하네스 계약과 schema 수정

- contract에 raw 다운로드를 Source Pack의 기본 목표로 반영한다.
- catalog schema를 추가한다.
- index schema에 새 경로와 catalog 참조를 반영한다.
- collector procedure에 transcript 수집 전 이전 `runs/*/download-log.jsonl`을 확인하는 단계를 추가한다.
- collector procedure는 동일 ticker, quarter, source 조합의 `blocked`, `paywalled`, `not_found`, `skipped_budget` 기록이 90일 이내에 있으면 자동 재시도하지 않는다.
- QA rubric에 raw 파일 존재, hash, catalog 일관성 항목을 추가한다.

### Phase 3. artifacts 구조 전환

- `artifacts/companies/`
- `artifacts/catalog/`
- `artifacts/runs/`
- `artifacts/raw/`
- `artifacts/derived/text/`

필요한 폴더와 placeholder 파일을 만든다.

### Phase 4. 기존 링크-only artifacts 처리

- 삭제 또는 archive 중 하나를 실행한다.
- `artifacts/README.md`에 legacy 처리 결과를 남긴다.

### Phase 5. SEC raw 다운로드 구현

우선순위:

1. 10-K
2. 10-Q
3. DEF 14A
4. 8-K Item 2.02
5. 주요 8-K events

각 파일은 raw에 저장하고, `download-log.jsonl`에서 검증 후 `catalog/files.jsonl`에 승격한다.

### Phase 6. IR/transcript 확장

- Company IR PDF/HTML 저장
- transcript 후보 저장
- 유료/로그인 자료는 사람 승인 필요

### Phase 7. derived text 추가

- HTML text extraction
- PDF text extraction
- raw 구조 mirror
- `text_status`와 text file record 추가

RAG chunking, embedding, vector DB는 별도 하네스 또는 다음 버전에서 설계한다.

## 15. QA 기준

Source Pack raw-enabled 구조는 아래 기준을 만족해야 한다.

### 15.1 구조 QA

- `companies/`, `catalog/`, `raw/`, `derived/`, `runs/` 역할이 분리되어 있다.
- `companies/{TICKER}/sources.jsonl` 같은 이중 원장을 만들지 않는다.
- `catalog/documents.jsonl`과 `catalog/files.jsonl`의 관계가 명확하다.
- `raw/`와 `derived/`는 git에서 제외된다.

### 15.2 catalog QA

- 모든 `document_id`가 안정적으로 생성된다.
- 모든 `file_id`는 hash 기반이다.
- `documents.jsonl`의 `primary_file_id`가 존재하면 `files.jsonl`에 대응 record가 있다.
- `files.jsonl`의 `local_path` 파일이 실제로 존재한다.
- `source_url`, `retrieved_at`, `sha256`, `size_bytes`가 누락되지 않는다.

### 15.3 raw QA

- SEC raw 경로는 CIK와 accession number를 따른다.
- IR/transcript raw 경로는 티커, 날짜, slug를 따른다.
- metadata.json이 원출처와 접근 시각을 보존한다.
- 다운로드 실패는 조용히 생략하지 않는다.
- transcript 접근 차단, 유료벽, 검색 실패는 전체 실패가 아니라 optional source 실패로 기록된다.
- transcript 수집은 정해진 시도 예산과 90일 냉각 기간을 따른다.
- 차단된 transcript URL을 반복 호출하지 않는다.

### 15.4 다음 하네스 전달 QA

- `companies/{TICKER}/index.md`에 다음 하네스 전달 요약이 있다.
- 다른 하네스가 읽을 수 있는 catalog record가 있다.
- raw 또는 derived text 경로가 명확하다.
- 확인 필요 항목과 실패 항목이 분리되어 있다.

## 16. 보류 사항

아래는 v1 청사진에서 구조만 준비하고 구현은 미룬다.

- RAG chunking
- embedding 생성
- vector database 적재
- full-text search index
- object storage 연동
- 대량 병렬 다운로드 최적화
- IR/transcript 유료 소스 연동
- transcript 번역, 요약, Q&A 구조화, 투자 관점 분석

이 항목들은 Source Pack이 raw 저장소로 안정화된 뒤 별도 하네스 또는 v2 구조로 설계한다.
특히 transcript 가공과 분석은 Source Pack이 아니라 별도 Transcript 하네스에서 설계한다.

## 17. 최종 결정 요약

- Source Pack은 raw 원자료 저장소와 catalog 원장 중심으로 간다.
- 링크-only 운영 모드는 두지 않는다.
- `catalog/documents.jsonl`이 문서 단위 단일 원장이다.
- `catalog/files.jsonl`이 실제 로컬 파일 단위 확정 원장이다.
- `runs/{run-id}/download-log.jsonl`은 실행 중 작업 일지다.
- `raw/`는 원자료 원본, `derived/text/`는 비해석 텍스트 추출본이다.
- `raw/transcripts/`는 transcript 원문 저장소이며, transcript 번역·요약·분석 산출물 저장소가 아니다.
- transcript 수집 실패는 Source Pack 전체 실패 조건이 아니다.
- transcript는 시도 예산, 접근 차단 중단 규칙, 90일 냉각 기간을 따른다.
- `raw/`와 `derived/`는 git에 넣지 않는다.
- 기존 링크-only AAPL/U artifacts는 운영 구조로 이전하지 않고 삭제 또는 archive한다.
- 다음 하네스는 외부 사이트가 아니라 Source Pack catalog와 raw/derived 경로를 입력으로 사용한다.
- Transcript 번역, 요약, Q&A 구조화, 투자 관점 분석은 별도 Transcript 하네스에서 수행한다.
