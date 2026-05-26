# System Design: Databases & Storage

---

## SQL vs NoSQL

### When to use SQL
- Data has clear relationships (candidates → jobs → interviews)
- Data integrity is critical (financial, transactional)
- Complex queries with joins
- Schema is stable
- ACID transactions needed

### When to use NoSQL
- Flexible/varied data per record (Instagram post — text, photo, or video)
- Speed and scale matter more than strict integrity
- Simple lookups by key
- Schema changes frequently
- Massive write throughput (logs, activity feeds)

---

## ACID Transactions

| Letter | Full form | Meaning |
|---|---|---|
| A | Atomicity | All or nothing — if one step fails, all steps roll back |
| C | Consistency | DB always moves from one valid state to another — rules never violated |
| I | Isolation | Concurrent transactions do not interfere with each other |
| D | Durability | Committed data is permanently saved even if server crashes |

**Real example:** Bank transfer ₹1000 — deduct from A and add to B. If server crashes between steps, Atomicity rolls back the deduction. Money is safe.

---

## MySQL vs PostgreSQL

| | MySQL | PostgreSQL |
|---|---|---|
| Best for | Simple, fast, high-volume reads | Complex queries, flexible data types |
| JSONB | Basic JSON support | Excellent — can index inside JSON |
| Row Level Security | No | Yes |
| Multi-tenant | Separate DB per tenant (Bandyl ATS) | RLS in shared DB (Bandyl CRM) |

Both are good. Choice depends on features needed and team expertise.

**Bandyl uses:**
- MySQL → ATS (separate DB per tenant, fast reads)
- PostgreSQL → CRM, SuperAdmin (JSONB, RLS, complex queries)

---

## Schema: SQL vs NoSQL

**MySQL — strict, lives in DB:**
```sql
CREATE TABLE candidates (id INT, name VARCHAR(100), email VARCHAR(255));
ALTER TABLE candidates ADD COLUMN linkedin_url VARCHAR(255); -- migration needed
```
Migration on 10M rows can lock table for minutes.

**MongoDB — flexible, no schema in DB:**
```js
// Just insert — no declaration needed
db.candidates.insertOne({ name: "John", email: "john@gmail.com" })
// Add new field tomorrow — no migration
db.candidates.insertOne({ name: "Jane", linkedin_url: "linkedin.com/jane" })
```
Old documents simply do not have the new field. Code must handle both cases.

**Mongoose** = Node.js library that adds optional schema validation on top of MongoDB. Schema lives in app code, not in the database.

---

## Scaling

### SQL Scaling
- Vertical scaling — bigger server (has ceiling)
- Read replicas — primary for writes, replicas for reads (80% of traffic)
- Sharding — split data across DBs by criteria (Bandyl ATS = one DB per tenant = functional sharding)
- Joins across shards = impossible

### NoSQL Scaling
- Designed for horizontal scaling from the start
- No joins, no foreign keys — documents are self-contained
- Adding servers just redistributes documents

**Real world:**
- Shopify → MySQL sharding by shop ID
- Netflix → Cassandra for viewing history (200M+ users)
- Twitter → Redis for timelines and caching

---

## All Database Options

### SQL
| Rank | DB | Why popular |
|---|---|---|
| 1 | PostgreSQL | Most feature-rich open source — JSONB, RLS, extensions |
| 2 | MySQL | Most widely deployed — fast, simple, battle-tested |
| 3 | SQLite | Serverless, one file — mobile/embedded |
| 4 | MSSQL | Microsoft/enterprise |
| 5 | Oracle | Banks, governments — expensive |
| 6 | MariaDB | MySQL fork |

### NoSQL — 4 Types

**Document** — flexible JSON documents
- MongoDB (most popular), Firestore, CouchDB

**Key-Value** — store value against key, extremely fast
- Redis (caching, sessions, queues — Bandyl uses this)
- DynamoDB (AWS managed, massive scale)
- Memcached (older, being replaced by Redis)

**Wide Column** — massive write throughput
- Cassandra (Netflix, Instagram, Discord)
- HBase (Facebook messages)

**Graph** — highly connected data
- Neo4j (social networks, fraud detection, recommendations)
- Amazon Neptune

---

## Object Storage

**Why not store files in DB:**
- DB size grows massively, slows all queries
- Serving files through app server wastes CPU/memory
- No CDN integration possible

**How it works:**
```
User uploads → backend receives → upload to S3 → S3 returns URL
→ save URL in DB → frontend loads directly from S3
```
Server only handles upload once. S3 serves file directly after.

**Options:**
| Service | By | Note |
|---|---|---|
| S3 | AWS | Industry standard |
| Cloudflare R2 | Cloudflare | No egress fees — best for Bandyl |
| Google Cloud Storage | Google | GCP ecosystem |
| MinIO | Open source | Self-hosted |

**Bandyl current state:** Files stored on server disk — works now but technical debt.
Problems: single point of failure, no horizontal scaling, disk space limit, no CDN.
**Recommended migration:** Cloudflare R2 — S3-compatible, no egress fees, free tier 10GB.

---

## Caching

**What it is:** Fast in-memory store (RAM) between app and DB.
- DB query: ~50ms
- Redis lookup: ~0.1ms (100x faster)

**Flow:**
```
Request → check cache → hit: return immediately
                      → miss: query DB → store in cache → return
```

**Cache invalidation strategies:**
- **TTL** — expires after X seconds (simple, may be slightly stale)
- **Write-through** — update cache when DB updates (always fresh, every write hits both)
- **Cache-aside** — update DB, delete cache entry, next read repopulates (most common)

**Eviction when cache full:**
- LRU (Least Recently Used) — remove item not accessed longest (Redis default)
- LFU (Least Frequently Used) — remove item accessed fewest times
- TTL — remove expired items first

**What to cache in Bandyl:**
- User permissions/roles (read every request, changes rarely)
- Job listings (same for all users)
- Dropdown options (departments, locations — never changes)
- Dashboard counts (expensive query, can be slightly stale)

**Redis maxmemory fix (Bandyl server):**
```bash
redis-cli config set maxmemory 2gb
redis-cli config set maxmemory-policy allkeys-lru
```
Default is `noeviction` — crashes app when full. Must fix.

---

## Bandyl Server Investigation (2026-05-24)

**Server:** 16GB RAM, single server — all apps + DBs on one machine
**Issue:** Swap hits 90% at peak, server slows down

**Findings:**
- Redis using only 1MB — not the problem
- Redis had no maxmemory limit + noeviction policy — fixed
- Node.js processes total ~1GB at idle
- ATS backend heap was at 90.67% — increased with `--max-old-space-size=512`
- After fix: heap dropped to 68.95% but event loop latency spiked to 2232ms
- 2 services (hrms, vms) had 1 crash/restart each

**Pending investigation:**
- High event loop latency on ATS backend (2232ms — should be <10ms)
- Possible memory leak or blocked DB connection pool
- Run `pm2 logs ats_backend --lines 100` to diagnose
