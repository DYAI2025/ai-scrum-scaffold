# REQ-F-payment-integration: Stripe checkout for reading purchase

**Type**: Functional

**Status**: Draft

**Priority**: Must-have

**Source**: [US-paywall-checkout](../user-stories/US-paywall-checkout.md)

**Source stakeholder**: [STK-founder](../stakeholders.md)

## Description

The system integrates Stripe Checkout to process payments for astrology readings. The checkout flow must handle the transition from teaser to paid reading seamlessly.

## Acceptance Criteria

- Given the user clicks the unlock/purchase CTA, when redirected to Stripe, then a Checkout Session is created with the correct price and product metadata
- Given payment succeeds, when Stripe redirects back, then the full reading is immediately accessible
- Given payment fails or is cancelled, when the user returns, then the teaser remains visible and a retry option is available
- Given a payment is completed, when verified server-side (webhook or session retrieval), then the reading unlock is persisted (not just client-side state)
