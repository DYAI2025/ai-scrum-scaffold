# DEC-teaser-server-strip: Trail

> Companion to `DEC-teaser-server-strip.md`.
> AI agents read this only when evaluating whether the decision is still
> valid or when proposing a change or supersession.

## Alternatives considered

### Option A: Server-side strip (chosen)
- Pros: full reading never leaves the server until payment verified; paywall is cryptographically enforced; satisfies CON-no-free-tier absolutely
- Cons: requires a second FuFirE API call at unlock time (can't cache the full reading without PII storage); slightly higher FuFirE rate limit consumption

### Option B: Send full reading, hide client-side
- Pros: only one FuFirE call; simpler BFF; no second unlock call needed
- Cons: full reading is visible in DevTools network tab; any user with browser DevTools can bypass the paywall; directly violates CON-no-free-tier and REQ-F-reading-unlock

### Option C: Encrypt full reading, send to client, decrypt on payment
- Pros: one FuFirE call; paywall enforced cryptographically on client
- Cons: encryption key must come from server (defeating the point); implementation complexity significantly higher; non-standard approach

## Reasoning

Option B is a security anti-pattern for a paid product — a trivially bypassable paywall would undermine trust and revenue. Option C adds cryptographic complexity with no advantage over Option A. The second FuFirE call at unlock time (Option A's main cost) is a minor rate-limit hit that is well within `ff_pro_` tier limits. The DEC-no-database decision (no PII caching) also makes Option A the natural fit — regenerate fresh rather than cache.

## Human involvement

**Type**: ai-proposed/human-approved

**Notes**: Proposed during Design phase kickoff; approved by founder 2026-04-14.

## Changelog

| Date | Change | Involvement |
|------|--------|-------------|
| 2026-04-14 | Initial decision | ai-proposed/human-approved |
