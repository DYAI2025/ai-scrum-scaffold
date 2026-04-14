# REQ-F-reading-unlock: Unlock full reading after payment

**Type**: Functional

**Status**: Approved

**Priority**: Must-have

**Source**: [US-paywall-checkout](../user-stories/US-paywall-checkout.md)

**Source stakeholder**: [STK-founder](../stakeholders.md)

## Description

After successful payment, the full reading replaces the teaser. The unlock must be tied to verified payment, not client-side manipulation.

## Acceptance Criteria

- Given payment is verified, when the reveal section renders, then the full reading content is displayed (complete personality analysis, element breakdown, all three system details)
- Given the user revisits after payment (e.g., via a unique URL or session token), when they access their reading, then the full content is still available without repayment
- Given no payment is verified, when the reveal section renders, then only the teaser is shown regardless of client-side state manipulation
