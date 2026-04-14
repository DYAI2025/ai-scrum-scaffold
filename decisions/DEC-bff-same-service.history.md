# DEC-bff-same-service: Trail

> Companion to `DEC-bff-same-service.md`.
> AI agents read this only when evaluating whether the decision is still
> valid or when proposing a change or supersession.

## Alternatives considered

### Option A: Single-service BFF monolith (chosen)
- Pros: one Railway service, no CORS config, secrets fully server-side, simple build pipeline, lowest operational cost
- Cons: frontend and backend in same process — a BFF crash takes down the static site too; no independent scaling

### Option B: Separate Railway services (frontend static + backend API)
- Pros: independent scaling and deployment; frontend can be served from CDN
- Cons: requires CORS configuration; two services to manage and monitor; more complex Railway project setup; unnecessary for launch traffic volume

### Option C: Serverless functions (e.g., Vercel Edge Functions)
- Pros: scales to zero; no server to manage
- Cons: incompatible with CON-railway-deployment; cold start latency for FuFirE proxy calls

## Reasoning

The 2-week deadline and single-developer context make operational simplicity the overriding factor. A single Railway service eliminates CORS, halves deployment surface, and maps directly to CON-railway-deployment. The risk of a BFF crash affecting static serving is acceptable at MVP scale — Railway auto-restarts containers. Independent scaling is not a requirement until traffic justifies it.

## Human involvement

**Type**: ai-proposed/human-approved

**Notes**: Proposed during Design phase kickoff; approved by founder 2026-04-14.

## Changelog

| Date | Change | Involvement |
|------|--------|-------------|
| 2026-04-14 | Initial decision | ai-proposed/human-approved |
