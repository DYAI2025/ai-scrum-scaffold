# DEC-fufire-bootstrap: Trail

> Companion to `DEC-fufire-bootstrap.md`.
> AI agents read this only when evaluating whether the decision is still
> valid or when proposing a change or supersession.

## Alternatives considered

### Option A: POST /v1/experience/bootstrap (chosen)
- Pros: single call returns full fused profile (Western + BaZi + Wu-Xing soulprint + signature blueprint); designed for product-level use; response is ready-to-display
- Cons: higher per-call weight than individual calculation endpoints; partnership = 2 calls

### Option B: Compose individual /v1/calculate/* endpoints
- Pros: granular control over which calculation components to include; potentially lighter per-call
- Cons: multiple round-trips per reading (3–4 calls); requires manual response assembly; more complex BFF logic; likely slower overall

### Option C: Client-side fallback only (src/utils/astrology.ts)
- Pros: no backend needed; zero latency for calculations
- Cons: simplified calculations; less accurate than FuFirE ephemeris; does not differentiate the product; ASM-fufire-api-available is now Verified so the fallback is not needed

## Reasoning

The bootstrap endpoint is purpose-built for experience-level product use — it returns a complete, fused profile in one call. Composing individual endpoints would replicate bootstrap's internal logic with more network overhead and BFF complexity. The partnership 2-call approach is the only available option since FuFirE has no dedicated compatibility endpoint; the rate limit impact (2× per partnership reading) is acceptable at `ff_pro_` tier.

## Human involvement

**Type**: ai-proposed/human-approved

**Notes**: Proposed during Design phase kickoff after FuFirE OpenAPI spec was retrieved; approved by founder 2026-04-14.

## Changelog

| Date | Change | Involvement |
|------|--------|-------------|
| 2026-04-14 | Initial decision | ai-proposed/human-approved |
