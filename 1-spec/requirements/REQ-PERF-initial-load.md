# REQ-PERF-initial-load: Initial page load under 3 seconds

**Type**: Performance

**Status**: Draft

**Priority**: Must-have

**Source**: [US-mobile-experience](../user-stories/US-mobile-experience.md), [US-scroll-journey](../user-stories/US-scroll-journey.md)

**Source stakeholder**: [STK-visitor](../stakeholders.md)

## Description

The landing page must reach First Contentful Paint (FCP) within 3 seconds on a simulated 4G connection (download 1.6 Mbps, upload 750 Kbps, RTT 150ms).

## Acceptance Criteria

- Given a first visit on a simulated 4G connection, when the page loads, then FCP occurs within 3 seconds
- Given a first visit on a desktop broadband connection, when the page loads, then FCP occurs within 1.5 seconds
- Given Google Fonts (Cormorant Garamond, Inter, IBM Plex Mono) are loading, when the page renders, then text is visible with fallback fonts before web fonts load (no FOIT)
