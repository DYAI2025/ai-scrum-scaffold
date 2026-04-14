# Tasks — Bazodiac Landing Page

## Status Legend

| Status | Meaning |
|--------|---------|
| Todo | Not started |
| In Progress | Currently being worked on |
| Blocked | Cannot proceed (see Notes) |
| Done | Completed and verified |
| Cancelled | No longer needed |

## Priority Legend

| Priority | Meaning |
|----------|---------|
| P0 | Infrastructure / blocking foundation |
| P1 | Must-have goal requirement |
| P2 | Should-have goal requirement |
| P3 | Could-have / post-launch |

---

## How to Update

When working on a task:
1. Set Status to `In Progress` and update the `Updated` date
2. When complete, set Status to `Done` and update the `Updated` date
3. If blocked, set Status to `Blocked` and add the reason in Notes

---

### Setup & Infrastructure

| ID | Task | Priority | Status | Req | Dependencies | Updated | Notes |
|----|------|----------|--------|-----|--------------|---------|-------|
| TASK-bff-express-scaffold | Create `server/` directory with Express + TypeScript scaffold (package.json, tsconfig, entry point) | P0 | Todo | - | - | 2026-04-14 | |
| TASK-bff-health-endpoint | Implement `GET /health` returning `{ "status": "ok" }` | P0 | Todo | [REQ-MNT-railway-deploy](../1-spec/requirements/REQ-MNT-railway-deploy.md) | TASK-bff-express-scaffold | 2026-04-14 | |
| TASK-bff-static-serving | Configure Express to serve Vite `dist/` as static files with SPA fallback | P0 | Todo | [REQ-MNT-railway-deploy](../1-spec/requirements/REQ-MNT-railway-deploy.md) | TASK-bff-express-scaffold | 2026-04-14 | |
| TASK-bff-env-config | Create env var configuration module with validation on startup | P0 | Todo | [REQ-MNT-railway-deploy](../1-spec/requirements/REQ-MNT-railway-deploy.md), [REQ-SEC-data-protection](../1-spec/requirements/REQ-SEC-data-protection.md) | TASK-bff-express-scaffold | 2026-04-14 | FUFIRE_API_KEY, FUFIRE_BASE_URL, STRIPE_SECRET_KEY, STRIPE_WEBHOOK_SECRET, STRIPE_PRICE_ID, PUBLIC_URL |
| TASK-bff-error-format | Implement consistent error response middleware (`{ error, code }` format) | P0 | Todo | [REQ-SEC-data-protection](../1-spec/requirements/REQ-SEC-data-protection.md) | TASK-bff-express-scaffold | 2026-04-14 | No PII in error responses |
| TASK-railway-build-config | Configure root package.json scripts for Railway build and start | P0 | Todo | [REQ-MNT-railway-deploy](../1-spec/requirements/REQ-MNT-railway-deploy.md) | TASK-bff-static-serving | 2026-04-14 | |
| TASK-shared-types | Create shared TypeScript types in `src/types/reading.ts` | P0 | Todo | - | - | 2026-04-14 | BirthData, ReadingRequest, TeaserReading, FullReading, PersonProfile, CheckoutRequest, CheckoutResponse |

---

### BFF (Backend-for-Frontend)

