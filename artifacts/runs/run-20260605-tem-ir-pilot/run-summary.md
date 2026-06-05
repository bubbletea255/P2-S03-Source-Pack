# Source Pack 실행 요약

## 실행 정보
- 실행 모드: test_collection
- 대상 티커: TEM
- run-id: run-20260605-tem-ir-pilot
- 설정 파일: config.md
- 회사별 index: 생성하지 않음
- catalog 원장: 운영 `documents.jsonl`, `files.jsonl`, `runs.jsonl` 병합 없음. `ir-taxonomy-candidates.jsonl`에 후보 관찰 1건 기록
- raw 저장: artifacts/raw/company-ir/TEM/
- derived/text 생성: 미실행
- run_scope: test only: TEM company-ir official Tempus AI Q1 2026 earnings release HTML, Q1 2026 Overview PDF, and Tempus 1Q26 Corporate Deck PDF; no SEC download; no SEC filings page harvesting; no webcast/audio/video; no transcript; no archive-wide crawling; no operating catalog/index merge until user approval

## 결과
| 티커 | 상태 | 문서 원장 | 파일 원장 | raw 파일 | 보안 격리 | derived/text | Transcript | IR 자료 | QA |
|---|---|---:|---:|---:|---:|---:|---|---|---|
| TEM | 부분 성공 | 0 | 0 | 3 | 0 | 0 | out_of_scope | 3/3 collected run-local | partial_pass |

해석:

```text
이번 run은 운영 catalog/index 병합 금지 조건이 있었으므로 documents/files/runs 원장에는 반영하지 않았다.
raw 파일 3건은 run-local pilot 산출물로 저장했고, 사용자 승인 전까지 후속 하네스의 확정 입력으로 쓰지 않는다.
```

## 대상 자료별 결과
| document_id | document_type | 대상 | 상태 | 근거 |
|---|---|---|---|---|
| ir-tem-earnings-release-fy2026-q1 | ir-earnings-release | Tempus Reports First Quarter 2026 Results HTML | collected_run_local | local_path 존재, 377256 bytes, SHA-256 검증, HTML title 확인 |
| ir-tem-financial-supplement-fy2026-q1-overview | ir-financial-supplement | Q1 2026 Overview PDF | collected_run_local | local_path 존재, 317921 bytes, SHA-256 검증, PDF header 확인 |
| ir-tem-deck-fy2026-q1-corporate-deck | ir-deck | Tempus 1Q26 Corporate Deck PDF | collected_run_local | local_path 존재, 7730240 bytes, SHA-256 검증, PDF header 확인 |

## Run-local 수집 카운트
| 티커 | raw_collected | raw_failed | security_quarantined | webcast/audio/video | transcript |
|---|---:|---:|---:|---|---|
| TEM | 3 | 0 | 0 | out_of_scope | out_of_scope |

## 운영 Catalog 기준 Incremental Sync 카운트
| 티커 | collected_new | skipped_existing | repair_required | failed | files_collected_new |
|---|---:|---:|---:|---:|---:|
| TEM | 0 | 0 | 0 | 0 | 0 |

주의: 운영 catalog/index 병합은 사용자 승인 전까지 수행하지 않는다.

## SEC overlap 확인
| IR document_id | SEC overlap 상태 | 확인 범위 | hash 동일 여부 | 처리 |
|---|---|---|---|---|
| ir-tem-earnings-release-fy2026-q1 | not_found_in_local_catalog | existing_local_catalog_files_raw_only | false | TEM SEC 후보가 로컬 catalog에 없어 company-ir raw만 run-local로 보존 |
| ir-tem-financial-supplement-fy2026-q1-overview | not_found_in_local_catalog | existing_local_catalog_files_raw_only | false | TEM SEC 후보가 로컬 catalog에 없어 company-ir raw만 run-local로 보존 |
| ir-tem-deck-fy2026-q1-corporate-deck | not_found_in_local_catalog | existing_local_catalog_files_raw_only | false | TEM SEC 후보가 로컬 catalog에 없어 company-ir raw만 run-local로 보존 |

해석 주의:

```text
not_found_in_local_catalog는 "SEC 중복 없음 확정"이 아니다.
현재 로컬 catalog/files/raw 안에서 같은 hash 또는 TEM SEC 후보를 찾지 못했다는 뜻이다.
SEC 다운로드와 SEC filings page harvesting은 수행하지 않았다.
```

