# 🎯 MVP Features — E-commerce System (Learning-first)

**Path:** `docs/requirements/mvp-features.md`

## 1. Purpose

This document captures the **MVP feature set** for the E-commerce case study, the rationale (business + learning), assumptions, and the next-step wishlist. The goal is to deliver a minimal end-to-end purchasing flow while maximizing learning across system design topics (auth, data modeling, transactions, background jobs, observability).

---

## 2. MVP Scope (Final)

Included in MVP:

1. **User authentication**
   - Register / Login / Logout
   - Profile (name, email, shipping address)
   - JWT-based auth + refresh token pattern (learning exercise)
2. **Product catalog**
   - Categories, listings, product details, images, price, stock
   - Pagination for product lists
3. **Basic Search & Filter**
   - DB-backed text search (Postgres `tsvector` or equivalent)
   - Filters: category, price range, availability
4. **Shopping Cart**
   - Add / Remove / Update items
   - Persist cart per user (support guest cart with merge on login)
5. **Checkout & Orders**
   - Place an order (create order + order_items)
   - Order statuses: `placed` → `processing` → `shipped` → `delivered`
   - Order history for users
6. **Mock Payment Gateway**
   - Simulated payment flow with webhook-style callback (success/fail)
7. **Admin minimal endpoints**
   - CRUD for products, view orders
   - Simple role-based access control (admin flag)
8. **Basic Email Notification**
   - Order confirmation email via background job (dev mailer or SMTP mock)

> **Rationale:** These features implement the core user journey (browse → cart → checkout → order) and give hands-on opportunities for key architecture topics: data consistency, transactions, background processing, and integration patterns.

---

## 3. Must-not include in MVP (deferred)

- Real payment gateway / PCI compliance
- SMS / Push notifications (email only for MVP)
- Chatbot / Advanced AI features
- Full multi-currency or multi-region deployment
- Full-text search service (Elasticsearch) — optional Phase 2
- Offline-first local sync (mobile) — advanced

---

## 4. Non-functional requirements (MVP)

- **Availability:** single-region, aim for 99.5% during development/testing
- **Performance:** API p95 < 300ms for core endpoints; page load < 2s for product pages
- **Security:** hashed passwords (bcrypt/argon2), HTTPS, JWT expiry + refresh tokens
- **Consistency:** strong consistency for inventory and orders (single RDBMS)
- **Scalability:** modular monolith with clear domain boundaries to enable future split

---

## 5. Key assumptions

- Start with a single relational database (Postgres recommended).
- Inventory managed by a simple stock decrement inside a DB transaction during checkout.
- Payment is simulated; real payments will be added later.
- Email provider can be swapped — start with a dev SMTP or mock service.

---

## 6. Learning checkpoints (per feature)

- **Auth:** password hashing, JWT, refresh tokens, session vs stateless tradeoffs
- **Catalog:** indexing, pagination, dealing with images (S3)
- **Search:** DB full-text vs external search engines
- **Cart:** persistence & guest cart merge patterns
- **Checkout:** transactions, idempotency keys, concurrency control to avoid oversell
- **Background jobs:** queue semantics, retries, dead-letter queues
- **Observability:** request IDs, structured logging, basic metrics

---

## 7. Next steps (short-term roadmap)

1. Create DB schema migrations for MVP entities.
2. Implement Auth + Product endpoints (CRUD + list).
3. Implement Cart + Checkout (transactional checkout flow).
4. Implement Mock Payment and webhook handler.
5. Add background worker to send order confirmation emails.
6. Add simple Admin endpoints and RBAC.
7. Add API tests for critical flows (signup, checkout).

---

## 8. Appendix: Quick checklist (developer view)

- [ ] DB migrations: users, products, categories, carts, cart_items, orders, order_items
- [ ] Auth middleware + JWT
- [ ] Product endpoints (list, detail, admin CRUD)
- [ ] Cart endpoints + guest-to-user merge
- [ ] Checkout endpoint with transaction + idempotency key
- [ ] Mock payment service + webhook handler
- [ ] Background queue + email worker
- [ ] Basic E2E tests for the purchase flow
