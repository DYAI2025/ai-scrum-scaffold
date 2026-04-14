# REQ-F-teaser-preview: Teaser preview before payment

**Type**: Functional

**Status**: Approved

**Priority**: Must-have

**Source**: [US-character-reading-flow](../user-stories/US-character-reading-flow.md), [US-paywall-checkout](../user-stories/US-paywall-checkout.md)

**Source stakeholder**: [STK-founder](../stakeholders.md)

## Description

After birth data is submitted, the user sees a teaser preview of their reading — enough to demonstrate value and personalization, but not the full analysis. The teaser must create desire to unlock the full reading.

## Acceptance Criteria

- Given a reading has been generated, when the reveal section is shown to an unpaid user, then it displays: zodiac sign, BaZi animal, Nakshatra name, and a brief personality summary
- Given the teaser is shown, when the user views it, then a clear CTA to unlock the full reading is visible
- Given the teaser is shown, when comparing to the full reading, then the teaser contains no more than 30% of the full reading content

## Related Constraints

- [CON-no-free-tier](../constraints/CON-no-free-tier.md) — the teaser must not give away the core value
