# Issuing Credentials

Issue a single credential to a recipient by email.

## Prerequisites

- Your organization is **approved**.
- Your organization has **signing keys** generated (otherwise you'll see a warning with a link to fix it).
- You have at least the **Issuer** role in the organization.
- You're under your monthly quota (Free & Professional: 1,000 unique recipients/month; Enterprise: 5,000).

## Issue flow

1. Go to **Credentials → Issue credential** (`/organizations/{orgId}/credentials/issue`). Or, from a badge detail page, click **Issue this credential** to pre-fill the badge.
2. Select the **badge** (or it's pre-selected if you came from a badge page).
3. Enter the recipient's **name** and **email**.
4. *(Optional)* Add an **evidence URL** — a link to proof of the achievement (a project, a transcript, a test result).
5. Choose **public** (default) or private. Public credentials can be verified by anyone with the link; private credentials are only visible to the recipient when signed in. See [public/private](#public-vs-private).
6. The form runs a **pre-issue check** as you type the email. See [Recipient check states](#recipient-check-states) below.
7. Click **Issue credential**.

The recipient receives an email with a **View & Verify** button. The credential is immediately public (if marked so) and verifiable.

## Recipient check states

As soon as you enter a valid email, the form queries the API for an existing credential of the same badge for that recipient. The result determines the banner colour and what happens on submit:

| State | Banner | Behaviour on submit |
|---|---|---|
| **`new`** — no prior credential | <span style="color:#16a34a">Green</span>: "First-time recipient." | Issues a fresh credential. |
| **`expired`** — prior credential exists and has expired | <span style="color:#2563eb">Blue</span>: "Renewing an expired credential." | Issues a new credential that **supersedes** the expired one (linked via `supersedesCredentialId`). See [Renewing Credentials](renewing-credentials.md). |
| **`active_time_bound`** — prior credential is still valid and time-limited | <span style="color:#d97706">Amber</span>: "An active credential exists. It will be revoked and a new one issued." | Revokes the existing credential, then issues a fresh one. Linked as a renewal. |
| **`active_non_expiring`** — prior credential is still valid and *never expires* | <span style="color:#dc2626">Red</span>: "Recipient already has this credential." | **Submit blocked.** Issuing again would create duplicate non-expiring credentials. Revoke the existing one first if you really need to re-issue. |
| **`revoked`** — most recent credential was revoked | <span style="color:#2563eb">Blue</span>: "Issuing replaces a revoked credential." | Issues a new credential, linking the revoked one as `supersedesCredentialId`. |

This logic prevents accidental duplicates and clearly signals what's about to happen.

## Public vs private

The **Make this credential publicly verifiable** checkbox (default checked) controls visibility:

- **Public** — anyone with the verification URL can view the credential. Embed code, LinkedIn share, and public profile inclusion all require this.
- **Private** — the verification page returns a "Private credential" notice to logged-out users. Only the recipient (when signed in) can see full details. Public profile and embed are disabled.

This setting is fixed at issue time. To change it, you'd need to revoke and re-issue.

## Email notifications

The recipient receives an issuance email by default. You can disable email notifications globally for the organization on the org detail page (the toggle suppresses *all* org emails).

If a credential email fails or wasn't received, owners and admins can **resend** from the credential row in the credentials list. There's a 7-day cooldown to prevent abuse, unless the previous send failed.

## After issuance

You're returned to the **Credentials** list (`/organizations/{orgId}/credentials`), with a toast confirming the issue. From this list you can:

- Click any credential to view its public verification page.
- **Revoke** a credential (Owner/Admin only). See [Revoking Credentials](revoking-credentials.md).
- **Resend email** (subject to cooldown).
- See renewal lineage — credentials that supersede or are superseded by another show ↻ icons with a clickable link.

## API alternative

To automate issuance, use the v1 API:

```
POST https://credentials.certdirectory.io/api/v1/credentials
Authorization: Bearer crd_live_...
Content-Type: application/json

{
  "badgeId": "BADGE_UUID",
  "recipient": {
    "name": "Jane Doe",
    "email": "jane@example.com"
  }
}
```

See the [Credentials API](../api/credentials.md).

## Quota

Each unique **recipient email per organization per calendar month** counts as one. Re-issuing or revoking + re-issuing for the same email in the same month does **not** consume an additional slot. The counter resets at the start of each month.

## Next

- [Bulk Issuance](bulk-issuance.md)
- [Renewing Credentials](renewing-credentials.md)
- [Revoking Credentials](revoking-credentials.md)
