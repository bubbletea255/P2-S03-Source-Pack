# Global Harness v5 Phase 6 Security Baseline Scope Note

- 작성일: 2026-06-07
- 상태: Phase 6 security-baseline v1 후보 작성 전 scope note. module template 본문이 아니다.
- 대상: `global-harness-security-baseline-template-v0.md`
- 기준 문서:
  - `global-harness-core-structure-template-v1.md`
  - `global-harness-v5-phase4-hook-cross-check-2026-06-06.md`
  - `global-harness-v5-phase6-module-diagnostic-note-2026-06-06.md`
  - `global-harness-approval-gate-template-v1.md`
  - `global-harness-qa-scaffold-template-v1.md`

## 1. 목적

이 note는 `security-baseline` v1 후보에 무엇을 직접 담고, 무엇을 하네스별 보안 절차, adapter, approval/QA module로 넘길지 정리한다.

핵심 질문:

```text
security-baseline은 모든 하네스가 따라야 할 최소 안전선인가,
아니면 프로젝트별 보안 운영 매뉴얼인가?
```

현재 판단은 전자다.

`security-baseline`은 secret, credential, `.env`, 비공개 대화 원문, 보안 격리 파일, 외부 공개 같은 흔한 실수를 막는 최소 안전선이다.
침해 대응, 키 회전, 조직 보안 정책, 법무 검토, 도메인별 raw data 공개 정책 전체를 전역에서 정의하지 않는다.

## 2. scope 작게 유지 원칙

이 module은 일부러 작아야 한다.

보안 항목은 범위를 넓히기 쉽다.
하지만 `security-baseline`이 너무 커지면 v5 module이 아니라 보안 운영 매뉴얼이 된다.

따라서 v1은 아래 범위만 맡는다.

- AI가 하네스 작업 중 실수로 secret이나 민감 자료를 노출하지 않게 한다.
- 읽기만 해도 위험할 수 있는 파일을 구분하게 한다.
- 외부 공개, 배포, 업로드 전 승인 필요성을 표시한다.
- 보안 이슈를 발견했을 때 자동 해결하지 않고 멈추게 한다.
- `approval-gate`와 `qa-scaffold`가 보안 이슈를 어디로 넘길지 알려준다.

v1이 맡지 않을 것:

- 조직 보안 정책 전체
- 비밀값 회전, 폐기, 사고 대응 절차
- 모든 파일명과 모든 민감 정보 패턴의 exhaustive list
- 하네스별 raw data 공개/비공개 정책
- adapter별 경고 UI 구현

## 3. 3층 분류

security-baseline v1은 안전선을 세 강도로 나눈다.

| 층위 | 의미 | v1에서 다루는 방식 |
|---|---|---|
| 절대 금지 | 조건 없이 전역 적용해야 하는 금지선 | 짧고 강하게 명시 |
| 조건부 원칙 | 하네스 성격에 따라 세부 기준이 달라지는 항목 | 전역 원칙과 판단 질문만 제공 |
| 인터페이스 원칙 | 다른 module이나 adapter로 넘기는 연결 규칙 | 연결 대상과 넘길 정보를 명시 |

이 구분을 두는 이유는 모든 보안 문구가 같은 강도로 읽히는 것을 막기 위해서다.

예를 들어 secret을 docs에 직접 쓰지 않는 것은 절대 금지에 가깝다.
반면 raw data를 git에서 제외할지는 하네스별 도메인과 공개 범위에 따라 달라진다.

## 4. v1에 직접 담을 것

### 4.1 절대 금지

