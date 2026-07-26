# Nurul Huda (Rasal)

**Senior Software Engineer**

Microservices · Multi-tenant SaaS · Distributed systems · Workforce platforms

7+ years building production systems · Dhaka, Bangladesh

[LinkedIn](https://www.linkedin.com/in/nhrasal/) · [nhrasal.cse@gmail.com](mailto:nhrasal.cse@gmail.com)

---

I design and build enterprise HR, attendance, and payroll platforms — NestJS, Laravel, and Flask
microservices over PostgreSQL, MongoDB, and Redis, behind an API gateway with asynchronous queue
processing. Attendance arrives from biometric access-control hardware and from live location
that fuses device GPS with cell-tower positioning, smoothed before it becomes a record. The
interface is a microfrontend web application and a PWA.

The work is proprietary, so what follows is the engineering: problems and tradeoffs, at pattern
level.

## Architecture & engineering focus

### Platform architecture

**Microservice decomposition.**
Services split by domain ownership — identity, attendance, scheduling, payroll, devices — each
owning its data. Gateway for synchronous calls, queue for anything crossing a boundary that
tolerates delay. No distributed transactions: consistency is eventual, carried by idempotent
consumers.

**Microfrontend composition (Module Federation).**
A host shell loading independently built remotes at runtime, shared dependencies negotiated as
singletons. Each remote deploys on its own cadence, and a missing one degrades to a fallback
instead of taking the page down.

**Multi-tenant data isolation.**
Database-per-tenant over schema-per-tenant or row-level security — hard separation and
per-tenant restore, paid for with connection pooling and migration fan-out.

### Payroll correctness

**Concurrency control for long-running jobs.**
Payroll regeneration is multi-minute and destructive, so it must be idempotent under retry. A
database-backed guard beats an external lock service: the lock shares a transaction boundary
with the state it protects, so the two cannot diverge.

**Point-in-time configuration snapshots.**
Each generated record persists a snapshot of the rules that produced it, keeping history
reproducible. Re-evaluating live rules against old records silently mutates the past.

**Transactional state machines for financial flows.**
Multi-tranche settlement as explicit states with terminal guards, so a settled record cannot be
reopened or double-paid. Invariants live in the data layer, not in convention.

**Read-path optimization.**
Aggregates maintained on write rather than derived on read, keeping list and reporting endpoints
flat as volume grows.

### Attendance ingestion

**Hardware abstraction layer (adapter pattern).**
Devices vary by vendor in protocol, push/pull model, and payload schema. A normalization layer
leaves the domain one canonical event stream and keeps vendor onboarding out of business logic.

**Low-level TCP/IP protocol implementation.**
A Python SDK implementing the devices' native socket protocol — framing, keep-alive,
command/acknowledgement, reconnection — so exactly one service carries that complexity.

**Event-driven ingestion via message queue.**
Devices publish to a broker that consumers own. Bursts, slow consumers, and restarts cannot
block a device or drop a scan; ingestion and processing scale independently.

### Mobile

**Offline-first synchronization.**
A React Native client for intermittent connectivity: local capture, durable queue,
reconciliation on reconnect, and spoof detection — an unverified reading becomes a payroll input.

**Geospatial query and geofencing.**
Geofence evaluation against tenant sites using spatial indexes rather than application-level
distance math, at a resolution that holds up when a dispute turns on location.

## Also built

- **Microfrontend commerce architecture** — runtime-federated modules composing a storefront,
  letting non-technical teams control layout without a release. Led a 7-person frontend team;
  CI/CD automation cut manual deployment work by ~70%.
- **Live video commerce** — multi-seller streaming sessions with concurrent-session handling and
  caching for low-latency product search.
- **Enterprise systems** — ERP, POS, CMS, and role-based access control across multiple
  industries.

## Working with

| | |
|---|---|
| **Languages** | TypeScript / JavaScript · PHP · Python · Java |
| **Backend** | NestJS · Express · Laravel · Spring Boot · Flask |
| **Frontend** | React · Next.js · Vue · Angular · React Native · RxJS · Module Federation · PWA |
| **Data** | PostgreSQL · MySQL · MongoDB · Redis · TypeORM |
| **APIs** | REST · GraphQL (Apollo Federation) · gRPC · WebRTC · Socket.io · SSO |
| **Infra** | Docker · GitHub Actions · Nginx · PM2 |
| **Architecture** | Microservices · hexagonal · SOA · CQRS · event-driven · multi-tenancy |

---

Open to conversations about distributed systems, multi-tenant architecture, and platform work.
