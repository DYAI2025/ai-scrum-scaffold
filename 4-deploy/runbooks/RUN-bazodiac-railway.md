# Runbook: Bazodiac on Railway

**Last updated**: 2026-04-14

**Phase coverage**: Phase 1 (BFF Scaffold & Railway Deployment)

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

```bash
# Build first
npm run build

# Start BFF server locally
PORT=3000 npm run start

# Verify
curl http://localhost:3000/health   # → {"status":"ok"}
open http://localhost:3000          # → SPA
```

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
