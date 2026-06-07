# Global Harness Security Baseline Template v1

- 작성일: 2026-06-07
- 기반: `global-harness-security-baseline-template-v0.md`와 Phase 6 security-baseline scope note
- 기준 문서: `global-harness-v5-phase6-security-baseline-scope-note-2026-06-07.md`
- 상태: 전역 하네스 후보 module. 아직 전역 `harness-lab` 반영 아님.

## 1. 목적

이 template는 새 하네스가 secret, credential, `.env`, 비공개 대화 원문, 보안 격리 파일, 외부 공개 같은 반복적인 보안 실수를 피하도록 돕는 최소 안전선이다.

목표는 보안 운영 매뉴얼을 만드는 것이 아니다.
모든 하네스가 최소한 "무심코 노출하지 않고, 위험하면 멈추고, 사용자에게 보이게 알리는" 기준을 갖게 하는 것이다.

핵심 문장:

```text
security-baseline은 보안 사고 대응 매뉴얼이 아니라,
하네스 작업 중 AI가 넘지 말아야 할 최소 안전선이다.
```

## 2. 범위와 비범위

이 module이 다루는 것:

- secret, credential, API key, token, password, session cookie, private credential 노출 방지
- `.env`, credential 파일, 로컬 설정, private checkpoint, private raw/input의 git 제외 후보
- 보안 격리 파일 열람, 복구, 운영 산출물 승격 금지
- 비공개 대화 원문과 compact checkpoint의 경계
- 외부 공개, 업로드, 배포 전 보안 확인 필요성
- 보안 이슈 발견 시 자동 해결 금지
- 사용자에게 즉시 보이는 알림 원칙
- `approval-gate`, `qa-scaffold`와의 연결 기준

이 module이 다루지 않는 것:

- 조직 보안 정책 전체
- 비밀값 회전, 폐기, 사고 대응 절차
- 모든 민감 정보 패턴과 모든 파일명의 exhaustive list
- 하네스별 raw data 공개/비공개 정책
- adapter별 modal, toast, CLI 출력, CI failure message 구현
- 법무 검토, compliance, 배포 보안 심사 전체

하네스별 세부 보안 절차는 각 하네스의 contract, runbook, MANIFEST, `.gitignore`, security note에서 정한다.

## 3. 최소 안전선 3층 분류

security-baseline은 안전선을 세 강도로 나눈다.

| 층위 | 의미 | 적용 방식 |
|---|---|---|
| 절대 금지 | 조건 없이 전역 적용해야 하는 금지선 | 짧고 강하게 명시한다. |
| 조건부 원칙 | 하네스 성격에 따라 세부 기준이 달라지는 항목 | 전역 원칙과 판단 질문만 제공한다. |
| 인터페이스 원칙 | 다른 module이나 adapter로 넘기는 연결 규칙 | 연결 대상과 넘길 정보를 명시한다. |

이 구분은 secret 노출 금지처럼 모든 하네스에 적용할 항목과, raw data 공개 범위처럼 하네스별 판단이 필요한 항목을 섞지 않기 위해 둔다.

## 4. 절대 금지 항목

아래 항목은 전역 최소 금지선이다.

| 금지 항목 | 기본 처리 |
|---|---|
| secret, credential, API key, token, password, session cookie, private credential을 코드, docs, artifacts에 직접 쓰기 | 쓰지 않는다. 발견하면 값은 표시하지 않고 멈춘다. |
| 보안 제품이 격리한 파일을 열기 | 열지 않는다. |
| 보안 격리 파일을 복구하거나 재시도하기 | 복구하지 않는다. 사용자 확인과 별도 보안 검토 전까지 진행하지 않는다. |
| 보안 격리 파일을 운영 산출물로 승격하기 | 승격하지 않는다. |
| 비공개 대화 원문을 무심코 원문 그대로 저장하기 | 원문 저장을 피한다. 필요하면 별도 승인과 제외 경로를 둔다. |
| 보안 이슈 발견 후 자동 삭제, 이동, 복구, 덮어쓰기, 공개 수행 | 자동 해결하지 않는다. 발견, 기록, 멈춤, 사용자 알림, 확인 순서를 따른다. |

