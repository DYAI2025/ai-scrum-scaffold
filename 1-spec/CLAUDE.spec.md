Phase-specific instructions for the **Specification** phase. Extends [../CLAUDE.md](../CLAUDE.md).

## Purpose

This phase defines **what** we're building and **why**. Focus on clarity, measurability, and alignment with stakeholder needs.

## Phase artifacts

| Artifact | Location | Purpose |
|----------|----------|---------|
| Stakeholders | [`stakeholders.md`](stakeholders.md) | Roles with interests and influence |
| Goals | [`goals/`](goals/) | High-level outcomes |
| User Stories | [`user-stories/`](user-stories/) | User-facing capabilities |
| Requirements | [`requirements/`](requirements/) | Testable system requirements |
| Assumptions | [`assumptions/`](assumptions/) | Beliefs taken as true but not verified |
| Constraints | [`constraints/`](constraints/) | Hard limits on design and implementation |

---

## AI Guidelines

### Per-artifact guidance

**Stakeholders**: ask who uses, funds, operates, or is affected by the system. Record influence level honestly — it drives conflict resolution. Add entries to [`stakeholders.md`](stakeholders.md).

**Goals**: decompose vague ideas into concrete, measurable outcomes. Use MoSCoW priority consistently.
Status lifecycle: `Draft → Approved → Achieved → Deprecated`. Only a human can approve or deprecate. The agent marks `Achieved` when all success criteria are met (linked requirements implemented).

**User Stories**: use "As a [role], I want [capability], so that [benefit]." The role must be an existing stakeholder ID. Acceptance criteria at the story level are high-level; detailed criteria live in requirements.
Status lifecycle: `Draft → Approved → Implemented → Deprecated`. Only a human can approve or deprecate. The agent marks `Implemented` when all linked requirements reach `Implemented`.

**Requirements**: use clear, testable language (not "should be fast" — use "response time < 200ms at p95"). Choose the correct requirement class.
Requirement classes: `REQ-F` Functional, `REQ-PERF` Performance, `REQ-SEC` Security, `REQ-REL` Reliability, `REQ-USA` Usability, `REQ-MNT` Maintainability, `REQ-PORT` Portability, `REQ-SCA` Scalability, `REQ-COMP` Compliance.
Status lifecycle: `Draft → Approved → Implemented → Deprecated`. Only a human can approve or deprecate. The agent marks `Implemented` when all linked tasks reach Done.

**Assumptions**: always record the risk level (what happens if wrong?) and a verification plan when possible.
Status lifecycle: `Unverified → Verified | Invalidated`. The agent marks `Verified` when the verification plan confirms the assumption. Only a human can mark `Invalidated` (triggers impact analysis on dependent artifacts).

**Constraints**: consider technical (platforms, dependencies), business (budget, timeline, team size), and operational (hosting, compliance) categories.
Status lifecycle: `Active → Lifted`. Only a human can lift a constraint.

### Conflict resolution

A conflict exists when two or more requirements cannot both be satisfied as stated.

**Never resolve a conflict silently.** Always surface it before acting.

1. **Identify**: note conflicting requirement IDs, source stakeholders, influence levels, and why they are incompatible.
2. **Ask the user**: present what makes them incompatible, stakeholders and influence levels, two or more resolution options, and a recommended option if one is clearly better.
3. **Wait for explicit approval** before modifying any file.
4. **Apply**: update affected requirement files and index rows. Update dependent user stories or goals if affected. Record a decision if the resolution imposes a recurring constraint.
5. **Verify**: no artifacts remain in a conflicting state after resolution.

### Assumption invalidation

When an assumption is found to be wrong or no longer holds:

1. **Identify impact**: list all artifacts (requirements, user stories, decisions) that depend on the invalidated assumption.
2. **Ask the user**: present the invalidated assumption, the affected artifacts, and proposed adjustments or alternatives.
3. **Wait for explicit approval** before modifying any file.
4. **Apply**: change the assumption's Status to `Invalidated`. Update or flag all dependent artifacts as directed.
5. **Verify**: no artifacts remain based on the invalidated assumption without acknowledgment.

### Artifact deprecation

When an artifact (goal, user story, requirement) is no longer relevant:

1. Propose deprecation to the user with rationale and downstream impact.
2. Wait for explicit approval.
3. Change Status to `Deprecated` in the artifact file. Update its index row.
4. Check for dependent artifacts — flag any that reference the deprecated item.

---

## Decisions Relevant to This Phase

| File | Title | Trigger |
|------|-------|---------|
<!-- Add rows as decisions are recorded. File column: [DEC-kebab-name](../decisions/DEC-kebab-name.md) -->

---

## Linking to Other Phases

- Goals, user stories, constraints, assumptions, and requirements are referenced in design documents (`2-design/`)
- Requirements determine the development tasks in `3-code/tasks.md`; each task references the requirements it fulfills
- Acceptance criteria inform test cases (`3-code/`)

---

## Goals Index

| File | Priority | Status | Summary |
|------|----------|--------|---------|
| [GOAL-convert-visitors](goals/GOAL-convert-visitors.md) | Must-have | Approved | Convert visitors into paying customers through the scroll funnel |
| [GOAL-immersive-experience](goals/GOAL-immersive-experience.md) | Must-have | Approved | Deliver premium, trust-building scroll experience (mobile, bilingual, fast) |
| [GOAL-legal-launch-readiness](goals/GOAL-legal-launch-readiness.md) | Must-have | Approved | DSGVO, Impressum, Cookie consent for DACH launch |
| [GOAL-sharing-virality](goals/GOAL-sharing-virality.md) | Could-have | Draft | Enable sharing and revisiting of readings (post-launch) |

