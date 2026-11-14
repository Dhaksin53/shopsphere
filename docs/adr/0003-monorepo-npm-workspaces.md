# ADR-0003: Monorepo with npm workspaces

- **Status:** Accepted
- **Date:** 2026-11-14
- **Sprint:** 1.0

## Context

ShopSphere has a web app and an API from Phase 1, shared types, and later functions, microservices and infrastructure code. Changes often touch several of them at once (for example, a new API field plus its UI).

## Decision

Use **one repository** with **npm workspaces**: `apps/*` for deployables, `packages/*` for shared libraries. Shared tooling (TypeScript base config, ESLint, Prettier, Husky, commitlint) is installed once at the root.

## Alternatives considered

- **Polyrepo (one repo per app):** clean ownership, but cross-cutting changes need several PRs and version bumps of shared packages.
- **pnpm workspaces:** faster and stricter, but one more tool to learn; npm is already required.
- **Nx / Turborepo:** task caching and dependency graphs, valuable at larger scale; overkill for two apps now.

## Consequences

- Positive: one install, one CI pipeline, atomic cross-app changes, shared types without publishing packages.
- Negative: CI runs everything on every change until we add path filters; a root script like `typecheck` fails while no workspace exists (seen in Sprint 1.0, fixed by the walking skeleton).

## Revisit when

CI time becomes painful (add Turborepo caching), or services are owned by different teams.