secret 값은 로그, 경고, QA finding, run summary, docs에도 그대로 쓰지 않는다.
위치나 유형은 기록할 수 있지만, 값 자체는 표시하지 않는다.

## 5. 조건부 원칙

아래 항목은 전역에서 방향을 제시하되, 최종 기준은 하네스별로 정한다.

| 항목 | 전역 원칙 | 하네스별로 정할 것 |
|---|---|---|
| `.gitignore` | credential, 로컬 설정, private checkpoint, private raw/input은 제외 후보로 검토한다. | 실제 파일명 세트 |
| 공개 저장소 | 공개 전 민감 정보 가능성을 확인하고 approval-gate를 거친다. | 공개 가능 파일, 비공개 파일, 승인자 |
| 비공개 저장소 | 비공개라도 secret 직접 저장은 허용하지 않는다. | 내부 공유 범위와 보호 경로 |
| raw data | 전역에서 일괄 삭제하거나 일괄 비공개로 단정하지 않는다. | raw data 보관 위치, 공개 범위, git 제외 여부 |
| compact checkpoint | 원문 전체가 아니라 요약 checkpoint는 허용 가능하다. | checkpoint 작성 방식, 저장 위치, 공개 여부 |
| 외부 업로드/배포 | 공개 범위와 민감 정보 가능성을 확인한다. | 배포 절차와 승인 단계 |

비공개 저장소라고 해서 secret을 저장해도 되는 것은 아니다.
공개 저장소라고 해서 모든 raw data를 무조건 금지하는 것도 아니다.

## 6. 하네스별로 정할 보안 항목

각 하네스가 직접 정해야 하는 항목:

- 보호해야 할 파일과 경로
- secret, credential, local config의 실제 파일명
- `.gitignore`의 최종 패턴
- private raw/input의 정의
- raw data의 보관 위치와 공개 범위
- 공개 저장소와 비공개 저장소 운영 기준
- 외부 업로드, 공개, 제출, 배포에 해당하는 행동
- 보안 검토가 필요한 runbook 단계
- 보안 이슈 발생 시 사용자 승인자 또는 승인 위치

전역 template는 이 항목을 단정하지 않는다.
각 하네스의 `harness/ORCHESTRATOR.md`, `harness/MANIFEST.md`, contract, runbook, security note에서 구체화한다.

## 7. 대화 원문, checkpoint, raw data 경계

대화 원문, checkpoint, raw data는 서로 다른 기준으로 다룬다.

| 항목 | 전역 판단 | 하네스별 판단 |
|---|---|---|
| 비공개 대화 원문 | 원문 그대로 저장하지 않는 것을 기본 원칙으로 둔다. 저장해야 한다면 별도 승인과 제외 경로가 필요하다. | 어떤 대화가 기록 대상인지, 저장 위치가 있는지 |
| compact checkpoint | 원문 전체가 아니라 요약 checkpoint는 허용 가능하다. 단 민감 정보가 들어갈 수 있으면 git 제외 후보로 본다. | checkpoint 작성 방식, 저장 위치, 공개 여부 |
| raw data | 전역에서 일괄 금지하거나 삭제하지 않는다. | 도메인별 raw data 공개 범위, 보관 위치, git 제외 여부 |

raw data는 도메인 의존성이 크다.
수집형 하네스의 raw 파일, 문서형 하네스의 초안, 이미지 하네스의 입력 파일, 로그 기반 하네스의 원시 로그는 모두 다르게 다뤄야 한다.

따라서 각 하네스는 raw data가 무엇인지, 다음 하네스가 읽어야 하는지, 공개 가능한지, git에 포함할지 직접 명시한다.

