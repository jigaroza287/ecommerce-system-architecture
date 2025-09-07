# 🔹 Component Diagram

**Path:** `docs/hld/component-diagram.md`

## Purpose

Zoom into the Monolith container and show major components / modules and their responsibilities.

## Component Diagram (Mermaid)

```mermaid
graph LR
  subgraph Monolith
    Auth["Auth Module"]
    Catalog["Catalog Module"]
    Cart["Cart Module"]
    Orders["Orders Module"]
    Admin["Admin Module"]
    Jobs["Jobs / Worker Module"]
  end
  Auth --> DB[(Postgres)]
  Catalog --> DB
  Cart --> DB
  Orders --> DB
  Jobs --> DB
  Catalog --> S3[(S3)]
  Orders --> MockPay[Mock Payment]
```

---

## Component responsibilities (brief)

- **Auth Module:** registration, login, JWT, refresh tokens, user profile.
- **Catalog Module:** product CRUD, categories, search index hooks, image metadata.
- **Cart Module:** cart lifecycle, guest merge, cart persistence.
- **Orders Module:** checkout transaction, order lifecycle, inventory decrement, idempotency handling.
- **Admin Module:** product management, order management, basic RBAC.
- **Jobs Module:** background workers for emails, webhook processing, and async tasks.
