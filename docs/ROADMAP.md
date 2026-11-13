# ShopSphere Roadmap

**Goal:** understand every layer of a modern full-stack system by building one e-commerce platform and growing it from a simple app into a production-grade distributed system on Azure.

**Time:** about 12–14 months at 15–20 hours a week. Depth matters more than speed.

---

## Ground rules

### Study loop (every concept)
1. Learn the theory (docs, book chapters).
2. Build the naive version.
3. Break it on purpose (load it, kill it, race it).
4. Fix it properly and measure the before and after.
5. Write a design note in `docs/design-notes/` (use the template).

### Azure cost safety (day 1)
- [ ] Activate Azure for Students or the Azure free account
- [ ] Create a budget alert before creating anything else
- [ ] Use free tiers: Azure SQL free offer, Cosmos DB free tier, App Service F1/B1, Functions consumption plan
- [ ] Delete expensive resources after each practice session (AKS, API Management, Data Explorer, Front Door Premium)

---

## Stage 0: Prerequisites (8–12 weeks)

### 0.1 How computers and the internet work (1 week)
- [ ] IP, TCP vs UDP, ports, DNS resolution
- [ ] HTTP in depth: methods, status codes, headers, cookies, caching headers (`Cache-Control`, `ETag`), HTTP/1.1 vs HTTP/2
- [ ] HTTPS/TLS: certificates, public/private keys, the handshake
- [ ] OS basics: processes vs threads, stack vs heap, concurrency vs parallelism
- [ ] ✅ Checkpoint: explain what happens when you open `https://amazon.in`

### 0.2 Developer tooling (1 week)
- [ ] Terminal and Linux basics: navigation, permissions, env variables, pipes, `curl`, `ssh`
- [ ] Git: commits, branches, merge vs rebase, conflicts, PRs, `.gitignore`, conventional commits
- [ ] VS Code debugger and extensions
- [ ] npm: `package.json`, semantic versioning, lock files
- [ ] ✅ Checkpoint: cause a merge conflict on purpose, resolve it, open a PR

### 0.3 HTML and CSS (1–2 weeks)
- [ ] Semantic HTML, forms, accessibility (ARIA, labels, keyboard navigation)
- [ ] Box model, specificity, positioning, Flexbox, Grid, responsive design, mobile-first
- [ ] Tailwind CSS
- [ ] ✅ Checkpoint: a responsive static product-listing page built by hand

### 0.4 JavaScript in depth (2–3 weeks)
- [ ] Types, coercion, `==` vs `===`, scope, hoisting, closures, `this`, prototypes, classes
- [ ] Array methods, destructuring, spread, modules
- [ ] The event loop, call stack, microtasks vs macrotasks
- [ ] Callbacks → promises → `async/await`, error handling, `Promise.all`
- [ ] DOM, events, `fetch`, `localStorage`
- [ ] ✅ Checkpoint: a vanilla-JS cart using a fake API (dummyjson.com)

### 0.5 TypeScript (1 week)
- [ ] Types, interfaces vs type aliases, unions, generics, utility types, narrowing, strict mode
- [ ] ✅ Checkpoint: convert the vanilla-JS cart to strict TypeScript

### 0.6 React (2 weeks)
- [ ] Components, JSX, props, state, `useState`, `useEffect`, `useRef`, `useMemo`, custom hooks
- [ ] Lists and keys, controlled forms, lifting state up, Context
- [ ] React Router, TanStack Query
- [ ] How rendering and re-rendering work
- [ ] ✅ Checkpoint: the cart in React with routing (list → detail → cart)

### 0.7 Backend fundamentals (2 weeks)
- [ ] Node.js: runtime, event loop on the server, modules, streams
- [ ] Express first, then NestJS
- [ ] Middleware, routing, request/response lifecycle, error handling, env config
- [ ] OOP, SOLID, dependency injection, layered architecture
- [ ] Design patterns: Factory, Strategy, Repository, Singleton, Observer
- [ ] ✅ Checkpoint: a notes CRUD API in Express, then in NestJS

