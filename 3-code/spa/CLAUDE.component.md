# SPA (Single-Page Application)

**Responsibility**: All client-side rendering, user interaction, scroll-driven animations, birth data input, teaser/full reading display, i18n, cookie consent, and legal pages.

**Technology**: React 19, TypeScript, Vite 7, Tailwind CSS v3, GSAP ScrollTrigger, shadcn/ui (new-york style, Radix primitives)

**Source code**: `../../src/` (repository root `src/`)

## Interfaces

- HTTP (fetch) → BFF: `POST /api/reading`, `POST /api/checkout`, `GET /api/reading/unlock`

## Requirements Addressed

| File | Type | Priority | Summary |
|------|------|----------|---------|
| [REQ-F-birth-data-input](../../1-spec/requirements/REQ-F-birth-data-input.md) | Functional | Must-have | Birth date/time form, single + partner mode |
| [REQ-F-teaser-preview](../../1-spec/requirements/REQ-F-teaser-preview.md) | Functional | Must-have | Teaser preview display before payment |
| [REQ-F-reading-unlock](../../1-spec/requirements/REQ-F-reading-unlock.md) | Functional | Must-have | Display full reading after verified payment |
| [REQ-F-i18n](../../1-spec/requirements/REQ-F-i18n.md) | Functional | Must-have | DE + EN, locale detection, manual toggle |
| [REQ-PERF-initial-load](../../1-spec/requirements/REQ-PERF-initial-load.md) | Performance | Must-have | FCP < 3s on 4G, < 1.5s on broadband |
| [REQ-USA-responsive-layout](../../1-spec/requirements/REQ-USA-responsive-layout.md) | Usability | Must-have | Responsive 320px–2560px, 44px tap targets |
| [REQ-USA-scroll-animations](../../1-spec/requirements/REQ-USA-scroll-animations.md) | Usability | Must-have | 60fps GSAP animations, graceful degradation |
| [REQ-COMP-cookie-banner](../../1-spec/requirements/REQ-COMP-cookie-banner.md) | Compliance | Must-have | Cookie consent, blocks non-essential until opted in |
| [REQ-COMP-impressum](../../1-spec/requirements/REQ-COMP-impressum.md) | Compliance | Must-have | TMG/DDG-compliant Impressum |
| [REQ-COMP-privacy-policy](../../1-spec/requirements/REQ-COMP-privacy-policy.md) | Compliance | Must-have | DSGVO-compliant Datenschutzerklärung |

## Relevant Decisions

| File | Title | Trigger |
|------|-------|---------|
| [DEC-bff-same-service](../../decisions/DEC-bff-same-service.md) | Express BFF + SPA in one Railway service | Build output must be `dist/` served by Express, not a separate static host |
| [DEC-teaser-server-strip](../../decisions/DEC-teaser-server-strip.md) | Teaser created server-side | SPA receives `TeaserReading` — never attempt to filter a full reading client-side |
