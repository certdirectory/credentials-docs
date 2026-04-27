# Renewing Credentials

Re-issue a credential to the same recipient for the same badge. The new credential **supersedes** the original, preserving the full history while making the latest one canonical.

## When to renew

- A credential has **expired** and you want the recipient to have a current one.
- A time-bound credential is still valid but you're reissuing early (e.g. recipient passed re-certification).
- A credential was previously **revoked** and you want to issue a new one (e.g. after correcting an error).

## How renewal works

There's no separate "Renew" button — renewal is just **issuing again** to the same email and the same badge. The platform detects the prior credential and creates the renewal automatically.

When you go to the Issue page (or use Bulk Issue) and the recipient already has a credential of this badge, the [recipient check](issuing-a-credential.md#recipient-check-states) banner shows the appropriate state:

| Prior state | What renewal does |
|---|---|
| **Expired** | Issues a new credential. New credential's `supersedesCredentialId` points to the expired one; expired credential's `supersededByCredentialId` points to the new one. |
| **Active, time-bound** | **Revokes** the active credential (with a "superseded by renewal" reason), then issues the new one. Same supersession links. |
| **Revoked** | Issues a new credential. Supersession links record the lineage; the revoked credential remains revoked but visible. |
| **Active, non-expiring** | **Blocked.** Renewal would create a duplicate. Revoke the existing one manually first. |

## What recipients see

The renewal email is differentiated from a fresh issue:

- Subject: e.g. "Your *AWS Fundamentals* credential has been renewed 🔄" (vs the standard "🏆 You've earned a new credential!").
- Body explicitly mentions this is a renewal and that the original credential's status is now expired/revoked.

On the verification page, the new credential displays:

- A **"Supersedes"** callout with a link to the prior credential's verification page.

The prior credential displays:

- A **"Superseded by"** callout with a link to the new one.

## Linking metadata

In the OB3 JSON-LD payload, supersession is captured via:

- `supersedesCredentialId` — UUID of the prior credential.
- `supersededByCredentialId` — UUID of the renewal.

These appear in API responses (`GET /api/v1/credentials/:id`) and in the public OB3 endpoint (`/credentials/:id/ob3`).

## Bulk renewal

The same recipient-state detection runs in [Bulk Issuance](bulk-issuance.md). Upload a CSV and rows for recipients with prior expired/revoked/time-bound credentials are flagged accordingly. Submit, and the platform issues renewals in bulk.

## What renewal *doesn't* do

- It does **not** copy the original credential's content. Each new credential is issued from the *current* state of the badge template (so if you've updated the badge image or skills, those changes appear in the renewed credential).
- It does **not** unrevoke a revoked credential — the prior credential stays revoked, it's just linked.

## API alternative

To renew programmatically, just call the issue endpoint again with the same recipient email and badge. The platform handles the supersession automatically.

```
POST /api/v1/credentials
{
  "badgeId": "BADGE_UUID",
  "recipient": { "name": "Jane Doe", "email": "jane@example.com" }
}
```

If the recipient has an `active_non_expiring` credential, you'll get a `409 Conflict` and need to revoke first.

## Next

- [Issuing Credentials](issuing-a-credential.md)
- [Revoking Credentials](revoking-credentials.md)