### 0.8 Databases: the basics (2 weeks)
- [ ] Tables, keys, relationships (1:1, 1:N, M:N), normalization (1NF→3NF), ER diagrams
- [ ] SQL: `SELECT`, `WHERE`, `ORDER BY`, all JOIN types, `GROUP BY`/`HAVING`, subqueries, DML, basic indexes
- [ ] NoSQL concepts: document, key-value, column-family, graph, and when to use each
- [ ] ✅ Checkpoint: SQLBolt + HackerRank Basic Select/Join, and an ER diagram for a library system

### 0.9 APIs and Postman (3–4 days)
- [ ] REST principles, idempotency of methods, JSON, OpenAPI/Swagger
- [ ] Postman: requests, collections, environments, variables, basic tests
- [ ] ✅ Checkpoint: Postman API Fundamentals Student Expert badge

### 0.10 Security basics (3–4 days)
- [ ] Authentication vs authorization; hashing vs encryption vs encoding
- [ ] Sessions vs tokens, cookie flags (`HttpOnly`, `Secure`, `SameSite`), CORS
- [ ] OWASP Top 10 overview

### 0.11 Cloud and Azure basics (1 week)
- [ ] IaaS/PaaS/SaaS, regions, availability zones, shared responsibility model
- [ ] Tenant → subscription → resource group → resource
- [ ] Azure Portal, Azure CLI, Entra ID basics, RBAC, pricing
- [ ] ✅ Checkpoint (optional): pass AZ-900

### 0.12 Testing basics (3–4 days)
- [ ] Testing pyramid, Jest unit tests, mocking, arrange-act-assert

---

## Phase 1: Monolith MVP (5–6 weeks)
**Goal:** a working store (browse, sign up, cart, order) deployed on Azure.

### Learn first
- [ ] REST design: naming, versioning, pagination (offset vs cursor), filtering, sorting, error format
- [ ] Layered architecture, DTOs, validation, global exception handling
- [ ] ORM vs raw SQL, migrations, seeding, the N+1 problem
- [ ] JWT, access and refresh tokens, bcrypt
- [ ] The 12-Factor App

### Build
- [ ] Auth: register, login, refresh, logout, protected routes, forgot password
- [ ] Catalog: categories, products, search, filter, sort, pagination
- [ ] Cart stored in SQL
- [ ] Checkout (address, order from cart, no payment yet)
- [ ] Order history and detail
- [ ] Admin: product CRUD, order list
- [ ] Frontend: React + Vite + TS, Tailwind, Router, TanStack Query, React Hook Form + Zod

### Database (Azure SQL)
- [ ] ER diagram, then hand-written DDL (PK, FK, UNIQUE, CHECK, DEFAULT)
- [ ] Seed 10,000 products
- [ ] Queries: paginated category listing, order with items (multi-JOIN), total spent per user, users with no orders, categories with >50 products
- [ ] Add indexes and compare with `SET STATISTICS IO, TIME ON`

### Postman
- [ ] Collection with a folder per resource; `local` and `azure-dev` environments
- [ ] Login test script saves the token
- [ ] Tests on every request (status, response time, JSON schema)
- [ ] Run with the Collection Runner

### Azure
- [ ] Resource group, Azure SQL (serverless), App Service (API), Static Web Apps (frontend)
- [ ] GitHub Actions deploy on push to `main`

### Break it
- [ ] Perform SQL injection on your own raw query, then fix it
- [ ] Find and fix an N+1 query

### ✅ Can you explain…
- [ ] Why PUT is idempotent and POST isn't
- [ ] Where to store a JWT, and the trade-offs
- [ ] What an index is (B-tree) and its read/write trade-off
- [ ] Why you normalize, and when to denormalize

---

## Phase 2: Correctness: money and concurrency (4–5 weeks)
**Goal:** never oversell, never double-charge, never lose an order.

### Learn first
- [ ] ACID, isolation levels and the anomalies each allows, RCSI
- [ ] Locks, deadlocks, optimistic vs pessimistic concurrency
- [ ] Idempotency keys, state machines
- [ ] OAuth 2.0 / OIDC, authorization code + PKCE

