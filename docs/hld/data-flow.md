# 🔁 Data Flow & Sequence Diagrams

**Path:** `docs/hld/data-flow.md`

## Purpose

Describe key end-to-end flows and show sequence diagrams for critical scenarios to highlight transactions, idempotency, and failure handling.

## 1) Checkout Flow (Mermaid sequence diagram)

```mermaid
sequenceDiagram
  participant U as User <Br>(Mobile/Browser)
  participant FE as Frontend
  participant API as API Server
  participant DB as Postgres
  participant MP as MockPayment
  participant Q as Queue
  participant W as Worker
  participant Email as EmailSvc

  U->>FE: Click "Checkout"
  FE->>API: POST /checkout { cart_id, payment_info }
  API->>DB: BEGIN TRANSACTION
  API->>DB: Verify stock & create order rows
  DB-->>API: OK
  API->>MP: POST /mockpay/pay (idempotency-key)
  MP-->>API: 200 (accepted)
  API->>DB: COMMIT
  API->>Q: enqueue(order-confirmation)
  MP-->>API: webhook /payment-callback { order_id, status }
  API->>DB: update payment status (idempotent)
  API->>Q: enqueue(payment-processed)
  Q->>W: process(order-confirmation)
  W->>Email: send(order confirmation)
```

---

## Notes

- **Transactions:** checkout uses DB transaction to atomically create order and decrement stock.
- **Idempotency:** external payment callbacks should be idempotent (use idempotency keys or check prior state).
- **Async:** email sending is offloaded to workers to keep API responsive.