## candidate_document_type 판단
| 자료 | 현재 document_type | candidate 판단 | 처리 |
|---|---|---|---|
| Tempus 1Q26 Corporate Deck PDF | ir-deck | candidate_document_type: ir-earnings-presentation | `artifacts/catalog/ir-taxonomy-candidates.jsonl`에 후보 관찰 1건 기록 |

판단:

```text
Corporate Deck은 기존 schema에서는 ir-deck으로 처리 가능하다.
다만 Q1 earnings call supporting material이므로 ir-earnings-presentation 후보로 관찰할 가치가 있다.
NTRA/APP/TEM 맥락에서 반복 관찰됐으므로 사용자 승인 기반 taxonomy review 후보로 남긴다.
schema는 자동 변경하지 않았다.
```

## 보안 격리 확인
| 파일 | 상태 | 근거 |
|---|---|---|
| earnings release HTML | not_quarantined_observed | local_path 존재, size/hash 검증 성공 |
| Q1 2026 Overview PDF | not_quarantined_observed | local_path 존재, PDF header, size/hash 검증 성공 |
| Tempus 1Q26 Corporate Deck PDF | not_quarantined_observed | local_path 존재, PDF header, size/hash 검증 성공 |

NTRA pilot 2와 같은 Bitdefender 격리 현상은 이번 TEM pilot에서 관찰되지 않았다.

## 주요 산출물
| 티커 | 파일 | 경로 |
|---|---|---|
| TEM | download-log | artifacts/runs/run-20260605-tem-ir-pilot/download-log.jsonl |
| TEM | run-summary | artifacts/runs/run-20260605-tem-ir-pilot/run-summary.md |
| TEM | qa | artifacts/runs/run-20260605-tem-ir-pilot/qa.md |
| TEM | taxonomy candidate ledger | artifacts/catalog/ir-taxonomy-candidates.jsonl |
| TEM | earnings release raw | artifacts/raw/company-ir/TEM/2026-05-05_fy2026-q1-earnings-release/document.html |
| TEM | Q1 overview raw | artifacts/raw/company-ir/TEM/2026-05-05_fy2026-q1-overview/document.pdf |
| TEM | Q1 corporate deck raw | artifacts/raw/company-ir/TEM/2026-05-05_fy2026-q1-corporate-deck/document.pdf |

## 실패와 확인 필요
| 티커 | 항목 | 상태 | 이유 | 권장 조치 |
|---|---|---|---|---|
| TEM | shell external HTTPS transport | warning | PowerShell/curl external HTTPS download attempts timed out with 0 bytes received | 공식 파일은 Node fetch와 BITS/localhost fallback으로 확보했으므로 raw QA 기준에서는 통과 |
| TEM | SEC overlap | pending_recheck | 현재 local catalog/files/raw에는 TEM SEC 후보가 없음 | TEM SEC 수집 시 같은 ticker 범위에서 SEC file hash와 재비교 |
| TEM | taxonomy | user_review_candidate | `ir-earnings-presentation` 후보가 NTRA/APP/TEM 맥락에서 반복 관찰됨 | 다음 taxonomy checkpoint에서 별도 type 승격 여부 논의 |

## 다음 하네스 입력
| 티커 | 입력 가능 여부 | 이유 |
|---|---|---|
| TEM | 제한적으로 가능 | run-local raw 3건은 검증됨. 단, 운영 catalog/index 미반영 상태이므로 후속 하네스 확정 입력으로 쓰려면 사용자 승인 후 운영 반영 필요 |

## 하네스 운영 관찰

이 섹션은 하네스 감량과 운영 개선을 위한 선택 메모다.
비어 있거나 누락되어도 QA 실패 또는 run 실패로 보지 않는다.

- instructions_files_consulted: 6
- instructions_lines_consulted_estimate: 대략 2200
- bottleneck_note: 공식 사이트 파일은 접근 가능했지만 shell external HTTPS transport가 0 bytes timeout으로 실패해, HTML은 Node fetch plus localhost bridge, PDF는 BITS로 저장해야 했다.
- trim_candidate: IR 파일 다운로드 transport fallback 절차. HTML dynamic page와 static PDF의 전송 방식을 분리한 작은 downloader/checker가 있으면 반복 시간을 줄일 수 있다.

## 다음 단계
- 사용자가 승인하면 TEM run-local raw 3건을 운영 catalog/index에 반영할지 결정한다.
- 운영 반영 전에 TEM SEC overlap은 여전히 `not_found_in_local_catalog` 상태임을 유지한다.
- `ir-earnings-presentation`을 정식 document_type으로 승격할지 다음 taxonomy checkpoint에서 논의한다.
