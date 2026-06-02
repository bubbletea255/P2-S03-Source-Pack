# Source Pack 설정

이 파일에서 Source Pack 하네스의 모든 설정값을 관리합니다.
하네스 실행 전 이 파일을 열어 설정값을 확인하고 수정할 수 있습니다.

---

## 수집 범위 (초기 전체 수집 기준)

- 10-K (연간 보고서): 10년
- 10-Q (분기 보고서): 12분기
- DEF 14A (Proxy Statement): 5년
- 8-K Item 2.02 (실적 발표 자료): 12분기 — EX-99.1 첨부 여부는 보조 확인
- 실적 발표 대본 (Transcript): 3년 — optional, 실패해도 오류 아님

---

## 수집 대상 공시

### Tier 1 (필수 수집)
- 10-K
- 10-Q
- DEF 14A
- 8-K Item 2.02 (실적 발표 자료, EX-99.1 첨부 여부 보조 확인)

### Tier 2 (선택 수집)
- 8-K 주요 이벤트 (아래 필터 적용)
- IR 투자자 프레젠테이션

---

## 8-K 필터 설정

### Item 번호 필터 (아래 번호는 항상 수집)
1.01, 1.02, 1.03, 2.01, 2.03, 2.05, 2.06, 3.01, 3.02, 4.01, 4.02, 5.01, 5.02

**각 Item 의미 요약**
| Item | 내용 |
|---|---|
| 1.01 | 중요 계약 체결 (M&A 계약, 신용한도 등) |
| 1.02 | 중요 계약 종료 |
| 1.03 | 파산·법정관리 |
| 2.01 | 인수·자산매각 완료 |
| 2.03 | 신규 부채 계약 |
| 2.05 | 구조조정 비용 |
| 2.06 | 중요 자산 손상 (영업권 손상 등) |
| 3.01 | 상장폐지 통보 |
| 3.02 | 비등록 주식 발행 (사모 유상증자) |
| 4.01 | 감사인 교체 |
| 4.02 | 재무제표 재작성 |
| 5.01 | 경영권 변경 |
| 5.02 | 임원·이사 교체 |

### 키워드 필터 (Item 7.01, 8.01은 제목에 아래 키워드가 있을 때만 수집)
dividend, repurchase, buyback, restructuring, settlement, acquisition, merger, spin-off, offering, convertible, covenant, impairment, restatement

---

## 속도 제한 (임의 변경 금지 — SEC·IR 사이트 차단 방지)

- SEC EDGAR API 호출 간격: 0.5초
- IR 사이트 요청 간격: 2.0초
- 429 오류(요청 초과) 발생 시 대기 시간: 30초
- 최대 재시도 횟수: 3회

---

## SEC 요청 헤더

SEC EDGAR 정책에 따라 모든 API 요청에 아래 User-Agent를 포함합니다.

- User-Agent: ValueInvestingResearch frisat789@gmail.com

이메일 변경이 필요하면 위 주소를 본인 이메일로 교체하세요.

---

## 저장 경로

- 회사별 index.md: artifacts/companies/{TICKER}/index.md
- 기계용 catalog: artifacts/catalog/*.jsonl
- 실행 기록: artifacts/runs/{run-id}/
- 원자료: artifacts/raw/...
- 비해석 텍스트: artifacts/derived/text/...

---

## CIK 조회 우선순위

1. `https://www.sec.gov/files/company_tickers_exchange.json`
2. `https://www.sec.gov/files/company_tickers.json`
3. `https://efts.sec.gov/LATEST/search-index`
4. `https://www.sec.gov/cgi-bin/browse-edgar`
