# System Design: CDN (Content Delivery Network)

---

## What it is

A network of edge servers distributed globally. Instead of every user hitting your origin server, they hit the nearest edge server which has cached your static content.

**Without CDN:**
```
User (US) → origin server (India) → back to user
Round trip: ~280ms
```

**With CDN:**
```
User (US) → Cloudflare edge (US) → back to user
Round trip: ~15ms

First request only: edge fetches from origin and caches it
All subsequent requests: served from edge, origin never touched
```

---

## What CDN serves vs what it doesn't

| CDN handles | CDN does NOT handle |
|---|---|
| React JS bundle | API calls (/api/...) |
| CSS files | WebSocket connections |
| Images / icons | Auth (login, JWT) |
| Fonts | Database queries |
| HTML (short TTL) | File uploads |

API responses are dynamic — different per user, per tenant, per request. CDN is for static content only.

---

## Cache invalidation problem

CDN edge servers cache your files. When you deploy new code, edge servers still have the old files until TTL expires or you manually purge.

**Three solutions:**

**1. Cache purge on deploy** — call CDN provider API after deploy to clear stale cache. Next request fetches fresh from origin.

**2. Versioned filenames (best)** — Vite does this automatically:
```
main.abc123.js   ← old build
main.xyz789.js   ← new build
```
Different filename = CDN treats as new file = no stale cache. `index.html` has 0 cache (or very short TTL) so it always points to latest filename.

**3. Short TTL** — set 5-min expiry. Stale resolves itself but users see old version for up to 5 min.

**In practice:** Vite handles it automatically with content hashing. Cache your JS/CSS forever (filename changes on update), never cache HTML.

---

## Bandyl context

**Current state:** No CDN. React build files served by Nginx from `/var/www/`. Every user worldwide hits the single India server.

**Migration path:**
1. Move domain DNS to Cloudflare
2. Frontend static files served through Cloudflare CDN — automatic
3. File uploads moved to Cloudflare R2 — replaces disk storage
4. Both CDN + object storage solved with one provider (Cloudflare), free tier covers current scale

**Cloudflare free tier:** Unlimited CDN bandwidth, R2 free tier 10GB storage, no egress fees.
