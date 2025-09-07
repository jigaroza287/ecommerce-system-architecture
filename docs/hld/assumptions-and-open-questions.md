# 🤔 Assumptions & Open Questions

**Path:** `docs/hld/assumptions-and-open-questions.md`

## Key assumptions

- MVP uses single Postgres DB for strong consistency.
- Payment processing is mocked; no PCI compliance required initially.
- Notifications limited to email for MVP.

## Open questions to revisit

- When should we introduce real payment gateway and begin PCI work?
- What traffic threshold (orders/sec) will force us to split services?
- Do we need multi-region deployment for latency or compliance?

## Actions

- Add monitoring thresholds and alerts to inform decision points for scaling/migration.
