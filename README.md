# ShopSphere

A full-stack e-commerce platform built to learn the **entire** modern full-stack and Azure cloud skill set in depth — from a simple monolith to a secure, observable, multi-region distributed system.

> This is a learning project built sprint by sprint. Learning happens while building: each sprint lists the concepts it uses as a self-check, with a deep-dive resource for anything I'm unsure of.
> **Self-check → build → break it on purpose → fix it properly and measure → write a design note.**

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

| Phase | Focus | Sprints | Status |
|---|---|---|---|
| Phase 1 | Monolith MVP | 1.0–1.7 | 🟨 Next: Sprint 1.0 |
| Phase 2 | Correctness: money and concurrency | 2.1–2.5 | ⬜ Not started |
| Phase 3 | Performance: caching, NoSQL, CDN | 3.1–3.5 | ⬜ Not started |
| Phase 4 | Reliability: async and event-driven | 4.1–4.4 | ⬜ Not started |
| Phase 5 | Microservices and containers | 5.1–5.8 | ⬜ Not started |
| Phase 6 | Scale: flash sale | 6.1–6.5 | ⬜ Not started |
| Phase 7 | Intelligence: search, recommendations, AI | 7.1–7.4 | ⬜ Not started |
| Phase 8 | Production-grade | 8.1–8.9 | ⬜ Not started |

Status key: ⬜ Not started · 🟨 In progress · ✅ Done

## Repository layout

```
shopsphere/
├── docs/
│   ├── ROADMAP.md          # The full learning roadmap with checklists
│   └── design-notes/       # One note per concept: problem, options, choice, why
└── README.md
```

From Sprint 1.0 this becomes an npm-workspaces monorepo: `apps/api` (NestJS), `apps/web` (React), `postman/`, and later `services/` and `infra/`.

## Docs

- [Full roadmap](docs/ROADMAP.md)
- [Design note template](docs/design-notes/TEMPLATE.md)
