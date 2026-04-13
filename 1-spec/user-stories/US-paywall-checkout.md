# US-paywall-checkout: Paywall and checkout before full reading

**As a** founder, **I want** visitors to pay before accessing their full reading, **so that** the business model is sustainable and the core value is not given away for free.

**Status**: Draft

**Priority**: Must-have

**Source stakeholder**: [STK-founder](../stakeholders.md)

**Related goal**: [GOAL-convert-visitors](../goals/GOAL-convert-visitors.md)

## Acceptance Criteria

- Given a visitor has entered birth data and a teaser is shown, when they click to unlock the full reading, then they are presented with a payment flow
- Given the visitor completes payment, when the transaction succeeds, then the full reading is unlocked immediately
- Given payment fails or is abandoned, when the visitor returns, then the teaser remains visible and they can retry payment

## Derived Requirements

- [REQ-F-teaser-preview](../requirements/REQ-F-teaser-preview.md)
- [REQ-F-payment-integration](../requirements/REQ-F-payment-integration.md)
- [REQ-F-reading-unlock](../requirements/REQ-F-reading-unlock.md)
