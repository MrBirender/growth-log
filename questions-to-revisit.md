# Questions to Revisit

> Things I couldn't fully answer, follow-ups I want to investigate, or interview questions I should be able to handle but currently can't.

---

## DSA

### Cache Locality — Why Arrays Beat Linked Lists in Practice
- **Question:** Why do arrays often outperform linked lists in real-world benchmarks despite linked lists having O(1) head/tail insert vs O(n) for arrays?
- **Hint:** CPU cache lines, memory layout, cache misses
- **Status:** Not yet answered — comes after Arrays & Hashing section
- **Where it belongs:** `dsa/arrays-and-hashing.md` or `dsa/linked-lists.md`

---

## System Design

### JWT Revocation Mid-Session
- **Question:** In Bandyl, if a super admin revokes a customer's permission mid-session, the JWT still has old permissions for up to 15 minutes. What are the strategies to fix this? What is the trade-off of each?
- **Status:** Next system design session
- **Where it belongs:** `system-design/auth-and-jwt.md`

### Bandyl Auth Architecture Review
- **Question:** Looking at the actual `customerAuth.controller.ts` and the JWT structure — what are the weaknesses, and what would you change?
- **Status:** After JWT revocation strategies
- **Where it belongs:** `system-design/auth-and-jwt.md`
