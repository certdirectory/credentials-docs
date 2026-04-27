# Verifying a Credential

Anyone can verify a CertDirectory credential — no account required. Verification confirms three things: **the credential is genuine**, **it hasn't been tampered with**, and **its current status** (valid / expired / revoked).

## Three ways to verify

### 1. By verification URL

Every credential has a public URL:

```
https://credentials.certdirectory.io/verify/{credential-id}
```

The credential ID looks like `CRD-ZBUYQMTA`. Open the URL in any browser — no sign-in needed (unless the credential is private). You'll see:

- The **status badge** (Valid, Pending, Expired, or Revoked).
- The **badge image**, name, and description.
- The **issuing organization** (with a link to their public profile and a Verified mark if their domain is verified).
- The **recipient** (name and partial email).
- **Issued**, **Expires**, **Claimed**, and **Credential ID** fields.
- The **skills** (tags).
- **Revocation reason** (if revoked).
- **Supersedes / Superseded by** links (if part of a renewal chain).
- A breakdown of detailed checks: signature, expiration, revocation.

### 2. By looking up the credential ID

If you have only the credential ID printed on a certificate, go to:

```
https://credentials.certdirectory.io/verify-lookup
```

Enter the ID (`CRD-…`) and you'll be redirected to the verification page.

### 3. By uploading a baked badge image

If you have a baked PNG (a badge image with the credential data embedded), upload it on the verification lookup page. The platform extracts the OB3 payload and verifies it against the issuer's public key — even works for credentials issued by other Open Badges 3.0 platforms.

## Status meanings

| Status | What it means |
|---|---|
| **Valid** | Signed correctly, not expired, not revoked. The credential is currently authentic. |
| **Pending** | The recipient hasn't yet claimed this credential. It's still cryptographically valid but unclaimed. |
| **Expired** | The credential's expiration date has passed. The issuer can issue a renewal that supersedes it. |
| **Revoked** | The issuer has revoked this credential. It's no longer considered valid. The reason (if provided) is shown. |

## What's checked under the hood

For each verification, the platform performs:

1. **Signature check** — verifies the Ed25519 signature on the credential against the issuer's published public key (resolved from `/issuers/{slug}/did.json`).
2. **Expiration check** — compares `expirationDate` against the current time.
3. **Revocation check** — looks up the credential's `revokedAt` field.
4. **Issuer status check** — confirms the issuing organization is approved and not suspended.

Each verification is logged for the issuer's analytics (anonymously — no PII is recorded about the verifier).

## Programmatic verification

Fetch the JSON API:

```http
GET https://credentials.certdirectory.io/api/verify/{credential-id}
```

Response:

```json
{
  "status": "valid",
  "credential": {
    "id": "...",
    "credentialId": "CRD-ZBUYQMTA",
    "recipientName": "Jane Doe",
    "issuanceDate": "2026-01-15T10:00:00.000Z",
    "expirationDate": "2027-01-15T10:00:00.000Z",
    "revokedAt": null,
    "...": "..."
  },
  "verification": {
    "signatureValid": true,
    "notRevoked": true,
    "notExpired": true
  },
  "ob3Credential": { "...": "OB3 JSON-LD payload..." },
  "features": { "qrCodeEnabled": true }
}
```

For the raw [Open Badges 3.0](open-badges.md) JSON-LD:

```http
GET https://credentials.certdirectory.io/credentials/{credential-id}/ob3
```

For the baked PNG:

```http
GET https://credentials.certdirectory.io/api/credentials/{credential-id}/baked
```

## Privacy

Private credentials return `403` to unauthenticated requests. Recipients who own the credential see full details when signed in.

## Verifying offline

The full verification logic relies on resolving the issuer's public key. To verify completely offline, fetch the issuer's `did.json` ahead of time — once you have the public key and the OB3 JSON, you can verify signatures with any Ed25519 library.

## Next

- [QR Codes](qr-codes.md)
- [Open Badges 3.0](open-badges.md)
