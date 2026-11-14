# ADR-0004: NestJS for the backend

- **Status:** Accepted
- **Date:** 2026-11-14
- **Sprint:** 1.1

## Context

The API starts as a modular monolith and is split into microservices in Phase 5. It needs validation, auth guards, consistent error handling, logging, OpenAPI docs and testable services from day one.

## Decision

Use **NestJS** (on Express). Each domain is a Nest **module** (controller + service + repository). Cross-cutting concerns use Nest building blocks: **pipes** (validation), **guards** (auth, roles), **interceptors** (logging, request IDs), **exception filters** (error format). Swagger is generated from decorators.

## Alternatives considered

- **Express / Fastify by hand:** minimal and flexible, but every structural decision is ours, and structure is what a growing codebase needs most.
- **Hono:** very fast and modern, but a small ecosystem for enterprise patterns.
- **.NET / Spring:** see ADR-0002.

## Consequences

- Positive: enforced structure similar to the Angular/Spring patterns used in enterprises; dependency injection makes unit testing with mocks easy; module boundaries become service boundaries; built-in microservice transports (gRPC, custom transports for Service Bus).
- Negative: decorators and DI add "magic" and a learning curve; more boilerplate than plain Express.

## Revisit when

A service needs extreme throughput (consider the Fastify adapter) or turns out to be trivially small (a plain Function may be enough).
