# Progress Log

> Update at the end of every learning session. One line per topic covered. Newest at top.

## System Design

### Pending next (in order)
- **Remaining capacity estimation sections** — to be covered
- **GraphQL beginner guide** — deeper than the intro already covered
- **Consistent hashing deeper dive** — virtual nodes, real implementation details
- **GraphQL beginner guide** — deeper than the intro already covered
- **Consistent hashing deeper dive** — virtual nodes, real implementation details
- *(after above)* **Start design problems** — TinyURL first, then Notification System
- **JWT revocation / logout / permission invalidation strategies** — grounded in Bandyl auth setup

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
  - Caching deep dive — read strategies (cache-aside vs read-through), write strategies (write-aside, write-through, write-behind), cache stampede problem, write amplification, Bandyl: cache-aside for reads, write-through recommended for permissions
  - Cloud computing — IaaS vs PaaS vs SaaS; Bandyl runs on IaaS (raw VM on EC2)
  - Logging vs monitoring — logging records events, monitoring watches real-time metrics (CPU, memory, latency, error rate); logs = debug after the fact, metrics = know something is wrong now
  - Hashing and consistent hashing — regular hashing (hash % N) remaps nearly all keys when N changes; consistent hashing uses a ring so only K/N keys move when a node is added/removed; virtual nodes fix uneven load distribution; used in Redis Cluster, DB sharding
  - CAP theorem — Consistency (refuse to serve stale data, return error), Availability (always respond, even if stale), Partition Tolerance (network partitions always happen, not optional); when partition occurs choose C or A; AP systems use eventual consistency — data syncs when partition heals; Bandyl RBAC = CP, notification delivery = AP
  - Apache Kafka — event streaming platform; Producer publishes to a Topic, multiple independent Consumer Groups subscribe and read at their own pace; events retained long-term (unlike BullMQ where jobs are deleted after processing); supports replay for new services and recovery after downtime; BullMQ is right for Bandyl's scale (15 peak RPS), Kafka is for millions of events/sec at Netflix/Uber scale
  - Database indexing — separate B+ tree structure (leaf nodes linked for range scans), stores (indexed value → row pointer); auto on primary key, manual on frequent query columns; selectivity is key — low selectivity columns (booleans, status) ignored by query optimizer; write overhead on every update
  - Capacity estimation — DAU → requests/day → RPS (÷86,400) → peak RPS (×2–3); Bandyl at 500 companies × 50 users × 20 req/day = 15 peak RPS — single server more than sufficient; storage estimation: request size × daily requests × days
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

### Re-solve queue (blind, every Sunday — pick 3)

| Problem | Pattern | Solved on | Last re-solved |
|---|---|---|---|
| hasDuplicate | HashSet existence check | 2026-05-20 | 2026-05-31 ✓ |
| Valid Anagram | HashMap frequency count | 2026-05-20 | 2026-05-31 ✗ (needed guided input + bracket bug, retry next Sunday) |
| Two Sum | HashMap complement lookup | 2026-05-20 | — |
| Group Anagrams | HashMap with sorted key | 2026-05-20 | — |
| Top K Frequent Elements | HashMap + sort by frequency | 2026-05-20 | — |
| Encode & Decode Strings | Length-prefix encoding | 2026-05-27 | — |
| Products of Array Except Self | Prefix + suffix running product | 2026-05-29 | — |
| Valid Sudoku | Composite Set keys | 2026-05-30 | — |

> Every Sunday: pick any 3, open a blank file, no notes, write from memory. Update "Last re-solved" column. If you fail one, review it and retry next Sunday.

### Pending next
- **Valid Anagram — O(1) space solution** — current HashMap solution is O(n) space; better approach exists using a fixed-size structure (discuss in next revision session)
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
- Encode & Decode Strings — length-prefix encoding (N#word), two-pointer decode: j scans digits to find #, length = parseInt(slice(i,j)), word = slice(j+1, j+1+length), i jumps to j+1+length; handles # inside word content safely
- Products of Array Except Self — brute force O(n²); optimal O(n): prefix product array (left) + suffix product array (right), both built with a single running product variable; output[i] = left[i] * right[i]; reset runningProduct to 1 between the two loops
- hasDuplicate — brute force O(n²) → optimized O(n) with plain object and Set (accepted)
- Valid Anagram — character count with plain object and Map, length check optimization, edge case: count going negative (accepted)
- Two Sum — brute force O(n²) → optimized O(n) with Map, store complement as key and index as value (accepted)
- Group Anagrams — sort each string as key, group originals in Map, return Array.from(map.values()) — O(n × k log k) time, O(n) space (accepted)
- Top K Frequent Elements — frequency Map, sort entries by count descending, slice top k — O(n log n) time, O(n) space (accepted)
- Valid Sudoku — composite Set keys: `r${i}${v}`, `c${j}${v}`, `b${box}${v}`; boxIndex = Math.floor(i/3)*3 + Math.floor(j/3); skip '.'; single pass O(81) = O(1) (accepted)
