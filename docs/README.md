# docs 문서 지도

이 폴더는 Source Pack 하네스의 설계 메모, 실행 전 조사, handoff, 템플릿, 참고 PDF를 보관한다.

## 제외 폴더

아래 폴더는 대화 저장과 체크포인트 보관용이므로 문서 정리 대상에서 제외한다.

- `session-checkpoints/`
- `대화 요약/`
- `대화 원본/`

## 현재 구조

| 폴더 | 역할 | 대표 파일 |
|---|---|---|
| `current/` | 현재 상태 지도와 v1 마감 문서 | `source-pack-ir-current-state-map-2026-06-05.md`, `source-pack-ir-v1-closeout-2026-06-05.md`, `ir-taxonomy-checkpoint-2026-06-05.md` |
| `design/` | 구조 설계, taxonomy, SEC-IR 중복/무결성, raw 구조 기준 | `ir-collection-design-notes-2026-06-04.md`, `sec-ir-deduplication-and-integrity-design-2026-06-05.md`, `source-pack-structure-decision-notes-2026-06-04.md` |
| `templates/` | 재사용 템플릿과 관측 가능성 템플릿 | `source-pack-observability-template.md`, `공용_하네스_템플릿_체크리스트_v4.md` |
| `pilots/` | IR preflight와 pilot 사후 메모 | `aapl-ir-preflight-2026-06-04.md`, `ntra-ir-pilot-2-postmortem-2026-06-04.md`, `tem-ir-preflight-2026-06-05.md` |
| `handoff/` | 특정 시점의 대화/작업 인계 문서 | `source-pack-handoff-2026-06-03.md`, `source-pack-ir-dedup-handoff-2026-06-05.md` |
| `reference/` | 외부 참고 PDF와 큰 배경 문서 | `Phase_2_-_Step_3_Source_Pack_IR_사이트_자료_수집_기준_.pdf`, `가치투자_리서치_21단계_구조화_버전.pdf` |

## 먼저 읽을 파일

새 채팅방이나 다른 도구가 현재 상태를 이어받을 때는 아래 순서로 읽는다.

1. `current/source-pack-ir-v1-closeout-2026-06-05.md`
2. `current/source-pack-ir-current-state-map-2026-06-05.md`
3. 필요한 경우 `design/ir-collection-design-notes-2026-06-04.md`
4. SEC-IR 중복/무결성을 다룰 때는 `design/sec-ir-deduplication-and-integrity-design-2026-06-05.md`

## 정리 원칙

- 파일 삭제 없이 이동만 했다.
- handoff 문서 내부의 과거 경로 참조는 역사 기록으로 보고 대부분 유지한다.
- 실행 절차나 현재 기준 문서가 참조하는 경로는 새 폴더 구조에 맞게 갱신한다.
