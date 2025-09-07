# 🏗️ Container Diagram

**Path:** `docs/hld/container-diagram.md`

## Purpose

Show deployable/runtime containers and how they communicate. This helps understand runtime boundaries and scaling responsibilities.

## Container Diagram (Mermaid)

```mermaid
graph TD
  LB[Load Balancer / CDN] --> App[App Servers (Monolith)]
  App --> DB[(Postgres - Primary)]
  App --> Cache[(Redis Cache)]
  App --> Queue[(Redis/RabbitMQ)]
  Queue --> Worker[Background Workers]
  Worker --> Email[Email Service]
  App --> S3[(Object Storage - S3)]
  App --> MockPay[Mock Payment Gateway]
```

---

## Container Narratives (C4-style) - short explanations & responsibilities

- **Load Balancer / CDN:** Entry point for HTTP requests. Terminates TLS, routes traffic to app instances, and serves cached static assets via CDN for product images and frontend assets.
- **App Servers (Monolith):** Node.js (or chosen runtime) monolith hosting API endpoints, server-side rendering (if applicable), and business logic. Responsibilities: auth, catalog, cart, checkout, order processing. Scale horizontally behind LB.
- **Postgres (Primary DB):** Single primary relational database for authoritative storage of users, products, carts, and orders. Responsible for transactional consistency (checkout workflows).
- **Redis Cache:** In-memory cache for sessions, frequently accessed product lists, and distributed locks (if needed). Also used for rate-limiting and short-lived data.
- **Queue (Redis/RabbitMQ):** Durable job queue for background tasks (email sending, webhook handling, report generation). Workers consume jobs from this queue.
- **Background Workers:** Process jobs from the queue; offload non-blocking work from request paths (emails, webhooks, image processing tasks).
- **S3 / Object Storage:** Stores product images and other static assets; served via CDN.
- **Mock Payment Gateway:** Development-only service to simulate payment authorizations and webhooks; used to test payment flows without a real payment provider.

---

## Example Endpoints & Ports (for clarity)

- `POST /api/v1/auth/login` (App Servers)
- `GET /api/v1/products` (App Servers)
- `POST /api/v1/cart/{cart_id}/checkout` (App Servers)
- `POST /webhooks/payments` (App Servers) - receives webhooks from payment gateway
- Common port: `443` (TLS), internal services communicate over private network ports (e.g., Postgres 5432, Redis 6379).

---
