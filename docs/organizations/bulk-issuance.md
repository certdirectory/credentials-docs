# Bulk Issuance

Issue many credentials at once by uploading a CSV. Bulk issuance runs as a background job and supports thousands of recipients per file.

!!! info "Plan requirement"
    Bulk issuance is available on **Professional** and **Enterprise** plans. Free-tier organizations can validate a CSV but cannot submit it for issuance.

## Prerequisites

- Your organization is **approved** with **signing keys** generated.
- You're on **Professional** or **Enterprise**.
- You have at least the **Issuer** role.
- You have headroom in your monthly recipient quota.

## CSV format

A simple two-column file:

```csv
name,email
Jane Doe,jane@example.com
Sam Smith,sam@example.com
Priya Patel,priya@example.com
```

- Header row is required.
- Columns: **`name`** and **`email`** (case-insensitive).
- Encoding: UTF-8.
- File limit: 1 MB, `.csv` extension only.

A downloadable template is available on the bulk issue page.

## Step-by-step

1. Go to **Credentials → Bulk issue** (`/organizations/{orgId}/credentials/bulk-issue`).
2. Pick the **badge** to issue.
3. Drop in or select your CSV file.
4. Click **Validate** — the platform parses the CSV and runs the same recipient check as single-issuance against every row.

### Review screen

You'll see a summary table:

| Status | Meaning | Issuable? |
|---|---|---|
| **New** | First-time recipient. | Yes |
| **Expired** (Renewal) | Recipient had an expired credential of this badge. | Yes — renews via supersession. |
| **Active (time-bound)** | Recipient has an active, time-limited credential. | Yes — existing one will be revoked, new one issued. |
| **Active (non-expiring)** | Recipient has an active, non-expiring credential. | **Skipped** (would create a duplicate). |
| **Revoked** | Recipient had a revoked credential. | Yes — issues a fresh credential. |
| **Invalid** | Missing fields, malformed email, or duplicate row in the CSV. | No. |

The table shows totals at the top: how many rows are valid, how many are renewals, how many are skipped, how many are invalid.

5. Review the table. If everything looks right, click **Submit**. Only **valid** rows (everything except `Invalid` and `Active (non-expiring)`) are sent for processing.

### Progress screen

The platform creates a **Bulk issuance job** with status `pending`, then transitions to `processing` as workers send credentials. You'll see live counts:

- Total recipients
- Successful
- Failed (with reasons)
- Skipped

When the job reaches `completed`, every successful recipient has received an email and a verifiable credential.

## After the job runs

- Each successful credential appears in the **Credentials list**.
- The job itself stays in your **Bulk jobs history** with a status, counts, and any error messages.
- Failed rows can be retried by re-uploading just the failed addresses.

## Quota interaction

Each **unique recipient email per month** counts once toward your monthly quota — bulk issuance is no different from single issuance in this regard. If a CSV would push you over your monthly quota, the over-quota recipients are rejected at validation time.

## Common pitfalls

- **CSV opened in Excel and re-saved** — Excel sometimes adds BOM characters or changes encoding. Save as **CSV UTF-8** explicitly.
- **Duplicate rows** — the same email twice in one CSV is flagged as invalid (only one will be issued).
- **Whitespace in emails** — leading/trailing spaces are trimmed automatically.
- **Submitting when no rows are valid** — the **Submit** button stays disabled until at least one row is issuable.

## API alternative

Programmatic bulk issuance can be implemented as a loop of single-issue calls against the [Credentials API](../api/credentials.md), respecting the per-API-key rate limit. A first-class bulk endpoint is on the roadmap.

## Next

- [Issuing Credentials](issuing-a-credential.md)
- [Renewing Credentials](renewing-credentials.md)
