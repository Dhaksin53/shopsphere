# ShopSphere Roadmap

**Goal:** understand every layer of a modern full-stack system by building one e-commerce platform and growing it from a simple app into a production-grade distributed system on Azure.

**Approach:** learn while building. There is no separate study stage. Every sprint is a build task, and the concepts it uses are listed next to it as a self-check.

**Time:** about 11–12 months at 15–20 hours a week. Most sprints are about 1 week.

---

## How each sprint works

1. **Self-check.** Read the concept questions for the sprint. If you can answer one confidently, skip it. If you can't, use the deep-dive resource next to it before building.
2. **Build** the checklist items.
3. **Break it** where a sprint has a "Break it" section, then fix it properly and measure.
4. **Write a design note** in `docs/design-notes/` for any important decision or break-and-fix (use the template).
5. **Ship:** feature branch → PR → merge into `main`, with conventional commit messages. Tick the boxes here.

## Azure cost safety

- Activate Azure only at **Sprint 1.7**. The free account's $200 credit expires after 30 days, so don't start the clock early.
- Create a **budget alert** before creating any other resource.
- Use free tiers wherever possible (Azure SQL free offer, Cosmos DB free tier, Functions consumption plan).
- Delete expensive resources after each practice session (AKS, API Management, Data Explorer, Front Door Premium).

## Architecture

The target architecture, phase-by-phase diagrams and the reason behind every technology are in [`ARCHITECTURE.md`](ARCHITECTURE.md). Each decision has an ADR in [`adr/`](adr/).

## Already done

- [x] Internet fundamentals: IP, TCP/UDP, DNS, HTTP, cookies, caching headers, TLS, OS basics

---

# Phase 1: Monolith MVP (8 weeks)

**Goal:** a working store (browse, sign up, cart, order, admin) deployed on Azure.

## Sprint 1.0: Project setup and tooling

**Build**

- [x] Turn on branch protection for `main` (PR required); CI jobs are required checks
- [x] Install Node LTS with **fnm** or **nvm-windows**; add `.nvmrc` (Node 24 pinned; fnm optional)
- [x] Monorepo with **npm workspaces**: `apps/api`, `apps/web`, `postman/` (workspaces configured; apps arrive with the walking skeleton)
- [x] Shared strict `tsconfig`, **ESLint + Prettier**
- [x] **Husky + commitlint** to enforce conventional commits
- [x] PR template (`.github/pull_request_template.md`)
- [x] GitHub Actions CI: install → lint → typecheck on every PR (+ format check and PR-title check)

**Self-check**

| Question                                                             | Deep-dive if unsure                        |
| -------------------------------------------------------------------- | ------------------------------------------ |
| Merge vs rebase, and why you never rebase a shared branch?           | learngitbranching.js.org, _Pro Git_ ch. 3  |
| `git reset` vs `git revert`; which is safe on pushed commits?        | _Pro Git_ ch. 7.7                          |
| `git fetch` vs `git pull`?                                           | _Pro Git_ ch. 2.5                          |
| What do `^1.2.3` and `~1.2.3` allow? Why commit `package-lock.json`? | docs.npmjs.com (about semantic versioning) |
| What is `PATH`, and why does "command not found" happen?             | OverTheWire Bandit levels 0–10             |
| How does an SSH key prove who you are without a password?            | GitHub Docs: "About SSH"                   |

## Sprint 1.1: API skeleton

**Build**

- [ ] NestJS app in `apps/api` with a `/health` endpoint
- [ ] Config module reading `.env` (commit `.env.example` only)
- [ ] Global validation pipe, global exception filter with a consistent error format
- [ ] Request logging with a request ID
- [ ] Swagger/OpenAPI at `/docs`, API versioning (`/api/v1`)
- [ ] Postman: first collection, `local` environment, a test on `/health`

**Self-check**

| Question                                                                                            | Deep-dive if unsure                           |
| --------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| How does the Node.js event loop handle 1,000 concurrent requests on one thread?                     | nodejs.org guide: "The Node.js Event Loop"    |
| Microtasks vs macrotasks: in what order do `setTimeout`, `Promise.then` and `process.nextTick` run? | javascript.info: "Event loop"                 |
| What is dependency injection, and why does NestJS use it?                                           | NestJS docs: Providers, Modules               |
| The NestJS request lifecycle: middleware → guards → interceptors → pipes → controller → filters?    | NestJS docs: "Request lifecycle"              |
| What are the SOLID principles? Give an example of each from this codebase.                          | refactoring.guru, "SOLID" articles            |
| Controller vs service vs repository: what belongs in each?                                          | NestJS docs + "Layered architecture" articles |
| What are the 12-Factor App principles about config?                                                 | 12factor.net                                  |

## Sprint 1.2: Database and catalog API

**Build**

- [ ] Draw the **ER diagram** first (`docs/er-diagram.md`, Mermaid): users, addresses, categories, products, carts, cart_items, orders, order_items
- [ ] Run **SQL Server 2022 in Docker** locally (`docker-compose.yml`); Azure SQL is compatible with it
- [ ] Write the DDL **by hand** once (`docs/sql/schema.sql`): PK, FK, UNIQUE, CHECK, DEFAULT
- [ ] Then set up **Prisma** with migrations matching that schema
- [ ] Seed script: 20 categories, 10,000 products
- [ ] Endpoints: list products (pagination, filter by category/price, sort, search by name), product detail, categories
- [ ] Write these queries by hand in `docs/sql/queries.sql`: paginated category listing, products with category name (JOIN), product count per category (`GROUP BY`), categories with >50 products (`HAVING`)
- [ ] Add indexes; compare with `SET STATISTICS IO, TIME ON` before and after
- [ ] Postman: Products folder with tests (status, schema, pagination)

