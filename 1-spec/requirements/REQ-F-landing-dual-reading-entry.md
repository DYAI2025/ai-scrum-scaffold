# REQ-F-landing-dual-reading-entry: Landing Dual Reading Entry

**Type**: Functional

**Status**: Draft

**Priority**: Must-have

**Source**: [GOAL-convert-visitors](../goals/GOAL-convert-visitors.md), [GOAL-immersive-experience](../goals/GOAL-immersive-experience.md)

**Source stakeholder**: [STK-visitor](../stakeholders.md), [STK-couple](../stakeholders.md)

## Description

The landing entry experience shall present Bazodiac as a centered, brand-dominant header and offer two distinct reading paths: Character Portrait and Partnership. The shared reading-choice composition shall be more illustrative than the current abstract split state and shall make both choices legible and actionable without copy ambiguity.

The Character Portrait path shall communicate a fused interpretation of Western signs, BaZi, Day Master, and Wu Xing balance in English. The Partnership path shall communicate resonance, tension, and shared timing between two people through Western synastry, BaZi, and fusion astrology in English.

The pre-auth birth-moment screens shall replace their current imagery with the provided assets. The post-submit portrait/result screen shall replace its current hero image with the provided Cosmic Portrait asset.

## Acceptance Criteria

- Given the landing entry header is rendered, when the page loads, then a centered "Bazodiac" wordmark is the primary header element.
- Given the centered wordmark is rendered, when it is styled, then it uses the existing brand heading typography (Cormorant Garamond) and gold accent token (`--gold` / `#C8A14A`) rather than an ad-hoc color treatment.
- Given any secondary top-left brand marker remains present, when the header is viewed, then the centered wordmark remains visually dominant.
- Given the reading-choice section is rendered (TwoPathsSection), when the user reaches the selector area, then Character Portrait is presented as the left path and Partnership as the right path around a centered selection prompt.
- Given the Character Portrait path is shown, when the descriptive copy is rendered, then it communicates a fused portrait of Western signs, BaZi, Day Master, and Wu Xing balance in English.
- Given the Partnership path is shown, when the descriptive copy is rendered, then it communicates resonance, tension, and shared timing between two souls through Western synastry, BaZi, and fusion astrology in English.
- Given the path CTAs are rendered, when the user sees them, then both options use a clear, minimal button treatment that is visually distinct from body copy.
- Given the single-person birth-moment screen (InputSection, character mode) is rendered, then `/birth_moment_1.png` or `/birth_moment_2.png` replaces the current `input_portrait.jpg`.
- Given the couple birth-moment screen (InputSection, partnership mode) is rendered, then the other birth-moment asset replaces the current image.
- Given both birth moment assets are mapped, then the assignment is consistent (same asset always maps to same mode).
- Given the post-submit reveal screen (RevealSection) is rendered, then `/cosmic_portrait.png` replaces the current `reveal_portrait.jpg`.

## Asset Contract

| Asset | File | Replaces | Screen |
|-------|------|----------|--------|
| Birth Moment 1 | `/birth_moment_1.png` | `input_portrait.jpg` | InputSection (character mode) |
| Birth Moment 2 | `/birth_moment_2.png` | `input_portrait.jpg` | InputSection (partnership mode) |
| Cosmic Portrait | `/cosmic_portrait.png` | `reveal_portrait.jpg` | RevealSection |

Note: Birth moment assets are ~10MB each and should be optimized (WebP, compressed) before production deployment.

## Related User Stories

- [US-character-reading-flow](../user-stories/US-character-reading-flow.md)
- [US-partnership-reading-flow](../user-stories/US-partnership-reading-flow.md)
- [US-scroll-journey](../user-stories/US-scroll-journey.md)

## Related Constraints

- [CON-bilingual](../constraints/CON-bilingual.md) — English copy required from launch

## Related Artifacts

- [REQ-USA-landing-reading-center-snap](REQ-USA-landing-reading-center-snap.md) — scroll snap behavior for the path selector
- [REQ-USA-scroll-animations](REQ-USA-scroll-animations.md) — animation performance targets