## 8. `.gitignore` 카테고리와 예시

전역 v1은 `.gitignore`의 exhaustive list를 제공하지 않는다.
아래는 검토해야 할 카테고리와 짧은 예시다.

| 카테고리 | 예시 | 비고 |
|---|---|---|
| credential 파일 | `.env`, `.env.*`, `*.pem`, `*.key`, `*.secret` | 예시일 뿐 최종 세트가 아님 |
| 로컬 설정 | local config, machine-specific settings | 도구별 파일명은 하네스별 결정 |
| session/checkpoint | session checkpoint, compact handoff, private conversation summary | 공개 여부와 원문 포함 여부에 따라 결정 |
| private raw/input | private user files, restricted raw data | 도메인별 판단 필요 |
| security quarantine | quarantined or recovered files | 열람, 복구, commit 금지 후보 |

원칙:

- `.gitignore`는 하네스별로 확정한다.
- 전역 v1은 무엇을 검토해야 하는지 알려준다.
- 전역 v1은 모든 언어, 모든 도구, 모든 secret 파일명을 열거하지 않는다.
- 예시 파일명을 반드시 이 세트만 쓰라는 뜻으로 읽지 않는다.

## 9. approval-gate 연결

security-baseline은 보안 위험을 분류한다.
approval-gate는 승인 수준과 승인 절차를 결정한다.

| 상황 | security-baseline 역할 | approval-gate 역할 |
|---|---|---|
| `.env` 또는 credential 파일을 읽어야 할 가능성 | 민감 파일로 분류하고 별도 주의 필요 표시 | read-only라도 별도 승인 대상인지 판단 |
| 보안 격리 파일 접근 요청 | 열람/복구 금지 원칙 제시 | 위험 작업으로 보고 중단 또는 명시 승인 요구 |
| 외부 공개, 업로드, 배포 | 민감 정보 점검 필요 표시 | 공개/배포 승인 절차 수행 |
| secret 노출 의심 | 실제 값 노출 없이 보안 검토 필요 표시 | 작업 중단, 사용자 승인, 다음 조치 결정 |

security-baseline이 approval-gate를 대신하지 않는다.
security-baseline은 "이건 보안 위험이다"를 말하고, approval-gate는 "그래서 어떤 승인 단계가 필요한가"를 정한다.

## 10. qa-scaffold 연결

qa-scaffold는 QA 결과 구조를 제공한다.
security-baseline은 보안 기준을 제공한다.

| 상황 | qa-scaffold 역할 | security-baseline 역할 |
|---|---|---|
| secret 의심 finding 발견 | `security_review_needed` 또는 violation으로 표시 | secret 값 노출 금지와 다음 조치 기준 제공 |
| 외부 공개 전 QA | 공개 전 확인 항목으로 기록 | 민감 정보 점검 원칙 제공 |
| 격리 파일 또는 private conversation issue | finding/evidence에 안전한 참조만 기록 | 열람/복구 금지와 사용자 알림 원칙 제공 |
| 보안 문제로 후속 사용이 위험함 | `stop_required`나 `unverified/fail` 후보로 표시 | 자동 해결 금지와 approval-gate 연결 기준 제공 |

security-baseline v1은 QA result schema를 만들지 않는다.
QA 결과 파일 구조, finding table, escalation 필드는 qa-scaffold가 맡는다.

security-baseline은 보안 항목을 QA 실패 조건으로 직접 확정하지 않는다.
어떤 보안 이슈가 fail, unverified, stopped로 이어지는지는 하네스별 QA procedure와 approval-gate 판단이 함께 결정한다.

## 11. do-not-proceed 조건

아래 상황이 발견되면 계속 진행하지 않는다.