| ID | Task | Priority | Status | Req | Dependencies | Updated | Notes |
|----|------|----------|--------|-----|--------------|---------|-------|
| TASK-bff-session-store | Implement in-memory session store (Map with 1h TTL eviction) | P1 | Todo | [REQ-F-reading-unlock](../1-spec/requirements/REQ-F-reading-unlock.md) | TASK-bff-express-scaffold | 2026-04-14 | |
| TASK-bff-reading-endpoint | Implement `POST /api/reading`: validate, call FuFirE bootstrap, strip to teaser, store session, return `{ teaser, reading_hash }` | P1 | Todo | [REQ-F-reading-generation](../1-spec/requirements/REQ-F-reading-generation.md), [REQ-F-teaser-preview](../1-spec/requirements/REQ-F-teaser-preview.md) | TASK-bff-env-config, TASK-bff-session-store, TASK-shared-types | 2026-04-14 | |
| TASK-bff-no-pii-logging | Configure request logging middleware that excludes `/api/reading` request bodies | P1 | Todo | [REQ-SEC-data-protection](../1-spec/requirements/REQ-SEC-data-protection.md) | TASK-bff-reading-endpoint | 2026-04-14 | |
| TASK-bff-checkout-endpoint | Implement `POST /api/checkout`: look up session, create Stripe Checkout Session, return `{ checkout_url }` | P1 | Todo | [REQ-F-payment-integration](../1-spec/requirements/REQ-F-payment-integration.md) | TASK-bff-env-config, TASK-bff-session-store | 2026-04-14 | |
| TASK-bff-unlock-endpoint | Implement `GET /api/reading/unlock`: verify Stripe session, call FuFirE bootstrap (full), return `{ full_reading }` | P1 | Todo | [REQ-F-reading-unlock](../1-spec/requirements/REQ-F-reading-unlock.md) | TASK-bff-checkout-endpoint, TASK-bff-session-store | 2026-04-14 | |
| TASK-bff-webhook-endpoint | Implement `POST /api/webhooks/stripe`: raw body middleware, signature validation, handle `checkout.session.completed` | P1 | Todo | [REQ-SEC-data-protection](../1-spec/requirements/REQ-SEC-data-protection.md) | TASK-bff-env-config | 2026-04-14 | |
| TASK-bff-partnership-reading | Extend `POST /api/reading` for partnership mode: two parallel FuFirE bootstrap calls, merge into partnership teaser | P1 | Todo | [REQ-F-reading-generation](../1-spec/requirements/REQ-F-reading-generation.md) | TASK-bff-reading-endpoint | 2026-04-14 | |
| TASK-bff-partnership-unlock | Extend `GET /api/reading/unlock` for partnership mode: two bootstrap calls, merge into partnership full reading | P1 | Todo | [REQ-F-reading-unlock](../1-spec/requirements/REQ-F-reading-unlock.md) | TASK-bff-unlock-endpoint | 2026-04-14 | |
| TASK-bff-security-audit | Verify: no API keys in client bundle, no PII in logs, HTTPS, webhook validation, opaque session tokens | P1 | Todo | [REQ-SEC-data-protection](../1-spec/requirements/REQ-SEC-data-protection.md) | TASK-bff-webhook-endpoint, TASK-bff-no-pii-logging | 2026-04-14 | |

---

### SPA (Single-Page Application)

