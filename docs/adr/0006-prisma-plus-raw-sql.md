# ADR-0006: Prisma for CRUD and migrations, raw SQL for depth

- **Status:** Accepted
- **Date:** 2026-11-14
- **Sprint:** 1.2

## Context

A goal of the project is to learn SQL deeply: joins, window functions, CTEs, locking, procedures, execution plans. At the same time, everyday CRUD and schema migrations should be safe and productive.

## Decision

- Write the first schema **by hand** in `docs/sql/schema.sql` to learn DDL (PK, FK, UNIQUE, CHECK, DEFAULT).
- Use **Prisma** for migrations and everyday typed CRUD.
- Use **raw parameterised SQL** (`$queryRaw` with tagged templates) for reports, locking, concurrency fixes and anything where the exact SQL matters. Hand-written queries live in `docs/sql/queries.sql`.

## Alternatives considered

- **TypeORM:** decorators fit NestJS, but weaker type safety and more runtime surprises.
- **Drizzle:** SQL-like and very well typed, but its SQL Server support is younger.
- **Knex / raw SQL only:** maximum control, but no schema/migration story and more boilerplate.

## Consequences

- Positive: a type-safe generated client; readable migrations; full SQL power where needed.
- Negative: two ways to query; Prisma's SQL Server support lacks a few features (we drop to raw SQL for those); risk of N+1 queries, which Sprint 1.5 finds and fixes on purpose.

## Revisit when

Prisma blocks a needed SQL Server feature too often.
