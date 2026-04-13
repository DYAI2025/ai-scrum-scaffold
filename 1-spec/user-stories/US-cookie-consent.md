# US-cookie-consent: Cookie consent control

**As a** visitor, **I want** to control which cookies are set on my device, **so that** my privacy is respected and I can make an informed choice.

**Status**: Draft

**Priority**: Must-have

**Source stakeholder**: [STK-visitor](../stakeholders.md)

**Related goal**: [GOAL-legal-launch-readiness](../goals/GOAL-legal-launch-readiness.md)

## Acceptance Criteria

- Given I visit the page for the first time, when the page loads, then a cookie consent banner is displayed before any non-essential cookies or tracking scripts are set
- Given the consent banner is shown, when I accept all or select specific categories, then only the consented categories are activated
- Given I have previously made a choice, when I return to the site, then my preference is remembered and the banner does not reappear
- Given I want to change my preference, when I access cookie settings (e.g., via footer link), then I can update my consent

## Derived Requirements

- [REQ-COMP-cookie-banner](../requirements/REQ-COMP-cookie-banner.md)