**Self-check**

| Question                                                                | Deep-dive if unsure                              |
| ----------------------------------------------------------------------- | ------------------------------------------------ |
| Normalize a table to 3NF; when would you denormalize?                   | _T-SQL Fundamentals_ ch. 1; any 1NF–3NF tutorial |
| INNER vs LEFT vs RIGHT vs FULL vs SELF join?                            | SQLBolt lessons 6–7                              |
| `WHERE` vs `HAVING`?                                                    | SQLBolt lessons 10–12                            |
| What is an index internally (B-tree), and why does it slow down writes? | use-the-index-luke.com ch. 1                     |
| Offset vs cursor pagination: which breaks at page 5,000, and why?       | use-the-index-luke.com: "Paging through results" |
| ORM vs raw SQL; what is a migration?                                    | Prisma docs: "Migrate"                           |
| What is a Docker container vs an image? (just the basics for now)       | docs.docker.com: "Get started"                   |
| REST: resource naming, which methods are idempotent, 200 vs 201 vs 204? | MDN HTTP methods; restfulapi.net                 |

## Sprint 1.3: Authentication

**Build**

- [ ] Register, login, logout, get current user
- [ ] Passwords hashed with **bcrypt**
- [ ] Short-lived access token (JWT) + **refresh token rotation** in an `HttpOnly`, `Secure`, `SameSite` cookie
- [ ] Auth guard, `@Roles()` decorator (customer/admin)
- [ ] Forgot/reset password (token printed to the log for now)
- [ ] CORS configured for the frontend origin
- [ ] Postman: login test script saves the token automatically; Auth folder with tests

**Break it**

- [ ] Write one raw SQL query with string concatenation, **SQL-inject it yourself**, then fix it with parameters
- [ ] Try to brute-force login; add basic rate limiting

**Self-check**

| Question                                                                                       | Deep-dive if unsure                                                                             |
| ---------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| Authentication vs authorization?                                                               | OWASP Authentication Cheat Sheet                                                                |
| Hashing vs encryption vs encoding; why bcrypt and not SHA-256 for passwords?                   | OWASP Password Storage Cheat Sheet                                                              |
| What's inside a JWT? Can the client read it? Can it change it?                                 | jwt.io introduction                                                                             |
| Where should tokens be stored (localStorage vs cookie), and what does each risk (XSS vs CSRF)? | OWASP: "JWT for Java Cheat Sheet" (concepts apply to any language), CSRF Prevention Cheat Sheet |
| What do `HttpOnly`, `Secure`, `SameSite` do?                                                   | MDN: "Using HTTP cookies"                                                                       |
| What is CORS, and what is a preflight request?                                                 | MDN: "CORS"                                                                                     |
| OWASP Top 10: what are the first five?                                                         | owasp.org/Top10                                                                                 |

## Sprint 1.4: Frontend foundation

**Build**

- [ ] Vite + React + TypeScript (strict) in `apps/web`
- [ ] Tailwind CSS, a responsive layout (header, footer, mobile menu)
- [ ] React Router: home, category, product detail, login, register
- [ ] **TanStack Query** for API calls (loading, error and empty states)
- [ ] Product grid with filters, sort and pagination synced to the URL
- [ ] Accessibility pass: semantic tags, labels, keyboard navigation, alt text

**Self-check**

| Question                                                                                 | Deep-dive if unsure                                            |
| ---------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| Flexbox vs Grid: when do you use each?                                                   | css-tricks.com guides to Flexbox and Grid                      |
| Box model, specificity, mobile-first media queries?                                      | MDN: "CSS building blocks"                                     |
| Closures, `this`, prototypes: explain with code.                                         | javascript.info, Part 1                                        |
| Promises vs `async/await`; what does `Promise.all` do on failure?                        | javascript.info: "Promises, async/await"                       |
| TypeScript: interface vs type, generics, utility types, narrowing?                       | TypeScript Handbook                                            |
| When does a React component re-render? What do `useMemo`/`useCallback` actually prevent? | react.dev: "Render and Commit", "You Might Not Need an Effect" |
| Why put server data in TanStack Query instead of `useState`?                             | TanStack Query docs: "Overview"                                |
| What are semantic HTML and ARIA, and why do they matter?                                 | MDN: "Accessibility"                                           |

## Sprint 1.5: Cart, checkout and orders

**Build**

- [ ] API: cart (add, update quantity, remove), checkout (address → order from cart), order history, order detail
- [ ] Checkout writes order + order items + clears the cart **in one transaction**
- [ ] Frontend: login/register forms with **React Hook Form + Zod**, protected routes, a token-refresh interceptor
- [ ] Frontend: cart page, checkout page, order history
- [ ] Hand-written queries: order with items and product names (multi-table JOIN), total spent per user, users with no orders (`LEFT JOIN … IS NULL`)
- [ ] Postman: a chained journey (login → add to cart → checkout → view order)

**Break it**

