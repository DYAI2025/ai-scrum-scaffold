# DEC-teaser-server-strip: Teaser is created server-side by stripping the full reading

**Status**: Active

**Category**: Architecture

**Scope**: backend

**Source**: [REQ-F-teaser-preview](../1-spec/requirements/REQ-F-teaser-preview.md), [REQ-F-reading-unlock](../1-spec/requirements/REQ-F-reading-unlock.md), [CON-no-free-tier](../1-spec/constraints/CON-no-free-tier.md)

**Last updated**: 2026-04-14

## Context

REQ-F-teaser-preview requires that at most 30% of the full reading is shown before payment. CON-no-free-tier requires that no full reading is accessible without payment. If the full reading were sent to the client and hidden via CSS or JavaScript, a technically literate user could extract it from the network response — defeating the paywall entirely.

## Decision

The BFF generates the teaser by calling FuFirE bootstrap, receiving the full reading, and returning only a stripped subset (≤30% of reading fields/content) to the client. The full reading is never sent to the browser until payment is verified.

## Enforcement

### Trigger conditions

- **Design phase**: when designing the `/api/reading` response contract or data model
- **Code phase**: when implementing the BFF reading handler and defining `TeaserReading` vs `FullReading` types

### Required patterns

- The `POST /api/reading` response type is `TeaserReading` — a defined subset of `BootstrapResponse`
- The `GET /api/reading/unlock` response type is `FullReading` — the complete `BootstrapResponse`
- The teaser must contain ≤30% of the meaningful content fields of a full reading (e.g., first soulprint sector summary only, no signature blueprint)
- `reading_hash` is an opaque server-generated token (e.g., `crypto.randomUUID()`) — it does not encode or expose birth data

### Required checks

1. Confirm `POST /api/reading` response does not include signature blueprint, full Wu-Xing analysis, or full BaZi pillar interpretation
2. Confirm `GET /api/reading/unlock` is gated behind Stripe session verification — calling it without a valid paid session returns HTTP 402
3. Confirm no full reading data appears in browser DevTools network tab before payment

### Prohibited patterns

- Do not send the full reading to the client and rely on client-side hiding (CSS `display:none`, conditional rendering) to enforce the paywall
- Do not include the full reading in a hidden field, data attribute, or localStorage entry on initial page load
- Do not derive the teaser from client-side filtering of a full response