| 항목 | v1 포함 이유 |
|---|---|
| secret, credential, API key, token, password, session cookie, private credential을 코드, docs, artifacts에 직접 쓰지 않는다 | 모든 하네스에 공통으로 적용되는 최소 안전선 |
| 보안 제품이 격리한 파일은 열지 않고, 복구하지 않고, 운영 산출물로 승격하지 않는다 | 읽기나 복구 자체가 위험할 수 있음 |
| 비공개 대화 원문을 무심코 원문 그대로 저장하지 않는다 | 프라이버시와 재사용 위험이 큼 |
| 보안 이슈 발견 후 자동 삭제, 이동, 복구, 덮어쓰기, 공개를 수행하지 않는다 | 증거 훼손과 추가 노출을 막기 위함 |

### 4.2 조건부 원칙

| 항목 | v1 포함 방식 |
|---|---|
| `.gitignore` 후보 | 카테고리와 짧은 예시만 제공. 최종 파일명 세트는 하네스별로 확정 |
| 공개 저장소와 비공개 저장소 차이 | 외부 공개 전 승인 필요와 민감 정보 점검 원칙만 제공 |
| raw data 공개/제외 | 전역에서 일괄 금지하지 않고 하네스별 contract, MANIFEST, security note로 위임 |
| 대화 checkpoint 저장 | compact checkpoint는 허용 가능하지만 원문 전체 저장은 피하고, git 제외 여부는 하네스별로 판단 |

### 4.3 인터페이스 원칙

| 연결 대상 | security-baseline의 역할 |
|---|---|
| `approval-gate` | 위험 분류와 승인 필요성을 넘긴다. 실제 승인 수준과 승인 방식은 approval-gate가 결정한다. |
| `qa-scaffold` | QA가 발견한 보안 의심 항목을 `security_review_needed`나 violation으로 넘길 기준을 제공한다. |
| adapter/UI | 사용자가 즉시 볼 수 있는 경고 채널을 구현한다. 구체 형식은 adapter가 정한다. |
| 하네스별 runbook/contract | raw data, 공개 범위, 보호 파일 목록을 하네스별로 확정한다. |

## 5. 하네스별로 둘 것

아래 항목은 `security-baseline` v1에 직접 고정하지 않는다.

| 항목 | 둘 위치 | 이유 |
|---|---|---|
| 보호 파일과 민감 경로의 실제 목록 | 하네스별 `harness/MANIFEST.md`, runbook, security note | 프로젝트마다 다름 |
| `.gitignore`의 최종 파일명 세트 | 하네스별 `.gitignore` | 언어, 도구, 산출물 구조마다 다름 |
| raw data 공개/비공개 정책 | 하네스별 contract, MANIFEST, runbook | 도메인별 차이가 큼 |
| 공개 저장소 운영 정책 | 프로젝트별 운영 정책 | 법무, 조직, 배포 환경과 연결됨 |
| secret 회전, 폐기, 사고 대응 | 조직/프로젝트 보안 절차 | 전역 harness template의 범위를 넘음 |
| adapter별 경고창, modal, CLI 출력 구현 | Codex/Claude Code/CLI/CI/app adapter | 실행 환경마다 다름 |

## 6. 대화 원문, checkpoint, raw data 경계

세 항목은 같은 "자료"처럼 보이지만 전역 처리 기준이 다르다.

| 항목 | 전역 판단 | 하네스별 판단 |
|---|---|---|
| 비공개 대화 원문 | 원문 그대로 저장하지 않는 것을 기본 원칙으로 둔다. 저장해야 한다면 별도 승인과 제외 경로가 필요하다. | 어떤 대화가 기록 대상인지, 저장 위치가 있는지 |
| compact checkpoint | 원문 전체가 아니라 요약 checkpoint는 허용 가능하다. 단 민감 정보가 들어갈 수 있으면 git 제외 후보로 본다. | checkpoint 작성 방식, 저장 위치, 공개 여부 |
| raw data | 전역에서 일괄 금지하거나 삭제하지 않는다. | 도메인별 raw data 공개 범위, 보관 위치, git 제외 여부 |