- [ ] Turn on query logging, find the **N+1 query** on the order history page, fix it

**Self-check**

| Question                                                                                | Deep-dive if unsure                                   |
| --------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| What is a database transaction, and what does each letter of ACID mean?                 | _Designing Data-Intensive Applications_ ch. 7 (intro) |
| What is the N+1 problem, and how do JOINs or `include` fix it?                          | Prisma docs: "Query optimization"                     |
| Controlled vs uncontrolled forms; why use Zod for validation on both client and server? | react-hook-form.com, zod.dev                          |
| How does the frontend refresh an expired token without logging the user out?            | Search: "axios interceptor refresh token"             |

## Sprint 1.6: Admin and tests

**Build**

- [ ] Admin pages: product CRUD, order list with status
- [ ] Role guard on admin APIs (a customer gets 403)
- [ ] **Unit tests** (Jest) for services, with mocked repositories
- [ ] **Integration tests** for API endpoints against a real test database in Docker
- [ ] Run the Postman collection with **Newman** locally
- [ ] CI runs unit + integration tests on every PR

**Self-check**

| Question                                               | Deep-dive if unsure                            |
| ------------------------------------------------------ | ---------------------------------------------- |
| The testing pyramid: unit vs integration vs E2E?       | martinfowler.com: "The Practical Test Pyramid" |
| What is mocking, and when does it make a test useless? | Jest docs: "Mock functions"                    |
| 401 vs 403?                                            | MDN HTTP status codes                          |

## Sprint 1.7: Deploy to Azure

**Build**

- [ ] Activate Azure (Students or free account) and **create a budget alert first**
- [ ] Resource group `rg-shopsphere-dev`
- [ ] **Azure SQL Database** (serverless / free offer); run migrations against it
- [ ] **App Service** (Linux) for the API; config in App Settings
- [ ] **Static Web Apps** for the frontend
- [ ] GitHub Actions: deploy both on merge to `main`
- [ ] Postman: `azure-dev` environment; the whole collection passes against Azure

**Self-check**

| Question                                                           | Deep-dive if unsure                                |
| ------------------------------------------------------------------ | -------------------------------------------------- |
| IaaS vs PaaS vs SaaS; which are App Service and Azure SQL?         | Microsoft Learn: AZ-900 path, module 1             |
| Tenant → subscription → resource group → resource?                 | Microsoft Learn: "Azure architecture and services" |
| Regions vs availability zones?                                     | Microsoft Learn: AZ-900                            |
| What is Azure RBAC, and how is it different from your app's roles? | Microsoft Learn: "Azure RBAC"                      |
| The shared responsibility model?                                   | Microsoft Learn: AZ-900                            |

### ✅ Phase 1 is done when

- [ ] The store works end to end on Azure
- [ ] CI runs lint, typecheck, tests and Newman on every PR
- [ ] At least 2 design notes are written (for example: offset vs cursor pagination; where tokens are stored)

---

# Phase 2: Correctness: money and concurrency (5 weeks)

**Goal:** never oversell, never double-charge, never lose an order.

## Sprint 2.1: Transactions and stock reservation

**Build**

- [ ] Stock reservation at checkout, released after 15 minutes if unpaid
- [ ] Stored procedure `usp_PlaceOrder` (transaction, `TRY…CATCH`, rollback)
- [ ] Optimistic concurrency with a `rowversion` column on products

**Break it**

- [ ] Stock = 1, fire 50 concurrent checkouts with **k6**, watch it oversell
- [ ] Fix with `UPDLOCK` / atomic `UPDATE … WHERE stock >= @qty`, then with optimistic concurrency; compare in a design note
- [ ] Cause a **deadlock** with two transactions locking rows in opposite order; read the deadlock graph; fix it

**Self-check**

| Question                                                                                                                    | Deep-dive if unsure                                             |
| --------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| The 4 isolation levels + Snapshot; which anomaly (dirty read, non-repeatable read, phantom, lost update) does each prevent? | _DDIA_ ch. 7                                                    |
| What is RCSI, and why is it Azure SQL's default?                                                                            | Microsoft Learn: "Transaction locking and row versioning guide" |
| Shared vs exclusive vs update locks?                                                                                        | Same guide                                                      |
| Optimistic vs pessimistic concurrency: when to use each?                                                                    | _DDIA_ ch. 7                                                    |

## Sprint 2.2: Payments and idempotency

**Build**

- [ ] Order state machine (`PENDING → PAID → PACKED → SHIPPED → DELIVERED`, plus `CANCELLED`/`REFUNDED`); invalid transitions rejected
- [ ] **Stripe** test mode: checkout, webhook with **signature verification**, refunds
- [ ] `Idempotency-Key` header on order and payment APIs, stored in an `idempotency_keys` table
- [ ] Postman: pre-request script that generates a valid Stripe webhook signature

**Break it**

- [ ] Deliver the same webhook twice; the order must be charged and updated only once

**Self-check**

| Question                                                                        | Deep-dive if unsure                      |
| ------------------------------------------------------------------------------- | ---------------------------------------- |
| Why is the webhook, not the frontend redirect, the source of truth for payment? | Stripe docs: "Webhooks"                  |
| What is an idempotency key, and how do you store and expire them?               | Stripe docs: "Idempotent requests"       |
| How does HMAC signature verification work?                                      | Stripe docs: "Verify webhook signatures" |

