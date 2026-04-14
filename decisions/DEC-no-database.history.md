# DEC-no-database: Trail

> Companion to `DEC-no-database.md`.
> AI agents read this only when evaluating whether the decision is still
> valid or when proposing a change or supersession.

## Alternatives considered

### Option A: In-memory Map with TTL (chosen)
- Pros: zero infrastructure, no env var, no provisioning time, GDPR-friendly (no PII at rest)
- Cons: state lost on container restart; does not scale horizontally (two instances would have split state)

### Option B: Redis (e.g., Railway Redis plugin)
- Pros: survives restarts; supports horizontal scaling
- Cons: adds a second Railway service and monthly cost; overkill for MVP single-instance deployment

### Option C: PostgreSQL / SQLite
- Pros: full persistence, query capability
- Cons: far exceeds the state management needs; schema migration overhead; PII storage risk

## Reasoning

The only state needed is a temporary mapping (reading_hash → birth_data) that expires within the checkout flow — typically under 10 minutes. An in-memory Map is sufficient, simpler, and avoids PII persistence. If Railway restarts the container mid-checkout, the user sees a "session expired" error and can re-enter birth data — a rare and recoverable scenario. Horizontal scaling is not a concern at MVP traffic levels. This decision should be reconsidered if: (a) multiple Railway replicas are needed, or (b) the sharing/revisiting feature (GOAL-sharing-virality) is activated post-launch.

## Human involvement

**Type**: ai-proposed/human-approved

**Notes**: Proposed during Design phase kickoff; approved by founder 2026-04-14.

## Changelog

| Date | Change | Involvement |
|------|--------|-------------|
| 2026-04-14 | Initial decision | ai-proposed/human-approved |
