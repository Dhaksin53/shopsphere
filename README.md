# ShopSphere

A full-stack e-commerce platform built to learn the **entire** modern full-stack and Azure cloud skill set in depth — from a simple monolith to a secure, observable, multi-region distributed system.

> This is a learning project. Every phase follows the same loop:
> **learn the theory → build the naive version → break it on purpose → fix it properly and measure → write a design note.**

## Tech stack

| Layer | Choice |
|---|---|
| Language | TypeScript (frontend and backend) |
| Frontend | React + Vite, later Next.js, Tailwind CSS, TanStack Query |
| Backend | NestJS (Node.js), Prisma + raw SQL |
| SQL databases | Azure SQL Database, Azure Database for PostgreSQL |
| NoSQL databases | Cosmos DB (NoSQL, Gremlin, Cassandra APIs), Azure Cache for Redis, Table Storage |
| Messaging | Service Bus, Event Grid, Event Hubs |
| Compute | App Service, Azure Functions, Container Apps, AKS |
| API testing | Postman + Newman |
| Infrastructure as code | Terraform or Bicep |
| CI/CD | GitHub Actions |

## Progress

| Stage | Focus | Status |
|---|---|---|
| Stage 0 | Prerequisites | ⬜ Not started |
| Phase 1 | Monolith MVP | ⬜ Not started |
| Phase 2 | Correctness: money and concurrency | ⬜ Not started |
| Phase 3 | Performance: caching, NoSQL, CDN | ⬜ Not started |
| Phase 4 | Reliability: async and event-driven | ⬜ Not started |
| Phase 5 | Microservices and containers | ⬜ Not started |
| Phase 6 | Scale: flash sale | ⬜ Not started |
| Phase 7 | Intelligence: search, recommendations, AI | ⬜ Not started |
| Phase 8 | Production-grade | ⬜ Not started |

Status key: ⬜ Not started · 🟨 In progress · ✅ Done

## Repository layout

```
shopsphere/
├── docs/
│   ├── ROADMAP.md          # The full learning roadmap with checklists
│   └── design-notes/       # One note per concept: problem, options, choice, why
└── README.md
```

Application code (`frontend/`, `backend/`, `postman/`, `infra/`) is added from Phase 1 onward.

## Docs

- [Full roadmap](docs/ROADMAP.md)
- [Design note template](docs/design-notes/TEMPLATE.md)
