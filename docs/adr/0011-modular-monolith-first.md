# ADR-0011: Modular monolith first, microservices later

- **Status:** Accepted
- **Date:** 2026-11-14
- **Sprint:** 1.1 (monolith), 5.1 (extraction)

## Context

Microservices bring independent deployment and scaling, at the cost of network failures, distributed data, sagas, tracing and much more infrastructure. Domain boundaries are not obvious at the start, and boundaries drawn wrong are expensive to fix once they span services.

## Decision

Build a **modular monolith** first: one NestJS deployable with strict module boundaries (modules call each other's services, never each other's tables). In Phase 5, extract services with the **strangler fig** pattern (Notification first, then Catalog, Inventory, Payment, Order, Shipping), each with its own database.

The Phase 5 extraction is done **for learning**; a design note records when microservices would _not_ be justified for a team this size.

## Alternatives considered

- **Microservices from day one:** pays every distributed-systems cost before the domain is understood.
- **Stay a monolith forever:** the right call for many real products, but skips the learning goals of Phase 5.

## Consequences

- Positive: fast early development; boundaries are proven before they are cut; the cost of distribution is felt and measured.
- Negative: some refactoring during extraction; module boundaries must be policed (lint rules or reviews) so the monolith does not become a big ball of mud.

## Revisit when

Phase 5, by design.