## Sprint 2.3: Coupons, RBAC and audit

**Build**

- [ ] Coupons (percentage/flat, expiry, usage limit; the limit is another race condition)
- [ ] Roles: customer, staff, admin, with permission-based guards
- [ ] **Temporal tables** for price history (`FOR SYSTEM_TIME AS OF`)
- [ ] A trigger-based audit log for stock changes; write a note on why triggers are often avoided

**Self-check**

| Question                                                    | Deep-dive if unsure                |
| ----------------------------------------------------------- | ---------------------------------- |
| RBAC vs permission-based vs attribute-based access control? | OWASP Authorization Cheat Sheet    |
| How do temporal tables work internally?                     | Microsoft Learn: "Temporal tables" |

## Sprint 2.4: Real identity and secrets

**Build**

- [ ] Move auth to **Microsoft Entra External ID** (OIDC); compare with your own JWT system in a design note
- [ ] **Key Vault** for Stripe and DB secrets
- [ ] **Managed Identity**: App Service reaches SQL and Key Vault without passwords
- [ ] Postman: **OAuth 2.0 helper** gets tokens from Entra

**Self-check**

| Question                                                | Deep-dive if unsure                                |
| ------------------------------------------------------- | -------------------------------------------------- |
| Draw the OAuth2 authorization code + PKCE flow on paper | oauth.net/2/pkce; Microsoft identity platform docs |
| ID token vs access token vs refresh token?              | Microsoft identity platform docs                   |
| How does Managed Identity get a token with no secret?   | Microsoft Learn: "Managed identities"              |

## Sprint 2.5: Admin reports (advanced SQL)

**Build**

- [ ] **Recursive CTE** for the category tree
- [ ] Window functions: top 3 products per category (`RANK`), running revenue (`SUM() OVER`), month-over-month growth (`LAG`), each customer's first and last order
- [ ] Reporting views; admin dashboard showing the results
- [ ] Postman: **data-driven tests** from a CSV; **Newman in GitHub Actions** with an HTML report

**Self-check**

| Question                                | Deep-dive if unsure                             |
| --------------------------------------- | ----------------------------------------------- |
| `ROW_NUMBER` vs `RANK` vs `DENSE_RANK`? | _T-SQL Fundamentals_ ch. 7                      |
| `PARTITION BY` vs `GROUP BY`?           | Same chapter                                    |
| How does a recursive CTE terminate?     | Microsoft Learn: "WITH common_table_expression" |

---

# Phase 3: Performance: caching, NoSQL, CDN (5 weeks)

**Goal:** fast pages at scale; know when NoSQL beats SQL.

## Sprint 3.1: Measure and tune SQL

**Build**

- [ ] **Application Insights** on the API and frontend; record p50/p95 latency for key pages **before** optimizing
- [ ] Find the 5 slowest queries in **Query Store**, read their execution plans, fix them (covering, composite, filtered indexes)

**Self-check**

| Question                                                 | Deep-dive if unsure                              |
| -------------------------------------------------------- | ------------------------------------------------ |
| Index seek vs scan vs key lookup?                        | use-the-index-luke.com ch. 1–2                   |
| Clustered vs nonclustered vs covering vs filtered index? | Microsoft Learn: "SQL Server index design guide" |
| Why does column order matter in a composite index?       | use-the-index-luke.com: "Concatenated keys"      |
| What makes a query non-SARGable?                         | Search: "SARGable queries"                       |

## Sprint 3.2: Catalog → Cosmos DB

**Build**

- [ ] Move products to **Cosmos DB (NoSQL API, free tier)**; products have different attributes per category
- [ ] Compare partition keys (`/categoryId` vs `/id` vs `/brand`); record the RU cost of each query
- [ ] Practice queries: filters, `ARRAY_CONTAINS`, intra-document `JOIN`, aggregates; point read vs cross-partition query
- [ ] Design note: "Why the catalog is in Cosmos DB and orders are in SQL"

**Break it**

- [ ] Use a bad partition key (`/isActive`), load it, watch 429 throttling

**Self-check**

| Question                                                  | Deep-dive if unsure                                 |
| --------------------------------------------------------- | --------------------------------------------------- |
| Embedding vs referencing in document models?              | Microsoft Learn: "Data modeling in Cosmos DB"       |
| What makes a good partition key? What is a hot partition? | Microsoft Learn: "Partitioning in Cosmos DB"        |
| What is a Request Unit?                                   | Microsoft Learn: "Request Units"                    |
| The 5 Cosmos DB consistency levels? CAP theorem?          | Microsoft Learn: "Consistency levels"; _DDIA_ ch. 9 |

## Sprint 3.3: Redis

**Build**

- [ ] **Azure Cache for Redis**
- [ ] Cart → Redis hash with TTL (abandoned carts expire)
- [ ] Cache-aside for product pages; invalidate on price update
- [ ] Bestsellers in a **sorted set**
- [ ] Login rate limiting per IP
- [ ] Measure p95 again and compare with Sprint 3.1

**Break it**

- [ ] **Cache stampede:** expire a hot key under load, watch the DB spike, fix with a lock or early refresh
- [ ] **Stale data:** change a price without invalidation, see the wrong price, fix it

**Self-check**

