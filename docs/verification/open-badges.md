# Open Badges 3.0

CertDirectory issues credentials that conform to the [Open Badges 3.0 (OB3)](https://www.imsglobal.org/spec/ob/v3p0) specification published by 1EdTech. This means our credentials are interoperable with any OB3-compatible wallet, viewer, or verifier.

## What OB3 gives you

- **A standard JSON-LD payload** for each credential — recipient, issuer, achievement, dates.
- **Cryptographic signing** — each credential carries a signature linked to the issuer's published public key.
- **Linked Data resolution** — issuers, achievements, and credentials all have public URLs that return JSON-LD.
- **Independent verification** — anyone with the credential payload and access to the issuer's public key can verify it without contacting CertDirectory.

## How CertDirectory implements OB3

### Credential format

Every issued credential includes a full OB3 `OpenBadgeCredential` JSON-LD document with:

- `@context` — standard W3C VC + OB3 contexts.
- `type` — `["VerifiableCredential", "OpenBadgeCredential"]`.
- `issuer` — full Issuer profile, with the issuer's URL, name, type, and DID.
- `validFrom` — issuance timestamp.
- `validUntil` (optional) — expiration timestamp.
- `credentialSubject` — recipient identity (name and a hashed identifier of their email by default).
- `credentialSubject.achievement` — the embedded Achievement.
- `proof` — the Ed25519 signature using `eddsa-rdfc-2022`.

You can fetch any credential's raw OB3 JSON-LD at:

```
GET https://credentials.certdirectory.io/credentials/{credential-id}/ob3
```

(Returns `403` for private credentials viewed unauthenticated.)

### Issuer profile (DID)

Each organization publishes:

- An **OB3 Profile** at `/issuers/{slug}` — JSON-LD describing the issuer.
- A **DID document** at `/issuers/{slug}/did.json` — contains the organization's public Ed25519 key as a verification method.
- An **Achievements collection** at `/issuers/{slug}/achievements` — JSON-LD listing all the issuer's badge templates.

The DID method used is `did:web` — the issuer's identifier is derived from the URL itself. This means anyone with the issuer URL can resolve the public key without trusting CertDirectory's API.

### Achievement format

Each badge template is published as a public OB3 Achievement at:

```
GET https://credentials.certdirectory.io/achievements/{org-slug}/{badge-slug}
```

The Achievement document includes:

- `@context`, `id`, `type` (`Achievement`).
- `name`, `description`, `criteria.narrative`.
- `image` (URL).
- `tag` array (skills).
- (Optionally) `creator` linking back to the Issuer.

### Signing algorithm

Credentials are signed with **Ed25519** using the **`eddsa-rdfc-2022`** Data Integrity proof suite. This is one of the OB3-conformant proof formats, and it's broadly supported across OB3 wallets and verifiers.

The signing happens server-side; private keys are encrypted at rest and never leave the API.

### Baked badge images

A baked PNG is the badge image with the OB3 credential JSON embedded as a PNG `iTXt` chunk. This is the OB3-recommended way to make a credential portable as a single file: someone can email or upload the PNG, and any OB3 verifier can extract and verify it.

Download a baked PNG at:

```
GET https://credentials.certdirectory.io/api/credentials/{credential-id}/baked
```

Re-verify a baked PNG (works for any OB3 issuer's PNG, not just ours):

```
POST https://credentials.certdirectory.io/api/credentials/verify-image
Content-Type: multipart/form-data
```

### Importing external OB3 credentials

Users can import OB3 credentials issued by other platforms (Credly, Badgr, Accredible, etc.) into their CertDirectory wallet. The credential's signature is verified against its issuer's published key, then stored in the user's wallet. Imported credentials show up on the user's [public profile](../recipients/public-profile.md) alongside platform-issued credentials, in a separate "Imported" section.

### Exporting your wallet

A user can export every credential in their wallet as a single **Verifiable Presentation (VP)** JSON file. The VP wraps every credential and is itself a standard W3C VC payload — useful for migrating to another OB3-compatible wallet.

```
GET https://credentials.certdirectory.io/api/credentials/export
Authorization: Bearer <session JWT>
```

## Interoperability notes

- The CertDirectory issuer/achievement/credential URLs are all dereferenceable (return JSON-LD). External verifiers can resolve the chain entirely from URLs.
- We follow the OB3 specification for the credential subject identifier — by default, recipient identity is hashed for privacy. We use a salted hash matching the OB3 [`IdentityObject`](https://www.imsglobal.org/spec/ob/v3p0/#identityobject) recommendation.
- We do **not** currently issue OB3 credentials in the JWT-VC format — only in the JSON-LD Data Integrity format. JWT support is on the roadmap.

## Resources

- [Open Badges 3.0 specification](https://www.imsglobal.org/spec/ob/v3p0)
- [W3C Verifiable Credentials Data Model](https://www.w3.org/TR/vc-data-model/)
- [DID Web specification](https://w3c-ccg.github.io/did-method-web/)

## Next

- [Verifying a Credential](verifying-a-credential.md)
- [API Reference](../api/overview.md)
