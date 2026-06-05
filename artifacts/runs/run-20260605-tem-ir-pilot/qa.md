# Source Pack QA - TEM

run_id: run-20260605-tem-ir-pilot
qa_date: 2026-06-05
overall_status: partial_pass

## 요약
- 통과: TEM IR raw 3건을 run-local로 저장했고, local_path 존재, size, SHA-256 hash, PDF header, HTML title을 검증했다.
- 실패: final document failures 0건.
- 경고: shell external HTTPS transport는 실패했으나, 공식 출처 파일은 BITS 또는 Node fetch plus localhost bridge로 확보했다.
- 미반영: 운영 `documents.jsonl`, `files.jsonl`, `runs.jsonl`, `artifacts/companies/TEM/index.md`는 사용자 승인 전까지 수정하지 않았다.
- 사람 승인 필요: `ir-earnings-presentation`을 정식 `document_type`으로 승격할지 taxonomy checkpoint에서 검토할 수 있다. schema 자동 변경은 하지 않았다.

## 1단계: run 범위 확인
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| run 폴더 존재 | pass | artifacts/runs/run-20260605-tem-ir-pilot 존재 | 없음 |
| run_scope 준수 | pass | 승인된 TEM IR HTML 1건과 PDF 2건만 수집 | 없음 |
| 금지 범위 | pass | SEC 다운로드, SEC filings harvesting, webcast/audio/video, transcript, archive-wide crawling 없음 | 없음 |
| 운영 catalog/index 병합 | pass | documents/files/runs/index 미수정 | 운영 반영은 사용자 승인 후 별도 진행 |

## 2단계: raw 파일 검증
| document_id | local_path | size | sha256 | 판정 |
|---|---|---:|---|---|
| ir-tem-earnings-release-fy2026-q1 | artifacts/raw/company-ir/TEM/2026-05-05_fy2026-q1-earnings-release/document.html | 377256 | ca6f1039f2de8638b7f499aff3430bf3bac2bf17fcea235a4f08513a043beac2 | pass |
| ir-tem-financial-supplement-fy2026-q1-overview | artifacts/raw/company-ir/TEM/2026-05-05_fy2026-q1-overview/document.pdf | 317921 | c6315a3b5c934e4a34681a4024f7d429fcf822ac81feb841888e640183e06e81 | pass |
| ir-tem-deck-fy2026-q1-corporate-deck | artifacts/raw/company-ir/TEM/2026-05-05_fy2026-q1-corporate-deck/document.pdf | 7730240 | db8a85c18645c01d9c96ce3d25123171559718ab660d3a6aed3dc0ef4de3bc31 | pass |

## 3단계: 파일 형식 검증
| 파일 | 판정 | 근거 | 조치 |
|---|---|---|---|
| earnings release HTML | pass | HTML title에 `Tempus Reports First Quarter 2026 Results` 확인 | 없음 |
| Q1 2026 Overview PDF | pass | 파일 시작 bytes가 `%PDF-` | 없음 |
| Tempus 1Q26 Corporate Deck PDF | pass | 파일 시작 bytes가 `%PDF-` | 없음 |

## 4단계: 보안 격리 확인
| 파일 | 판정 | 근거 | 조치 |
|---|---|---|---|
| earnings release HTML | pass | local_path 존재, hash 읽기 성공 | 없음 |
| Q1 2026 Overview PDF | pass | local_path 존재, hash 읽기 성공, PDF header 확인 | 없음 |
| Tempus 1Q26 Corporate Deck PDF | pass | local_path 존재, hash 읽기 성공, PDF header 확인 | 없음 |

판단:

```text
NTRA pilot 2와 같은 Bitdefender 격리 또는 삭제는 관찰되지 않았다.
격리 파일 복구/열람/승격 작업은 수행하지 않았다.
```

## 5단계: download-log 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| download-log 존재 | pass | artifacts/runs/run-20260605-tem-ir-pilot/download-log.jsonl 존재 | 없음 |
| 성공 attempt | pass | attempt-004, attempt-005, attempt-006 성공 | 없음 |
| 실패 attempt 기록 | pass_with_warning | attempt-001~003 transport 실패 기록 | 반복되면 downloader fallback 절차 정리 |

## 6단계: SEC overlap 확인
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| SEC 다운로드 | skipped_out_of_scope | 새 SEC 다운로드 없음 | 없음 |
| SEC filings harvesting | skipped_out_of_scope | SEC filings page harvesting 없음 | 없음 |
| 기존 TEM SEC records | partial | existing local catalog에서 TEM/Tempus 관련 SEC record 없음 | TEM SEC 수집 시 재확인 |
| exact hash match | partial | 기존 files.jsonl에서 이번 3건 hash와 동일한 file 없음 | 중복 없음 확정은 아님 |

