# DEC-fufire-bootstrap: Use /v1/experience/bootstrap as primary reading endpoint

**Status**: Active

**Category**: Architecture

**Scope**: backend

**Source**: [REQ-F-reading-generation](../1-spec/requirements/REQ-F-reading-generation.md), [ASM-fufire-api-available](../1-spec/assumptions/ASM-fufire-api-available.md)

**Last updated**: 2026-04-14

## Context

The FuFirE API exposes multiple calculation endpoints (`/v1/calculate/bazi`, `/v1/calculate/western`, `/v1/calculate/fusion`, `/v1/calculate/wuxing`) as well as a higher-level experience endpoint (`/v1/experience/bootstrap`). The landing page needs a tri-system reading (Western + BaZi + Nakshatra/Wu-Xing fusion) that serves as the product. Choosing which endpoint(s) to call determines response structure, latency, and the shape of the data model.

## Decision

Use `POST /v1/experience/bootstrap` as the sole FuFirE endpoint for generating readings. Do not call individual calculation endpoints (`/v1/calculate/*`) directly. For partnership readings, call bootstrap twice (once per person) and merge server-side.

## Enforcement

### Trigger conditions

- **Design phase**: when designing API contracts or data model for reading responses
- **Code phase**: when implementing the BFF `/api/reading` and `/api/reading/unlock` routes

### Required patterns

- The BFF `POST /api/reading` handler calls `POST /v1/experience/bootstrap` with the user's birth data
- Request body must include at minimum: birth date, time (if known), timezone, and `birth_time_known` flag
- For partnership mode: two sequential (or parallel) bootstrap calls — one for each person — merged into a `PartnershipReading` object before responding to the client
- The `X-API-Key` header must be injected from `process.env.FUFIRE_API_KEY` — never hardcoded

### Required checks

1. Confirm the bootstrap response schema matches the data model fields used in the SPA reading display
2. Confirm that `birth_time_known: false` is sent when the user omits birth time — FuFirE handles solar noon fallback internally
3. For partnership: confirm both bootstrap calls succeed before responding — if either fails, return an error

### Prohibited patterns

- Do not call `/v1/calculate/bazi`, `/v1/calculate/western`, or `/v1/calculate/fusion` individually and reassemble manually — use bootstrap to get the full fused profile
- Do not expose the FuFirE API key or base URL to the client under any circumstances
- Do not cache bootstrap responses server-side (PII risk) — regenerate on each request
