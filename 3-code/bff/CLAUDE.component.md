# BFF (Backend-for-Frontend)

**Responsibility**: Secret management, FuFirE API proxy, Stripe Checkout integration, webhook validation, in-memory session store, and static file serving for the SPA build.

**Technology**: Node.js, Express, TypeScript

**Source code**: `../../server/` (repository root `server/`, to be created)

## Interfaces

- HTTP ← SPA: receives `POST /api/reading`, `POST /api/checkout`, `GET /api/reading/unlock`
- HTTP → FuFirE API: `POST /v1/experience/bootstrap` with `X-API-Key` injection
- HTTP → Stripe: Create/Verify Checkout Sessions via Stripe Node SDK
- HTTP ← Stripe: `POST /api/webhooks/stripe` with signature validation
- Static file serving: `GET /*` serves `dist/` (Vite production build)

## Requirements Addressed

| File | Type | Priority | Summary |
|------|------|----------|---------|
| [REQ-F-reading-generation](../../1-spec/requirements/REQ-F-reading-generation.md) | Functional | Must-have | Tri-system reading via FuFirE bootstrap |
| [REQ-F-teaser-preview](../../1-spec/requirements/REQ-F-teaser-preview.md) | Functional | Must-have | Server-side teaser stripping (≤30%) |
| [REQ-F-payment-integration](../../1-spec/requirements/REQ-F-payment-integration.md) | Functional | Must-have | Stripe Checkout session creation |
| [REQ-F-reading-unlock](../../1-spec/requirements/REQ-F-reading-unlock.md) | Functional | Must-have | Verify Stripe session, return full reading |
| [REQ-SEC-data-protection](../../1-spec/requirements/REQ-SEC-data-protection.md) | Security | Must-have | HTTPS, no PII in logs, webhook signature validation, keys server-side |
| [REQ-MNT-railway-deploy](../../1-spec/requirements/REQ-MNT-railway-deploy.md) | Maintainability | Must-have | Health check, env-var config, Railway-compatible build |

## Relevant Decisions

| File | Title | Trigger |
|------|-------|---------|
| [DEC-bff-same-service](../../decisions/DEC-bff-same-service.md) | Express BFF + SPA in one Railway service | Single Express server, single Railway service, single port |
| [DEC-no-database](../../decisions/DEC-no-database.md) | No persistent storage for MVP | In-memory Map with 1h TTL, no DB provisioning |
| [DEC-fufire-bootstrap](../../decisions/DEC-fufire-bootstrap.md) | Use /v1/experience/bootstrap | Sole FuFirE endpoint; partnership = two calls merged server-side |
| [DEC-teaser-server-strip](../../decisions/DEC-teaser-server-strip.md) | Teaser created server-side | Strip full reading to ≤30% before sending to client |
