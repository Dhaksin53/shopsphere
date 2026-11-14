# ADR-0012: Container Apps first, then AKS

- **Status:** Accepted (applies from Phase 5)
- **Date:** 2026-11-14
- **Sprint:** 5.2 (Container Apps), 5.7 (AKS)

## Context

Microservices need a container platform. Kubernetes is the industry standard but has a steep learning curve and a cost (an always-on cluster). Learning containers, service discovery and scaling at the same time as Kubernetes internals is a lot at once.

## Decision

Package every service with **multi-stage Dockerfiles**, run them locally with **docker-compose**, push images to **Azure Container Registry**, and deploy first to **Azure Container Apps** (serverless containers with built-in ingress, KEDA and Dapr). In Sprint 5.7, move to **AKS with Helm** to learn Kubernetes directly: Deployments, Services, Ingress, ConfigMaps, Secrets from Key Vault, probes, HPA.

## Alternatives considered

- **AKS only:** maximum learning, but also maximum early complexity and cost.
- **App Service for containers:** simple, but weak for many services talking to each other.
- **Container Apps only:** cheaper and simpler, but hides Kubernetes concepts that the AZ-204/AZ-400 path and real jobs expect.

## Consequences

- Positive: a gentle ramp; the same images run on both platforms; a direct comparison for a design note.
- Negative: two deployment setups to maintain for a while; AKS must be deleted after practice to control cost.

## Revisit when

After Sprint 5.7, decide which platform the Phase 8 production setup uses, based on the comparison.
