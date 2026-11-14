# ADR-0002: TypeScript everywhere

- **Status:** Accepted
- **Date:** 2026-11-14
- **Sprint:** 1.0

## Context

One developer builds the frontend, the API, serverless functions and scripts. Switching languages between layers costs time, and the contract between frontend and backend (request and response shapes) is a common source of bugs.

## Decision

Use **TypeScript in strict mode** for all code, with `noUncheckedIndexedAccess`, `noImplicitOverride` and `noFallthroughCasesInSwitch` on top of `strict`. Pin **TypeScript `~6.0`**: TypeScript 7 (the Go-based compiler) does not ship the JavaScript compiler API yet, which typescript-eslint (`<6.1`) and `@nestjs/cli` (`~6.0`) depend on.

Shared types and Zod schemas live in `packages/shared` and are imported by both the API and the web app.

## Alternatives considered

- **C# / .NET backend + TypeScript frontend:** excellent on Azure, but two languages and duplicated DTOs.
- **Java / Spring:** mature, but heavier and again two languages.
- **Go:** fast and simple, but a smaller web-framework ecosystem and two languages.
- **Plain JavaScript:** no compile-time safety.

## Consequences

- Positive: one language and toolchain; contract changes break the build instead of production; huge ecosystem.
- Negative: Node.js is single-threaded, so CPU-heavy work must be offloaded; types are erased at runtime, so external input still needs runtime validation (class-validator / Zod).

## Revisit when

TypeScript 7 ships a compiler API and typescript-eslint and NestJS support it: then upgrade.
