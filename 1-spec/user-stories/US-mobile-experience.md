# US-mobile-experience: Full-quality mobile experience

**As a** mobile visitor, **I want** the same quality experience as on desktop, **so that** I can discover and purchase a reading from my phone.

**Status**: Draft

**Priority**: Must-have

**Source stakeholder**: [STK-visitor](../stakeholders.md)

**Related goal**: [GOAL-immersive-experience](../goals/GOAL-immersive-experience.md)

## Acceptance Criteria

- Given I visit on a phone (viewport < 768px), when I scroll through all sections, then layout, typography, and animations are adapted — no horizontal overflow, no cut-off text, no broken animations
- Given I am on a phone, when I fill in the birth data form, then input fields are touch-friendly and the keyboard does not obscure the form
- Given I am on a phone, when the page loads, then the initial load completes in under 3 seconds on a 4G connection

## Derived Requirements

- [REQ-USA-responsive-layout](../requirements/REQ-USA-responsive-layout.md)
- [REQ-PERF-initial-load](../requirements/REQ-PERF-initial-load.md)
