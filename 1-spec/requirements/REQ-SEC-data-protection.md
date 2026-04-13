# REQ-SEC-data-protection: Personal data and payment security

**Type**: Security

**Status**: Draft

**Priority**: Must-have

**Source**: [GOAL-legal-launch-readiness](../goals/GOAL-legal-launch-readiness.md), [CON-gdpr-compliance](../constraints/CON-gdpr-compliance.md)

**Source stakeholder**: [STK-founder](../stakeholders.md)

## Description

All personal data (birth dates, birth times) and payment data must be handled securely. The system must prevent accidental exposure of PII in logs, client-side state, or URLs.

## Acceptance Criteria

- Given any data transmission between client and server (FuFirE API, Stripe, Railway backend), when data is sent, then it uses HTTPS exclusively — no plaintext HTTP
- Given birth data is processed, when the application logs requests, then no PII (birth date, birth time, names) appears in server logs or browser console in production
- Given Stripe webhooks are received, when processing payment confirmation, then the webhook signature is validated before acting on the payload
- Given a user completes a reading, when the reading is stored or cached, then it is associated with a session token — not with raw PII in URLs or query parameters
- Given the application's client-side code, when inspected, then no API keys, Stripe secret keys, or backend credentials are exposed in the JavaScript bundle

## Related Constraints

- [CON-gdpr-compliance](../constraints/CON-gdpr-compliance.md) — DSGVO requires appropriate technical measures for personal data
