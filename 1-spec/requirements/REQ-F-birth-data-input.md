# REQ-F-birth-data-input: Birth data input form

**Type**: Functional

**Status**: Approved

**Priority**: Must-have

**Source**: [US-character-reading-flow](../user-stories/US-character-reading-flow.md), [US-partnership-reading-flow](../user-stories/US-partnership-reading-flow.md)

**Source stakeholder**: [STK-visitor](../stakeholders.md), [STK-couple](../stakeholders.md)

## Description

The input section provides a form for entering birth date and optional birth time. When the partnership path is selected, the form displays a second set of fields for the partner's birth data.

## Acceptance Criteria

- Given the character path is selected, when the input section is shown, then the form displays birth date (required) and birth time (optional) fields
- Given the partnership path is selected, when the input section is shown, then the form displays two sets of birth date/time fields (one per person)
- Given any field is invalid (e.g., future date, incomplete), when the user attempts to submit, then a clear validation message is shown and submission is prevented
- Given all required fields are valid, when the user submits, then the form data is passed to the reading generation step