| Question                                                                       | Deep-dive if unsure                                                                    |
| ------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------- |
| Cache-aside vs read-through vs write-through vs write-behind?                  | AWS/Azure docs: "Caching strategies"; Azure Architecture Center: "Cache-Aside pattern" |
| LRU vs LFU eviction?                                                           | redis.io: "Key eviction"                                                               |
| Redis data structures: string, hash, list, set, sorted set; one use case each? | redis.io: "Data types"                                                                 |
| RDB vs AOF persistence?                                                        | redis.io: "Persistence"                                                                |

## Sprint 3.4: Images and CDN

**Build**

- [ ] Browser uploads images **directly to Blob Storage** using a short-lived **SAS token**
- [ ] Serve images through **Azure Front Door / CDN**; set cache headers
- [ ] Responsive images (WebP, multiple sizes)

**Self-check**

| Question                                                 | Deep-dive if unsure                            |
| -------------------------------------------------------- | ---------------------------------------------- |
| Why upload directly to Blob instead of through your API? | Azure Architecture Center: "Valet Key pattern" |
| How does a CDN decide when to fetch from origin?         | Cloudflare Learning: "What is a CDN"           |

## Sprint 3.5: Next.js migration

**Build**

- [ ] Move the frontend to **Next.js**: SSR for product pages (SEO), SSG/ISR for the home page
- [ ] Image optimization, lazy loading, code splitting
- [ ] Lighthouse score above 90

**Self-check**

| Question                                    | Deep-dive if unsure          |
| ------------------------------------------- | ---------------------------- |
| CSR vs SSR vs SSG vs ISR: when to use each? | nextjs.org docs: "Rendering" |
| What are Core Web Vitals (LCP, INP, CLS)?   | web.dev: "Web Vitals"        |

---

# Phase 4: Reliability: async and event-driven (4 weeks)

**Goal:** checkout keeps working when other parts fail.

## Sprint 4.1: Domain events and the outbox

**Build**

- [ ] Publish `OrderPlaced`, `OrderPaid`, `OrderShipped` to a **Service Bus topic**
- [ ] **Transactional outbox:** order + event saved in one SQL transaction; a relay publishes the events

**Break it**

- [ ] Without the outbox: crash the app between saving and publishing, see the lost event; then fix it with the outbox

**Self-check**

| Question                                                                 | Deep-dive if unsure                                        |
| ------------------------------------------------------------------------ | ---------------------------------------------------------- |
| Queue vs topic vs event stream; Service Bus vs Event Grid vs Event Hubs? | Microsoft Learn: "Choose between Azure messaging services" |
| What is the dual-write problem?                                          | microservices.io: "Transactional outbox"                   |
| At-most-once vs at-least-once vs exactly-once?                           | _DDIA_ ch. 11                                              |

## Sprint 4.2: Subscribers

**Build**

- [ ] **Azure Functions** subscribers: email (**Azure Communication Services**), invoice PDF to Blob, loyalty points
- [ ] Idempotent consumers; retries with exponential backoff + jitter

**Break it**

- [ ] Stop the email Function, place 100 orders, watch the queue drain on restart
- [ ] Publish a malformed message, watch it reach the **dead-letter queue**
- [ ] Deliver a message twice; the user gets one email and one set of points

**Self-check**

| Question                                                      | Deep-dive if unsure                                      |
| ------------------------------------------------------------- | -------------------------------------------------------- |
| Why does at-least-once delivery require idempotent consumers? | Microsoft Learn: "Service Bus duplicate detection"       |
| What are poison messages and DLQs?                            | Microsoft Learn: "Service Bus dead-letter queues"        |
| Why add jitter to retries?                                    | AWS Architecture Blog: "Exponential backoff and jitter"  |
| Functions triggers vs bindings; what causes cold starts?      | Microsoft Learn: "Azure Functions triggers and bindings" |

## Sprint 4.3: Durable workflows and Event Grid

**Build**

- [ ] **Durable Functions** timer cancels unpaid orders after 15 minutes and releases stock
- [ ] Blob upload → **Event Grid** → Function generates thumbnails

**Self-check**

| Question                                                             | Deep-dive if unsure                       |
| -------------------------------------------------------------------- | ----------------------------------------- |
| How do Durable Functions survive restarts (event sourcing + replay)? | Microsoft Learn: "Durable orchestrations" |
| Push (Event Grid) vs pull (Service Bus) delivery?                    | Microsoft Learn: messaging comparison     |

## Sprint 4.4: Real-time order status

**Build**

- [ ] Live order status in the browser via **Azure Web PubSub** (or SignalR Service)
- [ ] `/health/live` and `/health/ready` endpoints
- [ ] Postman: **WebSocket request** test; tests that poll until the order state changes

**Self-check**

| Question                                                      | Deep-dive if unsure                         |
| ------------------------------------------------------------- | ------------------------------------------- |
| WebSockets vs SSE vs long polling?                            | MDN: "WebSockets API", "Server-sent events" |
| What is eventual consistency, and how do you show it in a UI? | _DDIA_ ch. 5                                |

---

# Phase 5: Microservices and containers (9 weeks)

**Goal:** independently deployable services; understand the cost of distribution.

## Sprint 5.1: Docker and domain boundaries

**Build**

