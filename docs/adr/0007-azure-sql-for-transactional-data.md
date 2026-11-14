# ADR-0007: Azure SQL for transactional data

- **Status:** Accepted
- **Date:** 2026-11-14
- **Sprint:** 1.2

## Context

Users, carts, orders, order items, payments and stock change together and involve money. They need ACID transactions, foreign keys, constraints and well-understood locking. Phase 2 deliberately explores race conditions, deadlocks and stored procedures.

## Decision

Use **Azure SQL Database** (serverless / free offer) for transactional data, with **SQL Server 2022 in Docker** locally (same engine, same T-SQL). In Phase 5 the Order service keeps Azure SQL; Inventory and Payment move to PostgreSQL to practise a second dialect.

## Alternatives considered

- **PostgreSQL for everything:** excellent and open source; used from Phase 5. Azure SQL is chosen first for its Azure integration (Managed Identity, Query Store, temporal tables, free offer) and because T-SQL is common in enterprise roles.
- **MySQL:** weaker feature set for the planned exercises.
- **A NoSQL store for orders:** multi-document transactions are limited and relational reporting is harder.

## Consequences

- Positive: strong consistency; rich tooling (Query Store, execution plans, temporal tables); serverless auto-pause keeps cost low.
- Negative: vertical scaling limits (addressed in Phase 6 with replicas and partitioning); serverless auto-pause adds a cold start of a few seconds in dev.

## Revisit when

Write throughput outgrows a single primary even after partitioning (consider sharding or Hyperscale).
