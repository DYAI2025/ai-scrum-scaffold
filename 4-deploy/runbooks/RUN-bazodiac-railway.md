# Runbook: Bazodiac on Railway

**Last updated**: 2026-04-14

**Phase coverage**: Phase 1–3 (BFF Scaffold, Teaser Flow, Payment & Full Reading)

---

## Overview

Single Railway service running the Express BFF which:
1. Builds the Vite SPA (`npm run build` → `dist/`)
2. Starts the Express server (`npm run start`) which serves `dist/` as static files

Railway auto-detects Node.js via Nixpacks. Config is in `railway.json`.

---

## First Deployment (Railway Dashboard)

### Option A: GitHub Integration (recommended)

1. Go to [railway.app](https://railway.app) → New Project → Deploy from GitHub repo
2. Select `DYAI2025/Bazodiac-Viteapp`
3. Railway will auto-detect Node.js and use `railway.json`
4. Add environment variables (see below)
5. Click **Deploy**

### Option B: Railway CLI

```bash
# Install CLI
npm install -g @railway/cli

# Login
railway login

# Create project from repo root
cd /path/to/Bazodiac-Viteapp/app
railway init
railway up
```

---

## Environment Variables

Set these in the Railway service settings → Variables tab:

| Variable | Value | Required now? |
|----------|-------|---------------|
| `PORT` | auto-injected by Railway | — |
| `FUFIRE_API_KEY` | `ff_pro_<secret>` | Phase 2+ |
| `FUFIRE_BASE_URL` | `https://bafe-production.up.railway.app` | Phase 2+ |
| `STRIPE_SECRET_KEY` | `sk_live_...` or `sk_test_...` | Phase 3+ |
| `STRIPE_WEBHOOK_SECRET` | `whsec_...` | Phase 3+ |
| `STRIPE_PRICE_ID` | `price_...` | Phase 3+ |
| `PUBLIC_URL` | Railway public domain (e.g. `https://bazodiac.up.railway.app`) | Phase 3+ |
| `VITE_STRIPE_PUBLISHABLE_KEY` | `pk_live_...` or `pk_test_...` | Phase 3+ (build-time) |

> `PORT` is automatically injected by Railway — do not set it manually.

---

## Build & Start Commands

| Command | Purpose |
|---------|---------|
| `npm run build` | TypeScript check + Vite production build → `dist/` |
| `npm run start` | Launch Express BFF on `$PORT` (default 3000) |

Railway executes `build` then `start` automatically.

---

## Phase 2 Environment Variables

Before testing Phase 2 features, set these in Railway service → Variables:

| Variable | Value |
|----------|-------|
| `FUFIRE_API_KEY` | `ff_pro_<your-key>` |
| `FUFIRE_BASE_URL` | `https://bafe-production.up.railway.app` |

For local testing: `FUFIRE_API_KEY=... FUFIRE_BASE_URL=... PORT=3000 npm run start`

---

## Phase 2 Manual Test Scenarios

### ✅ Test 5: Character Reading Flow

1. Open the app in a browser
2. On the Hero section, click the CTA button to scroll to path selection
3. Select **"Character Reading"**
4. On the Input section, enter a birth date (e.g. `1990-11-15`)
5. Optionally check "Birth Time known" and enter a time
6. Click **"Calculate my portrait"** — the button shows a spinner while loading
7. After ~2–5 seconds the page scrolls to the Reveal section

**Expected on Reveal section:**
- Sun sign name visible (e.g. "Scorpio")
- Chinese year animal (e.g. "Year of the Horse")
- Nakshatra name
- Element summary (e.g. "Water dominant (58%)")
- Preview text paragraph
- "Unlock full reading" button visible

### ✅ Test 6: Validation — Missing Birth Date

1. On the Input section, leave the Birth Date empty
2. The "Calculate my portrait" button should be disabled (greyed out)

### ✅ Test 7: Validation — API Error Handling

1. Temporarily misconfigure `FUFIRE_API_KEY` (wrong key)
2. Submit a valid birth date
3. An error message should appear below the form (not a blank screen or crash)

### ✅ Test 8: Partnership Mode

1. Select **"Partnership Reading"** from the path section
2. On Input, fill in both Birth Date fields
3. Click "Calculate my portrait"
4. Reveal section shows two subjects (subject + partner)

---

## Phase 3 Environment Variables

Before testing Phase 3, add these in Railway → Variables:

| Variable | Value |
|----------|-------|
| `STRIPE_SECRET_KEY` | `sk_test_...` or `sk_live_...` |
| `STRIPE_WEBHOOK_SECRET` | `whsec_...` (from Stripe Dashboard → Webhooks) |
| `STRIPE_PRICE_ID` | `price_...` (from Stripe Dashboard → Products) |
| `PUBLIC_URL` | `https://bazodiac-landingpage-production.up.railway.app` |

---

## Phase 3 Manual Test Scenarios

### ✅ Test 9: Checkout Redirect

1. Generate a teaser reading (enter birth date, click Calculate)
2. On the Reveal section, click **"Unlock full reading"**
3. Button should show spinner with "Redirecting to checkout…"
4. You should be redirected to Stripe Checkout page

**Expected:** Stripe Checkout page shows with correct product and price.

### ✅ Test 10: Successful Payment → Full Reading

1. Complete Test 9 and pay with Stripe test card `4242 4242 4242 4242` (any future expiry, any CVC)
2. After payment, Stripe redirects back to the app with `?session_id=cs_test_...`
3. The app should automatically call `/api/reading/unlock` and display the full reading

**Expected on full reading:**
- "Reading unlocked" badge with checkmark
- Western Astrology section: Sun, Moon, Rising signs
- Four Pillars section: year/month/day/hour with Stamm, Zweig, Tier
- Wu-Xing Balance: 5 element bars with percentages
- Soulprint Sectors: 12 values in a grid
- Harmony Index number

### ✅ Test 11: Cancelled Payment → Teaser Remains

1. Start checkout (click "Unlock full reading")
2. On Stripe Checkout, click the back arrow or close the tab
3. Navigate back to the app

**Expected:** Teaser is still visible, "Unlock full reading" button is still clickable, no error message.

### ✅ Test 12: Webhook Endpoint (via Stripe CLI)

```bash
stripe listen --forward-to https://bazodiac-landingpage-production.up.railway.app/api/webhooks/stripe
stripe trigger checkout.session.completed
```

**Expected:** Server logs `[webhook] Payment confirmed for reading <hash>` or `[webhook] checkout.session.completed for unknown session`.

### ✅ Test 13: Expired Session

1. Generate a teaser, wait >1 hour (or restart the Railway service to clear the in-memory store)
2. Try to unlock with a stale `session_id`

**Expected:** Error message "Reading session has expired — please generate a new reading" (HTTP 410).

---

## Phase 1 Manual Test Scenarios

After deployment, verify:

### ✅ Test 1: Health Check

```bash
curl https://<your-railway-domain>/health
# Expected: {"status":"ok"}
```

### ✅ Test 2: SPA Loads

Open `https://<your-railway-domain>` in a browser.
- Page loads without errors
- Assets reference `/assets/...` paths (not `./assets/...`)
- No 404 on CSS/JS assets

### ✅ Test 3: SPA Routing (deep link)

Navigate directly to `https://<your-railway-domain>/some/path`
- Should return `index.html` (not 404)
- React app loads and renders

### ✅ Test 4: Railway Health Check Passes

In Railway dashboard → Service → Deployments:
- Status shows **Active** (not Failed)
- Health check logs show `/health` returning 200 within 30s

---

## Local Testing

### Production build + BFF (Phase 1 — static files only)

```bash
npm run build
PORT=3000 npm run start
curl http://localhost:3000/health   # → {"status":"ok"}
open http://localhost:3000          # → SPA
```

### Development with HMR + BFF (Phase 2+ — live reload + API)

Run both processes in separate terminals:

```bash
# Terminal 1: BFF (API + serves static in production only)
FUFIRE_API_KEY=ff_pro_<key> FUFIRE_BASE_URL=https://bafe-production.up.railway.app PORT=3000 npm run start

# Terminal 2: Vite dev server (HMR, proxies /api to BFF on :3000)
npm run dev
```

Vite's dev proxy (configured in `vite.config.ts`) forwards `/api/*` calls to `http://localhost:3000`, so the SPA at `http://localhost:5173` can call the BFF without CORS issues.

---

## Redeploy After Code Changes

Railway redeploys automatically on push to `main` if GitHub integration is configured.

Manual redeploy:
```bash
railway up
```

---

## Rollback

In Railway dashboard → Service → Deployments → select a previous deployment → **Redeploy**.

---

## Troubleshooting

| Symptom | Cause | Fix |
|---------|-------|-----|
| `EADDRINUSE` on start | Port already in use locally | Use a different PORT: `PORT=3001 npm run start` |
| 404 on `dist/index.html` | Build not run before start | Run `npm run build` first |
| Assets 404 in browser | Old `base: './'` in vite.config | Already fixed to `base: '/'` — rebuild |
| Railway deploy fails at build | TypeScript error | Run `npm run build` locally to reproduce |
` locally to reproduce |
