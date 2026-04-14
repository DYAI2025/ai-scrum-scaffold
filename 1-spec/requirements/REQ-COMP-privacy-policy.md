# REQ-COMP-privacy-policy: Comprehensive privacy policy

**Type**: Compliance

**Status**: Approved

**Priority**: Must-have

**Source**: [US-legal-pages](../user-stories/US-legal-pages.md)

**Source stakeholder**: [STK-founder](../stakeholders.md)

## Description

The site must include a Datenschutzerklarung (privacy policy) that covers all data processing activities, compliant with DSGVO/GDPR.

## Acceptance Criteria

- Given any page state, when the user looks for privacy information, then a Datenschutz link is visible (e.g., in the footer)
- Given the user clicks the Datenschutz link, when the page loads, then it discloses: what personal data is collected (birth date, birth time, payment data), the legal basis for processing, how data is stored and for how long, third-party processors (Stripe, FuFirE API if used), and user rights (access, deletion, portability, complaint to supervisory authority)
- Given birth data is transmitted to the FuFirE API, when the privacy policy is reviewed, then API data transmission is explicitly disclosed with the legal basis
- Given the site is bilingual, when viewing the privacy policy, then at minimum the German version is available

## Related Constraints

- [CON-gdpr-compliance](../constraints/CON-gdpr-compliance.md) — DSGVO/GDPR compliance required
