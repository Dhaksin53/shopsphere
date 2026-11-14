# ADR-0009: Redis for cache, cart and counters

- **Status:** Accepted (applies from Sprint 3.3)
- **Date:** 2026-11-14
- **Sprint:** 3.3

## Context

Hot product pages are read thousands of times between changes. Carts are temporary and should expire. Rate limiting and flash-sale stock need atomic counters faster than a relational database can provide.

## Decision

Use **Azure Cache for Redis** for: cache-aside on product pages (invalidated on price change), carts as hashes with a TTL, bestsellers in a sorted set, per-IP and per-user rate limiting, and (Phase 6) atomic stock decrements with a Lua script.

## Alternatives considered

- **In-process memory cache:** each instance has its own copy, so instances disagree, and data is lost on restart.
- **Memcached:** a simple cache with no data structures, no persistence and no Lua.
- **CDN cache only:** covers anonymous pages but not personalised data.

## Consequences

- Positive: big latency and database-load reduction; rich data structures.
- Negative: cache invalidation and stampedes (both broken and fixed on purpose in Sprint 3.3); another service to secure and pay for; a cart in Redis can be lost if the cache is flushed, which is acceptable.

## Revisit when

Cost outweighs the measured benefit (compare p95 before and after).