### Build
- [ ] Inventory reservation with 15-minute expiry
- [ ] Order state machine
- [ ] Stripe test payments, webhook signature verification, refunds
- [ ] `Idempotency-Key` support
- [ ] Coupons with usage limits
- [ ] RBAC: customer, staff, admin
- [ ] Move auth to Microsoft Entra External ID
- [ ] Audit trail for price and stock changes
- [ ] Admin sales dashboard

### Database
- [ ] Stored procedure `usp_PlaceOrder` with transaction and `TRY…CATCH`
- [ ] Optimistic concurrency with `rowversion`
- [ ] Temporal tables for price history
- [ ] Recursive CTE for the category tree
- [ ] Window functions: top 3 per category, running totals, MoM growth (`LAG`), first/last order
- [ ] Views, and a trigger for auditing

### Postman
- [ ] OAuth 2.0 helper with Entra
- [ ] Data-driven tests from CSV
- [ ] Newman in GitHub Actions on every PR
- [ ] Simulated Stripe webhook with a generated signature

### Azure
- [ ] Key Vault, Managed Identity, Entra External ID

### Break it
- [ ] 50 concurrent checkouts on stock = 1 with k6, then fix (pessimistic and optimistic, compared)
- [ ] Cause and fix a deadlock
- [ ] Deliver the same webhook twice

### ✅ Can you explain…
- [ ] Which isolation level prevents which anomaly
- [ ] Optimistic vs pessimistic locking
- [ ] Why the webhook is the source of truth for payment
- [ ] The OAuth2 code + PKCE flow, drawn on paper

---

## Phase 3: Performance: caching, NoSQL, CDN (4–5 weeks)
**Goal:** fast pages at scale; know when NoSQL beats SQL.

### Learn first
- [ ] Latency numbers every programmer should know
- [ ] Execution plans, seeks vs scans, index types, SARGability, statistics, Query Store
- [ ] Caching strategies, eviction, invalidation, cache stampede
- [ ] Redis data structures and persistence
- [ ] NoSQL modeling, partition keys, RUs, Cosmos DB consistency levels, CAP
- [ ] CDNs, image optimization

### Build
- [ ] App Insights baseline (p50/p95) before optimizing
- [ ] Tune the 5 slowest SQL queries
- [ ] Catalog → Cosmos DB
- [ ] Cart → Redis with TTL
- [ ] Cache-aside product pages with invalidation
- [ ] Bestsellers in a Redis sorted set
- [ ] Login rate limiting in Redis
- [ ] Direct-to-Blob uploads with SAS, served via CDN
- [ ] Migrate the frontend to Next.js (SSR/SSG), Lighthouse > 90

### Database
- [ ] Compare Cosmos partition keys and record RU costs
- [ ] Practice every Redis data structure
- [ ] Design note: why the catalog is in Cosmos DB and orders are in SQL

### Break it
- [ ] Hot partition (429s)
- [ ] Cache stampede
- [ ] Stale cached price

### ✅ Can you explain…
- [ ] How to read an execution plan
- [ ] How to choose a partition key
- [ ] Your cache invalidation strategy
- [ ] SSR vs SSG vs CSR

---

## Phase 4: Reliability: async and event-driven (4 weeks)
**Goal:** checkout keeps working when other parts fail.

### Learn first
- [ ] Sync vs async, queues vs topics vs streams
- [ ] Delivery guarantees, idempotent consumers, backoff + jitter, DLQs, poison messages
- [ ] Transactional outbox, eventual consistency
- [ ] Azure Functions, Durable Functions
- [ ] WebSockets vs SSE vs polling

### Build
- [ ] Domain events on a Service Bus topic
- [ ] Function subscribers: email, invoice PDF, loyalty points, analytics
- [ ] Outbox table and relay
- [ ] Durable Functions timer cancels unpaid orders
- [ ] Blob → Event Grid → thumbnail Function
- [ ] Live order status via Web PubSub / SignalR
- [ ] Health check endpoints

### Break it
- [ ] Stop the email Function, place 100 orders, watch the queue drain
- [ ] Poison message lands in the DLQ
- [ ] Duplicate delivery handled idempotently
- [ ] Lost event without an outbox, then fixed

