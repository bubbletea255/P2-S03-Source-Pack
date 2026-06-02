# Artifacts

이 폴더는 Source Pack v2 하네스의 실행 결과와 개선 기록을 저장한다.

## 폴더 구조

```text
artifacts/
├── README.md
├── improvement-log.md
├── {TICKER}/
│   └── phase2/
│       └── step3-source-pack/
│           └── index.md
├── run-YYYYMMDD-source-pack-{ticker}/
├── run-YYYYMMDD-source-pack-{ticker}-claude/
├── run-YYYYMMDD-source-pack-{ticker}-codex/
└── comparison-run-YYYYMMDD-source-pack-{ticker}.md
```

## 회사별 최종 산출물

| 경로 | 역할 |
|---|---|
| `{TICKER}/phase2/step3-source-pack/index.md` | 다음 가치투자 하네스가 읽는 회사별 원자료 인덱스 |

## 수집된 회사 목록

| 티커 | 회사명 | 마지막 수집일 | schema | index.md 경로 |
|---|---|---|---|---|
| AAPL | Apple Inc. | 2026-05-29 | source-pack-v2 | `artifacts/AAPL/phase2/step3-source-pack/index.md` |
| U | Unity Software Inc. | 2026-05-29 | source-pack-v2 | `artifacts/U/phase2/step3-source-pack/index.md` |

## 다음 하네스에서 읽는 방법

분석 하네스는 먼저 아래 파일을 읽는다.

```text
artifacts/{TICKER}/phase2/step3-source-pack/index.md
```

우선 확인할 섹션:

1. 회사 메타데이터
2. 수집 현황 요약
3. 최신 10-K, 10-Q, Proxy, 실적 발표 자료
4. 수동 확인 필요 목록
5. 다음 하네스 전달 요약

## 품질 기준

산출물은 `harness/schemas/source-pack-index.schema.md`와 `harness/rubrics/source-pack-qa.rubric.md`를 따른다.