- secret, credential, API key, token, password, session cookie, private credential이 파일, docs, artifacts에 노출된 것으로 보인다.
- credential, 비공개 대화 원문, 보안 격리 파일을 읽어야 할 가능성이 생겼다.
- 보안 격리 파일의 열람, 복구, 재시도, 운영 산출물 승격을 요청받았다.
- 외부 공개, 업로드, 배포, 저장소 공개 전 민감 정보 가능성이 확인되지 않았다.
- 승인되지 않은 공개 공유나 외부 제출 가능성이 생겼다.
- 보안 이슈를 발견했지만 사용자 확인 없이 해결해야 할 것처럼 보인다.

보안 이슈를 발견했을 때 좋은 의도로 자동 해결하지 않는다.

금지:

- 자동 삭제
- 자동 이동
- 자동 복구
- 자동 덮어쓰기
- 자동 공개/비공개 전환
- secret 값을 포함한 자세한 출력

기본 순서:

```text
발견
-> 값 노출 없이 안전하게 기록
-> 멈춤
-> 사용자가 즉시 볼 수 있는 채널로 알림
-> 사용자 확인 후 다음 조치 결정
```

자동 삭제나 덮어쓰기는 증거를 없애거나 더 큰 노출을 만들 수 있다.
보안 격리 파일 복구는 더 위험할 수 있다.

## 12. 사용자 알림 원칙

silent log만 남기는 것은 충분하지 않다.
사용자가 즉시 볼 수 있는 채널로 알려야 한다.

가능한 채널:

- Codex/Claude Code 대화 메시지
- CLI console 출력
- CI failure message
- app modal, toast, banner
- 하네스별 run summary의 명시적 blocked section

알림 원칙:

1. 실제 secret 값을 경고에 포함하지 않는다.
2. 유형과 안전한 위치 또는 참조만 알린다.
3. 수행하지 않은 조치, 예를 들어 삭제, 이동, 복구, 덮어쓰기, 공개를 하지 않았음을 알린다.
4. 필요한 다음 조치와 사용자 확인 지점을 알린다.
5. 구체적 경고 형식은 adapter 또는 하네스별 UI가 정한다.

이 module은 경고 메시지의 고정 format을 강제하지 않는다.
Codex, Claude Code, CLI, CI, app UI는 실행 환경에 맞는 형식을 사용하되 위 원칙을 지켜야 한다.

## 13. 적용 전 체크리스트

새 하네스에 이 module을 적용하기 전에 확인한다.

- [ ] secret, credential, API key, token, password, session cookie, private credential을 코드, docs, artifacts에 직접 쓰지 않기로 했다.
- [ ] `.env`, credential 파일, 로컬 설정, private checkpoint, private raw/input의 git 제외 후보를 검토했다.
- [ ] `.gitignore`의 최종 파일명 세트는 하네스별로 확정한다.
- [ ] 보안 격리 파일은 열지 않고, 복구하지 않고, 운영 산출물로 승격하지 않기로 했다.
- [ ] 비공개 대화 원문과 compact checkpoint의 저장 기준을 구분했다.
- [ ] raw data의 보관 위치, 공개 범위, git 제외 여부를 하네스별로 정했다.
- [ ] 외부 공개, 업로드, 제출, 배포 전 approval-gate를 거치기로 했다.
- [ ] QA에서 보안 의심 항목이 발견되면 `security_review_needed` 또는 violation으로 표시한다.
- [ ] 보안 이슈 발견 시 자동 삭제, 이동, 복구, 덮어쓰기, 공개를 하지 않는다.
- [ ] 보안 이슈는 값 노출 없이 기록하고, 멈추고, 사용자가 즉시 볼 수 있는 채널로 알린다.
- [ ] 실제 secret 값을 경고, 로그, QA finding, run summary, docs에 포함하지 않는다.
- [ ] adapter별 경고 UI와 승인 요청 방식은 adapter 영역에 둔다.
- [ ] 조직 보안 정책, secret 회전, 사고 대응, 법무 검토는 하네스별 또는 조직별 절차로 둔다.
