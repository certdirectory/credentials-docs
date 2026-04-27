# Troubleshooting

Common issues and how to resolve them.

## Sign-up & email verification

### "Verification email never arrived"

1. Check your spam/junk folder.
2. Wait 1–2 minutes — email delivery can be slightly delayed.
3. From the login page, click **Resend verification email** (allowed 3 times per 15 minutes).
4. Add `noreply@certdirectory.io` to your contacts.
5. Some corporate email systems block external senders. Try a personal email if it's available.

### "Email verification link expired"

Verification links are valid for a limited time. Sign in (you'll get a `EMAIL_NOT_VERIFIED` error), then use **Resend verification email** from the login screen — that sends a fresh link.

### "I get `EMAIL_NOT_VERIFIED` when signing in"

Click the verification link in your email first. If the email isn't there, see above.

---

## Issuing credentials

### "Signing keys not generated"

Open the organization detail page, find the **Signing keys** section, and click **Generate signing keys**. You must be the **Owner**. Keys can only be generated once.

### "Organization not approved"

You can fully set up your organization while in `pending_review`, but issuance is blocked. Wait for approval (typically one business day). If your organization was rejected, you'll get an email with the reason — update the org and resubmit.

### "Recipient already has this credential" (red banner, submit blocked)

The recipient has an **active, non-expiring** credential of the same badge. Issuing again would create a duplicate. To proceed:

1. Revoke the existing credential first ([Revoking Credentials](../organizations/revoking-credentials.md)).
2. Then issue the new one — it'll be linked as a renewal of the revoked credential.

Or, if you actually want to renew with a different cadence: change the badge to have `expiresInDays`, so future credentials are time-bound.

### "Quota exceeded"

You've reached your monthly recipient quota. Options:

- Wait until the 1st of the next month for the quota to reset.
- [Upgrade your plan](../organizations/billing.md) — Professional has the same Free quota; Enterprise has 5,000.
- Note that re-issuing to recipients you've already issued to *this month* doesn't count again — only new emails do.

---

## Bulk issuance

### "Bulk issuance is only available on paid plans"

Upgrade to [Professional or Enterprise](../organizations/billing.md). Free-tier organizations can validate a CSV but cannot submit it.

### "Invalid CSV"

Common causes:

- Missing or misnamed header row. The required headers are `name` and `email` (case-insensitive).
- Wrong file format. Must be `.csv` (no `.xlsx`, `.tsv`, etc.).
- File over 1 MB. Split into smaller batches.
- BOM character at the start of the file (some Excel exports). Re-save as UTF-8 from a plain text editor or `csv` library.

### "All my rows show as 'Active (non-expiring)'"

This means every recipient already has an active, non-expiring credential of this badge. Either:

- The badge has no `expiresInDays`. Set one if recurring issuance makes sense.
- These recipients genuinely already have the credential — bulk-issuing again is unnecessary.

### "Bulk job is stuck on `processing`"

Bulk jobs are processed by background workers. If a job stays `processing` for more than ~10 minutes for under 1,000 recipients, contact support with the job ID.

---

## Verification

### "Credential not found" (404)

- Double-check the URL — credential IDs look like `CRD-ZBUYQMTA` (8 alphanumeric chars after `CRD-`).
- The credential may have been deleted (rare; only happens if the issuing organization was removed entirely).

### "Private credential" (403)

The credential exists but is marked private. Only the signed-in recipient can view it. If you're the issuer, sign in with the issuing org and retrieve from your credentials list.

### "Signature invalid" / red signature check on verification page

This is rare and indicates the credential's signature doesn't match the issuer's published public key. Causes:

- Tampered credential data (someone modified the JSON after issuance).
- The issuer rotated keys (we don't currently support rotation in the UI).
- A bug — please report it.

### Verification page is slow to load

Common causes:

- Cold start after a period of low traffic — try again, it should be fast on subsequent loads.
- Rate limit hit — wait a minute and reload.

---

## Domain verification

### "TXT record not found"

- Wait 5–10 minutes after adding the record. DNS propagation can take time.
- Verify with a third-party DNS tool: `dig TXT _certdirectory.example.com` or [whatsmydns.net](https://www.whatsmydns.net/).
- Confirm the **value** matches the token shown in the UI exactly. Trailing whitespace or quotation marks added by the DNS provider can cause mismatches.

### "Domain verification feature unavailable"

You're on the Free plan. [Upgrade to Professional](../organizations/billing.md) to enable domain verification.

### "Website URL is required"

Set your organization's website on the org detail page first, then start verification.

---

## API

### "401 Unauthorized" on every request

- Check the `Authorization: Bearer <key>` header is set correctly.
- Confirm the key hasn't been revoked.
- If you regenerated the key, make sure your code uses the new value.

### "403 Forbidden" with "API access requires Professional plan"

The organization the API key belongs to is on Free. Upgrade to Professional or higher.

### "429 Too Many Requests"

- Check the `X-RateLimit-Reset` header for when the window resets.
- Implement exponential backoff (see [Rate Limits](../api/rate-limits.md)).
- For sustained high throughput, contact us about raising your per-key limit.

### "409 Conflict" when issuing

The recipient has an active, non-expiring credential of the same badge. Revoke it first, or issue from a different badge. See [Renewing Credentials](../organizations/renewing-credentials.md).

---

## Billing

### "I upgraded but the new features aren't showing"

- Refresh the page. Plan changes take effect within seconds, but the UI state caches briefly.
- Sign out and back in.
- If you've waited more than a minute, check Stripe — the subscription should be active. If it isn't, the payment might have failed; check email and your card.

### "Invoice / receipt not received"

All invoices come from Stripe. Open **Manage Billing → Invoice history** in the Stripe Customer Portal to download. If invoices aren't there, the most recent payment may have failed.

### "Wrong amount charged"

Pricing in Stripe and pricing displayed in-app should match. If they don't, contact us.

---

## Still stuck?

- Check the [FAQ](faq.md).
- Use **Contact us** in the [main app](https://credentials.certdirectory.io/contact).
- Open an issue on the [docs repo](https://github.com/certdirectory/credentials-docs/issues) for documentation problems.
