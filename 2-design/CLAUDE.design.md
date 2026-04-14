Phase-specific instructions for the **Design** phase. Extends [../CLAUDE.md](../CLAUDE.md).

## Purpose

This phase defines **how** we're building the system. Focus on architecture, data models, APIs, and key technical decisions.

## Files in This Phase

| File | Purpose |
|------|---------|
| [`architecture.md`](architecture.md) | System architecture overview and diagrams |
| [`data-model.md`](data-model.md) | Data structures, schemas, and relationships |
| [`api-design.md`](api-design.md) | API specifications and contracts |

---

## Decisions Relevant to This Phase

| File | Title | Trigger |
|------|-------|---------|
| [DEC-bff-same-service](../decisions/DEC-bff-same-service.md) | Express BFF + SPA in one Railway service | Deployment topology, API layer location, Railway service setup |
| [DEC-no-database](../decisions/DEC-no-database.md) | No persistent storage for MVP | BFF session store, any state that outlives a single request |
| [DEC-fufire-bootstrap](../decisions/DEC-fufire-bootstrap.md) | Use /v1/experience/bootstrap as primary reading endpoint | BFF reading handler, FuFirE API integration |
| [DEC-teaser-server-strip](../decisions/DEC-teaser-server-strip.md) | Teaser created server-side by stripping full reading | /api/reading response contract, TeaserReading vs FullReading types |

---

## Linking to Other Phases

- Reference requirements from `1-spec/` to justify design choices
- Design documents guide implementation in `3-code/`
- Infrastructure design informs deployment in `4-deploy/`
