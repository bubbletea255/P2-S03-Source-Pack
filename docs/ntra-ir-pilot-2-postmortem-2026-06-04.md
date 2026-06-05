# NTRA IR Pilot 2 사후 메모

작성일: 2026-06-04

## 목적

이 메모는 NTRA IR pilot 2에서 발생한 수집 실패와 보안 격리 사건을 정리한다.

이번 메모는 투자 분석이 아니다.
IR collector와 QA 절차를 더 안전하게 만들기 위한 실행 기록이다.

## 참조 산출물

| 항목 | 경로 |
|---|---|
| preflight 메모 | `docs/ntra-ir-preflight-2026-06-04.md` |
| download-log | `artifacts/runs/run-20260604-ntra-ir-pilot/download-log.jsonl` |
| run-summary | `artifacts/runs/run-20260604-ntra-ir-pilot/run-summary.md` |
| qa | `artifacts/runs/run-20260604-ntra-ir-pilot/qa.md` |

## 승인된 run_scope

```text
test only: NTRA company-ir official Natera IR Q1 2026 earnings press release HTML, Q1 2026 earnings presentation PDF, and 44th Annual J.P. Morgan Healthcare Conference presentation PDF; no SEC download; no SEC filings page harvesting; no webcast/audio/video; no transcript; no archive-wide crawling; no operating catalog/index merge until user approval
```

## 대상 3건과 최종 상태

| document_id | document_type | 자료 | 최종 상태 | 판단 |
|---|---|---|---|---|
| `ir-ntra-earnings-release-fy2026-q1` | `ir-earnings-release` | Q1 2026 earnings press release HTML | failed | HTTP 403 transport failure |
| `ir-ntra-deck-fy2026-q1` | `ir-deck` | Q1 2026 earnings presentation PDF | security_quarantined | download-log 성공 후 Bitdefender 격리 |
| `ir-ntra-deck-2026-01-13-jpm-healthcare-conference` | `ir-deck` | 44th Annual J.P. Morgan Healthcare Conference presentation PDF | collected | raw 파일과 metadata 검증 성공 |

## 사건 정리

### 1. HTML 보도자료

Natera 공식 IR HTML 보도자료는 download-log `attempt-001`에서 HTTP 403으로 실패했다.

판단:

- 이는 Bitdefender 격리와 별도 사건으로 본다.
- 사이트가 단순 자동 요청 또는 현재 요청 헤더를 거절한 transport 문제에 가깝다.
- 이 자료는 raw 파일이 없으므로 운영 catalog/index에 반영하지 않는다.

### 2. Q1 2026 earnings presentation PDF

Q1 2026 earnings presentation PDF는 download-log `attempt-002`에 HTTP 200, size, sha256이 기록됐다.
그러나 QA 시점에는 아래 raw 파일이 존재하지 않았다.

```text
artifacts/raw/company-ir/NTRA/2026-05-07_fy2026-q1-earnings-presentation/document.pdf
```

사용자가 제공한 Bitdefender 알림에 따르면 동일 경로의 `document.pdf`가 위험 요소 파일로 탐지되어 격리됐다.

기록된 탐지 정보:

| 항목 | 값 |
|---|---|
| 보안 제품 | Bitdefender |
| 탐지 계층 | 지능형 위협 탐지(ATD) 보안 계층 |
| 표시된 프로세스 흐름 | `codex.exe` -> `powershell.exe` |
| 탐지 ID | `SuspiciousBehavior.182793FD17B38920` |
| 격리 파일 | `artifacts/raw/company-ir/NTRA/2026-05-07_fy2026-q1-earnings-presentation/document.pdf` |

판단:

- 공식 Natera IR 페이지에서 연결된 Q4 CDN PDF를 대상으로 한 것은 맞다.
- 다만 공식 출처라는 사실만으로 로컬 실행 환경에서 안전하다고 보장할 수는 없다.
- Bitdefender 탐지가 파일 자체의 악성 여부인지, `codex.exe`가 `powershell.exe`를 통해 자동 다운로드/파일 생성을 수행한 행위 패턴 때문인지는 이 메모만으로 확정하지 않는다.
- 격리된 파일은 복구하거나 열지 않는다.
- 격리된 파일은 후속 운영 `files.jsonl`, `documents.jsonl`, 회사별 `index.md`에 승격하지 않는다.

### 3. J.P. Morgan Healthcare Conference presentation PDF

J.P. Morgan Healthcare Conference presentation PDF는 raw 파일과 metadata가 존재하고, metadata의 hash와 실제 파일 hash가 일치했다.

판단:

- 이 1건은 수집 자체는 성공했다.
- 하지만 이번 pilot 전체가 QA `fail`이고 운영 catalog/index 병합이 승인되지 않았으므로, 단독으로 운영 catalog/index에 반영하지 않는다.

## QA 판정

최종 QA는 `fail`로 유지한다.

이유:

- 목표 3건 중 1건만 완전한 raw 파일로 남았다.
- 1건은 HTTP 403으로 수집 실패했다.
- 1건은 보안 제품에 의해 격리됐다.
- 운영 catalog/index 병합은 승인 전 범위 밖이다.

## Candidate document_type 판단

이번 pilot은 document_type 설계에는 유효한 관찰을 남겼다.

| 자료 | 현재 document_type | candidate_document_type | 판단 |
|---|---|---|---|
| Q1 2026 earnings press release HTML | `ir-earnings-release` | 없음 | 기존 schema로 충분 |
| Q1 2026 earnings presentation PDF | `ir-deck` | `ir-earnings-presentation` | 반복 등장 여부 관찰 |
| J.P. Morgan Healthcare Conference presentation PDF | `ir-deck` | `ir-investor-conference-presentation` | 반복 등장 여부 관찰 |

아직 schema 확장은 하지 않는다.
`ir-deck`으로 출발하되, earnings presentation과 investor conference presentation이 여러 회사에서 반복되면 별도 타입 추가를 검토한다.

## IR collector에 반영할 원칙

보안 제품이 IR raw 파일을 격리하거나 삭제한 경우:

1. 해당 파일을 복구하거나 열지 않는다.
2. 해당 파일은 운영 catalog/index에 승격하지 않는다.
3. download-log는 보존한다.
4. run-summary와 qa에는 `security_quarantined` 또는 그에 준하는 상태를 남긴다.
5. 필요하면 보안 제품명, 탐지 ID, 탐지 경로를 notes 또는 사후 메모에 기록한다.
6. 후속 처리는 사용자 승인과 별도 보안 검토 뒤에만 진행한다.

## 다음 단계 제안

NTRA pilot 2를 바로 운영 반영하지 않는다.

권장 순서:

1. IR collector에 보안 격리 파일 처리 원칙을 최소 문구로 추가한다.
2. NTRA는 현 상태를 실패 사례로 보존한다.
3. 같은 NTRA 자료를 재시도하기 전, HTML 403 문제와 Bitdefender 격리 문제를 분리해 판단한다.
4. IR taxonomy 판단은 다른 회사 preflight 또는 pilot을 1~2건 더 본 뒤 결정한다.

## 결론

NTRA pilot 2는 수집 성공 사례는 아니지만, 하네스 검증 체계가 제대로 작동한다는 강한 사례다.

특히 download-log 성공만 믿지 않고 실제 local_path 존재 여부를 QA에서 확인했기 때문에, 격리되어 사라진 파일을 운영 catalog에 잘못 승격하는 일을 막았다.
