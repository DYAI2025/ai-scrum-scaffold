# US-language-switch: Switch between German and English

**As a** visitor, **I want** to use the site in German or English, **so that** I can understand the content and readings in my preferred language.

**Status**: Draft

**Priority**: Must-have

**Source stakeholder**: [STK-visitor](../stakeholders.md)

**Related goal**: [GOAL-immersive-experience](../goals/GOAL-immersive-experience.md)

## Acceptance Criteria

- Given I visit the page, when my browser locale is German, then the page defaults to German
- Given I visit the page, when my browser locale is not German, then the page defaults to English
- Given I am on the page in any language, when I use the language toggle, then all user-facing text switches to the selected language without page reload
- Given I switch language, when I am mid-flow (e.g., on the input section), then my progress and entered data are preserved

## Derived Requirements

- [REQ-F-i18n](../requirements/REQ-F-i18n.md)
