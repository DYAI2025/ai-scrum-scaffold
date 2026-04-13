# REQ-F-i18n: Internationalization — German and English

**Type**: Functional

**Status**: Draft

**Priority**: Must-have

**Source**: [US-language-switch](../user-stories/US-language-switch.md)

**Source stakeholder**: [STK-visitor](../stakeholders.md)

## Description

All user-facing text must be externalized into translation files. The system supports German (de) and English (en) with browser locale detection and a manual language toggle.

## Acceptance Criteria

- Given the user's browser locale is `de` or `de-*`, when the page loads, then content is displayed in German
- Given the user's browser locale is anything other than `de`, when the page loads, then content is displayed in English
- Given the user clicks the language toggle, when the language changes, then all visible text updates without page reload and without losing form state or scroll position
- Given a new UI string is added, when it is hardcoded instead of using the i18n system, then the build or lint step flags it as an error (or equivalent enforcement mechanism)
- Given astrology terminology (zodiac signs, BaZi terms, Nakshatras), when displayed, then the translations are reviewed for domain correctness in both languages

## Related Constraints

- [CON-bilingual](../constraints/CON-bilingual.md) — German + English from launch
