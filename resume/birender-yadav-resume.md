# BIRENDER YADAV

**Backend Engineer · Node.js · TypeScript · System Design · DevOps**

Bangalore, Karnataka
dev.birender@gmail.com | linkedin.com/in/devbirender | github.com/MrBirender

---

## SUMMARY

Backend engineer with 2+ years of experience building multi-tenant B2B SaaS from scratch. At Bandyl, designed and owned critical platform infrastructure — a JWT-embedded RBAC system with row-level visibility, a 3-tier multi-channel notification microservice, and per-customer isolated database connection pools supporting 500+ tenants. Takes features from architecture decision to schema design to production deployment.

---

## EXPERIENCE

### Full-Stack Developer — Winspire Tech Pvt. Ltd., Bangalore
**May 2024 – Present**

Bandyl — multi-tenant B2B SaaS platform for HR and recruitment operations.

- Architected a **fully customizable RBAC system** — ~40–50 granular permission keys embedded directly into JWT (zero runtime API calls for auth checks); row-level visibility (Own / Team / Reportees / All) backed by ContactHierarchy and Team models; checkPermission middleware applied across all ATS, HRMS, and CRM routes
- Built a **multi-channel notification microservice** — 3-tier provider scope (Org → App → User fallback chain), 6 delivery channels (SMTP, SendGrid, AWS SES, WhatsApp, Twilio, push), AES-256-GCM credential encryption, auto-provisions notification tenant on customer signup with retry; wired 14 ATS event triggers; built the full admin settings UI (Providers, Templates, Action Bindings, Test Email)
- Replaced a shared 100-connection MySQL pool with **per-customer isolated connection pools** — CustomerPoolManager with formula-based limits (ceil(maxUsers/3) + 2), 5-min config cache, 30-min idle cleanup; worst-case 3,880 active connections across 500+ tenants, well within MySQL's 9,190-connection ceiling
- Designed and built the **Super Admin portal** from scratch — tenant onboarding, provisioning status with retry/reset, RBAC configuration, notification channel setup; full ownership from DB schema to production deployment
- Developed **ATS, HRMS, CRM, and Vendor Management modules** — candidate pipeline, employee records, project engagement tracking, timesheet management, delivery workflows
- Set up **CI/CD pipelines** with GitHub Actions (self-hosted + cloud runners); configured Nginx as reverse proxy routing traffic across multiple Node.js services; managed production server (16GB RAM, MySQL + PostgreSQL + Redis) via PM2

**Tech:** Node.js, TypeScript, Express.js, React.js, MySQL, PostgreSQL, Redis, BullMQ, Socket.IO, Nginx, Docker, GitHub Actions, AWS SES, Cloudflare

---

## KEY PROJECTS

**Notification Microservice — Bandyl**
Designed a 3-tier provider scope (ORG / APP / USER level) with automatic fallback — if a user hasn't configured a provider, it falls back to app-level, then org-level. Per-action channel preferences configurable; provider credentials encrypted with AES-256-GCM using a rotating key. Auto-provisions a notification tenant on every customer signup (retry on failure). 6 delivery channels, action-template binding with compile-time variable warnings, 14 ATS event controllers migrated to the unified sender.

**RBAC + Row-Level Visibility — Bandyl**
Centralized permission registry (~40–50 keys across ATS, HRMS, CRM) seeded from a master list and embedded into JWT at login — no auth API call on protected routes. Row-level data visibility (Own / Team / Reportees / All) resolved via ContactHierarchy (reporting chain) and Team + TeamMember models. VisibilityHelper caches resolved scope per user; QueryFormatterV2 builds dynamic SQL WHERE clauses per request.

**Per-Customer Connection Pool Manager — Bandyl**
CustomerPoolManager replaced a single shared 100-connection MySQL pool. Lazily creates an isolated pool per customer on first request; limit formula: ceil(maxUsers/3) + 2; 5-min config TTL; 30-min idle cleanup. Prevents noisy-neighbor contention across tenants. Supports 500+ customers with ~3,880 worst-case active connections — 42% of MySQL's 9,190 ceiling.

---

## SKILLS

**Backend:** Node.js, TypeScript, Express.js, REST APIs, JWT, BullMQ, Socket.IO
**Databases:** PostgreSQL, MySQL, MongoDB, Redis
**DevOps:** GitHub Actions (CI/CD), Nginx, Docker, PM2, Linux
**Frontend:** React.js, JavaScript, HTML, CSS
**Cloud & Tools:** AWS SES, Cloudflare, Git, Postman

---

## EDUCATION

### Full Stack Web Development — Masai School, Bangalore
**May 2023 – Apr 2024 · 1-Year Intensive Program**
JavaScript, Node.js, React.js, SQL, Data Structures & Algorithms, System Design