Source Pack의 `artifacts/raw/` 같은 구조는 Source Pack 고유 사례다.
다른 하네스에서는 raw data가 이미지, 문서, 로그, 모델 입력, 사용자 파일 등 전혀 다른 의미를 가질 수 있다.

따라서 v1은 raw data를 "항상 금지"하지 않는다.
대신 각 하네스가 raw data의 공개 범위와 보관 정책을 명시하도록 요구한다.

## 7. `.gitignore` 카테고리와 예시

v1은 `.gitignore`의 exhaustive list를 제공하지 않는다.
전역에서 제공할 것은 카테고리와 짧은 예시다.

| 카테고리 | 예시 | 비고 |
|---|---|---|
| credential 파일 | `.env`, `.env.*`, `*.pem`, `*.key`, `*.secret` | 예시일 뿐 최종 세트가 아님 |
| 로컬 설정 | local config, machine-specific settings | 도구별 파일명은 하네스별 결정 |
| session/checkpoint | session checkpoint, compact handoff, private conversation summary | 공개 여부와 원문 포함 여부에 따라 결정 |
| private raw/input | private user files, restricted raw data | 도메인별 판단 필요 |
| security quarantine | quarantined or recovered files | 열람, 복구, commit 금지 후보 |

원칙:

- `.gitignore`는 하네스별로 확정한다.
- 전역 v1은 "무엇을 검토해야 하는가"를 알려준다.
- 전역 v1은 모든 언어, 모든 도구, 모든 secret 파일명을 열거하지 않는다.
- 예시 파일명을 "반드시 이 세트만 쓰라"는 의미로 읽지 않는다.

## 8. approval-gate 연결

`security-baseline`은 위험을 분류한다.
`approval-gate`는 승인 수준과 승인 절차를 결정한다.

경계:

| 상황 | security-baseline 역할 | approval-gate 역할 |
|---|---|---|
| `.env` 또는 credential 파일을 읽어야 할 가능성 | 민감 파일로 분류하고 별도 주의 필요 표시 | read-only라도 별도 승인 대상인지 판단 |
| 보안 격리 파일 접근 요청 | 열람/복구 금지 원칙 제시 | 위험 작업으로 보고 중단 또는 명시 승인 요구 |
| 외부 공개, 업로드, 배포 | 민감 정보 점검 필요 표시 | 공개/배포 승인 절차 수행 |
| secret 노출 의심 | 실제 값 노출 없이 보안 검토 필요 표시 | 작업 중단, 사용자 승인, 다음 조치 결정 |

중요:

security-baseline이 approval-gate를 대신하지 않는다.
security-baseline은 "이건 보안 위험이다"를 말하고, approval-gate는 "그래서 어떤 승인 단계가 필요한가"를 정한다.

## 9. qa-scaffold 연결

`qa-scaffold`는 QA 결과 구조를 제공한다.
`security-baseline`은 보안 기준을 제공한다.

경계:

| 상황 | qa-scaffold 역할 | security-baseline 역할 |
|---|---|---|
| secret 의심 finding 발견 | `security_review_needed` 또는 violation으로 표시 | secret 값 노출 금지와 다음 조치 기준 제공 |
| 외부 공개 전 QA | 공개 전 확인 항목으로 기록 | 민감 정보 점검 원칙 제공 |
| 격리 파일 또는 private conversation issue | finding/evidence에 안전한 참조만 기록 | 열람/복구 금지와 사용자 알림 원칙 제공 |
| 보안 문제로 후속 사용이 위험함 | `stop_required`나 `unverified/fail` 후보로 표시 | 자동 해결 금지와 approval-gate 연결 기준 제공 |

security-baseline v1은 QA result schema를 만들지 않는다.
QA 결과 파일 구조, finding table, escalation 필드는 `qa-scaffold`가 맡는다.

security-baseline은 보안 항목을 QA 실패 조건으로 직접 확정하지 않는다.
어떤 보안 이슈가 fail, unverified, stopped로 이어지는지는 하네스별 QA procedure와 approval-gate 판단이 함께 결정한다.

