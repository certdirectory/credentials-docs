# Revoking Credentials

Revoke a credential when it should no longer be considered valid — for example because the recipient violated a policy, a mistake was made at issue time, or the underlying achievement was reversed.

## What revocation does

- Sets `revokedAt` on the credential and stores your **reason**.
- Updates the credential's status to **Revoked**, surfaced everywhere it appears: the verification page, the public profile, the dashboard, and the OB3 JSON.
- Triggers verification logs to record subsequent verification attempts as failed.

The credential **remains accessible** at its verification URL — that's the whole point of a tamper-evident system: you can always see that something *was* a credential, but the platform clearly marks it as no longer valid.

## Who can revoke

**Owners** and **Admins** of the issuing organization. Issuers and Viewers cannot revoke.

## How to revoke

1. Open **Credentials** (`/organizations/{orgId}/credentials`).
2. Find the credential row. (Use search and filters to narrow down.)
3. Click **Revoke** at the end of the row.
4. The confirmation dialog asks for a **reason** (required, non-empty). Examples:
    - `Recipient withdrew from program.`
    - `Issued in error — wrong recipient email.`
    - `Re-certification failed.`
5. Click **Confirm Revoke**.

The reason is stored on the credential and may be shown publicly on the verification page.

## What recipients & verifiers see

On the **public verification page** (`/verify/{credential-id}`):

- The status badge changes to **Revoked**.
- A red banner appears with "Revoked on {date}" and the reason (if provided).
- The detailed verification checks show **Not Revoked** as failed (red).
- Sharing actions (LinkedIn, embed) are still possible but display the revoked status prominently.

On the **public profile** (`/u/{slug}`) and the **dashboard**, revoked credentials show a small `Revoked <date>` tagline under the credential card and use red accents for the date label.

## Revocation is intentional and durable

There's currently no UI to **un-revoke** a credential. If you revoke by mistake, you'll need to **issue a new credential** to the same recipient — the platform treats that as a renewal that supersedes the revoked one (see [Renewing Credentials](renewing-credentials.md)).

## Email notifications

The recipient is **not** automatically notified by email when a credential is revoked. If you need to inform them, please send a separate communication. (A revocation-notification email is on the roadmap.)

## Bulk revocation

There's no built-in bulk-revoke UI. For programmatic bulk revocation, loop the API:

```
DELETE /api/organizations/{orgId}/credentials/{credentialId}
Content-Type: application/json
Authorization: Bearer <session JWT>

{ "reason": "Program ended early." }
```

(Note: revocation requires session JWT auth, not API key auth — currently a session-only operation.)

## After revocation

Revoked credentials:

- **Stay** in your credentials list for audit purposes (filter `status=revoked`).
- **Don't free up** monthly quota — the recipient was already counted that month.
- **Cannot be claimed** if they were unclaimed at the time of revocation.
- **Can be replaced** by issuing again, which creates a renewal-style supersession link.

## Next

- [Renewing Credentials](renewing-credentials.md)
- [Issuing Credentials](issuing-a-credential.md)
