# Nurul Huda (Rasal)

**Software Architect — distributed systems, multi-tenant SaaS, payroll & workforce platforms.**
Dhaka, Bangladesh · [LinkedIn](https://www.linkedin.com/in/nhrasalcse/)

I design and build the backend of a multi-tenant HR, attendance, and payroll platform:
microservices in NestJS/TypeScript over PostgreSQL and Redis, an API gateway, async job
processing, and hardware integrations with biometric attendance devices.

Most of that work is private, so the notes below describe the *systems and the decisions*
rather than the code.

---

## Systems I've designed

**Multi-tenant isolation — database-per-tenant.**
Tenants resolve from request domain through a Redis-backed lookup to their own database and
connection. Chose DB-per-tenant over schema-per-tenant and row-level isolation for hard data
separation, per-tenant restore, and blast-radius control — accepting the cost of connection
management and migration fan-out. Runtime provisioning creates tenant schema on first use.

**Async payroll recalculation with a distributed lock.**
Payroll regeneration is long-running and must never run twice for the same period. Timestamp
columns (`started_at` / `recalculated_at`) act as both the lock and the audit trail — one
mechanism, no separate lock store to drift out of sync. Clients poll for completion; failures
release the lock and surface rather than stranding a period mid-recalc.

**A tiered deduction-policy rule engine.**
Configurable policies (late-coming, early-exit, absence, missing check-in/out) evaluated against
tiered thresholds with nested fallbacks. Every payslip stores a *snapshot* of the policy that
produced it, so historical payroll stays reproducible after policies change — the alternative,
re-evaluating live rules, silently rewrites the past.

**Partial disbursement as a state machine.**
Payslips pay out across multiple tranches. Per-tranche records roll up into a payslip-level
status, with a terminal-state guard so a fully-paid slip can't be reopened or double-paid.
Money paths get invariants, not conventions.

**Biometric device integration.**
Attendance devices over both pull (OTAP) and push (ADMS) protocols — device registration,
per-device data mode, and normalization of vendor punch formats into a single attendance stream.

**Pre-computed aggregates.**
Periodic payroll totals are maintained on write rather than derived on read, keeping listing and
reporting endpoints flat as record counts grow.

---

## Working with

**Languages** TypeScript · Java · PHP · Python
**Backend** NestJS · Node.js · Spring Boot · Laravel · GraphQL (Apollo Federation) · REST
**Data** PostgreSQL · MongoDB · Redis · TypeORM
**Frontend** React · Next.js · Vue · Redux
**Practices** Domain modeling · API design · event/queue-driven processing · multi-tenancy ·
authn/authz (JWT, RBAC) · knowledge-graph-assisted code review

---

## Public repositories

Mostly production-shaped starting points I've extracted from real projects:

- **[Rest-API](https://github.com/nhrasal/Rest-API)** — NestJS + TypeORM REST scaffolding: auth, RBAC, Swagger, device management
- **[GraphQL](https://github.com/nhrasal/GraphQL)** — NestJS + Apollo Federation, JWT, MongoDB, Redis
- **[Auth-Service](https://github.com/nhrasal/Auth-Service)** — Spring Boot authentication service (JPA, Spring Security, JWT)
- **[NextJs-Boilerplate](https://github.com/nhrasal/NextJs-Boilerplate)** — Next.js + TypeScript, SSR/SSG/CSR patterns

---

<!-- Fill these in before committing:
     - Add years of experience if you want it stated ("~N years building …")
     - Add a personal site / blog URL if you set one up
     - Add a contact email if you want inbound
-->

Open to conversations about distributed systems design, multi-tenant architecture, and
platform work.