## 10. do-not-proceed 조건과 사용자 알림 원칙

### 10.1 do-not-proceed 조건

아래 상황이 발견되면 계속 진행하지 않는다.

- secret, credential, API key, token, password, session cookie, private credential이 파일, docs, artifacts에 노출된 것으로 보인다.
- credential, 비공개 대화 원문, 보안 격리 파일을 읽어야 할 가능성이 생겼다.
- 보안 격리 파일의 열람, 복구, 재시도, 운영 산출물 승격을 요청받았다.
- 외부 공개, 업로드, 배포, 저장소 공개 전 민감 정보 가능성이 확인되지 않았다.
- 승인되지 않은 공개 공유나 외부 제출 가능성이 생겼다.
- 보안 이슈를 발견했지만 사용자 확인 없이 해결해야 할 것처럼 보인다.

### 10.2 자동 해결 금지

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

### 10.3 사용자 알림 원칙

silent log만 남기는 것은 충분하지 않다.
사용자가 즉시 볼 수 있는 채널로 알려야 한다.

가능한 채널 예:

- Codex/Claude Code 대화 메시지
- CLI console 출력
- CI failure message
- app modal, toast, banner
- 하네스별 run summary의 명시적 blocked section

v1에 들어갈 원칙:

1. 실제 secret 값을 경고에 포함하지 않는다.
2. 유형과 안전한 위치 또는 참조만 알린다.
3. 수행하지 않은 조치, 예를 들어 삭제, 이동, 복구, 덮어쓰기, 공개를 하지 않았음을 알린다.
4. 필요한 다음 조치와 사용자 확인 지점을 알린다.
5. 구체적 경고 형식은 adapter 또는 하네스별 UI가 정한다.

scope note에만 둘 권장 형식 예시:

```text
보안 검토 필요: 민감 정보로 보이는 값이 발견됐습니다.
위치: {파일 경로 또는 안전한 참조}
내용: 값은 표시하지 않음
현재 조치: 삭제/이동/복구/덮어쓰기/공개를 수행하지 않음
필요한 확인: 사용자 승인 후 다음 조치 결정
```

이 예시는 adapter 구현을 고정하지 않는다.
v1 본문에는 원칙만 남기고, 실제 메시지 형식은 Codex, Claude Code, CLI, CI, app UI가 각자 정한다.

## 11. v1 권장 목차

`global-harness-security-baseline-template-v1.md`를 만든다면 아래 목차를 권장한다.

1. 목적
2. 범위와 비범위
3. 최소 안전선 3층 분류
4. 절대 금지 항목
5. 조건부 원칙
6. 하네스별로 정할 보안 항목
7. 대화 원문, checkpoint, raw data 경계
8. `.gitignore` 카테고리와 예시
9. approval-gate 연결
10. qa-scaffold 연결
11. do-not-proceed 조건
12. 사용자 알림 원칙
13. 적용 전 체크리스트

v1 파일에는 아래 판단을 반영한다.

- 경고 형식은 고정 format이 아니라 원칙으로 둔다.
- 권장 형식 예시는 scope note에만 둔다.
- 실행에 필요한 내용은 v1 template로 승격한다.
- 판단 근거와 작업 이력은 scope note 또는 archive에 둔다.
- 작업 진행 상태는 work map에 둔다.

다음 결정:

```text
이 scope note를 Claude Code와 교차검증한 뒤,
global-harness-security-baseline-template-v1.md 후보 파일을 만들지 결정한다.
```

현재 권장안은 "교차검증 후 만든다"이다.

이유:

- v0는 방향은 맞지만 최소 안전선의 강도 구분이 얇다.
- approval-gate와 qa-scaffold v1이 이미 보안 escalation 연결을 갖고 있다.
- security-baseline도 v1으로 맞춰야 module registry의 현재 후보군이 균형을 갖는다.
