# S03 APP Source Pack Top-up Request for S04 Full Industry Primer

## 1. Request Metadata

| Field | Value |
|---|---|
| Date | 2026-06-13 |
| Requesting harness | P2-S04 Industry Primer |
| Receiving harness | P2-S03 Source Pack |
| Target company | APP / AppLovin Corporation |
| Purpose | Prepare source coverage for the S04 full Industry Primer official run |
| Request status | Handoff request draft; SEC/sector item_id preflight applied in S03; not executed in S04 |
| Scope boundary | S04 does not perform Source Pack top-up. S03 owns raw collection, catalog/index updates, run-summary, and QA. |

This handoff exists because the APP/adtech first slice pilot was completed and officially accepted in S04, and S04 now needs stronger input coverage before a full Industry Primer official run. This note is a request package for S03; it does not authorize S04 to collect Source Pack materials or write the full Industry Primer.

## 2. Current S04 Input State

S04 currently references APP Source Pack inputs from:

```text
../P2-S03-Source-Pack/artifacts/companies/APP/index.md
```

The first slice pilot used the available APP Source Pack and pilot sources, including the Q1 2026 earnings release, Q1 2026 financial update PDF, and Q1 2026 8-K / EX-99.1 materials.

Known limitations to resolve before the full Industry Primer official run:

| Gap | Current impact |
|---|---|
| APP 10-K and 10-Q coverage is not yet confirmed as available in the S03 APP Source Pack | Needed for durable business description, risk factors, segment/company context, and official filing language |
| Product-level official documentation is not yet confirmed as collected | Needed to connect APP product language to adtech roles without speculation |
| Sector/entity metadata remains unresolved | Needed to close metadata quality gaps before S04 intake |
| PDF table grid reconstruction was not performed | Non-blocking for first slice; may matter later if full-run sections require exact financial table context |

## 3. Required S03 Top-up Items

| Item ID | Priority | Requested item | Collection reason | S04 sections affected | Expected S03 output |
|---|---|---|---|---|---|
| APP-S03-REQ-20260613-001 | Required | Latest APP Form 10-K | Provides official business description, risk factors, operating context, segment/company language, and durable company framing | Sections 2, 7, 8, 11, 12, 14 | Raw filing/source record, catalog entry, APP index update, run-summary note, QA coverage note |
| APP-S03-REQ-20260613-002 | Required | Latest APP Form 10-Q | Provides latest quarterly updates, current risk language, financial/business context, and bridges Q1 IR materials to SEC filing language | Sections 7, 8, 11, 12, 14 | Raw filing/source record, catalog entry, APP index update, run-summary note, QA coverage note |
| APP-S03-REQ-20260613-003 | Required | Sector/entity metadata | Needed to close the existing sector metadata gap and improve S04 intake consistency | Intake, source register, Section 1/14 framing | Updated metadata field or explicit unresolved reason with evidence |
| Phase 4 pending; starts at APP-S03-REQ-20260613-004 | Required | APP product-level official docs/pages | Needed to verify official product language for MAX, AXON, AppDiscovery, mediation, advertising, measurement, and adjacent platform terms | Sections 4, 7, 12, 13 | URLs, titles, access status, raw/local capture where S03 normally stores it, catalog/index entries |

S03 should preserve Source Pack provenance and mark whether each item was collected, skipped, failed, or deferred.
Phase 1 SEC/sector run-summary and QA should map results to item IDs APP-S03-REQ-20260613-001 through APP-S03-REQ-20260613-003.
Product-level official docs/pages remain required, but their page-level item IDs are deferred until the Phase 4 company-official page_key/canonical_url preflight.

## 4. Optional or Deferred S03 Items

