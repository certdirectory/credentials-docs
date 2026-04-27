# API Overview

The CertDirectory Credentials REST API lets you programmatically:

- Manage badge templates.
- Issue and revoke credentials.
- List and query credentials.
- Verify credentials publicly.

!!! info "Plan requirement"
    Programmatic API access (the `/api/v1/*` endpoints) is available on the **Professional** and **Enterprise** plans. Free-tier organizations can still use the public verification endpoints.

## Base URL

```
https://credentials.certdirectory.io
```

All authenticated API endpoints are under the `/api/v1` prefix. Public verification endpoints are under `/api`. Public OB3 discovery endpoints (issuer profiles, achievements, credentials JSON-LD) are at the root.

## Authentication

Two mechanisms exist:

| Method | Used for | Example header |
|---|---|---|
| **API key** | All `/api/v1/*` endpoints (programmatic). | `Authorization: Bearer crd_live_...` |
| **Session JWT** | The web app's own UI calls (issued at sign-in). | `Authorization: Bearer <jwt>` |

For automation, use **API keys**. See [Authentication](authentication.md).

## Content type

All requests and responses use `application/json` unless otherwise noted (image uploads use `multipart/form-data`). Always include:

```
Content-Type: application/json
Accept: application/json
```

## Rate limits

- **Global IP limit:** 100 requests/minute.
- **Per API key:** default 60 requests/minute (configurable per key).
- **Auth endpoints (e.g. resend verification):** 3 requests / 15 minutes.

Responses include the headers `X-RateLimit-Limit`, `X-RateLimit-Remaining`, and `X-RateLimit-Reset`. See [Rate Limits](rate-limits.md).

## Errors

The API returns standard HTTP status codes. Error responses always have JSON in this shape:

```json
{
  "error": "VALIDATION_ERROR",
  "message": "Human-readable explanation.",
  "details": { "field": "..." }
}
```

Common codes you'll see:

| Status | Code | Meaning |
|---|---|---|
| `400` | `VALIDATION_ERROR` | Invalid request body or query parameters. |
| `401` | `UNAUTHORIZED` | Missing or invalid auth header. |
| `403` | `FORBIDDEN` | Authenticated but lacking permission, or feature not on your plan. |
| `404` | `NOT_FOUND` | Resource doesn't exist or isn't visible to you. |
| `409` | `CONFLICT` | E.g. recipient already has this credential. |
| `429` | `RATE_LIMITED` | Throttled. Back off using `X-RateLimit-Reset`. |
| `500` | `INTERNAL_ERROR` | Server-side error. Retry with backoff. |

## Endpoint catalogue

| Resource | Endpoints |
|---|---|
| **[Authentication](authentication.md)** | API key management |
| **[Badges](badges.md)** | List & retrieve badge templates |
| **[Credentials](credentials.md)** | List, retrieve, issue credentials |
| **Verification (public)** | `GET /api/verify/{credential-id}` |
| **OB3 (public, JSON-LD)** | `/issuers/...`, `/achievements/...`, `/credentials/.../ob3` |
| **[Webhooks](webhooks.md)** | Inbound Stripe; outbound on roadmap |

## SDKs

There are no first-party SDKs yet. The API is straightforward enough to use directly with `fetch`, `axios`, `requests`, etc. Example wrappers may follow.

## Quick example

Issue a credential to a new recipient:

```bash
curl -X POST https://credentials.certdirectory.io/api/v1/credentials \
  -H "Authorization: Bearer crd_live_YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "badgeId": "550e8400-e29b-41d4-a716-446655440000",
    "recipient": {
      "name": "Jane Doe",
      "email": "jane@example.com"
    }
  }'
```

Response:

```json
{
  "id": "...",
  "credentialId": "CRD-ZBUYQMTA",
  "verificationUrl": "https://credentials.certdirectory.io/verify/CRD-ZBUYQMTA",
  "status": "pending",
  "...": "..."
}
```

## Next

- [Authentication](authentication.md)
- [Credentials API](credentials.md)
- [Rate Limits](rate-limits.md)
