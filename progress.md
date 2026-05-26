# Progress Log

> Update at the end of every learning session. One line per topic covered. Newest at top.

## System Design

### Pending next (pick one to resume)
- **Start Udemy course designs** — TinyURL first (good starter), then Notification System (most relevant to Bandyl)
- **JWT revocation / logout / permission invalidation strategies** — grounded in Bandyl: 15-min access + 7-day refresh, large JWT payload with permissions/visibility/tenants embedded
- **Critical review of Bandyl's current auth architecture** — using actual code in `bandyl_superadmin_backend/src/controllers/customerAuth.controller.ts`

### Covered
- 5-step interview framework: Requirements → Capacity Estimation → API Design → HLD → Deep Dive
- Functional vs non-functional requirements distinction
- Capacity estimation — DAU/MAU, throughput, storage, network bandwidth (with formulas)
- Core terms: client-server, database, vertical vs horizontal scaling, load balancer, DB sharding, DB replication, cache, CDN, monolithic vs microservices, message queue, API gateway
- Design goals: scalability, availability, consistency (strong vs eventual), fault tolerance, SPOF
- Database & Storage deep dive:
  - SQL vs NoSQL — when to use each, schema flexibility, ACID (Atomicity, Consistency, Isolation, Durability)
  - MySQL vs PostgreSQL — strengths, JSONB, RLS, multi-tenant approaches
  - SQL scaling — vertical scaling, read replicas, sharding; NoSQL — horizontal scaling built-in
  - All SQL options: PostgreSQL, MySQL, SQLite, MSSQL, Oracle, MariaDB
  - NoSQL types: Document (MongoDB), Key-Value (Redis, DynamoDB), Wide Column (Cassandra, HBase), Graph (Neo4j)
  - Object storage — why not store files in DB, how S3 works, Cloudflare R2, Bandyl currently uses disk (technical debt)
  - Cache — what it is, cache hit/miss, TTL, write-through, cache-aside, LRU eviction, where cache lives
  - What to cache in Bandyl: permissions, job listings, dropdowns, dashboard counts
  - CDN — origin server vs edge servers, geographic distribution, what CDN serves vs doesn't (static vs dynamic), cache invalidation problem (purge / versioned filenames / TTL), Vite content hashing solves it automatically, Cloudflare CDN + R2 as Bandyl migration path
- Networking: IP, DNS, client-server relation, protocols (TCP vs UDP), HTTP, WebSocket, forward vs reverse proxy
- IP deep dive: IPv4 vs IPv6, public vs private IP, NAT, reserved private ranges, MAC address, real machine IPs mapped
- Communication: API as the only way machines communicate, REST — methods, request structure (method + URL + headers + body), response structure (status code + headers + body), stateless principle, common status codes
- GraphQL — overfetching/underfetching problem, single endpoint, client-defined queries, pros/cons, when to use (multiple clients with different needs), JSON vs Protobuf size comparison
- gRPC — Google Remote Procedure Call, binary Protobuf format, HTTP/2, strongly typed .proto contract, used for internal microservice communication, ~65% smaller than JSON
- Message queues — producer/consumer pattern, async processing, retry on failure, decoupling, traffic spike handling, dead letter queue; BullMQ (Bandyl), RabbitMQ, Kafka
- Kafka vs BullMQ — Kafka stores event stream for multiple consumers/replay, BullMQ processes and discards; Kafka for massive scale, BullMQ right for Bandyl now
- Kubernetes — orchestrates Docker containers, auto-scaling, auto-restart, load balancing; not needed at Bandyl's current scale
- TCP vs UDP — TCP guarantees delivery and order, UDP is fast but lossy; HTTP/WebSocket built on TCP
- WebSocket — persistent two-way connection, used in Bandyl collab service via Socket.IO
- Reverse proxy — Nginx in Bandyl routes /collab-api → port 3004, /api → port 3001 etc; Vite dev server does the same locally
- Full request journey in Bandyl: relative URL → Vite/Nginx proxy → correct backend port
- HLD vs LLD distinction
- CDN — edge caching, integration into single-server architecture
- DNS — hierarchical lookup, record types, zone files
- SQL vs NoSQL trade-offs (CRM/ATS context)
- Stateful vs stateless architecture
- Session/auth architecture using Bandyl's real login payload (concluded: stateless JWT with Redux-stored auth context, 15-min renewal)
- Trade-offs of large JWT payloads (no DB lookup vs revocation/staleness/size)

## DSA

### Pending next
- **Why arrays often outperform linked lists in real-world benchmarks** despite worse theoretical complexity in some operations (cache locality / memory layout / CPU cache friendliness)

### Covered
- Arrays internals: contiguous memory, O(1) indexing, shifting cost
- JS arrays vs classical arrays
- Linear search vs binary search
- Dynamic arrays / amortized O(1) append
- Array vs linked list operation complexities (with corrections on head/tail/middle insert-delete)
- Linked list use cases (LRU cache via doubly linked list + hashmap)
- Hash tables — plain object vs Set vs Map, O(1) add/lookup, when to use each
- Array/Map looping — for...in (objects), Object.keys/values/entries, map.values(), Array.from()
- Array sorting — sort((a,b) => b-a) descending, sort((a,b) => a-b) ascending, costs O(n log n)
- Array creation — new Array(n).fill(null).map(() => []) for fixed-size array of independent empty arrays
- hasDuplicate — brute force O(n²) → optimized O(n) with plain object and Set (accepted)
- Valid Anagram — character count with plain object and Map, length check optimization, edge case: count going negative (accepted)
- Two Sum — brute force O(n²) → optimized O(n) with Map, store complement as key and index as value (accepted)
- Group Anagrams — sort each string as key, group originals in Map, return Array.from(map.values()) — O(n × k log k) time, O(n) space (accepted)
