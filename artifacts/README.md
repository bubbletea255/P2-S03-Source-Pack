# 산출물 지도

이 폴더에는 Source Pack 하네스가 생성한 파일이 저장됩니다.

## 폴더 구조

```
artifacts/
├── README.md                          ← 이 파일 (산출물 위치 안내)
├── improvement-log.md                 ← 하네스 개선 기록
└── {TICKER}/
    └── phase2/
        └── step3-source-pack/
            └── index.md               ← 회사별 공시 링크 모음 (최종 산출물)
```

## 파일 역할

| 파일 | 생성 시점 | 역할 |
|---|---|---|
| `{TICKER}/phase2/step3-source-pack/index.md` | source-pack-collector 실행 시 | 회사별 SEC 공시·IR 자료 링크 목차 |
| `improvement-log.md` | 하네스 개선 시 | 변경 사유·날짜 기록 |

## 수집된 회사 목록

하네스 실행 후 이 목록이 자동으로 갱신됩니다.

| 티커 | 회사명 | 마지막 업데이트 | index.md 경로 |
|-----|------|--------------|--------------|
| AAPL | Apple Inc. | 2026-05-29 | artifacts/AAPL/phase2/step3-source-pack/index.md |
| U | Unity Software Inc. | 2026-05-29 | artifacts/U/phase2/step3-source-pack/index.md |

## 다음 Phase에서 읽는 방법

Phase 3~5 하네스는 분석 시작 전에 아래 경로의 파일을 먼저 읽습니다.

```
artifacts/{TICKER}/phase2/step3-source-pack/index.md
```

이 파일의 헤더 섹션에 회사 기본 정보 (CIK, IR 사이트, 회계연도 등)가 있고,
이후 섹션에 10-K, 10-Q, 실적발표 등 공시 링크가 정리되어 있습니다.
