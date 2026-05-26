# System Design Core Terms

> From Udemy course appendix. These are the building blocks used in every design.

---

## Architecture

| Term | What it means |
|---|---|
| Client-Server | Client sends requests, server processes and responds |
| Monolithic | Single deployable unit — all features in one codebase |
| Microservices | Each feature is a separate service, deployed independently |
| API Gateway | Single entry point for all client requests — routes to correct microservice |
| Message Queue | Async communication between services — sender drops message, receiver picks it up when ready (e.g. RabbitMQ, BullMQ) |

---

## Scaling

| Term | What it means |
|---|---|
| Vertical Scaling | Make one server bigger (more CPU, RAM) — has a ceiling |
| Horizontal Scaling | Add more servers — no ceiling, but needs load balancer |
| Load Balancer | Distributes incoming requests across multiple servers |
| Database Sharding | Split one large DB into smaller pieces across multiple servers (horizontal DB scaling) |
| Database Replication | Copy data to multiple servers — one primary (writes), multiple replicas (reads) |

---

## Performance

| Term | What it means |
|---|---|
| Cache | Store frequently accessed data in fast memory (Redis) to avoid hitting DB every time |
| CDN | Servers at edge locations that cache static content close to users |

---

## Design Goals

| Goal | What it means |
|---|---|
| Scalability | System handles more load by adding resources |
| Availability | System is up and accessible (measured as % uptime — e.g. 99.9%) |
| Consistency — Strong | Every read returns the most recent write, always |
| Consistency — Eventual | Reads may return stale data temporarily, but will catch up |
| Fault Tolerance | System continues working even when some components fail |
| SPOF (Single Point of Failure) | One component whose failure brings down the whole system — always design to eliminate SPOFs |

---

## Networking & Protocols

| Term | What it means |
|---|---|
| TCP | Transmission Control Protocol — guarantees delivery and order, used by HTTP/WebSocket |
| UDP | User Datagram Protocol — fast, no guarantee, used by live video/gaming |
| HTTP | HyperText Transfer Protocol — request/response, built on TCP |
| HTTPS | HTTP + TLS encryption — port 443 |
| WebSocket | Persistent two-way connection — server can push to client anytime (`wss://`) |
| Forward Proxy | Hides the client — e.g. VPN |
| Reverse Proxy | Hides the server — e.g. Nginx, Vite dev server |

### Bandyl real-world examples
- Nginx routes `/collab-api` → `localhost:3004`, `/api` → `localhost:3001` (reverse proxy)
- Vite dev server does the same routing locally — mirrors Nginx config
- Note creation uses HTTP POST; Socket.IO broadcasts the update to other users via WebSocket
- Frontend always uses relative URLs (`/collab-api/...`) — proxy handles routing in both dev and prod
- `localhost:3004` in Nginx means the backend running on the same production machine, not your laptop

---

## API Types

| Type | Full form | Used for | Data format |
|---|---|---|---|
| REST | Representational State Transfer | Client-server, browser/mobile | JSON |
| GraphQL | Graph Query Language | Multiple clients with different data needs | JSON |
| gRPC | Google Remote Procedure Call | Internal microservice communication | Binary (Protobuf) |

**REST:** stateless, HTTP methods (GET/POST/PUT/DELETE), fixed response shape, easy caching
**GraphQL:** one endpoint, client defines exactly what it needs, fixes overfetching/underfetching, hard to cache
**gRPC:** strongly typed `.proto` contract, HTTP/2, ~65% smaller than JSON, very fast

### Message Queues
- **Producer** — puts message in queue
- **Consumer** — picks up and processes message
- **Dead letter queue** — failed messages after max retries
- Benefits: async processing, retry on failure, decoupling, traffic spike absorption
- Tools: BullMQ (Bandyl — Redis-backed), RabbitMQ (general), Kafka (massive scale + event replay)

### Kafka vs BullMQ
- BullMQ: process job → gone. One consumer.
- Kafka: event stored in stream, multiple consumers can read, can replay past events
- Bandyl uses BullMQ — right choice at current scale

### Kubernetes (K8s)
- Manages Docker containers at scale — auto-restart, auto-scaling, load balancing
- Docker = one container. Kubernetes = fleet manager for hundreds of containers
- Not needed at Bandyl's current scale

---

## Key Trade-off: CAP Theorem (to cover in depth later)
In a distributed system you can only guarantee two of three: **Consistency**, **Availability**, **Partition Tolerance**.