| ID | Task | Priority | Status | Req | Dependencies | Updated | Notes |
|----|------|----------|--------|-----|--------------|---------|-------|
| TASK-spa-input-form-api | Refactor InputSection to submit birth data to `POST /api/reading` and receive teaser response | P1 | Todo | [REQ-F-birth-data-input](../1-spec/requirements/REQ-F-birth-data-input.md) | TASK-bff-reading-endpoint, TASK-shared-types | 2026-04-14 | |
| TASK-spa-teaser-display | Refactor RevealSection to display TeaserReading from API (zodiac, BaZi, Nakshatra, element, preview text) with unlock CTA | P1 | Todo | [REQ-F-teaser-preview](../1-spec/requirements/REQ-F-teaser-preview.md) | TASK-spa-input-form-api | 2026-04-14 | |
| TASK-spa-checkout-flow | Implement checkout: redirect to Stripe on CTA click, handle success redirect (`?session_id=`), call `/api/reading/unlock`, handle cancel/failure | P1 | Todo | [REQ-F-payment-integration](../1-spec/requirements/REQ-F-payment-integration.md), [REQ-F-reading-unlock](../1-spec/requirements/REQ-F-reading-unlock.md) | TASK-bff-checkout-endpoint, TASK-bff-unlock-endpoint, TASK-spa-teaser-display | 2026-04-14 | |
| TASK-spa-full-reading-display | Implement full reading display: Western, BaZi Four Pillars, Wu-Xing balance, Nakshatra, soulprint sectors, signature blueprint | P1 | Todo | [REQ-F-reading-unlock](../1-spec/requirements/REQ-F-reading-unlock.md) | TASK-spa-checkout-flow | 2026-04-14 | |
| TASK-spa-partner-input | Extend InputSection with partner birth data fields when partnership path is selected | P1 | Todo | [REQ-F-birth-data-input](../1-spec/requirements/REQ-F-birth-data-input.md) | TASK-spa-input-form-api, TASK-bff-partnership-reading | 2026-04-14 | |
| TASK-spa-partnership-display | Extend RevealSection for partnership teaser and full reading (both persons) | P1 | Todo | [REQ-F-teaser-preview](../1-spec/requirements/REQ-F-teaser-preview.md), [REQ-F-reading-unlock](../1-spec/requirements/REQ-F-reading-unlock.md) | TASK-spa-partner-input, TASK-bff-partnership-unlock | 2026-04-14 | |
| TASK-spa-i18n-setup | Install and configure i18n library, create `de.json` and `en.json` translation files, set up locale detection | P1 | Todo | [REQ-F-i18n](../1-spec/requirements/REQ-F-i18n.md) | - | 2026-04-14 | |
| TASK-spa-i18n-sections | Externalize all hardcoded strings in Hero, TwoPaths, HowItWorks, SampleReadings, and Closing sections | P1 | Todo | [REQ-F-i18n](../1-spec/requirements/REQ-F-i18n.md) | TASK-spa-i18n-setup | 2026-04-14 | |
| TASK-spa-i18n-forms | Externalize strings in InputSection and RevealSection (labels, validation, placeholders, CTA text) | P1 | Todo | [REQ-F-i18n](../1-spec/requirements/REQ-F-i18n.md) | TASK-spa-i18n-setup | 2026-04-14 | |
| TASK-spa-language-toggle | Implement visible language toggle; switch must not reset form state or scroll position | P1 | Todo | [REQ-F-i18n](../1-spec/requirements/REQ-F-i18n.md) | TASK-spa-i18n-setup | 2026-04-14 | |
| TASK-spa-i18n-astrology-terms | Review and add correct DE/EN translations for astrology terminology | P1 | Todo | [REQ-F-i18n](../1-spec/requirements/REQ-F-i18n.md) | TASK-spa-i18n-sections | 2026-04-14 | Zodiac signs, BaZi terms, Nakshatra names, element names |
| TASK-spa-cookie-banner | Implement cookie consent banner: first-visit display, granular options, persist preference, block non-essential scripts | P1 | Todo | [REQ-COMP-cookie-banner](../1-spec/requirements/REQ-COMP-cookie-banner.md) | - | 2026-04-14 | |
| TASK-spa-impressum | Create Impressum page/modal with legally required content, accessible via footer | P1 | Todo | [REQ-COMP-impressum](../1-spec/requirements/REQ-COMP-impressum.md) | - | 2026-04-14 | German version mandatory |
| TASK-spa-privacy-policy | Create Datenschutzerklärung page/modal covering all data processing (birth data, Stripe, FuFirE API) | P1 | Todo | [REQ-COMP-privacy-policy](../1-spec/requirements/REQ-COMP-privacy-policy.md) | - | 2026-04-14 | German version mandatory |
| TASK-spa-footer-legal-links | Add persistent footer with Impressum, Datenschutz, and cookie settings links | P1 | Todo | [REQ-COMP-impressum](../1-spec/requirements/REQ-COMP-impressum.md), [REQ-COMP-privacy-policy](../1-spec/requirements/REQ-COMP-privacy-policy.md), [REQ-COMP-cookie-banner](../1-spec/requirements/REQ-COMP-cookie-banner.md) | TASK-spa-impressum, TASK-spa-privacy-policy, TASK-spa-cookie-banner | 2026-04-14 | |
| TASK-spa-font-loading | Configure font-display: swap for Cormorant Garamond, Inter, IBM Plex Mono to prevent FOIT | P1 | Todo | [REQ-PERF-initial-load](../1-spec/requirements/REQ-PERF-initial-load.md) | - | 2026-04-14 | |
| TASK-spa-bundle-optimization | Analyze bundle size, configure code splitting, verify FCP targets with Lighthouse | P1 | Todo | [REQ-PERF-initial-load](../1-spec/requirements/REQ-PERF-initial-load.md) | TASK-spa-full-reading-display, TASK-spa-i18n-sections | 2026-04-14 | FCP < 3s on 4G, < 1.5s on broadband |
| TASK-spa-responsive-audit | Test and fix layout across 320px, 768px, 1440px, 2560px viewports; ensure 44px minimum tap targets | P1 | Todo | [REQ-USA-responsive-layout](../1-spec/requirements/REQ-USA-responsive-layout.md) | TASK-spa-full-reading-display | 2026-04-14 | |
| TASK-spa-animation-performance | Audit GSAP ScrollTrigger performance, ensure >55fps, implement graceful degradation, verify CLS < 0.1 | P1 | Todo | [REQ-USA-scroll-animations](../1-spec/requirements/REQ-USA-scroll-animations.md) | TASK-spa-full-reading-display | 2026-04-14 | |

---

### Deploy & Operations

