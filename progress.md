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
- Networking: IP, DNS, client-server relation, protocols (TCP vs UDP), HTTP, WebSocket, forward vs reverse proxy
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
- hasDuplicate — brute force O(n²) → optimized O(n) with plain object and Set (accepted)
- Valid Anagram — character count with plain object and Map, length check optimization, edge case: count going negative (accepted)
- Two Sum — brute force O(n²) → optimized O(n) with Map, store complement as key and index as value (accepted)
- Group Anagrams — sort each string as key, group originals in Map, return Array.from(map.values()) — O(n × k log k) time, O(n) space (accepted)
