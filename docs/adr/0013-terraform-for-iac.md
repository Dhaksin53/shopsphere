# ADR-0013: Terraform for infrastructure as code

- **Status:** Accepted
- **Date:** 2026-11-14
- **Sprint:** 8.1 (full coverage); resources created by hand earlier are imported

## Context

By Phase 8, ShopSphere uses dozens of Azure resources across `dev`, `staging` and `prod`. Creating them by clicking in the portal is not repeatable, reviewable or recoverable. The roadmap left "Terraform or Bicep" open.

## Decision

Use **Terraform** with the `azurerm` provider:

- Reusable **modules** (network, data, app platform, observability).
- **Remote state** in an Azure Storage account, with state locking.
- One configuration per environment (`dev` / `staging` / `prod`).
- `terraform plan` runs on every PR; `apply` runs from CI after approval, authenticated with OIDC.
- Goal: destroy and recreate an environment with one command.

## Alternatives considered

- **Bicep:** Azure-native, no state file to manage, new Azure features available on day one, simpler syntax. But it only works on Azure, and it hides the concept of state, which is central to most IaC tools.
- **Pulumi:** real programming languages (TypeScript), but a smaller community and job market than Terraform.
- **Portal / Azure CLI scripts:** not declarative, so drift is invisible.

## Consequences

- Positive: the most widely used IaC tool across clouds (a transferable skill); plans show exactly what will change; state teaches drift, import and locking.
- Negative: the state file must be protected (it can contain secrets) and locked; brand-new Azure features may lag in the provider (the `azapi` provider fills the gaps).

## Revisit when

A needed Azure feature stays unavailable in both `azurerm` and `azapi` for a long time.