| Candidate item | Suggested handling | Reason |
|---|---|---|
| Earnings call transcript / webcast / audio / video | Optional candidate; collect only if within approved S03 top-up scope | Helpful for management framing, but not required if SEC filings and official product docs are sufficient for S04 full Industry Primer |
| PDF table extraction or derived table text | Defer unless separately approved | Exact table reconstruction may be useful later, but it is a derived extraction task and should not be silently added |
| Proxy / DEF 14A | Defer | Governance/incentive material is not required for the S04 Industry Primer official run |

## 5. S04 External Source Expansion Candidates

The following materials are relevant to the S04 full Industry Primer, but they are not requested as S03 Source Pack top-up items unless S03 separately owns them in its process.

| S04 candidate area | Why S04 may need it |
|---|---|
| Submarket taxonomy | To distinguish mobile advertising, app monetization, programmatic advertising, mediation, attribution, and privacy measurement at industry level |
| Growth drivers | To support Section 6 without relying only on company claims |
| Regulation and platform policy | To support Section 9, including ATT, SKAdNetwork / AdAttributionKit context, Privacy Sandbox, and privacy-constrained measurement |
| Technology change | To support Section 10 around AI optimization, automation, measurement, attribution, and platform shifts |
| Structural risks | To support Section 11 without prematurely making competition, moat, valuation, or investment conclusions |

These should be handled by S04 external source expansion after the S03 top-up intake is reviewed and approved.

## 6. S03 Execution Boundary

S03 should:

- Update the S03 APP Source Pack artifacts in S03, not S04.
- Keep raw/source files under S03's normal raw/catalog structure.
- Update APP company index, catalog entries, run-summary, and QA according to S03 rules.
- Record collection failures, access limits, skipped sources, and deferred items explicitly.
- Avoid investment, valuation, moat, market-share, or competition conclusions.

S04 should not:

- Copy S03 raw materials into S04.
- Run Source Pack collection silently.
- Write the full Industry Primer before S03 intake and S04 external-source expansion gates are cleared.

## 7. S04 Intake Criteria After S03 Completion

After S03 completes the top-up, S04 should perform a read-only intake check against the updated APP Source Pack.

| Intake check | Pass condition |
|---|---|
| Updated APP index exists | S04 can read the updated S03 APP index and identify the latest top-up run/source updates |
| 10-K coverage | Latest APP 10-K is present in the S03 catalog/index with source path or access record |
| 10-Q coverage | Latest APP 10-Q is present in the S03 catalog/index with source path or access record |
| Product-level official docs | Relevant APP product docs/pages are listed with title, URL, access status, and local/source record where applicable |
| Sector/entity metadata | Sector/entity metadata is resolved, or the unresolved reason is explicitly documented |
| Run-summary and QA | S03 run-summary and QA explain what was collected, skipped, failed, or deferred |
| No S04 raw copy | Source Pack raw materials remain in S03; S04 references them by path |
| No conclusion leakage | S03 top-up does not introduce market-share, competition, moat, valuation, or investment conclusions |

If any required item remains unavailable, S04 should classify it as either non-blocking, blocking, or requiring a separate user-approved top-up path before full Industry Primer writing begins.

## 8. Suggested Approval Language for S03

The user can use the following instruction in the S03 workspace or S03 thread:

```text
APP Source Pack top-up을 승인해.
목적은 P2-S04 full Industry Primer official run 준비야.
범위는 최신 APP 10-K, 최신 APP 10-Q, APP product-level official docs/pages, sector/entity metadata 확인이야.
transcript/webcast/audio/video는 선택 후보로만 기록하고, PDF table extraction/derived text는 별도 승인 전까지 하지 마.
수집 결과는 S03 company index, raw/catalog, run-summary, QA에 반영해줘.
S04 파일은 수정하지 마.
```

## 9. Next S04 Step After S03 Top-up

Once S03 completes this top-up, S04 should run a read-only APP Source Pack intake check. After that, S04 should decide whether external source expansion is sufficient for submarket taxonomy, growth drivers, regulation, technology change, and structural risk coverage before approving the full Industry Primer official run.
