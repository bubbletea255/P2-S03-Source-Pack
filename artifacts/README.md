# Artifacts

이 폴더는 Source Pack v2 하네스의 실행 결과, 원자료, catalog 원장, 개선 기록을 저장한다.

Source Pack은 링크 모음이 아니라 다음 하네스가 반복해서 읽을 수 있는 원자료 저장소다.

## 폴더 구조

```text
artifacts/
├── README.md
├── improvement-log.md
│
├── companies/
│   └── {TICKER}/
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
│   ├── company-ir/
│   └── transcripts/
│
├── derived/
│   └── text/
│       ├── sec-edgar/
│       ├── company-ir/
│       └── transcripts/
│
└── runs/
    └── {run-id}/
        ├── download-log.jsonl
        ├── run-summary.md
        └── qa.md
```

`artifacts/raw/`와 `artifacts/derived/`는 대용량 원자료이므로 git에 커밋하지 않는다.

## 운영 산출물

| 경로 | 역할 |
|---|---|
| `companies/{TICKER}/index.md` | 사람이 읽는 회사별 원자료 지도 |
| `catalog/entities.jsonl` | 회사 메타데이터 원장 |
| `catalog/documents.jsonl` | 문서 단위 원장, 다음 하네스의 기본 source of truth |
| `catalog/files.jsonl` | 실제 파일 단위 원장 |
| `catalog/runs.jsonl` | 실행 이력 기계용 포인터 |
| `runs/{run-id}/download-log.jsonl` | 실행 중 다운로드 attempt 기록 |
| `runs/{run-id}/run-summary.md` | 실행 요약 |
| `runs/{run-id}/qa.md` | QA 결과 |
| `raw/...` | 원자료 파일 |
| `derived/text/...` | 원자료에서 추출한 비해석 텍스트 |

삭제된 legacy link-only 구조인 `artifacts/{TICKER}/phase2/step3-source-pack/index.md`는 운영 입력이나 출력으로 사용하지 않는다.

## 수집된 회사 목록

새 구조에서는 회사 목록을 이 파일에 직접 중복 기록하지 않는다.
현재 회사 목록은 `artifacts/catalog/entities.jsonl`과 `artifacts/companies/{TICKER}/index.md`를 기준으로 확인한다.

## 다음 하네스에서 읽는 방법

다음 하네스는 아래 순서로 Source Pack 산출물을 읽는다.

1. `artifacts/companies/{TICKER}/index.md`
2. `artifacts/catalog/documents.jsonl`
3. `artifacts/catalog/files.jsonl`
4. 필요한 경우 `artifacts/raw/...`
5. 필요한 경우 `artifacts/derived/text/...`
6. `artifacts/runs/{run-id}/qa.md`

`catalog_status`가 `fail`, `unverified`, `[확인 필요:]`인 자료는 확정 입력으로 사용하지 않는다.

## 품질 기준

산출물은 아래 공통 원장을 따른다.

- `harness/contracts/source-pack.contract.md`
- `harness/schemas/source-pack-catalog.schema.md`
- `harness/schemas/source-pack-index.schema.md`
- `harness/procedures/source-pack-qa.md`
- `harness/rubrics/source-pack-qa.rubric.md`
