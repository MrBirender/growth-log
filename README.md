# System Design and DSA — Learning Workspace

Long-term workspace to study System Design and DSA grounded in the real Bandyl codebase.

## Structure

- `system-design/` — One file per topic. Each file: concept, how it appears (or doesn't) in Bandyl, gaps/improvements, follow-up questions.
- `dsa/` — One file per topic. Each file: problem area, key patterns, worked examples, weak spots to revisit.
- `progress.md` — Single source of truth for what's done and what's next on both tracks.
- `questions-to-revisit.md` — Things I couldn't answer fully or want to come back to.

## Source-of-truth references (don't duplicate content here)

Architecture docs live in `C:\Users\sonia\Desktop\Github\Company\`:

- `bandyl-architecture.md` — Overall platform shape (Frontend, Super Admin, ATS, routing, auth, multi-tenant)
- `BANDYL_RBAC_IMPLEMENTATION.md` — Centralized RBAC across apps + row-level visibility
- `approval_flow_architecture.md` — Requisition approval flow internals
- `database-connection-pool-implementation.md` — Per-customer MySQL connection pools
- `BANDYL_NOTIFICATION_INTEGRATION_PLAN.md` — Notification microservice integration (3-tier providers, action bindings)
- `Architecture like Zoho/ARCHITECTURE.md` — CRM/UoM platform architecture (PostgreSQL + RLS + JSONB)
- `Architecture like Zoho/COLLAB_SERVICE_ARCHITECTURE.md` — Collaboration service (notes/comments/files/tags + Socket.IO)
- `CRM_IMPLEMENTATION_PLAN.md` — CRM build phases

Code repos in `C:\Users\sonia\Desktop\Github\Company\`:
- `Bandyl/` — Frontend (React)
- `Bandyl main folder/bandyl_superadmin_backend/` — Super Admin (Express + TS + Prisma + PostgreSQL)
- `hiremagic/hiremagic_ats_backend/` — ATS Backend (Express + JS + multi-tenant MySQL)
- `bandyl_crm_backend/`, `bandyl_collab_backend/`, `email_microservice/` — newer services

## How to use

When the user asks a learning question, the assistant:
1. Reads the relevant architecture doc(s) above for grounding
2. Reads code in the repo if a specific function/pattern is being discussed
3. Updates the topic file in `system-design/` or `dsa/` with the discussion outcome
4. Updates `progress.md` with what was covered
