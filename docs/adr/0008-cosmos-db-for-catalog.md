# ADR-0008: Cosmos DB for the catalog

- **Status:** Accepted (applies from Sprint 3.2)
- **Date:** 2026-11-14
- **Sprint:** 3.2

## Context

Product attributes differ per category (phones: RAM, storage; shirts: size, fabric). In SQL this becomes an entity-attribute-value table or a wide, sparse table. The catalog is read far more than written and does not need multi-entity transactions.

## Decision

Move products to **Azure Cosmos DB for NoSQL** (free tier) in Sprint 3.2. Choose the partition key by measuring the RU cost of real queries for `/categoryId`, `/id` and `/brand` (expected winner: `/categoryId`). Keep orders in SQL. The change feed later feeds Azure AI Search (Phase 7).

## Alternatives considered

- **Keep products in SQL with a JSON column:** works and is simpler; we use SQL in Phase 1 and compare.
- **MongoDB Atlas:** a similar model, but outside Azure. Cosmos DB's MongoDB API is tried optionally in Phase 7.

## Consequences

- Positive: flexible schema; single-digit-millisecond point reads; global distribution and change feed available.
- Negative: a bad partition key causes hot partitions and 429 throttling (Sprint 3.2 does this on purpose); cross-partition queries cost more; data now lives in two stores, so product details shown in orders are copied (snapshot) at checkout.

## Revisit when

Catalog queries become relational (complex joins across entities).
