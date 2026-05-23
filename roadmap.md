# Learning Roadmap

> This is the long-term plan. Check off topics as they are added to progress.md.
> Order matters — topics build on each other. Don't skip ahead.
> Five pillars: DSA · System Design · English · Flute · Books

---

## Monthly Goals (template)

At the start of each month, fill in:
- DSA: finish ___ problems, reach ___ section
- System Design: cover ___ topics
- English: complete ___ writing exercises, ___ weekly reviews
- Flute: reach ___ (specific skill or piece)
- Books: finish ___ book(s)

### May 2026
- DSA: finish Arrays & Hashing section (7 problems remaining)
- System Design: JWT revocation + Bandyl auth review
- English: 2 writing exercises, 1 weekly review
- Flute: establish the daily habit (consistency over skill this month)
- Books: start and log at least 10 sessions

---

## DevOps (informal — log as you do it)
Not a daily practice. Tracked in `devops/devops-log.md` whenever DevOps work happens in company time.
Already done: Nginx reverse proxy, SSL, CI/CD pipeline with GitHub Actions + SSH + rsync.

---

## DSA Goal
Master interview problems common in small company interviews — arrays, strings, linked lists, hash maps.
Also master JS built-in data types: Array methods, String methods, Map, Set, Object methods.

---

## DSA Roadmap (NeetCode Blind 75 — by topic)

### 1. Arrays & Hashing ← YOU ARE HERE
- [x] Array internals (contiguous memory, O(1) index, shifting cost)
- [x] JS arrays vs classical arrays (V8 optimizations)
- [x] Linear search vs binary search
- [x] Dynamic arrays / amortized O(1) append
- [x] hasDuplicate — brute force O(n²) → optimized O(n) with Set
- [x] Contains Duplicate (LeetCode 217)
- [x] Valid Anagram (LeetCode 242)
- [x] Two Sum (LeetCode 1)
- [x] Group Anagrams (LeetCode 49)
- [ ] Top K Frequent Elements (LeetCode 347)
- [ ] Product of Array Except Self (LeetCode 238)
- [ ] Longest Consecutive Sequence (LeetCode 128)

### 2. Two Pointers
- [ ] Valid Palindrome (LeetCode 125)
- [ ] Two Sum II (LeetCode 167)
- [ ] 3Sum (LeetCode 15)
- [ ] Container With Most Water (LeetCode 11)

### 3. Sliding Window
- [ ] Best Time to Buy and Sell Stock (LeetCode 121)
- [ ] Longest Substring Without Repeating Characters (LeetCode 3)
- [ ] Longest Repeating Character Replacement (LeetCode 424)
- [ ] Minimum Window Substring (LeetCode 76)

### 4. Stack
- [ ] Valid Parentheses (LeetCode 20)
- [ ] Min Stack (LeetCode 155)
- [ ] Evaluate Reverse Polish Notation (LeetCode 150)
- [ ] Generate Parentheses (LeetCode 22)
- [ ] Daily Temperatures (LeetCode 739)
- [ ] Car Fleet (LeetCode 853)

### 5. Binary Search
- [ ] Binary Search (LeetCode 704)
- [ ] Search a 2D Matrix (LeetCode 74)
- [ ] Koko Eating Bananas (LeetCode 875)
- [ ] Find Minimum in Rotated Sorted Array (LeetCode 153)

### 6. Linked List ← partially covered (theory)
- [x] Singly/doubly structure, pointer operations
- [x] Head/tail/middle complexity
- [x] LRU Cache (doubly linked list + hashmap)
- [ ] Reverse Linked List (LeetCode 206)
- [ ] Merge Two Sorted Lists (LeetCode 21)
- [ ] Linked List Cycle (LeetCode 141)
- [ ] Reorder List (LeetCode 143)

### 7. Trees
- [ ] Invert Binary Tree (LeetCode 226)
- [ ] Maximum Depth of Binary Tree (LeetCode 104)
- [ ] Same Tree (LeetCode 100)
- [ ] Subtree of Another Tree (LeetCode 572)
- [ ] Lowest Common Ancestor (LeetCode 235)
- [ ] Binary Tree Level Order Traversal (LeetCode 102)
- [ ] Validate Binary Search Tree (LeetCode 98)

### 8. Heap / Priority Queue
- [ ] Kth Largest Element (LeetCode 215)
- [ ] Task Scheduler (LeetCode 621)
- [ ] Design Twitter (LeetCode 355)

### 9. Graphs
- [ ] Number of Islands (LeetCode 200)
- [ ] Clone Graph (LeetCode 133)
- [ ] Pacific Atlantic Water Flow (LeetCode 417)
- [ ] Course Schedule (LeetCode 207)

### 10. Dynamic Programming
- [ ] Climbing Stairs (LeetCode 70)
- [ ] House Robber (LeetCode 198)
- [ ] Coin Change (LeetCode 322)
- [ ] Longest Common Subsequence (LeetCode 1143)

---

## System Design Roadmap

### Foundations ← mostly covered
- [x] HLD vs LLD
- [x] DNS — hierarchical lookup, record types
- [x] CDN — edge caching, integration
- [x] SQL vs NoSQL trade-offs
- [x] Stateful vs stateless architecture
- [x] Session/auth architecture (JWT, stateless, Redux auth context)
- [x] Trade-offs of large JWT payloads

### Auth & Security ← YOU ARE HERE
- [ ] JWT revocation / logout / permission invalidation strategies ← next
- [ ] Critical review of Bandyl's current auth architecture
- [ ] Refresh token rotation
- [ ] RBAC — role-based vs attribute-based access control
- [ ] Row-level security (PostgreSQL RLS — already in Bandyl CRM)

### Scalability & Infrastructure
- [ ] Load balancing — round robin, least connections, consistent hashing
- [ ] Horizontal vs vertical scaling
- [ ] Database replication (primary/replica)
- [ ] Sharding strategies
- [ ] Message queues (RabbitMQ / BullMQ — relevant to Bandyl notifications)
- [ ] Rate limiting — token bucket, leaky bucket
- [ ] Caching strategies — write-through, write-behind, cache-aside

### Real-world Architecture Deep Dives (Bandyl-grounded)
- [ ] Multi-tenant connection pooling (already implemented — review and explain)
- [ ] Notification microservice architecture (3-tier providers)
- [ ] WebSocket / Socket.IO at scale (Collab service)
- [ ] Approval flow state machines
- [ ] CRM with PostgreSQL RLS + JSONB

### Classic System Design Interviews
- [ ] Design URL Shortener
- [ ] Design Rate Limiter
- [ ] Design a Chat System
- [ ] Design a Notification System (most relevant — you're building one)
- [ ] Design a Job Board / ATS (most relevant — Bandyl)

---

## English Communication

### Goals
- Write clearly with correct grammar in messages
- Articulate technical concepts precisely (interviews, code reviews)
- Comfortable in written async communication (Slack, PRs, docs)

### Practice (ongoing)
- Every session message is practice — errors will be modeled correctly
- [ ] Start Sunday weekly review (write 150–200 words about the week in English)
- [ ] Write one technical explanation per week (can be a system design concept)
