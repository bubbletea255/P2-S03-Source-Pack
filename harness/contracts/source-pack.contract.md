# Source Pack Contract

## 목적

미국 상장 주식 한 회사 또는 여러 회사의 신뢰 가능한 원자료 링크를 수집해 다음 가치투자 하네스가 읽을 수 있는 회사별 `index.md`를 만든다.

이 하네스는 투자 판단, 매수/매도 의견, 밸류에이션, 경쟁우위 결론을 작성하지 않는다.

## 활성화 조건

- "source pack 만들어줘"
- "source pack 실행해줘"
- "{티커} 공시 자료 수집해줘"
- "{티커} 자료 정리해줘"
- "관심 종목 source pack 업데이트해줘"
- "공시 링크 수집해줘"
- "Phase 2 Step 3 원자료 모아줘"

## 입력

- 대상 티커
- `watchlist.md`
- `config.md`
- 기존 회사별 `index.md`
- SEC EDGAR, 기업 IR 사이트, 필요 시 Quartr/AlphaStreet 등 transcript 후보 소스

## 출력

필수:

- `artifacts/{TICKER}/phase2/step3-source-pack/index.md`

선택:

- `artifacts/README.md` 갱신
- `artifacts/improvement-log.md` 갱신
- 비교 모드 실행 리포트

## 수집 대상

| 계층 | 자료 | 기본 범위 | 처리 |
|---|---|---|---|
| Tier 1 | 10-K | 10년 | 필수 |
| Tier 1 | 10-Q | 12분기 | 필수 |
| Tier 1 | DEF 14A / Proxy | 5년 | 필수 |
| Tier 1 | 8-K Item 2.02 실적 발표 자료 | 12분기 | 필수 |
| Tier 2 | 주요 8-K 이벤트 | 필터 기준 | 선택 |
| Tier 2 | IR 투자자 프레젠테이션 | 발견 가능한 범위 | 선택 |
| Optional | Earnings Transcript | 3년 | 실패해도 오류 아님 |
| Optional | 산업/경쟁 자료 링크 | 발견 가능한 범위 | 다음 하네스 보조 |

## 최소 완료 기준

- 회사 CIK 또는 CIK 확인 실패 사유가 기록되어 있다.
- 10-K, 10-Q, Proxy, 실적 발표 자료의 수집/누락 건수가 기록되어 있다.
- 각 표가 Markdown 표로 파싱 가능하다.
- 수집 실패 항목은 `수동 확인 필요 목록`에 이유와 함께 남아 있다.
- `last_updated`가 현재 실행일로 기록되어 있다.
- 다음 하네스가 읽을 `다음 하네스 전달 요약`이 있다.

## 우수 산출물 기준

- CIK 조회는 SEC ticker mapping을 우선 사용하고 fallback을 명시한다.
- 최신 자료와 과거 자료가 명확히 구분된다.
- Item 2.02와 EX-99.1의 관계가 혼동되지 않는다.
- Transcript와 IR 자료는 출처 우선순위와 미수집 이유가 분리된다.
- 다음 Phase에서 바로 사용할 핵심 링크와 확인 필요 항목이 한눈에 보인다.

## 금지사항

- 투자 의견, 목표주가, 매수/매도 판단을 작성하지 않는다.
- 확인되지 않은 IR 사이트나 transcript 링크를 확정처럼 쓰지 않는다.
- SEC/IR 요청을 병렬로 폭주시키지 않는다.
- 수집 실패를 조용히 생략하지 않는다.
- 공통 업무 규칙을 Claude/Codex adapter에 길게 복사하지 않는다.

## 사람 승인 필요

- 수집 대상 티커 확정
- `config.md` 속도 제한 또는 수집 범위 변경
- 유료/로그인 소스 사용
- 기존 산출물 삭제 또는 대규모 덮어쓰기
