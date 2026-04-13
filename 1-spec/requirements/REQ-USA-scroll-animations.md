# REQ-USA-scroll-animations: Smooth GSAP scroll animations

**Type**: Usability

**Status**: Draft

**Priority**: Must-have

**Source**: [US-scroll-journey](../user-stories/US-scroll-journey.md)

**Source stakeholder**: [STK-visitor](../stakeholders.md)

## Description

All GSAP ScrollTrigger animations must run at 60fps without jank, layout shifts, or visual glitches on modern browsers (Chrome, Safari, Firefox — latest two major versions).

## Acceptance Criteria

- Given a user scrolls through pinned sections, when the scrub animation plays, then the frame rate stays above 55fps (measured via Chrome DevTools Performance panel)
- Given a user scrolls between pinned sections, when they release, then the snap settles within 350ms with no overshoot or bounce
- Given a user on a low-powered mobile device (e.g., mid-range Android), when scrolling, then animations degrade gracefully (reduced complexity) rather than janking
- Given the page loads, when all sections are initialized, then no Cumulative Layout Shift (CLS) exceeds 0.1
