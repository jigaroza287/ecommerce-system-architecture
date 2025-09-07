# ⚠️ Risks & Mitigations

**Path:** `docs/hld/risks-and-mitigations.md`

- **Single DB = single point of failure**: mitigate with automated backups, snapshots, and RDS multi-AZ for production.
- **Overselling inventory during checkout**: mitigate with DB transactions and/or optimistic locking (version column) and retry logic.
- **Tight coupling in monolith**: enforce module boundaries, code reviews, and package-level separation; write ADRs when extracting services.
- **Webhook security & retries**: verify signatures, idempotency, and implement retry/backoff for processing webhooks.
- **Email delivery problems**: use provider with high deliverability; implement retries & DLQ for failed sends.
