# ADR-0005: React + Vite first, Next.js in Phase 3

- **Status:** Accepted
- **Date:** 2026-11-14
- **Sprint:** 1.4 (Vite), 3.5 (Next.js)

## Context

The store needs a responsive UI with routing, forms and API state. Product pages benefit from server-side rendering (SEO, first paint), but SSR adds concepts (server vs client components, caching, hydration) on top of React itself.

## Decision

Start with **React + Vite** (client-side SPA) plus React Router, TanStack Query, Tailwind CSS, React Hook Form and Zod. Migrate to **Next.js** in Sprint 3.5, when performance measurements and SEO give a concrete reason: SSR for product pages, SSG/ISR for the home page.

## Alternatives considered

- **Next.js from day one:** fewer migrations, but React fundamentals get mixed up with framework behaviour.
- **Angular:** batteries included, but less common for new e-commerce frontends; more boilerplate.
- **Vue / SvelteKit:** pleasant to use, but fewer jobs in the roles this project targets.

## Consequences

- Positive: learn React without framework magic; the migration itself teaches what SSR solves and what it costs.
- Negative: one planned migration (routing and data fetching change).

## Revisit when

Not before Phase 3; the migration is part of the plan.
