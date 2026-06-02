# Source Pack Index Schema

회사별 최종 산출물 `artifacts/{TICKER}/phase2/step3-source-pack/index.md`는 아래 형식을 따른다.

## 필수 섹션

```md
# Source Pack - {TICKER} ({company_name})

last_updated: YYYY-MM-DD
ticker: {TICKER}
cik: {10-digit CIK or [확인 필요: reason]}
exchange: {exchange or [확인 필요: reason]}
sector: {sicDescription or [확인 필요: reason]}
fiscal_year_end: {month or [확인 필요: reason]}
ipo_date: {date or [확인 필요: reason]}
ir_site: {url or [확인 필요: reason]}
collection_mode: 신규 수집 | 증분 업데이트 | 부분 재검토
source_priority: SEC EDGAR → Company IR → Quartr/AlphaStreet 후보 → 수동 확인
수집 범위: {summary}

---

## 수집 현황 요약
| 항목 | 요청 | 수집 | 누락 | 비고 |
|---|---:|---:|---:|---|

## SEC 메타데이터
| 항목 | 값 | 출처 |
|---|---|---|

## 10-K (Annual Reports)
| 회계연도 | 제출일 | Form | 링크 |
|---|---|---|---|

## 10-Q (Quarterly Reports)
| 분기 | 제출일 | Form | 링크 |
|---|---|---|---|

## Proxy Statement (DEF 14A)
| 연도 | 제출일 | Form | 링크 |
|---|---|---|---|

## 실적 발표 자료 (8-K Item 2.02)
| 분기 | 제출일 | Item | Exhibit 확인 | 링크 |
|---|---|---|---|---|

## 8-K 주요 이벤트
| 날짜 | Item | 제목 | 링크 |
|---|---|---|---|

## 실적 발표 대본 (Earnings Transcript)
| 분기 | 출처 | 링크/경로 | 상태 | 비고 |
|---|---|---|---|---|

## IR 자료
| 날짜 | 제목 | 링크 | 종류 | 상태 |
|---|---|---|---|---|

## 산업·경쟁 자료 후보
| 자료 | 링크 | 용도 | 상태 |
|---|---|---|---|

## 수동 확인 필요 목록
| 항목 | 기간/대상 | 이유 | 권장 확인 방법 |
|---|---|---|---|

## 다음 하네스 전달 요약
- Industry Primer가 먼저 읽을 자료:
- Value Chain 하네스가 먼저 읽을 자료:
- Business Model 하네스가 먼저 읽을 자료:
- Market/Share 하네스가 먼저 읽을 자료:
- Competition 하네스가 먼저 읽을 자료:
- 확인 필요:
```

## 형식 규칙

- Markdown 표 구분선과 첫 데이터 행은 반드시 다른 줄이어야 한다.
- 누락 건수는 음수일 수 없다.
- 요청보다 많이 수집한 경우 누락은 `0`이고 비고에 `추가 수집`을 쓴다.
- 확인되지 않은 정보는 `[확인 필요: {이유}]` 형식으로 쓴다.
- 추정 정보는 `(추정: {근거})` 형식으로 쓴다.
- 사람 승인 필요 항목은 `▶ 사람 승인 필요: {내용}` 형식으로 쓴다.