- [ ] Multi-stage Dockerfiles for the API and web app; the whole system runs with **docker-compose**
- [ ] Design doc: bounded-context map (Catalog, Inventory, Order, Payment, Shipping, Notification) and which database each owns
- [ ] Design note: "When NOT to use microservices", and why you're doing it anyway (learning)

**Self-check**

| Question                                          | Deep-dive if unsure                              |
| ------------------------------------------------- | ------------------------------------------------ |
| Image layers and caching; why multi-stage builds? | docs.docker.com: "Build best practices"          |
| Volumes vs bind mounts; Docker networks?          | docs.docker.com                                  |
| Bounded context, aggregate, ubiquitous language?  | _Domain-Driven Design Distilled_ (Vaughn Vernon) |
| Why database-per-service?                         | microservices.io: "Database per service"         |

## Sprint 5.2: First extractions

**Build**

- [ ] Use the **strangler fig** pattern: extract **Notification**, then **Catalog** (Cosmos DB)
- [ ] Push images to **Azure Container Registry**; deploy to **Azure Container Apps**

**Self-check**

| Question                           | Deep-dive if unsure                                |
| ---------------------------------- | -------------------------------------------------- |
| What is the strangler fig pattern? | Azure Architecture Center: "Strangler Fig pattern" |

## Sprint 5.3: Inventory service (PostgreSQL + gRPC)

**Build**

- [ ] Inventory service on **PostgreSQL Flexible Server**
- [ ] Order → Inventory reservation over **gRPC** (protobuf)
- [ ] PostgreSQL practice: `RETURNING`, `JSONB` queries, `SELECT … FOR UPDATE SKIP LOCKED`

**Self-check**

| Question                                    | Deep-dive if unsure     |
| ------------------------------------------- | ----------------------- |
| REST vs gRPC vs messaging between services? | grpc.io: "Introduction" |
| T-SQL vs PostgreSQL differences you hit?    | postgresql.org docs     |

## Sprint 5.4: Payment, Order and the saga

**Build**

- [ ] Payment service (PostgreSQL) and Order service (Azure SQL)
- [ ] **Orchestrated checkout saga:** reserve stock → charge → confirm; compensate on failure
- [ ] Resilience: timeouts, retries, **circuit breaker**, bulkhead

**Break it**

- [ ] Kill Payment mid-saga: stock must be released and the order marked failed
- [ ] Slow Inventory by 5s: the circuit breaker opens and checkout fails fast

**Self-check**

| Question                                          | Deep-dive if unsure                                      |
| ------------------------------------------------- | -------------------------------------------------------- |
| Choreography vs orchestration sagas?              | microservices.io: "Saga"; _Microservices Patterns_ ch. 4 |
| Why is two-phase commit avoided?                  | _DDIA_ ch. 9                                             |
| Circuit breaker states (closed, open, half-open)? | Azure Architecture Center: "Circuit Breaker pattern"     |
| What is CQRS?                                     | Azure Architecture Center: "CQRS pattern"                |

## Sprint 5.5: Shipping service

**Build**

- [ ] Delivery location history in **Cosmos DB Cassandra API** (query-first table design)
- [ ] "Delivery partners within 5 km" with **PostGIS** (`ST_DWithin`)

**Self-check**

| Question                                                    | Deep-dive if unsure                    |
| ----------------------------------------------------------- | -------------------------------------- |
| Partition key vs clustering key in Cassandra; why no JOINs? | Apache Cassandra docs: "Data modeling" |
| How does a geospatial index work?                           | PostGIS docs: "Spatial indexing"       |

## Sprint 5.6: Gateway and observability

**Build**

- [ ] **Azure API Management** in front: routing, JWT validation, rate limits, versioning, developer portal
- [ ] **OpenTelemetry** in all services; correlation IDs; distributed traces in App Insights
- [ ] KQL queries: failures per service, p95 per endpoint, one order's full trace

**Self-check**

| Question                                         | Deep-dive if unsure                      |
| ------------------------------------------------ | ---------------------------------------- |
| API Gateway vs BFF?                              | microservices.io: "API gateway"          |
| Logs vs metrics vs traces?                       | opentelemetry.io: "Observability primer" |
| KQL basics: `where`, `summarize`, `bin`, `join`? | Microsoft Learn: "KQL tutorial"          |

## Sprint 5.7: Kubernetes

**Build**

- [ ] Move from Container Apps to **AKS** with **Helm** charts
- [ ] Ingress, ConfigMaps, Secrets (from Key Vault), resource requests/limits
- [ ] Liveness/readiness probes, **HPA**

**Break it**

- [ ] Delete a pod and watch it self-heal; break a readiness probe and watch traffic stop

**Self-check**

| Question                                               | Deep-dive if unsure                  |
| ------------------------------------------------------ | ------------------------------------ |
| Pod vs Deployment vs ReplicaSet vs Service?            | kubernetes.io: "Concepts"            |
| ClusterIP vs NodePort vs LoadBalancer vs Ingress?      | kubernetes.io: "Service"             |
| Liveness vs readiness vs startup probes?               | kubernetes.io: "Probes"              |
| Requests vs limits; what happens when you exceed each? | kubernetes.io: "Resource management" |

## Sprint 5.8: API testing at scale

**Build**

- [ ] One Postman collection per service + an end-to-end checkout journey
- [ ] Collections generated from each service's **OpenAPI** spec; **contract tests**
- [ ] Postman **mock servers** for frontend work
- [ ] Debug a failed order using distributed tracing alone

