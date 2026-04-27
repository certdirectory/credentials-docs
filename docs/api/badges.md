# Badges API

Manage and query your organization's badge templates.

All endpoints require an [API key](authentication.md) and operate within the organization the key belongs to.

## List badges

```http
GET /api/v1/badges
```

### Query parameters

| Param | Type | Default | Notes |
|---|---|---|---|
| `limit` | integer | 50 | Page size. Max 100. |
| `offset` | integer | 0 | For pagination. |
| `active` | boolean | – | Filter by active/deactivated. |

### Response

```json
{
  "data": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "name": "AWS Fundamentals",
      "slug": "aws-fundamentals",
      "description": "Demonstrates a foundational understanding of AWS core services.",
      "criteria": "Pass the 60-question multiple choice exam with a score of 70% or higher.",
      "imageUrl": "https://storage.googleapis.com/.../aws-fundamentals.png",
      "expiresInDays": 730,
      "tags": ["aws", "cloud", "fundamentals"],
      "isActive": true,
      "createdAt": "2026-01-15T10:00:00.000Z",
      "publicUrl": "https://credentials.certdirectory.io/o/acme/badges/aws-fundamentals",
      "achievementUrl": "https://credentials.certdirectory.io/achievements/acme/aws-fundamentals"
    }
  ],
  "pagination": {
    "limit": 50,
    "offset": 0,
    "total": 12
  }
}
```

## Get a badge

```http
GET /api/v1/badges/{badgeId}
```

`badgeId` is the badge UUID.

### Response

Same shape as a single item from the list endpoint.

### Errors

- `404 NOT_FOUND` if the badge doesn't exist or doesn't belong to your organization.

## Create / update / delete

Badge **mutation** is currently exposed via the web app and the session-authenticated `/api/organizations/{orgId}/badges` routes. Direct mutation through `/api/v1/badges` is on the roadmap.

If you need to create badges programmatically today, you can:

1. Generate a session JWT by calling `/api/auth/login`.
2. Call `POST /api/organizations/{orgId}/badges` with that JWT.

(See the integration testing examples in the source repo for working samples.)

## Field reference

| Field | Type | Description |
|---|---|---|
| `id` | UUID | Internal identifier; use this when issuing credentials. |
| `name` | string | Display name. |
| `slug` | string | URL-safe identifier. Part of `publicUrl` and `achievementUrl`. |
| `description` | string | What this badge represents. Plain text. |
| `criteria` | string | What the recipient did to earn it. |
| `imageUrl` | URL | Badge image (CDN-hosted PNG/JPEG/WebP/SVG). |
| `expiresInDays` | integer \| null | If set, credentials expire this many days after issuance. |
| `tags` | string[] | Skills associated with the badge. |
| `isActive` | boolean | If `false`, no new credentials can be issued from this badge. |
| `publicUrl` | URL | Public marketing-style page. |
| `achievementUrl` | URL | OB3 JSON-LD Achievement document. |

## Next

- [Credentials API](credentials.md) — issue credentials from a badge.
- [Authentication](authentication.md)
