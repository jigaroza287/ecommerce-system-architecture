# 🚀 Scalability & Deployment

**Path:** `docs/hld/scalability-and-deployment.md`

## Deployment model (MVP)

- Single-region, multiple app instances (monolith) behind a load balancer.
- DB: single primary Postgres (backups enabled), optional read replica for heavy reads.
- Object storage: S3 + CDN for images.

## Scaling roadmap

1. **Vertical scaling** of app instances and DB for initial traffic spikes.
2. **Horizontal scaling** of app instances behind LB.
3. **Read replicas** for Postgres for read-heavy queries (catalog pages).
4. **Extract services** using the strangler pattern when a module needs independent scaling (e.g., search or recommendations).
5. **Event-driven** async patterns and CQRS for heavy write/read separation if needed.

## Deployment practices

- Use CI/CD (GitHub Actions) to run tests, build artifacts, and deploy.
- Prefer rolling or blue/green deploys to minimize downtime.