### ✅ Phase 5 checkpoint

- [ ] You can trace a request: browser → Front Door → APIM → ingress → pod → database
- [ ] 🎓 Ready for **AZ-204**

---

# Phase 6: Scale: flash sale (5 weeks)

**Goal:** survive 100x traffic.

## Sprint 6.1: Estimate and baseline

**Build**

- [ ] Capacity estimate for 10 million users (DAU, peak QPS, storage, bandwidth)
- [ ] Baseline load test with **Azure Load Testing** or **k6** (load, stress, spike, soak)

**Self-check**

| Question                                                        | Deep-dive if unsure                    |
| --------------------------------------------------------------- | -------------------------------------- |
| How do you do back-of-envelope estimation?                      | _System Design Interview_ Vol 1, ch. 2 |
| Vertical vs horizontal scaling; why must services be stateless? | Same book, ch. 1                       |

## Sprint 6.2: Flash sale

**Build**

- [ ] Atomic stock decrement in Redis with a **Lua script**; orders processed asynchronously from a queue
- [ ] **Virtual waiting room** ("You're #4,213 in line")
- [ ] Per-user rate limiting (APIM + Redis sliding window)

**Self-check**

| Question                                                 | Deep-dive if unsure                                    |
| -------------------------------------------------------- | ------------------------------------------------------ |
| Token bucket vs leaky bucket vs fixed vs sliding window? | _System Design Interview_ Vol 1, ch. 4                 |
| Queue-based load leveling, backpressure?                 | Azure Architecture Center: "Queue-Based Load Leveling" |

## Sprint 6.3: Database scale

**Build**

- [ ] Azure SQL **read replica** for reads; handle replication lag (read-your-writes)
- [ ] **Table partitioning** of `orders` by month
- [ ] **KEDA** autoscales the order processor on queue length

**Self-check**

| Question                                          | Deep-dive if unsure                                  |
| ------------------------------------------------- | ---------------------------------------------------- |
| Replication lag and read-your-writes consistency? | _DDIA_ ch. 5                                         |
| Sharding strategies, consistent hashing?          | _DDIA_ ch. 6; _System Design Interview_ Vol 1, ch. 5 |
| CAP vs PACELC?                                    | _DDIA_ ch. 9                                         |

## Sprint 6.4: Multi-region

**Build**

- [ ] **Front Door** with a second region (active-passive); test failover

**Self-check**

| Question                                                   | Deep-dive if unsure                                 |
| ---------------------------------------------------------- | --------------------------------------------------- |
| L4 vs L7 load balancing; round robin vs least connections? | Azure Architecture Center: "Load-balancing options" |
| Active-passive vs active-active?                           | Azure Well-Architected Framework: Reliability       |

## Sprint 6.5: Clickstream analytics

**Build**

- [ ] Every click → **Event Hubs** → **Azure Data Explorer**
- [ ] KQL: clicks per minute (`bin()`), trending products, conversion funnel, anomaly detection (`series_decompose_anomalies`)
- [ ] ✅ Final design doc: **"Design ShopSphere's flash sale for 10 million users"**, with load-test results and every bottleneck you fixed

**Self-check**

| Question                                   | Deep-dive if unsure                    |
| ------------------------------------------ | -------------------------------------- |
| Stream vs batch processing?                | _DDIA_ ch. 10–11                       |
| Event Hubs partitions and consumer groups? | Microsoft Learn: "Event Hubs features" |

---

# Phase 7: Intelligence: search, recommendations, AI (4 weeks)

## Sprint 7.1: Search

**Build**

- [ ] **Azure AI Search** index fed from the Cosmos DB change feed (indexer)
- [ ] Autocomplete, typo tolerance, facets (brand, price, rating), boosting

**Self-check**

| Question                                           | Deep-dive if unsure                                    |
| -------------------------------------------------- | ------------------------------------------------------ |
| Inverted index, tokenization, analyzers, stemming? | Microsoft Learn: "Full-text search in Azure AI Search" |
| How does BM25 score relevance?                     | Same docs: "Relevance scoring"                         |

## Sprint 7.2: Vector and hybrid search

**Build**

- [ ] Product embeddings; **hybrid keyword + vector search** with semantic ranking ("comfortable running shoes under ₹3000")

**Self-check**

| Question                                                   | Deep-dive if unsure              |
| ---------------------------------------------------------- | -------------------------------- |
| What is an embedding? What does cosine similarity measure? | Microsoft Learn: "Vector search" |
| What is approximate nearest neighbour search (HNSW)?       | Same docs                        |

## Sprint 7.3: Recommendations

**Build**

- [ ] "Frequently bought together" and "customers also viewed" with **Cosmos DB Gremlin**
- [ ] Gremlin traversals, for example `g.V('p1').in('bought').out('bought').where(neq('p1')).groupCount()`

**Self-check**

| Question                                                                | Deep-dive if unsure                     |
| ----------------------------------------------------------------------- | --------------------------------------- |
| Vertices, edges, traversals; when is a graph database the right choice? | tinkerpop.apache.org: "Getting started" |

## Sprint 7.4: AI assistant

**Build**

- [ ] Shopping assistant: **Azure OpenAI** + RAG over products, FAQs and return policy; tool calls for order status
- [ ] Review sentiment and summaries with **Azure AI Language**
- [ ] (Optional) **MongoDB API** on Cosmos DB with aggregation pipelines for review analytics

