# Nurul Huda (Rasal)

**Senior Software Engineer & Software Architect**
Multi-tenant SaaS · distributed systems · workforce platforms
7+ years building production systems · Dhaka, Bangladesh

[LinkedIn](https://www.linkedin.com/in/nhrasal/) · nhrasal.cse@gmail.com

---

I design and build the backend of enterprise HR, attendance, and payroll platforms — NestJS and
TypeScript microservices over PostgreSQL and Redis, behind an API gateway, with async job
processing and integrations into biometric access-control hardware.

The work itself is proprietary, so what follows is the **engineering** — the problems and the
tradeoffs, at pattern level.

## How I think about these problems

**Multi-tenant isolation.**
Database-per-tenant over schema-per-tenant or row-level isolation: hard data separation,
per-tenant backup and restore, and a contained blast radius, paid for with connection
management and migration fan-out. Tenant context resolves per request through a cached lookup
before anything touches data.

**Long-running jobs that must not run twice.**
Payroll regeneration takes minutes and is destructive if concurrent. I favour a database-backed
guard over a separate lock service — the lock lives in the same transaction boundary as the
state it protects, so the two can't drift apart. Timestamps do double duty as lock and audit
trail. Failures release and surface rather than stranding work half-done.

**Configuration that changes, records that shouldn't.**
Rule-driven deductions and pay adjustments are evaluated against tiered thresholds with
fallbacks. Each generated record stores a snapshot of the configuration that produced it, so
history stays reproducible after the rules change. Re-evaluating live rules against old records
silently rewrites the past — an auditor's worst finding.

**Money as a state machine.**
Payments that settle in tranches need explicit states and terminal guards, so a fully-settled
record can't be reopened or paid twice. Financial paths get enforced invariants, not
conventions and code review.

**Hardware behind an adapter.**
Attendance devices differ by vendor in protocol, push-vs-pull model, and payload shape. A
normalization layer absorbs those differences so the domain sees one clean event stream and
adding a vendor doesn't reach into business logic.

**Offline-first mobile capture.**
Field attendance can't assume connectivity: capture locally, queue, reconcile on reconnect,
and detect spoofed location — because an unverified reading becomes a payroll input.

**Read paths that stay flat.**
Aggregates maintained on write rather than derived on read, so listing and reporting endpoints
don't degrade as record counts grow.

## Also built

- **Microfrontend commerce architecture** — independently deployable frontend modules composing
  a storefront dynamically, letting non-technical teams control layout and content without an
  engineering release. Led a 7-person frontend team; automated CI/CD that cut manual deployment
  work by ~70%.
- **Live video commerce** — real-time multi-seller streaming sessions with concurrent-session
  handling and a caching layer for low-latency product search under load.
- **Enterprise systems** — ERP, POS, CMS, and role-based access control across multiple
  industries.

## Working with

**Languages** TypeScript / JavaScript · PHP · Python · Java
**Backend** NestJS · Express · Laravel · Spring Boot · Flask
**Frontend** React · Next.js · Vue · Angular · React Native · RxJS
**Data** PostgreSQL · MySQL · MongoDB · Redis · TypeORM
**APIs** REST · GraphQL (Apollo Federation) · gRPC · WebRTC · Socket.io · SSO
**Infra** Docker · GitHub Actions · Nginx · PM2
**Architecture** Microservices · hexagonal · SOA · CQRS · event-driven · multi-tenancy

## Public repositories

Reference implementations and starting points:

- **[Rest-API](https://github.com/nhrasal/Rest-API)** — NestJS + TypeORM REST service: auth, RBAC, Swagger, device management
- **[GraphQL](https://github.com/nhrasal/GraphQL)** — NestJS + Apollo Federation, JWT, MongoDB, Redis
- **[Auth-Service](https://github.com/nhrasal/Auth-Service)** — Spring Boot authentication service (JPA, Spring Security, JWT)
- **[NextJs-Boilerplate](https://github.com/nhrasal/NextJs-Boilerplate)** — Next.js + TypeScript, SSR/SSG/CSR patterns

---

Open to conversations about distributed systems, multi-tenant architecture, and platform work.
