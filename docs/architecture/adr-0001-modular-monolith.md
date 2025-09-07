---

# 3) `docs/architecture/adr-0001-modular-monolith.md` (ADR)

```markdown
# ADR 0001 — Start with a Modular Monolith

## Status

Accepted

## Context

We need a fast learning cycle and broad coverage of system design topics (auth, transactions, background processing, integration patterns). Operational overhead should be minimized initially so the team can focus on core architecture patterns and design trade-offs.

## Decision

Implement the system as a **modular monolith** (single deployable artifact) organized by well-defined domain modules:

- `auth`
- `catalog`
- `cart`
- `orders`
- `admin`
- `jobs`

Use a single primary relational database (Postgres). Use a job queue (Redis/RQ or RabbitMQ) for asynchronous work.

## Consequences

- **Pros:** faster iteration, simpler infra, unified transactions, easier local development and debugging.
- **Cons:** risk of tight coupling if modules are not well separated. Extraction to microservices will require additional refactoring.
- **Mitigations:** enforce module boundaries via package structure, use internal API interfaces, write ADRs for future service extraction, and consider using the strangler pattern when splitting services.

## When to revisit

- When the deployment velocity or team size increases beyond what a monolith supports.
- When service-specific scaling needs (e.g., search, recommendation) justify separate services.
```