### ✅ Can you explain…
- [ ] Service Bus vs Event Grid vs Event Hubs
- [ ] Why at-least-once needs idempotent consumers
- [ ] The dual-write problem and the outbox

---

## Phase 5: Microservices and containers (8–10 weeks)
**Goal:** independently deployable services; understand the cost of distribution.

### Learn first
- [ ] When NOT to use microservices
- [ ] DDD: bounded contexts, aggregates
- [ ] Database per service
- [ ] Docker in depth
- [ ] Kubernetes: pods, deployments, services, ingress, ConfigMaps, Secrets, probes, limits, HPA, Helm
- [ ] REST vs gRPC vs messaging
- [ ] Saga (choreography vs orchestration), CQRS, why not 2PC
- [ ] Resilience: timeouts, retries, circuit breaker, bulkhead
- [ ] API Gateway, BFF
- [ ] Observability: logs, metrics, traces, OpenTelemetry
- [ ] Strangler fig pattern

### Build (extract in this order)
- [ ] Notification service
- [ ] Catalog service (Cosmos DB)
- [ ] Inventory service (PostgreSQL)
- [ ] Payment service (PostgreSQL)
- [ ] Order service (Azure SQL) with the orchestrated checkout saga
- [ ] Shipping/tracking service (Cosmos DB Cassandra API, PostGIS for nearby partners)
- [ ] API Management in front
- [ ] docker-compose locally → Container Apps → AKS

### Database
- [ ] PostgreSQL vs T-SQL differences, `JSONB`, `FOR UPDATE SKIP LOCKED`, PostGIS
- [ ] Cassandra query-first table design
- [ ] KQL: failures per service, p95 per endpoint, one order's full trace

### Postman
- [ ] One collection per service + an end-to-end checkout journey
- [ ] Collections generated from OpenAPI, contract tests
- [ ] Mock servers

### Break it
- [ ] Kill Payment mid-saga, verify compensation
- [ ] Slow Inventory, watch the circuit breaker open
- [ ] Delete a pod, break a readiness probe
- [ ] Debug a failed order with tracing only

### ✅ Can you explain…
- [ ] What you lost by leaving the monolith
- [ ] Choreography vs orchestration
- [ ] The full request path: browser → Front Door → APIM → ingress → pod
- [ ] How to debug across 4 services

🎓 Certification: AZ-204

---

## Phase 6: Scale: flash sale (4–5 weeks)
**Goal:** survive 100x traffic.

### Learn first
- [ ] Capacity estimation
- [ ] Vertical vs horizontal scaling, statelessness
- [ ] Load balancing (L4/L7, algorithms)
- [ ] Replication and lag, sharding, consistent hashing
- [ ] CAP and PACELC
- [ ] Rate limiting algorithms
- [ ] Backpressure, queue-based load leveling, HPA/KEDA
- [ ] Multi-region active-passive vs active-active
- [ ] Stream vs batch processing

### Build
- [ ] Flash sale with atomic Redis Lua stock decrement + async order queue
- [ ] Virtual waiting room
- [ ] Per-user rate limiting (APIM + Redis sliding window)
- [ ] Azure SQL read replica, handle lag
- [ ] KEDA autoscaling on queue length
- [ ] Front Door multi-region failover
- [ ] Clickstream: Event Hubs → Azure Data Explorer

### Database
- [ ] KQL time series: clicks/minute, trending products, funnel, anomaly detection
- [ ] Partition the `orders` table by month

### Load testing
- [ ] Baseline, load, stress, spike, soak tests
- [ ] Document each bottleneck → fix → re-test

### ✅ Deliverable
- [ ] Design doc: "Design ShopSphere's flash sale for 10 million users"

---

## Phase 7: Intelligence: search, recommendations, AI (4 weeks)

### Learn first
- [ ] Inverted index, analyzers, BM25, facets, fuzzy matching
- [ ] Embeddings, cosine similarity, vector search, HNSW, hybrid search
- [ ] RAG, prompt engineering, grounding, content safety
- [ ] Graph modeling