| ID | Task | Priority | Status | Req | Dependencies | Updated | Notes |
|----|------|----------|--------|-----|--------------|---------|-------|
| TASK-phase-1-manual-testing | Create deploy runbook with startup instructions and health check verification | P0 | Todo | - | TASK-railway-build-config | 2026-04-14 | |
| TASK-phase-2-manual-testing | Update runbook with birth data entry → teaser display test scenario | P1 | Todo | - | TASK-spa-teaser-display | 2026-04-14 | |
| TASK-phase-3-manual-testing | Update runbook with complete character purchase flow test scenario (Stripe test mode) | P1 | Todo | - | TASK-spa-full-reading-display | 2026-04-14 | |
| TASK-phase-4-manual-testing | Update runbook with partnership reading flow test scenario | P1 | Todo | - | TASK-spa-partnership-display | 2026-04-14 | |
| TASK-phase-5-manual-testing | Update runbook with language switching test scenarios | P1 | Todo | - | TASK-spa-i18n-astrology-terms, TASK-spa-language-toggle | 2026-04-14 | |
| TASK-phase-6-manual-testing | Update runbook with cookie consent, Impressum, and privacy policy test scenarios | P1 | Todo | - | TASK-spa-footer-legal-links | 2026-04-14 | |
| TASK-phase-7-manual-testing | Final runbook with Lighthouse audit, responsive testing matrix, security verification | P1 | Todo | - | TASK-bff-security-audit, TASK-spa-animation-performance, TASK-spa-responsive-audit, TASK-spa-bundle-optimization | 2026-04-14 | |

---

## Execution Plan

### Phase 1: BFF Scaffold & Railway Deployment

**Capabilities delivered:**
- Express server starts and serves existing SPA
- Health check responds for Railway probes
- Deployable to Railway with env var configuration

**Tasks:**
1. TASK-bff-express-scaffold
2. TASK-bff-health-endpoint
3. TASK-bff-static-serving
4. TASK-bff-env-config
5. TASK-bff-error-format
6. TASK-railway-build-config
7. TASK-phase-1-manual-testing

### Phase 2: Character Reading Flow (Teaser)

**Capabilities delivered:**
- User enters birth data → receives personalized teaser reading from FuFirE
- Teaser shows zodiac, BaZi, Nakshatra, element summary, preview text

**Tasks:**
1. TASK-shared-types
2. TASK-bff-session-store
3. TASK-bff-reading-endpoint
4. TASK-bff-no-pii-logging
5. TASK-spa-input-form-api
6. TASK-spa-teaser-display
7. TASK-phase-2-manual-testing

### Phase 3: Payment & Full Reading Unlock

**Capabilities delivered:**
- Complete character purchase flow: teaser → pay → full reading
- Stripe Checkout integration with webhook validation

**Tasks:**
1. TASK-bff-checkout-endpoint
2. TASK-bff-unlock-endpoint
3. TASK-bff-webhook-endpoint
4. TASK-spa-checkout-flow
5. TASK-spa-full-reading-display
6. TASK-phase-3-manual-testing

### Phase 4: Partnership Path

**Capabilities delivered:**
- Partnership reading: two birth data sets → teaser for both → pay → full reading for both

**Tasks:**
1. TASK-bff-partnership-reading
2. TASK-bff-partnership-unlock
3. TASK-spa-partner-input
4. TASK-spa-partnership-display
5. TASK-phase-4-manual-testing

### Phase 5: Internationalization (DE + EN)

**Capabilities delivered:**
- All text externalized, browser locale detection, manual toggle, bilingual astrology terms

**Tasks:**
1. TASK-spa-i18n-setup
2. TASK-spa-i18n-sections
3. TASK-spa-i18n-forms
4. TASK-spa-language-toggle
5. TASK-spa-i18n-astrology-terms
6. TASK-phase-5-manual-testing

### Phase 6: Legal & Compliance

**Capabilities delivered:**
- Cookie consent, Impressum, Datenschutzerklärung, legal footer links

**Tasks:**
1. TASK-spa-cookie-banner
2. TASK-spa-impressum
3. TASK-spa-privacy-policy
4. TASK-spa-footer-legal-links
5. TASK-phase-6-manual-testing

### Phase 7: Performance, Responsive Audit & Security Hardening

**Capabilities delivered:**
- FCP < 3s on 4G, responsive 320px–2560px, 60fps animations, security verification

**Tasks:**
1. TASK-spa-font-loading
2. TASK-spa-bundle-optimization
3. TASK-spa-responsive-audit
4. TASK-spa-animation-performance
5. TASK-bff-security-audit
6. TASK-phase-7-manual-testing