---

## User Stories Index

| File | Role | Priority | Status | Summary |
|------|------|----------|--------|---------|
| [US-character-reading-flow](user-stories/US-character-reading-flow.md) | STK-visitor | Must-have | Draft | Enter birth date, receive character reading with teaser → paywall → full |
| [US-partnership-reading-flow](user-stories/US-partnership-reading-flow.md) | STK-couple | Must-have | Draft | Enter both birth dates, receive compatibility reading with teaser → paywall → full |
| [US-paywall-checkout](user-stories/US-paywall-checkout.md) | STK-founder | Must-have | Draft | Payment required before full reading is unlocked |
| [US-scroll-journey](user-stories/US-scroll-journey.md) | STK-visitor | Must-have | Draft | Smooth animated scroll with pinned sections and snap |
| [US-mobile-experience](user-stories/US-mobile-experience.md) | STK-visitor | Must-have | Draft | Full-quality responsive experience on mobile |
| [US-language-switch](user-stories/US-language-switch.md) | STK-visitor | Must-have | Draft | Switch between German and English with locale detection |
| [US-cookie-consent](user-stories/US-cookie-consent.md) | STK-visitor | Must-have | Draft | Cookie consent banner with granular control |
| [US-legal-pages](user-stories/US-legal-pages.md) | STK-founder | Must-have | Draft | Impressum and Datenschutzerklarung accessible from all states |

---

## Requirements Index

| File | Type | Priority | Status | Summary |
|------|------|----------|--------|---------|
| [REQ-F-birth-data-input](requirements/REQ-F-birth-data-input.md) | Functional | Must-have | Approved | Birth date/time form, single + partner mode |
| [REQ-F-reading-generation](requirements/REQ-F-reading-generation.md) | Functional | Must-have | Approved | Tri-system reading (API or client-side fallback) |
| [REQ-F-teaser-preview](requirements/REQ-F-teaser-preview.md) | Functional | Must-have | Approved | Teaser preview before payment (max 30% of full reading) |
| [REQ-F-payment-integration](requirements/REQ-F-payment-integration.md) | Functional | Must-have | Approved | Stripe Checkout for reading purchase |
| [REQ-F-reading-unlock](requirements/REQ-F-reading-unlock.md) | Functional | Must-have | Approved | Unlock full reading after verified payment |
| [REQ-F-i18n](requirements/REQ-F-i18n.md) | Functional | Must-have | Approved | DE + EN, locale detection, manual toggle |
| [REQ-PERF-initial-load](requirements/REQ-PERF-initial-load.md) | Performance | Must-have | Approved | FCP < 3s on 4G, < 1.5s on broadband |
| [REQ-USA-responsive-layout](requirements/REQ-USA-responsive-layout.md) | Usability | Must-have | Approved | Responsive 320px-2560px, 44px tap targets |
| [REQ-USA-scroll-animations](requirements/REQ-USA-scroll-animations.md) | Usability | Must-have | Approved | 60fps GSAP animations, graceful degradation |
| [REQ-COMP-cookie-banner](requirements/REQ-COMP-cookie-banner.md) | Compliance | Must-have | Approved | Cookie consent, blocks non-essential until opted in |
| [REQ-COMP-impressum](requirements/REQ-COMP-impressum.md) | Compliance | Must-have | Approved | TMG/DDG-compliant Impressum |
| [REQ-COMP-privacy-policy](requirements/REQ-COMP-privacy-policy.md) | Compliance | Must-have | Approved | DSGVO-compliant Datenschutzerklarung |
| [REQ-SEC-data-protection](requirements/REQ-SEC-data-protection.md) | Security | Must-have | Approved | HTTPS, no PII in logs, Stripe webhook validation |
| [REQ-MNT-railway-deploy](requirements/REQ-MNT-railway-deploy.md) | Maintainability | Must-have | Approved | Build and deploy on Railway with env-var config |
| [REQ-F-landing-dual-reading-entry](requirements/REQ-F-landing-dual-reading-entry.md) | Functional | Must-have | Draft | Centered Bazodiac header, dual reading choice copy, image asset swaps |
| [REQ-USA-landing-reading-center-snap](requirements/REQ-USA-landing-reading-center-snap.md) | Usability | Must-have | Draft | 1s center snap/hold prevents selector overshoot; reverse-scroll escape |

---

## Assumptions Index

| File | Category | Status | Risk | Summary |
|------|----------|--------|------|---------|
| [ASM-fufire-api-available](assumptions/ASM-fufire-api-available.md) | Technology | Verified | High | FuFirE API is deployed and accessible for integration |

---

## Constraints Index

| File | Category | Status | Summary |
|------|----------|--------|---------|
| [CON-railway-deployment](constraints/CON-railway-deployment.md) | Technical | Active | Deployment on Railway |
| [CON-launch-deadline](constraints/CON-launch-deadline.md) | Business | Active | Production-ready within ~2 weeks (target 2026-04-27, negotiable) |
| [CON-gdpr-compliance](constraints/CON-gdpr-compliance.md) | Operational | Active | DSGVO/GDPR, Impressum, Cookie consent required |
| [CON-bilingual](constraints/CON-bilingual.md) | Business | Active | German + English from launch |
| [CON-no-free-tier](constraints/CON-no-free-tier.md) | Business | Active | No freemium — all readings are paid |