### Build
- [ ] Azure AI Search fed from the Cosmos change feed (autocomplete, facets, typo tolerance)
- [ ] Hybrid keyword + vector search
- [ ] "Frequently bought together" with Cosmos DB Gremlin
- [ ] AI shopping assistant: Azure OpenAI + RAG + tool calls
- [ ] Review sentiment and summaries

### Database
- [ ] Gremlin traversals
- [ ] Search filters, facets, scoring profiles
- [ ] (Optional) MongoDB aggregation pipelines

---

## Phase 8: Production-grade (8–10 weeks)

### 8.1 Infrastructure as code
- [ ] Everything in Terraform/Bicep with modules, remote state, dev/staging/prod
- [ ] Delete and recreate the environment with one command

### 8.2 CI/CD
- [ ] Lint → unit → integration (Testcontainers) → image → Trivy → CodeQL → staging → Playwright + Newman → approval → canary → auto-rollback
- [ ] OIDC federated credentials (no stored secrets)
- [ ] Blue-green via slots / revisions
- [ ] Backward-compatible migrations (expand/contract)

### 8.3 Security
- [ ] VNet, subnets, NSGs, Private Endpoints, Private DNS
- [ ] WAF on Front Door
- [ ] Defender for Cloud, Azure Policy
- [ ] Least-privilege RBAC, secret rotation
- [ ] OWASP ZAP, Dependabot
- [ ] STRIDE threat model for checkout

### 8.4 Observability
- [ ] SLIs, SLOs, error budgets
- [ ] Dashboards, alerts, action groups
- [ ] Runbooks, mock incident, blameless postmortem

### 8.5 Reliability and DR
- [ ] Backups, point-in-time restore, SQL failover groups, Cosmos multi-region
- [ ] DR drill with measured RTO/RPO
- [ ] Azure Chaos Studio experiments

### 8.6 Configuration
- [ ] App Configuration + feature flags

### 8.7 Data and analytics
- [ ] Data Factory / Fabric → Data Lake Gen2 → warehouse
- [ ] Star schema, SCD Type 2
- [ ] `ROLLUP`, `CUBE`, `GROUPING SETS`, `PIVOT`, cohort retention, funnels, RFM
- [ ] Power BI embedded dashboard

### 8.8 Testing pyramid
- [ ] Unit, integration, contract, API, E2E, load, chaos
- [ ] Postman Monitors on production health

### 8.9 Cost
- [ ] Cost Management, tags, budgets, right-sizing, monthly cost report

### 8.10 Documentation
- [ ] C4 diagrams
- [ ] ADRs for major decisions
- [ ] OpenAPI per service in the APIM developer portal
- [ ] README, setup guide, demo video

🎓 Certification: AZ-400 (and optionally DP-900)

---

## Parallel tracks (every week)
- [ ] DSA: 3–4 problems
- [ ] System design: one chapter, redrawn from memory
- [ ] SQL: 3 problems (LeetCode SQL 50 / HackerRank)
- [ ] Postman: basics → scripts → Newman → OAuth → mocks → contracts → monitors
- [ ] Git hygiene: branches, PRs, conventional commits, releases
- [ ] Design notes for every break-and-fix

---

## Resources
| Topic | Resource |
|---|---|
| JavaScript | javascript.info, MDN |
| TypeScript | TypeScript Handbook |
| React | react.dev |
| NestJS | Official docs |
| SQL | SQLBolt → *T-SQL Fundamentals* (Itzik Ben-Gan) → use-the-index-luke.com |
| Databases | *Designing Data-Intensive Applications* (Martin Kleppmann) |
| System design | *System Design Interview* Vol 1 & 2 (Alex Xu) |
| Microservices | *Microservices Patterns* (Chris Richardson), microservices.io |
| Azure | Microsoft Learn (AZ-900/204/400), Azure Architecture Center, Well-Architected Framework |
| Kubernetes | Official docs, *Kubernetes Up & Running* |
| Postman | Postman Learning Center, Student Expert badge |
| Security | OWASP Top 10, OWASP Cheat Sheet Series |
