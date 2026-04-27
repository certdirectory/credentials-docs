# Frequently Asked Questions

## General

### Is this Open Badges 3.0 compatible?

Yes. Every credential is a fully signed Open Badges 3.0 JSON-LD document, signed with `eddsa-rdfc-2022` over Ed25519, and the issuer profile is published as a `did:web` DID document. See [Open Badges 3.0](../verification/open-badges.md).

### Is the platform free to use?

There's a generous **Free** tier — you get 1,000 unique recipients per month, up to 100 badge templates, and one organization owner. Paid plans add bulk CSV issuance, API access, QR codes, domain verification, and more. See [Billing & Plans](../organizations/billing.md).

### Where is the source?

The product is currently closed-source. The documentation site you're reading is open and lives at [github.com/certdirectory/credentials-docs](https://github.com/certdirectory/credentials-docs).

---

## For recipients

### Do I need an account to receive a credential?

No. The credential email contains a verification link that anyone can open. You only need an account if you want to **claim** the credential to your wallet, build a public profile, or import external credentials.

### Why does my credential show as "Pending"?

It's been issued but you haven't claimed it yet. Open the verification link in your email and click **Claim This Credential** after signing in.

### I claimed the credential — but it's not showing up on my public profile.

Two possibilities:

1. The issuer marked the credential as **private**. Private credentials never appear on your public profile.
2. You haven't set a **profile slug** yet. Go to **Profile → Edit** and set one.

### The email I was issued to is no longer my main email. Can I still claim it?

You'll need to sign in with the email the credential was sent to. Either sign in to that account (if you have one), or ask the issuer to re-issue to your current email. The platform deliberately doesn't let you claim a credential issued to someone else's address.

### Can I make a credential private after it's been issued?

Not from the recipient side. Public/private is currently set by the issuer at issue time. Contact the issuer to revoke and re-issue if needed.

### Can I import credentials from Credly, Badgr, or other Open Badges platforms?

Yes — go to **Credentials → Import** (`/credentials/import`). Upload the baked PNG, the OB3 JSON file, or paste the JSON directly. Imported credentials appear on your public profile in a separate "Imported" section.

---

## For organizations

### How do I get my organization approved?

Submit your organization with a clear name, working website, and accurate description. Approval is typically completed within one business day.

### Can I issue credentials before approval?

No. You can set up everything (badges, members, signing keys) but not issue until approved.

### What counts toward my monthly recipient quota?

Each **unique recipient email per organization per calendar month** counts as one. Re-issuing or revoking-and-reissuing the same email in the same month does **not** count again. The counter resets on the 1st of each month.

### Can I issue from multiple organizations?

Yes. A user can own and operate any number of organizations, each with its own subscription, members, and badges.

### How do I migrate from another credentialing platform?

Export your existing credentials in OB3 JSON-LD format from your current platform. Recipients can self-import via [Import credentials](../recipients/public-profile.md#importing-external-credentials). For bulk migration of historical credentials issued under your own organization, contact us — we have tooling to ingest your existing data.

### Why does my badge image look pixelated?

Aim for at least 512×512px, square, with no text near the edges (since the QR code overlay is sometimes added). PNG with transparency works best.

### Can I customize the credential email?

Email subject and copy are platform-controlled today. **Branded emails** (your logo, your sender domain) are available on the **Enterprise** plan — contact us to set up custom email branding.

### Can I have a credential automatically expire and renew?

Set `expiresInDays` on the badge template. When the credential expires, you (or your integration) can re-issue to the same recipient — that creates a renewal that supersedes the expired one. There's no automatic re-issuance trigger built in; integrate via the API if you need scheduled renewals.

---

## API

### How do I get an API key?

Upgrade to **Professional** or **Enterprise** plan, then generate one from your organization's API Keys page. See [Authentication](../api/authentication.md).

### What's the difference between the credential ID and the credential UUID?

- **Credential ID** (`CRD-ZBUYQMTA`) — the human-friendly short identifier shown on emails, URLs, and certificates.
- **Credential UUID** (`f1d2c3b4-...`) — the internal database identifier. Used in API responses (`id` field) and required when fetching `/api/v1/credentials/{credentialId}`.

The public verification URL accepts either.

### Can I bulk-issue via the API?

Today, loop the `POST /api/v1/credentials` endpoint, respecting your per-key rate limit (60/min by default). A first-class bulk endpoint is on the roadmap. For one-off batches, use the [Bulk CSV upload](../organizations/bulk-issuance.md) in the UI — it doesn't consume API quota.

### What happens if I issue the same credential twice?

The platform detects an existing credential of the same badge for the same recipient and behaves accordingly:

- If the prior credential is **expired**, **time-bound** (active), or **revoked** → a renewal is created (linked via `supersedesCredentialId`).
- If the prior credential is **active and non-expiring** → the request is rejected with `409 CONFLICT`.

See [Renewing Credentials](../organizations/renewing-credentials.md).

### Are there SDKs?

Not yet. The API is small enough to use directly with `fetch`/`axios`/`requests`. SDKs in JavaScript and Python are on the roadmap.

---

## Verification

### Can I verify a credential offline?

Once you've fetched the issuer's public key (from `/issuers/{slug}/did.json`) and the OB3 JSON for the credential, you can verify the signature with any Ed25519 library — no internet connection needed for that step. To check current revocation/expiration status, you'd still need to query our API or the issuer.

### What does the "Verified" mark next to an issuer mean?

The issuing organization has [verified ownership of their domain](../organizations/domain-verification.md) via DNS TXT record. It's a stronger signal of issuer authenticity than a self-declared name alone.

### Why does the verification page require sign-in?

Either the credential is marked **private** (only viewable by the recipient when signed in), or the credential doesn't exist (a 404 response).

---

## Still have questions?

Reach out via **Contact us** on the [main app](https://credentials.certdirectory.io/contact), or open an issue on the [docs repository](https://github.com/certdirectory/credentials-docs/issues).
