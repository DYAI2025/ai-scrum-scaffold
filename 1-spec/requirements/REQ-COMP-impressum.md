# REQ-COMP-impressum: Legally compliant Impressum

**Type**: Compliance

**Status**: Approved

**Priority**: Must-have

**Source**: [US-legal-pages](../user-stories/US-legal-pages.md)

**Source stakeholder**: [STK-founder](../stakeholders.md)

## Description

The site must include an Impressum page that meets the requirements of German TMG/DDG (Telemediengesetz / Digitale-Dienste-Gesetz) for commercial websites.

## Acceptance Criteria

- Given any page state, when the user looks for legal information, then an Impressum link is visible (e.g., in the footer or navigation)
- Given the user clicks the Impressum link, when the page loads, then it contains: full legal name, postal address, contact email, phone number (or equivalent contact form), and any applicable registration numbers
- Given the site is bilingual, when viewing the Impressum, then at minimum the German version is available

## Related Constraints

- [CON-gdpr-compliance](../constraints/CON-gdpr-compliance.md) — legal compliance for DACH market
