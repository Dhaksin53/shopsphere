# ADR-0010: Service Bus with a transactional outbox

- **Status:** Accepted (applies from Sprint 4.1)
- **Date:** 2026-11-14
- **Sprint:** 4.1

## Context

After an order is placed, several side effects must happen: email, invoice PDF, loyalty points, later shipping. Doing them inside the checkout request makes checkout slow and fragile. Publishing an event right after the database commit can lose the event if the process crashes between the two steps (the dual-write problem).

## Decision

- Save the order **and** an event row in an **outbox table in the same SQL transaction**.
- A **relay** reads unpublished outbox rows and publishes them to an **Azure Service Bus topic**, then marks them published.
- Subscribers (Azure Functions) are **idempotent** (dedupe by event id), retry with exponential backoff + jitter, and send poison messages to the **dead-letter queue**.

## Alternatives considered

- **Publish after commit:** simplest, but loses events on a crash (demonstrated in Sprint 4.1).
- **Storage Queues:** cheaper, but no topics, sessions or built-in duplicate detection.
- **Kafka / Event Hubs:** built for streaming at huge scale, not for business commands with per-message settlement; Event Hubs is used for clickstream in Phase 6.
- **RabbitMQ:** capable, but self-hosted on Azure.

## Consequences

- Positive: no lost events; checkout stays fast and works when consumers are down; consumers scale independently.
- Negative: **eventual consistency** (an email may arrive seconds later); delivery is at-least-once, so every consumer must be idempotent; the relay is one more moving part.

## Revisit when

Event volume needs streaming semantics (replay, very high throughput).
