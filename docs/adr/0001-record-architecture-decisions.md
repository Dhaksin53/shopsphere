# ADR-0001: Record architecture decisions

- **Status:** Accepted
- **Date:** 2026-11-14
- **Sprint:** before 1.1

## Context

ShopSphere will make dozens of technology and design decisions over about a year. Months later, "why did we pick this?" is hard to answer from code alone, and the learning value of the project depends on understanding each trade-off.

## Decision

Record every significant decision as an **Architecture Decision Record (ADR)** in `docs/adr/`, numbered and never deleted. A decision that changes later gets a **new** ADR that marks the old one `Superseded by ADR-XXXX`.

Format: Status, Date, Sprint, Context, Decision, Alternatives considered, Consequences, Revisit when.

ADRs are for **decisions**. Design notes (`docs/design-notes/`) are for **experiments** (break it, fix it, measure it).

## Alternatives considered

- **Wiki pages:** drift away from the code and are not reviewed in PRs.
- **Comments in code:** can't explain decisions that span many files.
- **Nothing:** decisions get re-argued and the reasoning is lost.

## Consequences

- Positive: every decision is reviewable in a PR and searchable later; the ADRs double as interview material.
- Negative: a little writing per decision.

## Revisit when

Never; this is the process itself.
