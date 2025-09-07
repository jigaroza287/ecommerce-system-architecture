# 🔗 Integration Points

**Path:** `docs/hld/integration-points.md`

## Overview

List of external systems and the integration contract / expectations.

## Email Service (SMTP / Dev Mailer)

- Purpose: send order confirmations and notifications.
- Contract: SMTP or provider API (SendGrid / SES). Send JSON payload to worker which calls provider.
- Retries: worker should retry with exponential backoff; after N attempts move to dead-letter queue (DLQ).

## Object Storage (S3)

- Purpose: store product images and assets.
- Contract: app uploads to pre-signed URLs; images served via CDN (CloudFront).

## Mock Payment Gateway (Development)

- Purpose: simulate payment authorization and webhook callbacks.
- Contract: POST /mockpay/pay returns `{"status":"accepted","payment_id":"..."}` and later issues webhook to `/webhooks/payments`.
- Webhook: include `order_id`, `status`, and `signature` header (HMAC) for verifying authenticity.

## Future integrations (deferred)

- Real Payment Gateway (Stripe/Adyen) — handle PCI scope, 3DS.
- SMS & Push providers.
- Search service (Elasticsearch) for advanced search.
