# ADR-0014: GitHub Actions for CI/CD, with OIDC to Azure

- **Status:** Accepted
- **Date:** 2026-11-14
- **Sprint:** 1.0 (CI), 1.7 (deploy), 8.2 (full pipeline)

## Context

The repo is on GitHub and public. Every change must pass the same checks before reaching `main`, and deployments must not depend on long-lived cloud secrets stored in the CI system.

## Decision

- **GitHub Actions** for CI and CD, with workflows in `.github/workflows/`.
- `main` is protected: PR required, required status checks (`Lint, format, typecheck` and `PR title is a conventional commit`), squash merge only.
- Deployments authenticate to Azure with **OIDC federated credentials** (GitHub issues a short-lived token that Azure trusts) instead of stored service principal secrets.
- Workflows use least-privilege `permissions:` and pass untrusted input through `env:` to prevent script injection.

## Alternatives considered

- **Azure DevOps Pipelines:** strong Azure integration and on the AZ-400 syllabus, but a second platform next to GitHub.
- **Jenkins:** self-hosted and flexible, but maintenance-heavy.

## Consequences

- Positive: CI lives next to the code and PRs; free minutes for public repos; no stored cloud secrets.
- Negative: some AZ-400 topics are Azure DevOps-specific and are studied separately.

## Revisit when

Not expected to change during this project.
