# Nurul Huda (Rasal)

**Senior Software Engineer**

Microservices · Multi-tenant SaaS · Distributed systems · Event-driven architecture

7+ years building production systems · Dhaka, Bangladesh

[LinkedIn](https://www.linkedin.com/in/nhrasal/) · [nhrasal.cse@gmail.com](mailto:nhrasal.cse@gmail.com)

---

I design and build multi-tenant enterprise SaaS — NestJS, Laravel, and Flask microservices over
PostgreSQL, MongoDB, and Redis, behind an API gateway with asynchronous queue processing.
Ingestion runs from two sources: access-control hardware over its native socket protocol, and
live location that fuses device GPS with cell-tower positioning, smoothed before anything is
persisted. The interface is a microfrontend web application and a PWA.

Alongside the architecture I set technical direction across product lines, run code review, and
have led a 7-person frontend team.

The work is proprietary, so what follows is the engineering: problems and tradeoffs, at pattern
level.

## Architecture & engineering focus

### Platform architecture

**Microservice decomposition.**
Services split along domain boundaries, each owning its data and exposing it only through a
contract. Gateway for synchronous calls, queue for anything crossing a boundary that tolerates
delay. No distributed transactions: consistency is eventual, carried by idempotent consumers.

**Microfrontend composition (Module Federation).**
A host shell loading independently built remotes at runtime, shared dependencies negotiated as
singletons. Each remote deploys on its own cadence, and a missing one degrades to a fallback
instead of taking the page down.

**Multi-tenant data isolation.**
Database-per-tenant over schema-per-tenant or row-level security — hard separation and
per-tenant restore, paid for with connection pooling and migration fan-out.

### Security

**Authentication and authorization.**
JWT with short-lived access tokens and refresh rotation, role- and permission-based access
evaluated at the gateway and re-checked in-service — an upstream claim is input, not proof.
SSO where the tenant requires it.

**The tenant boundary is the security boundary.**
Tenant context resolves before any data access, and isolation is enforced at the connection
level rather than by a `WHERE` clause — a forgotten filter cannot leak across tenants.

**Untrusted input at the edges.**
Devices and mobile clients authenticate before they can publish, payloads are validated at the
boundary, and client-supplied readings are treated as claims to verify rather than facts to
store.

### Mobile

**Offline-first synchronization.**
A React Native client for intermittent connectivity: local capture, durable queue,
reconciliation on reconnect, and spoof detection — an unverified reading must never be persisted
as fact.

**Geospatial query and geofencing.**
Geofence evaluation against tenant sites using spatial indexes rather than application-level
distance math, at a resolution that holds up when a stored position is later disputed.

## Also built

- **Microfrontend commerce architecture** — runtime-federated modules composing a storefront,
  letting non-technical teams control layout without a release. CI/CD automation cut manual
  deployment work by ~70%.
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
| **Design patterns** | Adapter · Factory Method · Observer · Decorator · Facade · Builder · Repository · Singleton |

---

Open to conversations about distributed systems, multi-tenant architecture, and platform work.
