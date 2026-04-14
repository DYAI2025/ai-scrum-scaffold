# REQ-COMP-cookie-banner: Cookie consent banner

**Type**: Compliance

**Status**: Approved

**Priority**: Must-have

**Source**: [US-cookie-consent](../user-stories/US-cookie-consent.md)

**Source stakeholder**: [STK-visitor](../stakeholders.md)

## Description

A GDPR-compliant cookie consent banner must be displayed on first visit. No non-essential cookies or tracking scripts may fire before the user grants consent.

## Acceptance Criteria

- Given a first-time visitor, when the page loads, then the cookie banner is displayed and no non-essential cookies are set
- Given the banner is displayed, when the user selects their preferences (accept all, reject non-essential, or granular selection), then only consented cookie categories are activated
- Given the user has previously consented, when they return, then their preference is persisted (e.g., via a first-party cookie) and the banner does not reappear
- Given the user wants to change their preference, when they click the cookie settings link (in footer), then the preference UI is shown and changes take effect immediately
- Given the "reject all non-essential" option, when selected, then zero third-party scripts load (no analytics, no tracking pixels)

## Related Constraints

- [CON-gdpr-compliance](../constraints/CON-gdpr-compliance.md) — DSGVO/GDPR compliance required
