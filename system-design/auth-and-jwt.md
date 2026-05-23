# System Design: Auth & JWT

> Grounded in Bandyl's real auth architecture.
> Relevant code: `bandyl_superadmin_backend/src/controllers/customerAuth.controller.ts`
> Relevant doc: `bandyl-architecture.md`

---

## Covered

### Stateful vs Stateless Auth
- **Stateful (sessions):** server stores session in DB/Redis; every request hits the store to validate
- **Stateless (JWT):** token is self-contained; server validates signature only, no DB lookup
- Bandyl uses stateless JWT — token contains all needed data, no session store

### Bandyl's JWT Structure
- Access token: 15-minute expiry
- Refresh token: 7-day expiry
- Payload embeds: permissions, visibility, tenants — large payload
- Auth state stored in Redux (client-side), not server-side sessions

### Trade-offs of Large JWT Payloads

| Factor | Impact |
|---|---|
| No DB lookup on each request | Fast — O(1) validation |
| Payload grows with permissions/tenants | Bigger cookie/header on every request |
| Token is valid until expiry | Can't revoke mid-session if permissions change |
| Staleness risk | User's actual permissions may change; token shows old state |

---

## Pending (Next Session)

### JWT Revocation / Logout / Permission Invalidation Strategies
The core problem: once issued, a JWT is valid until expiry. How do you invalidate it early?

**Strategies to cover:**
1. Short expiry + refresh rotation (already partially in place — 15 min access)
2. Token blocklist (Redis-based denylist) — store revoked JTIs
3. Token versioning — embed a `version` field; DB stores current version; mismatch = invalid
4. Refresh token rotation — on each refresh, issue new refresh token and invalidate old one
5. Forced logout via permission change — bump version on RBAC change

**Grounding question:** In Bandyl, if a super admin revokes a customer's permission mid-session, what happens? The JWT still has the old permissions for up to 15 minutes. Is that acceptable? What would you change?

---

## Earlier Topics (Foundations)

### CDN
- Edge servers cache static assets close to user
- Reduces latency, offloads origin server
- Integration: DNS points to CDN; CDN fetches from origin on cache miss

### DNS
- Hierarchical: Root → TLD (`.com`) → Authoritative
- Record types: A (IPv4), AAAA (IPv6), CNAME (alias), MX (mail), TXT (verification)
- TTL controls how long resolvers cache the record

### SQL vs NoSQL (CRM/ATS context)
- SQL: strong consistency, joins, ACID — right for ATS/CRM relational data
- NoSQL: flexible schema, horizontal scale — right for logs, activity feeds, unstructured data
- Bandyl uses both: MySQL (ATS multi-tenant), PostgreSQL (Super Admin, CRM with RLS)
