# ADR-0015: Postman and Newman for API testing

- **Status:** Accepted
- **Date:** 2026-11-14
- **Sprint:** 1.1 onward

## Context

Every sprint adds API endpoints that must be explored by hand while building and then checked automatically on every PR, against local and Azure environments. Later we need contract tests, mocks for frontend work, OAuth flows and production monitoring.

## Decision

Keep **Postman collections and environments** in `postman/` (exported JSON, versioned in git). Each folder has test scripts. **Newman** runs the collections from the command line and in GitHub Actions with an HTML report. Later: collections generated from OpenAPI, contract tests, mock servers and Monitors.

Unit and integration tests in code (Jest, supertest, Testcontainers) remain the primary safety net; Postman covers black-box API behaviour and user journeys.

## Alternatives considered

- **Bruno / Insomnia:** git-friendly files, but fewer team and monitoring features.
- **VS Code REST Client:** great for quick calls, but no test runner.
- **Code-only API tests:** precise, but no interactive exploration or shareable docs.

## Consequences

- Positive: one artefact for exploration, documentation and automated tests; an industry-standard tool.
- Negative: exported collection JSON is noisy in diffs; tests are JavaScript snippets inside JSON.

## Revisit when

Collection diffs become unmanageable (consider Postman's newer YAML collection format or Bruno).