적용 상태:

```text
overlap_status: not_found_in_local_catalog
exact_hash_match: false
overlap_check_scope: existing_local_catalog_files_raw_only
```

## 7단계: document_type / candidate 검증
| 자료 | document_type | candidate | 판정 |
|---|---|---|---|
| Q1 2026 earnings release HTML | ir-earnings-release | 없음 | pass |
| Q1 2026 Overview PDF | ir-financial-supplement | 없음 | pass |
| Tempus 1Q26 Corporate Deck PDF | ir-deck | ir-earnings-presentation | pass_with_review |

판단:

```text
Corporate Deck은 현재 schema에서는 ir-deck으로 처리한다.
다만 earnings call supporting material이므로 ir-earnings-presentation 후보로 기록했다.
schema는 자동 변경하지 않았다.
```

## 8단계: taxonomy candidate ledger 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| 후보 원장 존재 | pass | artifacts/catalog/ir-taxonomy-candidates.jsonl 존재 | 없음 |
| TEM 후보 record | pass | candidate:ir-earnings-presentation:tem:2026-06-05:q1-corporate-deck 추가 | 없음 |
| 사용자 승인 조건 | review_recommended | NTRA/APP/TEM 맥락에서 반복 관찰, TEM raw 성공 | 다음 taxonomy checkpoint에서 검토 |
| schema 자동 변경 | pass | source-pack-catalog.schema.md 수정 없음 | 사용자 승인 전까지 유지 |

## 9단계: 금지 내용 검증
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| source-pack-collector.md | pass | 수정하지 않음 | 없음 |
| 운영 catalog/index | pass | 사용자 승인 없이 documents/files/runs/index 수정하지 않음 | 없음 |
| SEC 다운로드 | pass | 수행하지 않음 | 없음 |
| SEC filings page harvesting | pass | 수행하지 않음 | 없음 |
| webcast/audio/video | pass | 수집하지 않음 | 없음 |
| transcript | pass | 수집하지 않음 | 없음 |
| archive-wide crawling | pass | 수행하지 않음 | 없음 |
| NTRA 격리 파일 | pass | 열람/복구/수정하지 않음 | 없음 |
| 투자 분석/요약/번역 | pass | 수행하지 않음 | 없음 |

## 10단계: QA 결과
| 기준 | 판정 | 근거 | 조치 |
|---|---|---|---|
| raw 수집 | pass | 승인된 3건 모두 local_path 존재 및 hash 검증 | 없음 |
| 운영 반영 | skipped_by_scope | 사용자 승인 전까지 운영 catalog/index 미반영 | 승인 후 별도 작업 |
| SEC overlap | partial | 기존 local catalog 기준 TEM SEC 후보 없음 | TEM SEC 수집 후 재확인 |
| taxonomy | pass_with_review | `ir-earnings-presentation` 후보 기록 | taxonomy checkpoint에서 검토 |

## Rubric Score

total_score: 86
overall_status: partial_pass

| 항목 | 가중치 | 점수(1-5) | 환산점 | 근거 |
|---|---:|---:|---:|---|
| Catalog 구조와 schema 준수 | 20 | 4 | 16 | 운영 catalog/index는 범위상 미반영, 후보 원장은 schema에 맞게 기록 |
| Raw/File 무결성 | 20 | 5 | 20 | 목표 3건 모두 raw 파일 존재, size/hash/형식 확인 |
| 수집 완전성과 출처 추적성 | 15 | 5 | 15 | 공식 TEM IR 3건만 제한 수집 |
| Run log와 실패 처리 | 15 | 4 | 12 | transport 실패와 fallback 성공을 download-log에 기록 |
| Index와 다음 하네스 전달성 | 10 | 3 | 6 | TEM index는 범위상 미생성, 운영 반영 전까지 제한적 |
| IR/Transcript optional 처리 | 10 | 5 | 10 | webcast/audio/video/transcript 제외 범위 준수 |
| 금지 내용과 모델 중립성 | 10 | 5 | 10 | SEC/분석/요약/번역/우회 없음 |

## Rubric 판정 근거
- 자동 실패 조건: 없음
- partial_pass 근거: raw 3건은 성공했지만, 운영 catalog/index 미반영 및 SEC overlap 미확정 상태다.
- 다음 조치: 사용자 승인 후 TEM raw 3건을 운영 catalog/index에 반영할지 결정한다. 이후 TEM SEC 수집이 수행되면 같은 ticker 범위에서 SEC-IR overlap을 재확인한다.
