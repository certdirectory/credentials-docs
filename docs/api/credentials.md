# Credentials API

Issue, list, and retrieve credentials.

All endpoints require an [API key](authentication.md) and operate within the organization the key belongs to.

## Issue a credential

```http
POST /api/v1/credentials
```

### Request body

```json
{
  "badgeId": "550e8400-e29b-41d4-a716-446655440000",
  "recipient": {
    "name": "Jane Doe",
    "email": "jane@example.com"
  }
}
```

| Field | Type | Required | Notes |
|---|---|:-:|---|
| `badgeId` | UUID | ✅ | The badge template UUID (from the [Badges API](badges.md)). |
| `recipient.name` | string | ✅ | Display name on the credential. |
| `recipient.email` | string | ✅ | Recipient's email. The credential is sent here and matched at claim time. |

### Response

```json
{
  "id": "f1d2c3b4-...",
  "credentialId": "CRD-ZBUYQMTA",
  "verificationUrl": "https://credentials.certdirectory.io/verify/CRD-ZBUYQMTA",
  "status": "pending",
  "issuanceDate": "2026-01-15T10:00:00.000Z",
  "expirationDate": "2028-01-15T10:00:00.000Z",
  "recipientName": "Jane Doe",
  "recipientEmail": "jane@example.com",
  "supersedesCredentialId": null
}
```

### Side effects

- An email is sent to the recipient with the verification link, **unless** your organization has email notifications disabled.
- If the recipient already has a credential of this badge:
    - **Expired** → the new credential supersedes the expired one (`supersedesCredentialId` is set).
    - **Active, time-bound** → the existing credential is revoked, and the new one supersedes it.
    - **Revoked** → the new credential supersedes the revoked one.
    - **Active, non-expiring** → returns `409 CONFLICT`. Revoke the existing credential first.

### Errors

| Status | Code | Reason |
|---|---|---|
| `400` | `VALIDATION_ERROR` | Missing or malformed fields. |
| `403` | `FORBIDDEN` | Plan doesn't allow API access, or org isn't approved, or signing keys not generated. |
| `404` | `NOT_FOUND` | `badgeId` not found or not in your organization. |
| `409` | `CONFLICT` | Recipient already has an active, non-expiring credential. |
| `429` | `RATE_LIMITED` | Per-key or quota limit hit. |

## List credentials

```http
GET /api/v1/credentials
```

### Query parameters

| Param | Type | Default | Notes |
|---|---|---|---|
| `limit` | integer | 50 | Page size. Max 100. |
| `offset` | integer | 0 | Pagination. |
| `badgeId` | UUID | – | Filter by badge. |
| `status` | string | – | Filter by `valid`, `pending`, `expired`, or `revoked`. |

### Response

```json
{
  "data": [
    {
      "id": "...",
      "credentialId": "CRD-ZBUYQMTA",
      "badgeId": "...",
      "badgeName": "AWS Fundamentals",
      "recipientName": "Jane Doe",
      "recipientEmail": "jane@example.com",
      "status": "valid",
      "issuanceDate": "2026-01-15T10:00:00.000Z",
      "expirationDate": "2028-01-15T10:00:00.000Z",
      "claimedAt": "2026-01-15T11:30:00.000Z",
      "revokedAt": null,
      "isPublic": true,
      "verificationUrl": "https://credentials.certdirectory.io/verify/CRD-ZBUYQMTA",
      "supersedesCredentialId": null,
      "supersededByCredentialId": null
    }
  ],
  "pagination": {
    "limit": 50,
    "offset": 0,
    "total": 247
  }
}
```

## Get a credential

```http
GET /api/v1/credentials/{credentialId}
```

`credentialId` is the credential's internal UUID (the value of `id`, not the `CRD-...` short ID).

### Response

Same shape as a single item from the list endpoint, plus the full OB3 JSON-LD payload under `ob3Credential`.

## Revoke a credential

Revocation via the v1 API is not yet available. To revoke programmatically today, use the session-authenticated endpoint:

```http
DELETE /api/organizations/{orgId}/credentials/{credentialId}
Authorization: Bearer <session JWT>
Content-Type: application/json

{ "reason": "Recipient withdrew from program." }
```

A v1 endpoint for revocation is on the roadmap. See [Revoking Credentials](../organizations/revoking-credentials.md) for the in-app flow.

## Verify a credential (public)

No authentication required:

```http
GET /api/verify/{credential-id}
```

`credential-id` here is either the short ID (`CRD-…`) or the UUID. See [Verifying a Credential](../verification/verifying-a-credential.md) for the response shape.

## Get the OB3 JSON-LD (public)

```http
GET /credentials/{credential-id}/ob3
```

Returns the signed Open Badges 3.0 JSON-LD payload. Returns `403` if the credential is private and the request is unauthenticated.

## Get the baked PNG (public)

```http
GET /api/credentials/{credential-id}/baked
```

Returns the badge image with the OB3 credential embedded as PNG metadata. Useful for portable, file-based credentials.

## Quota

Each unique recipient email per organization per calendar month counts once toward your monthly quota:

| Plan | Recipients/month |
|---|---|
| **Free** | 1,000 |
| **Professional** | 1,000 |
| **Enterprise** | 5,000 |

Exceeding the quota returns `403 FORBIDDEN` with a message indicating the limit. The counter resets on the 1st of each month.

## Next

- [Authentication](authentication.md)
- [Rate Limits](rate-limits.md)
- [Webhooks](webhooks.md)
