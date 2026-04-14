# DEC-bff-same-service: Express BFF and SPA in one Railway service

**Status**: Active

**Category**: Architecture

**Scope**: system-wide

**Source**: [REQ-MNT-railway-deploy](../1-spec/requirements/REQ-MNT-railway-deploy.md), [CON-railway-deployment](../1-spec/constraints/CON-railway-deployment.md), [CON-launch-deadline](../1-spec/constraints/CON-launch-deadline.md)

**Last updated**: 2026-04-14

## Context

The landing page requires a backend component to hold API secrets (FuFirE API key, Stripe secret key) server-side. Railway supports multiple services per project, but multiple services increase deployment complexity, cost, and operational overhead. The 2-week launch deadline (CON-launch-deadline) and single-developer team favor the simplest deployable unit.

## Decision

Deploy the Express BFF server and the Vite-built React SPA as a single Railway service. The Express server serves `dist/` as static files and exposes `/api/*` routes. There is no separate frontend hosting service.

## Enforcement

### Trigger conditions

- **Design phase**: when defining the deployment topology or API layer location
- **Code phase**: when setting up the `server/` directory, writing `package.json` start scripts, or configuring Railway nixpacks detection
- **Deploy phase**: when configuring Railway service settings or adding new services

### Required patterns

- The repository root `package.json` `start` script must launch the Express server, not a static file server
- Express must serve `dist/` with `express.static` as the catch-all fallback after all `/api/*` routes
- Railway Nixpacks must detect a Node.js app (not a static site) — ensure `server/index.ts` or equivalent is the entry point
- All `/api/*` routes must be registered before the static file middleware

### Required checks

1. Verify no second Railway service is created for frontend hosting
2. Confirm `dist/` is built before the Express server starts (build step in Railway's build command)
3. Confirm the health check route (`/v1/health` proxy or a dedicated `/health`) responds within 30s of container start

### Prohibited patterns

- Do not deploy the frontend to a separate Vercel, Netlify, or CDN service while keeping the BFF on Railway — this creates CORS complexity and violates the single-service intent
- Do not serve API and static files from separate ports — use one Express instance on one port
