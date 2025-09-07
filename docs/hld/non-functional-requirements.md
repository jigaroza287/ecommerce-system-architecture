# ⚙️ Non-Functional Requirements (NFRs)

**Path:** `docs/hld/non-functional-requirements.md`

## Availability & Reliability

- Target: 99.5% availability for core APIs during MVP.
- Single-region deployment with backups and automated DB snapshots.

## Performance (SLOs)

| Metric                                          |                       Target |
| ----------------------------------------------- | ---------------------------: |
| API p95 latency (core endpoints)                |                     < 300 ms |
| Product list page load (initial render)         |                        < 2 s |
| Error rate (5xx)                                |                       < 0.5% |
| Time to recover (RTO) from a primary DB failure | < 30 minutes (with failover) |

## Security

- Passwords: store hashed with `bcrypt` or `argon2`.
- Transport: TLS for all external traffic.
- Auth: JWT with short expiry + refresh token rotation.
- Sensitive data: avoid storing full card details (mock payments only in MVP).

## Scalability

- Horizontal scaling for app servers behind LB.
- Read replicas for Postgres when read traffic grows.
- Cache frequently read data to reduce DB load.

## Observability

- Structured logs with request IDs.
- Basic metrics (request latency, error rates, queue lengths).
- Error tracking (Sentry or similar).

## Backup & DR

- Daily DB backups, point-in-time recovery enabled for Postgres on RDS.

## When to revisit NFRs / Triggers

- Re-evaluate when sustained orders/sec > 100 or when p95 latency exceeds target for > 24 hours.
- Any production incidents related to inventory consistency should trigger an immediate review of transaction strategy.
