# DEC-no-database: No persistent storage for MVP

**Status**: Active

**Category**: Architecture

**Scope**: backend

**Source**: [CON-launch-deadline](../1-spec/constraints/CON-launch-deadline.md), [REQ-F-reading-unlock](../1-spec/requirements/REQ-F-reading-unlock.md)

**Last updated**: 2026-04-14

## Context

The landing page has no user accounts, no saved readings, and no admin interface. The only state that needs to survive a request cycle is: (a) the association between a `reading_hash` and the birth data used to generate it, so the BFF can regenerate the full reading after payment; and (b) the Stripe `session_id`-to-`reading_hash` mapping. Both are short-lived (minutes to an hour). Provisioning a database for this would add cost, infrastructure complexity, and setup time.

## Decision

The BFF holds all session state in an in-memory `Map` with a 1-hour TTL. No database, Redis, or external cache is provisioned for MVP. Readings are generated fresh on each request — there is no reading persistence beyond the active session.

## Enforcement

### Trigger conditions

- **Design phase**: when designing the data model or API unlock flow
- **Code phase**: when implementing the BFF session store or any state that outlives a single request

### Required patterns

- Use a module-level `Map<string, SessionEntry>` in the BFF for `reading_hash → { birth_data, created_at }`
- Implement a TTL cleanup: entries older than 1 hour are deleted on each write (or via a `setInterval`)
- Session entries must never contain raw PII in logs — store hashed or opaque references only

### Required checks

1. Confirm no database connection string is required in environment variables
2. Confirm session entries are bounded in size (TTL cleanup prevents unbounded memory growth)
3. Confirm that a container restart results in a graceful "session not found" error to the user, not a crash

### Prohibited patterns

- Do not introduce a database, Redis, or any external state store without superseding this decision
- Do not persist birth data to disk or any external service
- Do not store full reading results server-side — regenerate on unlock to avoid PII at rest
