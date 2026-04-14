# API Design: Bazodiac BFF

**Last updated**: 2026-04-14

## Overview

The BFF exposes five endpoints. All are on the same Express server that serves the Vite SPA, so there is no CORS configuration. All `/api/*` endpoints except `/api/webhooks/stripe` use JSON. The webhook endpoint requires a raw body for Stripe signature validation.

Base path: `/api`

Types referenced here are defined in [`data-model.md`](data-model.md).

---

## Endpoints

### `POST /api/reading`

Generate a teaser reading from birth data. Calls FuFirE `/v1/experience/bootstrap` server-side and returns a stripped teaser (≤30% of full reading content). See [DEC-fufire-bootstrap](../decisions/DEC-fufire-bootstrap.md) and [DEC-teaser-server-strip](../decisions/DEC-teaser-server-strip.md).

**Auth**: None

**Content-Type**: `application/json`

**Request body** (`ReadingRequest`):
```json
{
  "mode": "character",
  "birth_data": {
    "date": "1990-11-15",
    "time": "14:30",
    "birth_time_known": true,
    "timezone": "Europe/Berlin",
    "lat": 52.52,
    "lon": 13.40
  }
}
```

For partnership mode, include `partner_birth_data` with the same structure:
```json
{
  "mode": "partnership",
  "birth_data": { ... },
  "partner_birth_data": { ... }
}
```

**Validation**:
- `mode` must be `"character"` or `"partnership"`
- `birth_data.date` must be a valid ISO 8601 date (`YYYY-MM-DD`)
- `birth_data.timezone` must be a valid IANA timezone string
- `birth_data.time` format `HH:MM` (24h), omit entirely if unknown
- `birth_time_known` must match whether `time` is present
- `partner_birth_data` required when `mode = "partnership"`, forbidden otherwise

**Logging**: request body **must not** be logged (PII — `REQ-SEC-data-protection`)

**Response `200`**:
```json
{
  "teaser": {
    "mode": "character",
    "subject": {
      "sun_sign": "Scorpio",
      "chinese_year_animal": "Year of the Horse",
      "nakshatra": "Anuradha",
      "element_summary": "Water dominant (58%)",
      "preview_text": "Your chart reveals a rare convergence of..."
    }
  },
  "reading_hash": "a3f1c7d2-8b4e-4f1a-9c2d-1e5f6a7b8c9d"
}
```

For partnership, `teaser` includes both `subject` and `partner` (`PersonTeaser` each).

**Error responses**:

| Status | Code | Condition |
|--------|------|-----------|
| `400` | `INVALID_INPUT` | Missing/invalid fields |
| `429` | `RATE_LIMIT` | FuFirE rate limit exceeded (propagated from upstream 429) |
| `502` | `FUFIRE_ERROR` | FuFirE API unreachable or returned 5xx |

---

### `POST /api/checkout`

Create a Stripe Checkout Session for a reading. Returns the hosted Checkout URL to redirect the user.

**Auth**: None

**Content-Type**: `application/json`

**Request body** (`CheckoutRequest`):
```json
{
  "reading_hash": "a3f1c7d2-8b4e-4f1a-9c2d-1e5f6a7b8c9d",
  "locale": "de"
}
```

**Stripe Checkout Session configuration**:
- `mode: "payment"`
- `line_items`: one item using `STRIPE_PRICE_ID` env var, quantity 1
- `locale`: mapped from request `locale` (`"de"` → `"de"`, `"en"` → `"en"`)
- `success_url`: `${PUBLIC_URL}/?session_id={CHECKOUT_SESSION_ID}`
- `cancel_url`: `${PUBLIC_URL}/`
- `metadata.reading_hash`: stored for webhook correlation

**Response `200`** (`CheckoutResponse`):
```json
{
  "checkout_url": "https://checkout.stripe.com/pay/cs_live_..."
}
```

**Error responses**:

| Status | Code | Condition |
|--------|------|-----------|
| `400` | `INVALID_INPUT` | Missing or malformed `reading_hash` or `locale` |
| `404` | `SESSION_NOT_FOUND` | `reading_hash` unknown or expired (TTL elapsed) |
| `502` | `STRIPE_ERROR` | Stripe API unreachable or returned error |

---

### `GET /api/reading/unlock`

Verify a completed Stripe payment and return the full reading. Calls FuFirE bootstrap again with the stored birth data. The full reading is generated fresh — it is not cached server-side.

**Auth**: None (protected by Stripe session verification)

**Query parameter**: `session_id` — Stripe Checkout Session ID (format: `cs_live_...` or `cs_test_...`)

**Example**: `GET /api/reading/unlock?session_id=cs_live_a1b2c3`

**Server logic**:
1. Retrieve Stripe session via `stripe.checkout.sessions.retrieve(session_id)`
2. Confirm `session.payment_status === "paid"`
3. Look up `reading_hash` from `session.metadata.reading_hash`
4. Look up `SessionEntry` by `reading_hash` from in-memory store
5. Call FuFirE `/v1/experience/bootstrap` with stored birth data (full, no stripping)
6. For partnership: two bootstrap calls, merge into `FullReading`
7. Return `FullReading`

