# US-character-reading-flow: Complete character reading flow

**As a** first-time visitor, **I want** to enter my birth date and receive a personalized tri-system character reading, **so that** I can discover my astrology profile across Western, BaZi, and Vedic systems.

**Status**: Draft

**Priority**: Must-have

**Source stakeholder**: [STK-visitor](../stakeholders.md)

**Related goal**: [GOAL-convert-visitors](../goals/GOAL-convert-visitors.md)

## Acceptance Criteria

- Given I select the "character" path, when I enter my birth date and time, then the system generates a fused reading combining Western zodiac, BaZi, and Nakshatra
- Given I have not paid, when the reading is generated, then I see a teaser/preview but not the full analysis
- Given I complete payment, when redirected back, then I see my complete personalized reading

## Derived Requirements

- [REQ-F-birth-data-input](../requirements/REQ-F-birth-data-input.md)
- [REQ-F-reading-generation](../requirements/REQ-F-reading-generation.md)
- [REQ-F-teaser-preview](../requirements/REQ-F-teaser-preview.md)
