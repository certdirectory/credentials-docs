# Webhooks

Outbound webhooks let your systems react in real time to credential events — e.g. log issuances to a data warehouse, trigger downstream onboarding, or notify recipients via your own channels.

!!! info "Status: roadmap"
    Outbound webhooks to customer-controlled URLs are flagged as an **Enterprise** feature on the [billing page](../organizations/billing.md). The endpoint and event surface are **not yet generally available**. If you're an Enterprise customer who needs webhooks today, contact us — we can set you up with a custom integration.

This page documents the planned model so you can design integrations against it.

## Planned events

The following events are intended to fire to your configured webhook URL(s):

| Event | When |
|---|---|
| `credential.issued` | A credential has just been issued (single or bulk). |
| `credential.claimed` | The recipient has claimed the credential into their account. |
| `credential.revoked` | A credential has been revoked. |
| `credential.expired` | A credential has crossed its `expirationDate`. |
| `credential.renewed` | A new credential has been issued that supersedes a prior one. |
| `bulk_issuance.completed` | A bulk-issuance job has finished. |

## Planned payload

A standard webhook payload shape is:

```json
{
  "id": "evt_...",
  "type": "credential.issued",
  "createdAt": "2026-01-15T10:00:00.000Z",
  "data": {
    "credentialId": "CRD-ZBUYQMTA",
    "credentialUuid": "...",
    "badgeId": "...",
    "recipientEmail": "jane@example.com",
    "...": "..."
  }
}
```

Headers:

- `X-CertDirectory-Signature` — HMAC-SHA256 of the body, signed with your webhook secret.
- `X-CertDirectory-Event` — duplicates the `type` field for easy routing.

## Inbound webhooks (Stripe → CertDirectory)

The platform exposes one inbound webhook today, used by Stripe for billing events:

```http
POST /api/webhooks/stripe
```

This endpoint:

- Validates the `Stripe-Signature` header against the platform's Stripe webhook secret.
- Processes events: `checkout.session.completed`, `customer.subscription.created/updated/deleted`, `invoice.paid`, `invoice.payment_failed`.
- Updates the affected organization's subscription state.

You don't call this endpoint directly — it's there for Stripe to call when subscriptions change.

## Today's alternatives

Until outbound webhooks are GA, you can poll the API:

- **List recently issued credentials** — `GET /api/v1/credentials?limit=100` and filter by `issuanceDate`.
- **Filter by status** — `GET /api/v1/credentials?status=revoked` to find revoked credentials.

Combined with a simple cron and a watermark on `createdAt`, this works well for low-frequency syncs.

## Next

- [Credentials API](credentials.md)
- [Rate Limits](rate-limits.md)
