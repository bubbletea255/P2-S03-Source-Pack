---
name: source-pack-orchestrator
description: 미국 상장 주식의 SEC 공시와 IR 자료 링크를 수집해서 회사별 index.md를 만드는 Source Pack 하네스의 전체 흐름을 관리합니다. "source pack 만들어줘", "source pack 실행해줘", "공시 자료 수집해줘", "관심 종목 업데이트", "source pack 업데이트", "{티커} 자료 정리해줘", "{티커} source pack" 같은 요청에 실행됩니다. 신규 수집과 증분 업데이트를 모두 처리합니다.
---

# Source Pack Orchestrator

## 역할

`watchlist.md`와 `config.md`를 읽고, 사용자가 선택한 회사들의
SEC 공시 링크 수집을 순차적으로 진행합니다.
각 회사의 실제 수집은 `source-pack-collector` 에이전트가 담당합니다.

## 실행 모드 판단

각 회사마다 아래 기준으로 모드를 결정합니다.

| 조건 | 모드 | 처리 방식 |
|---|---|---|
| `artifacts/{TICKER}/phase2/step3-source-pack/index.md` 없음 | 전체 수집 | config 전체 범위 수집 |
| index.md 있음 | 증분 업데이트 | `last_updated` 이후 새 공시만 추가 |

---

## 실행 절차

### 1단계: 관심 종목 목록 표시

`watchlist.md`를 읽어 관심 종목 목록을 번호와 함께 표시합니다.
파일이 없으면 아래 안내를 출력하고 종료합니다:
> "watchlist.md 파일이 없습니다. 프로젝트 루트에 watchlist.md를 만들고 티커를 추가해주세요."

```
수집할 회사를 선택하세요.

[관심 종목 목록]
  1. AAPL
  2. MSFT
  (이하 watchlist.md 내용)

입력 옵션:
  - 전체 수집:       all
  - 번호로 선택:     1, 3  (쉼표로 구분)
  - 티커 직접 입력:  NVDA  (목록에 없는 회사도 가능)
  - 복수 직접 입력:  U, APP, DDOG
```

### 2단계: config 설정 확인

`config.md`를 읽어 핵심 설정값을 요약해서 표시합니다.
파일이 없으면 기본값으로 진행한다고 안내합니다.

```
현재 설정

  수집 범위: 10-K 10년 / 10-Q 12분기 / Proxy 5년 / Transcript 3년
  속도 제한: SEC 0.5초 대기 / IR 2.0초 대기 / 재시도 3회
  8-K Item 필터: 1.01, 1.02, 1.03, 2.01, 2.03, 2.05, 2.06, 3.01, 3.02, 4.01, 4.02, 5.01, 5.02

이 설정으로 진행할까요?
설정을 변경하려면 config.md 파일을 직접 수정하세요.
진행하려면 "확인" 또는 "진행"이라고 입력하세요.
```

### 3단계: 사람 승인 대기

사용자가 "확인", "진행", "yes", "네", "좋아" 등으로 응답하면 다음 단계로 넘어갑니다.
설정 변경을 요청하면 config.md 수정 방법을 안내하고 다시 확인합니다.

### 4단계: 순차 수집 실행

선택된 티커 목록을 순서대로 처리합니다. **병렬 실행 금지** (SEC 속도 제한).

각 티커마다:
1. `artifacts/{TICKER}/phase2/step3-source-pack/index.md` 존재 여부 확인 → 모드 결정
2. `source-pack-collector` 에이전트 호출 (티커 + config 설정 전달)
3. 완료 보고 수신 후 진행 상황 출력

진행 상황 출력 형식:
```
[1/3] AAPL 수집 중... (전체 수집 모드)
[1/3] AAPL 완료 ✓  10-K 10건 / 10-Q 12건 / Proxy 5건 / 실적발표 12건 / 8-K이벤트 23건 / 수동확인 2건
[2/3] MSFT 수집 중... (증분 업데이트 모드)
...
```

수집 실패 시 해당 회사를 건너뛰고 다음 회사를 계속 처리합니다.

### 5단계: 완료 요약 출력

모든 티커 처리 완료 후 결과를 요약합니다.

```
수집 완료

✓ AAPL  10-K 10건 / 10-Q 12건 / Proxy 5건 / 실적발표 12건 / 8-K이벤트 23건
✓ MSFT  10-K 10건 / 10-Q 12건 / Proxy 5건 / 실적발표 12건 / 8-K이벤트 18건
✗ NVDA  CIK 조회 실패 — 수동 확인 필요

수동 확인 필요 항목:
  AAPL: 2건 → artifacts/AAPL/phase2/step3-source-pack/index.md 맨 아래 확인
  MSFT: 0건

산출물 위치: artifacts/{TICKER}/phase2/step3-source-pack/index.md
```

### 6단계: artifacts/README.md 갱신

최초 실행이거나 새 티커가 추가된 경우, `artifacts/README.md`에 해당 티커와 index.md 경로를 추가합니다.

---

## 후속 요청 처리

이미 index.md가 있는 회사에 대한 업데이트 요청도 이 Skill이 처리합니다.

| 요청 예시 | 처리 방식 |
|---|---|
| "AAPL source pack 업데이트해줘" | AAPL만 증분 모드로 실행 |
| "관심 종목 전체 업데이트해줘" | watchlist 전체를 증분 모드로 실행 |
| "U 추가해서 수집해줘" | U를 신규 수집(전체) 모드로 실행 |
| "AAPL, MSFT 다시 수집해줘" | 해당 티커들을 각각 모드 판단 후 실행 |

---

## 오류 처리

| 상황 | 처리 방법 |
|---|---|
| watchlist.md 없음 | 파일 생성 방법 안내 후 종료 |
| config.md 없음 | 기본값으로 진행 (10-K 10년 / 10-Q 12분기 / Proxy 5년) |
| collector 실패 | 해당 회사 건너뛰고 다음 진행. 완료 요약에 실패 표시 |
| 사용자가 없는 티커 입력 | collector가 처리. CIK 조회 실패 시 수동 확인 필요로 기록 |
