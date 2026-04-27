# Authentication

The `/api/v1/*` endpoints authenticate via **API keys**. API keys are scoped to a single organization — using a key acts as that organization for all operations.

!!! info "Plan requirement"
    API keys are available on **Professional** and **Enterprise** plans. Free-tier organizations cannot generate them.

## Generate an API key

1. Sign in as an **Owner** of your organization. (Only owners can manage API keys.)
2. Go to **API Keys** under the organization (`/organizations/{orgId}/api-keys`).
3. Click **Generate new key**.
4. Enter a **name** (for your reference only — e.g. `production`, `staging`, `analytics-pipeline`).
5. The full key is shown **once only**. Copy it now — once you close the dialog, only the prefix and last 4 characters remain visible.

The key looks like:

```
crd_live_AbCdEf1234567890abcdef1234567890
```

## Use the key

Include it in the `Authorization: Bearer` header:

```http
GET /api/v1/badges HTTP/1.1
Host: credentials.certdirectory.io
Authorization: Bearer crd_live_AbCdEf1234567890abcdef1234567890
Accept: application/json
```

Examples:

=== "cURL"

    ```bash
    curl https://credentials.certdirectory.io/api/v1/badges \
      -H "Authorization: Bearer crd_live_..."
    ```

=== "Node.js (fetch)"

    ```javascript
    const res = await fetch('https://credentials.certdirectory.io/api/v1/badges', {
      headers: {
        Authorization: `Bearer ${process.env.CERTDIRECTORY_API_KEY}`,
      },
    });
    const data = await res.json();
    ```

=== "Python (requests)"

    ```python
    import os, requests
    r = requests.get(
        "https://credentials.certdirectory.io/api/v1/badges",
        headers={"Authorization": f"Bearer {os.environ['CERTDIRECTORY_API_KEY']}"},
    )
    r.raise_for_status()
    print(r.json())
    ```

## Rate limit

Each API key has its own rate limit (default **60 requests / minute**). The platform returns the following response headers on every API-key request:

| Header | Meaning |
|---|---|
| `X-RateLimit-Limit` | Your per-minute quota for this key. |
| `X-RateLimit-Remaining` | Requests left in the current window. |
| `X-RateLimit-Reset` | Unix timestamp when the window resets. |

See [Rate Limits](rate-limits.md) for backoff guidance.

## Revoke a key

If a key is compromised — committed to a public repo, leaked in logs — revoke it immediately:

1. Go to **API Keys**.
2. Find the key by its prefix.
3. Click **Revoke**. Subsequent requests return `401`.

## Number of keys per organization

| Plan | Active API keys |
|---|---|
| **Free** | 0 |
| **Professional** | 1 |
| **Enterprise** | Unlimited |

If you hit the cap, revoke an unused key first or upgrade.

## Security best practices

- **Never commit keys** to source control. Use environment variables or a secrets manager.
- **Rotate periodically** — revoke and regenerate at least once a year, or whenever a team member with access leaves.
- **Use distinct keys per environment** (production, staging, CI). Easier to revoke individually.
- **Restrict deployment access** — anyone who can read the key can act as your organization.
- **Monitor usage** — watch for unexpected spikes in the API Keys page.

## Session JWTs (for the web app)

The web app authenticates via short-lived **session JWTs** issued at sign-in. These aren't intended for API integrations and aren't documented here, but if you're calling the same endpoints the UI uses (e.g. `/api/auth/me`, the management endpoints under `/api/organizations/...`), you'll need a session JWT.

API consumers should always use API keys via the `/api/v1/*` endpoints.

## Next

- [Badges API](badges.md)
- [Credentials API](credentials.md)
- [Rate Limits](rate-limits.md)
