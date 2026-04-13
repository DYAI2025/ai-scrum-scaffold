# ASM-fufire-api-available: FuFirE API is available for integration

**Category**: Technology

**Status**: Unverified

**Risk if wrong**: High — if the API is not ready or stable, the landing page must fall back to simplified client-side calculations, which are less accurate and less differentiated.

## Statement

The FuFirE (Fusion Firmament Engine) API is deployed and accessible, providing accurate tri-system astrology calculations (Western zodiac, BaZi, Vedic Nakshatra) that the landing page can call to generate readings.

## Rationale

The `src/utils/astrology.ts` file currently contains simplified client-side calculations. The FuFirE API offers astronomically precise results using real ephemerides and the full fusion algorithm, which would significantly improve reading quality and differentiation.

## Verification Plan

Check FuFirE API availability and endpoint compatibility before Design phase. Confirm: base URL, authentication method, response schema, latency, and rate limits.

## Related Artifacts

- [CON-railway-deployment](../constraints/CON-railway-deployment.md) — if API is used, CORS or proxy configuration on Railway is needed
