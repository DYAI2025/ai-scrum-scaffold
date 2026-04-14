# REQ-F-reading-generation: Tri-system reading generation

**Type**: Functional

**Status**: Approved

**Priority**: Must-have

**Source**: [US-character-reading-flow](../user-stories/US-character-reading-flow.md), [US-partnership-reading-flow](../user-stories/US-partnership-reading-flow.md)

**Source stakeholder**: [STK-visitor](../stakeholders.md), [STK-couple](../stakeholders.md)

## Description

Given valid birth data, the system generates a fused astrology reading combining Western zodiac, Chinese BaZi (Heavenly Stems + Earthly Branches), and Vedic Nakshatra. For partnership readings, the system additionally generates compatibility analysis between the two profiles.

## Acceptance Criteria

- Given valid birth data for one person, when a character reading is requested, then the system returns Western zodiac sign, BaZi pillars (year stem/branch at minimum), Nakshatra, and a fused element profile
- Given valid birth data for two people, when a partnership reading is requested, then the system returns individual profiles plus a compatibility analysis
- Given the FuFirE API is available, when a reading is requested, then the system uses the API for calculation
- Given the FuFirE API is unavailable, when a reading is requested, then the system falls back to client-side calculation (`src/utils/astrology.ts`)

## Related Assumptions

- [ASM-fufire-api-available](../assumptions/ASM-fufire-api-available.md) — API availability determines calculation accuracy