**Response `200`** (`{ full_reading: FullReading }`):
```json
{
  "full_reading": {
    "mode": "character",
    "subject": {
      "sun_sign": "Scorpio",
      "moon_sign": "Pisces",
      "ascendant": "Gemini",
      "dominant_planets": ["Pluto", "Mars"],
      "four_pillars": {
        "year":  { "stem": "Geng", "branch": "Wu", "animal": "Horse" },
        "month": { "stem": "Ding", "branch": "Hai" },
        "day":   { "stem": "Yi",   "branch": "Wei" },
        "hour":  { "stem": "Jia",  "branch": "Shen" }
      },
      "day_master": "Yi Wood",
      "element_balance": { "wood": 0.22, "fire": 0.15, "earth": 0.18, "metal": 0.10, "water": 0.35 },
      "nakshatra": "Anuradha",
      "nakshatra_lord": "Saturn",
      "soulprint_sectors": [
        { "name": "Identity", "score": 78, "description": "..." },
        { "name": "Relationships", "score": 65, "description": "..." }
      ],
      "signature_blueprint": "Across three traditions, your chart speaks..."
    }
  }
}
```

**Error responses**:

| Status | Code | Condition |
|--------|------|-----------|
| `400` | `INVALID_INPUT` | `session_id` query param missing or malformed |
| `402` | `PAYMENT_REQUIRED` | Stripe session exists but `payment_status !== "paid"` |
| `404` | `SESSION_NOT_FOUND` | `session_id` unknown to Stripe |
| `410` | `SESSION_EXPIRED` | `reading_hash` found in Stripe metadata but TTL elapsed in BFF session store |
| `502` | `FUFIRE_ERROR` | FuFirE API unreachable or returned 5xx |
| `502` | `STRIPE_ERROR` | Stripe API unreachable during session retrieval |

---

### `POST /api/webhooks/stripe`

Receive Stripe webhook events. Secondary confirmation path — the primary unlock path is `GET /api/reading/unlock`. This endpoint handles `checkout.session.completed` for any future asynchronous processing needs (e.g., sending a confirmation email post-launch).

**Auth**: Stripe signature validation (see below)

**Content-Type**: `application/json` (raw body required — do **not** use `express.json()` middleware on this route)

**Middleware**: `express.raw({ type: 'application/json' })` — raw body buffer must be preserved for `stripe.webhooks.constructEvent`

**Signature validation** (`REQ-SEC-data-protection`):
```ts
const event = stripe.webhooks.constructEvent(
  req.body,               // raw Buffer
  req.headers['stripe-signature'],
  process.env.STRIPE_WEBHOOK_SECRET
);
// Throws on invalid signature — return 400
```

**Handled events**:
- `checkout.session.completed` — log completion; no further action required for MVP (unlock is pull-based via `GET /api/reading/unlock`)

**Unhandled events**: return `200` immediately (Stripe requires acknowledgment for all events).

**Response `200`**: `{ "received": true }` — always returned immediately after event handling

**Error responses**:

| Status | Code | Condition |
|--------|------|-----------|
| `400` | `INVALID_SIGNATURE` | `stripe.webhooks.constructEvent` throws — invalid or missing signature |

---

### `GET /health`

Railway health check probe. Must respond within 30 seconds of container start.

**Auth**: None

**Response `200`**:
```json
{ "status": "ok" }
```

No error response — if the server is not up, Railway receives no response and fails the health check.

---

## Error Response Format

All error responses use a consistent envelope:

```json
{
  "error": "Human-readable description of the error",
  "code": "MACHINE_READABLE_CODE"
}
```

Codes are listed per endpoint above. No stack traces, no internal paths, no PII in error responses.

---

## Cross-Cutting Concerns

### CORS
Not configured. The SPA and BFF are served from the same origin (same Railway service, same port). No cross-origin requests.

### Middleware order (Express)
```
express.raw({ type: 'application/json' })   → /api/webhooks/stripe only
express.json()                              → all other /api/* routes
express.static('dist')                      → catch-all (after all /api/* routes)
```

The static middleware must be registered **after** all API routes to avoid serving `index.html` for unmatched API paths.

### Request logging
- All routes: log method, path, status, response time
- `/api/reading` request body: **never logged** (contains birth date/time — PII per `REQ-SEC-data-protection`)
- `/api/reading/unlock` query param `session_id`: safe to log (Stripe ID, not PII)

### Rate limiting
No application-level rate limiter for MVP. FuFirE returns `429` when its tier limit is exceeded — the BFF propagates this as `429 RATE_LIMIT` to the client. Stripe has its own rate limits (not a concern at launch volume).

### Versioning
No versioning. All endpoints are under `/api/` with no version prefix. If breaking changes are needed post-launch, introduce `/api/v2/` at that point.

---

## Requirement Coverage

| Requirement | Endpoint(s) |
|-------------|-------------|
| REQ-F-birth-data-input | `POST /api/reading` (accepts `BirthData`) |
| REQ-F-reading-generation | `POST /api/reading` → FuFirE bootstrap |
| REQ-F-teaser-preview | `POST /api/reading` response (`TeaserReading`) |
| REQ-F-payment-integration | `POST /api/checkout` → Stripe Checkout Session |
| REQ-F-reading-unlock | `GET /api/reading/unlock` (Stripe verify → full reading) |
| REQ-F-i18n | `locale` field in `POST /api/checkout` |
| REQ-SEC-data-protection | Webhook signature validation; no PII in logs/URLs; HTTPS via Railway |
| REQ-MNT-railway-deploy | `GET /health` (Railway health check) |
