# REQ-USA-responsive-layout: Fully responsive layout

**Type**: Usability

**Status**: Approved

**Priority**: Must-have

**Source**: [US-mobile-experience](../user-stories/US-mobile-experience.md)

**Source stakeholder**: [STK-visitor](../stakeholders.md)

## Description

All sections, components, and interactive elements must be fully functional and visually correct across viewports from 320px to 2560px width.

## Acceptance Criteria

- Given a viewport width of 320px (small phone), when scrolling through all sections, then no horizontal overflow occurs, no text is cut off, and all interactive elements are reachable
- Given a viewport width of 768px (tablet), when viewing the input section, then the form layout adapts appropriately (no wasted space, no cramped fields)
- Given a viewport width of 1440px+ (desktop), when viewing the page, then the layout uses the available space without stretching content beyond readable widths
- Given any viewport, when interacting with form fields on touch devices, then inputs are at least 44x44px tap targets