**Self-check**

| Question                                              | Deep-dive if unsure                       |
| ----------------------------------------------------- | ----------------------------------------- |
| What is RAG, and why does it reduce hallucinations?   | Microsoft Learn: "RAG in Azure AI Search" |
| Prompt engineering basics, grounding, content safety? | Azure OpenAI docs                         |

---

# Phase 8: Production-grade (9 weeks)

## Sprint 8.1: Infrastructure as code

- [ ] Every resource in **Terraform** ([ADR-0013](adr/0013-terraform-for-iac.md)): modules, remote state, dev/staging/prod
- [ ] Delete the environment and recreate it with one command

## Sprint 8.2: Full CI/CD

- [ ] Lint → unit → integration (Testcontainers) → image → Trivy → CodeQL → staging → Playwright + Newman → approval → canary → auto-rollback
- [ ] **OIDC federated credentials** (no stored secrets in GitHub)
- [ ] Blue-green via deployment slots / revisions
- [ ] Backward-compatible migrations (expand/contract)

## Sprint 8.3: Network security

- [ ] VNet, subnets, NSGs, **Private Endpoints** for SQL/Cosmos/Redis/Key Vault, Private DNS
- [ ] WAF on Front Door

## Sprint 8.4: Application security

- [ ] Defender for Cloud, Azure Policy, least-privilege RBAC, secret rotation
- [ ] OWASP ZAP (DAST), Dependabot
- [ ] **STRIDE** threat model for checkout

## Sprint 8.5: Observability and operations

- [ ] SLIs, SLOs, error budgets
- [ ] Dashboards, alerts, action groups
- [ ] Runbooks; a mock incident and a blameless postmortem
- [ ] Postman **Monitors** on production health

## Sprint 8.6: Disaster recovery and chaos

- [ ] Backups, point-in-time restore, SQL failover groups, Cosmos DB multi-region
- [ ] DR drill with measured RTO/RPO
- [ ] **Azure Chaos Studio** experiments

## Sprint 8.7: Feature flags and analytics

- [ ] **App Configuration** + feature flags (dark launch, percentage rollout)
- [ ] **Data Factory / Fabric** → Data Lake Gen2 → warehouse; star schema; SCD Type 2
- [ ] Analytical SQL: `ROLLUP`, `CUBE`, `GROUPING SETS`, `PIVOT`, cohort retention, funnels, RFM segmentation
- [ ] **Power BI** dashboard embedded in the admin panel

## Sprint 8.8: Testing and cost

- [ ] Complete testing pyramid: unit, integration, contract, API, **Playwright E2E**, load, chaos
- [ ] Cost Management, tags, budgets, right-sizing; monthly cost report

## Sprint 8.9: Documentation

- [ ] **C4 diagrams**, **ADRs** for major decisions
- [ ] OpenAPI per service in the APIM developer portal
- [ ] README, setup guide, demo video
- [ ] 🎓 Ready for **AZ-400** (optionally DP-900)

**Self-check (Phase 8)**

| Question                                                  | Deep-dive if unsure                                      |
| --------------------------------------------------------- | -------------------------------------------------------- |
| Terraform state: why remote, and what is locking?         | developer.hashicorp.com: "State"                         |
| Blue-green vs canary vs rolling deployments?              | Azure Well-Architected Framework: Operational Excellence |
| What do Private Endpoints change about how traffic flows? | Microsoft Learn: "Private Link"                          |
| SLI vs SLO vs SLA; what is an error budget?               | Google SRE book ch. 4 (free online)                      |
| RTO vs RPO?                                               | Azure Well-Architected Framework: Reliability            |
| OLTP vs OLAP; star schema; SCD Type 2?                    | _The Data Warehouse Toolkit_ (Kimball) ch. 1–2           |
| STRIDE categories?                                        | Microsoft Threat Modeling Tool docs                      |

---

## Parallel tracks (every week)

- [ ] DSA: 3–4 problems
- [ ] SQL: 3 problems (LeetCode SQL 50 → window functions)
- [ ] System design: one chapter, redrawn from memory
- [ ] Design notes for every break-and-fix

## Main resources

| Topic                             | Resource                                                                                |
| --------------------------------- | --------------------------------------------------------------------------------------- |
| JavaScript                        | javascript.info, MDN                                                                    |
| TypeScript                        | TypeScript Handbook                                                                     |
| React                             | react.dev                                                                               |
| NestJS                            | Official docs                                                                           |
| SQL                               | SQLBolt → _T-SQL Fundamentals_ (Itzik Ben-Gan) → use-the-index-luke.com                 |
| Databases and distributed systems | _Designing Data-Intensive Applications_ (Martin Kleppmann)                              |
| System design                     | _System Design Interview_ Vol 1 & 2 (Alex Xu)                                           |
| Microservices                     | _Microservices Patterns_ (Chris Richardson), microservices.io                           |
| Azure                             | Microsoft Learn (AZ-900/204/400), Azure Architecture Center, Well-Architected Framework |
| Kubernetes                        | kubernetes.io, _Kubernetes Up & Running_                                                |
| Postman                           | Postman Learning Center                                                                 |
| Security                          | OWASP Top 10, OWASP Cheat Sheet Series                                                  |
