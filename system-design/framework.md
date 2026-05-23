# System Design Interview Framework

> From Udemy course. Apply this to every design problem — interview or real Bandyl work.
> 10 designs in the course: Instagram, YouTube, TinyURL, Rate Limiter, WhatsApp, Search System, Airbnb, Notification System, Distributed Login, OTP Service.

---

## The 5-Step Framework

### Step 1 — Requirements

**Functional** — what the system does (features)
- Example (YouTube): user can upload a video, user can stream a video

**Non-Functional** — how the system performs (quality)
- Example (YouTube): upload can take hours, streaming must not pause, must handle 100M users
- Common non-functionals: scalability, availability, reliability, latency, consistency, durability

---

### Step 2 — Capacity Estimation

Four dimensions:

| Dimension | What it measures | Example |
|---|---|---|
| DAU / MAU | Daily / Monthly active users | 100M DAU |
| Throughput | Requests per second (RPS) | 100M users × 100 req/user = 10B req/day → ~115K RPS |
| Storage | How much data you store over time | 100M uploads × 500MB avg = 50PB/year |
| Network / Bandwidth | Data in and out per second | 100M × 100 req × 10MB = 100PB/day in bandwidth |

**Formula for bandwidth:**
`users × requests_per_user × data_per_request = total data transferred`

---

### Step 3 — API Design

Three parts per endpoint:

1. **Endpoint** — method + path: `POST /upload`
2. **Body** — what you send: `{ file, title, tags }`
3. **Response** — what comes back: `{ videoId, success }`

---

### Step 4 — High Level Design (HLD)

- Draw a diagram — boxes and arrows
- Start simple (client → server → DB)
- Gradually add components to meet functional requirements
- Then add components to meet non-functional requirements (CDN, cache, load balancer, queues)

---

### Step 5 — Deep Dive

Go into detail on the hardest or most interesting parts:
- DB selection and schema design
- Caching strategy
- Any specific component that needs explaining
- Trade-offs of choices made in HLD

---

## Course Appendix Topics (to cover as needed)
- Design goals
- Networking buzzwords
- Communication buzzwords
- Database and storage buzzwords
- Caching deep dives
- Extra buzzwords

---

## Designs to Complete (in order)

- [ ] TinyURL — good starter, covers hashing, redirects, storage
- [ ] Rate Limiter — covers algorithms (token bucket, leaky bucket)
- [ ] Notification System — most relevant to Bandyl
- [ ] Distributed Login / OTP Service — most relevant to Bandyl auth
- [ ] WhatsApp — covers real-time messaging, WebSocket
- [ ] Instagram — covers media storage, feed generation
- [ ] YouTube — covers video processing, CDN, streaming
- [ ] Search System — covers indexing, Elasticsearch
- [ ] Airbnb — covers search, booking, availability
