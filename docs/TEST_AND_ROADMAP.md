# QA, Test & Parallel Roadmap — PublicAuction

## P0 test matrix
- Schema contract fixtures: normal, missing nullable fields, malformed amount, unsupported category, duplicate asset IDs, changed notice.
- Time: KST/UTC conversion, daylight boundaries, past/future auction status, ended/retracted notice.
- Identity: repeated notice multiple rounds; unrelated assets same title must never join.
- Stats: eligible denominator, zero denominator, insufficient sample, unavailable current-bid count, category mismatch and conflicting aggregate scope.
- Price: appraisal/reserve/winning amount separate; currency scale; stale values.
- UI: filter deep links, mobile, screen reader, no JS fallback links, official URL validity, ad labeling, no misleading claims.
- Security: API key absent client bundle, injection payloads, SSRF blocking, CORS, rate limits, secret scanning.
- Resilience: public API outage, timeout, partial pagination, retry, dedup, checkpoint resume, cache freshness.

## Release gates
G0 APIs approved, license/quotas documented.
G1 100 official records sampled and manually checked by stratified category/time.
G2 referential integrity & dedup tests pass.
G3 web preview SEO/mobile/accessibility verified.
G4 beta feedback + decision on monetization/legal counsel.

## Parallel milestone plan (not promises)
Sprint 0: source audit, 20 sample responses, schema proposal.
Sprint 1: collectors, snapshot tests, seeded DB.
Sprint 2: search / category UI / SEO and analytics.
Sprint 3: aggregated trend and comparison with source caveats.
Sprint 4: beta, error reports, maintenance runbook.

## Acceptance evidence
Record replay fixtures and expected normalized records; source response checksum; before/after ingestion diff; query explain plans for top queries; Playwright screenshots; 2-device browser tests; security checklist.

## Explicit no-go
No automatic bid placement, escrow, unlicensed representation/mediation, lottery-like mechanics or guarantees of profit.
