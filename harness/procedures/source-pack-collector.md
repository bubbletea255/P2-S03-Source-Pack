# Source Pack Collector Procedure

## 목적

한 회사의 SEC 공시, IR 자료, 실적 발표 자료, transcript 후보 링크를 수집해 회사별 `index.md`를 만든다.

## 0단계: 모드 결정

`artifacts/{TICKER}/phase2/step3-source-pack/index.md` 존재 여부를 확인한다.

- 파일 없음: 신규 수집
- 파일 있음: `last_updated` 이후 증분 업데이트

## 1단계: CIK 조회

CIK 조회 우선순위:

1. `https://www.sec.gov/files/company_tickers_exchange.json`
2. `https://www.sec.gov/files/company_tickers.json`
3. `https://efts.sec.gov/LATEST/search-index?q="{TICKER}"`
4. `https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK={TICKER}`

CIK는 10자리로 패딩한다. 예: `320193` → `0000320193`.

CIK 조회 실패 시 수동 확인 필요 목록에 기록하고 해당 회사 수집을 중단한다.

## 2단계: 회사 메타데이터 수집

CIK 확보 후 아래 API를 호출한다.

```text
GET https://data.sec.gov/submissions/CIK{10자리CIK}.json
```

추출 항목:

- `name`
- `tickers`
- `exchanges`
- `sic`
- `sicDescription`
- `fiscalYearEnd`
- 최근/과거 filings
- 상장일 추정: 가장 오래된 관련 10-K, S-1, 10-Q 또는 IPO 자료 기반

## 3단계: SEC 공시 링크 수집

`filings.recent`와 `filings.files`를 함께 확인한다.

직접 링크 형식:

```text
https://www.sec.gov/Archives/edgar/data/{CIK숫자}/{accessionNumber하이픈제거}/{primaryDocument}
```

수집 대상:

- 10-K: 최대 10년
- 10-Q: 최대 12분기
- DEF 14A: 최대 5년
- 8-K Item 2.02: 최대 12분기
- 주요 8-K 이벤트: config의 Item/키워드 필터

## 4단계: 실적 발표 자료 처리

실적 발표 자료의 주 기준은 `8-K Item 2.02`다.
`EX-99.1`은 실적 발표 보도자료 또는 shareholder letter가 첨부되었는지 확인하는 보조 기준이다.

기록할 항목:

- 분기
- 제출일
- Item
- primary document 링크
- exhibit 확인 여부

## 5단계: Transcript 수집

Transcript는 optional이다.

우선순위:

1. SEC 8-K 또는 exhibit에 transcript가 있는지 확인
2. 회사 IR 사이트의 earnings call / conference call / webcast / transcript 탐색
3. Quartr 후보
4. AlphaStreet 후보
5. 없으면 `[확인 필요]`로 기록

실패해도 하네스 실패로 보지 않는다.

## 6단계: IR 자료 수집

IR 사이트는 submissions 응답, 회사 홈페이지, 검색 후보를 통해 확인한다.

탐색 대상:

- investor presentation
- shareholder letter
- annual report
- investor day
- earnings presentation

자동 탐색 실패 시 수동 확인 필요로 기록한다.

## 7단계: index.md 작성

`harness/schemas/source-pack-index.schema.md`를 따른다.

필수:

- 필수 섹션 모두 포함
- Markdown 표 파싱 가능
- 누락 건수 음수 금지
- 수동 확인 필요 항목 명시
- 다음 하네스 전달 요약 포함

## 속도 제한

- SEC EDGAR API 호출 간격: 최소 0.5초
- IR 사이트 요청 간격: 최소 2.0초
- 429 오류: 30초 대기 후 최대 3회 재시도

모든 SEC 요청에는 `config.md`의 User-Agent를 포함한다.
