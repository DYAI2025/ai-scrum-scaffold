# REQ-USA-landing-reading-center-snap: Landing Reading Center Snap

**Type**: Usability

**Status**: Draft

**Priority**: Must-have

**Source**: [GOAL-immersive-experience](../goals/GOAL-immersive-experience.md)

**Source stakeholder**: [STK-visitor](../stakeholders.md), [STK-couple](../stakeholders.md)

## Description

The landing reading selector (TwoPathsSection) shall provide a stable, selectable center state when the Character Portrait and Partnership frames reach the central reading-choice zone. The interaction shall use a one-second snap/hold state so the user can stop scrolling and still select either path without overshooting the target state.

The hold must not trap the user. Reverse-direction scroll must cancel the hold immediately and allow the selector to move back in the opposite direction.

## Current Defect

The selector currently overshoots the centered selectable state during normal scroll input. Users can briefly reach the intended Character Portrait / Partnership alignment, but continued wheel or trackpad movement pushes the frames past the selectable moment. This makes selection feel unstable and requires corrective micro-scrolling.

**Reproduction:**
1. Open the landing page
2. Scroll toward the point where Character Portrait and Partnership align around the center reading-choice prompt
3. Continue a normal wheel or trackpad gesture
4. Observe that the selector overshoots and drifts back toward the outer positions

**Suspected area:** `TwoPathsSection.tsx` — scroll interpolation, snap-window logic, or GSAP ScrollTrigger `scrub` configuration around the center choice position.

## Acceptance Criteria

- Given the reading selector is moving toward the center choice state, when either frame enters the defined snap corridor around the centered selectable position, then the selector snaps into the centered state.
- Given the selector has snapped into the centered state, when the hold begins, then the centered state remains stable for 1000ms +/- 150ms.
- Given the selector is in the hold state, when the user continues scrolling in the same direction, then the selector remains within the snap corridor until the hold window ends.
- Given the selector is in the hold state, when the user scrolls in the opposite direction, then the hold is cancelled within 100ms and motion resumes in the reverse direction.
- Given the selector is in the centered hold state, when the user taps or clicks either path, then both paths are selectable without requiring corrective micro-scroll.
- Given desktop mouse-wheel and trackpad input, when QA repeats the interaction five times from rest, then the centered selectable state can be reached and activated in 5/5 attempts.
- Given touch input on mobile, when the selector reaches the centered state, then the user can activate either option without accidental drift out of the selectable zone.
- Given the selector is rendered, when the user perceives the snap transition, then the motion uses eased, deliberate animation (not instant jumps) consistent with GSAP ScrollTrigger patterns.

## Related User Stories

- [US-scroll-journey](../user-stories/US-scroll-journey.md)
- [US-character-reading-flow](../user-stories/US-character-reading-flow.md)
- [US-partnership-reading-flow](../user-stories/US-partnership-reading-flow.md)

## Related Constraints

- None

## Related Artifacts

- [REQ-F-landing-dual-reading-entry](REQ-F-landing-dual-reading-entry.md) — visual and copy changes to the same section
- [REQ-USA-scroll-animations](REQ-USA-scroll-animations.md) — 60fps target, graceful degradation
- [REQ-USA-responsive-layout](REQ-USA-responsive-layout.md) — touch target sizes and mobile behavior
