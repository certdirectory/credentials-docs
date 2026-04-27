# Key Concepts

A short reference for the core entities and terminology you'll encounter throughout the platform.

## User

A personal account, identified by email. A user can:

- Receive and claim credentials (as a **recipient**).
- Belong to one or more organizations (as a **member** with a role).
- Maintain a public profile at `/u/{profile-slug}`.

## Organization

An **issuer**. Every credential is issued *by* an organization. Each organization has:

- A profile — name, slug, website, description, logo.
- A pair of Ed25519 **signing keys** used to sign every credential.
- A **status**: `pending_review`, `approved`, `rejected`, or `suspended`. Only **approved** organizations can issue credentials.
- A **subscription tier**: `Free`, `Professional`, or `Enterprise`. Tier governs limits and feature access (see [Billing & Plans](../organizations/billing.md)).
- A **public OB3 issuer profile** at `/issuers/{slug}` and a **DID document** at `/issuers/{slug}/did.json`.

A user can create or join multiple organizations; each has its own tier, members, and credentials.

## Badge (a.k.a. Achievement)

A **template** — the reusable definition of "what" you're awarding. A badge has:

- A name, slug, description, and criteria.
- A square image.
- Optional **skills** (tags).
- An optional **expires-in-days** (e.g. `365` means credentials issued from this badge expire 365 days after issuance).
- A public page at `/o/{org-slug}/badges/{badge-slug}` and a JSON-LD definition at `/achievements/{org-slug}/{badge-slug}`.

> In the API and OB3 specification, this concept is called an **Achievement**. The platform UI calls it a **Badge** (template). Both terms refer to the same thing.

## Credential

A specific **instance** issued from a badge to a recipient. A credential has:

- A unique short ID (e.g. `CRD-ZBUYQMTA`) and a stable URN UUID.
- A recipient email and name.
- An issuance date, optional expiration date, and optional revocation date and reason.
- The full signed [Open Badges 3.0](../verification/open-badges.md) JSON-LD payload.
- A status: **valid**, **pending** (unclaimed), **expired**, or **revoked**.
- A public verification URL: `/verify/{credential-id}`.

A credential can be **claimed** by the recipient (linking it to their user account) or remain unclaimed; either way it's verifiable.

## Status lifecycle

```
                  ┌─────────────────────► expired (issuance + expiresInDays elapsed)
                  │
issued ──► pending ──► (recipient claims) ──► valid ──► expired
   │                                            │
   └────────► revoked  ◄────────────────────────┘
```

- **pending** — issued but not yet claimed by the recipient.
- **valid** — claimed (or unclaimed and public), unexpired, unrevoked.
- **expired** — past its expiration date.
- **revoked** — explicitly revoked by an org admin or owner; remains verifiable but flagged.

## Renewal & supersession

Re-issuing a credential to the same recipient for the same badge creates a **new** credential that **supersedes** the original. Both are linked via:

- `supersedesCredentialId` — points back to the original.
- `supersededByCredentialId` — points forward to the replacement.

This preserves the full history while making the most-recent valid credential the canonical one. See [Renewing Credentials](../organizations/renewing-credentials.md).

## Members & roles

Each organization member has one of four roles:

| Role | Can do |
|---|---|
| **Owner** | Everything — billing, members, signing keys, all org actions. |
| **Admin** | Manage members (except other owners), badges, issue, revoke. |
| **Issuer** | Create badges and issue credentials; cannot revoke or manage members. |
| **Viewer** | Read-only. |

See [Members & Roles](../organizations/members-and-roles.md) for the full permission matrix.

## Identifiers used in URLs and APIs

| Identifier | Format | Example | Where used |
|---|---|---|---|
| Profile slug | URL-safe string | `jane-doe` | `/u/{profile-slug}` |
| Organization slug | URL-safe string | `acme-academy` | `/o/{org-slug}`, `/issuers/{org-slug}` |
| Badge slug | URL-safe string | `aws-fundamentals` | `/o/{org-slug}/badges/{badge-slug}` |
| Credential short ID | `CRD-` + 8 chars | `CRD-ZBUYQMTA` | `/verify/{credential-id}` |
| Credential UUID | UUID | `550e8400-e29b-...` | API responses & database IDs |
| API key prefix | `crd_` | `crd_live_xxxxx` | `Authorization: Bearer crd_...` |

## Next

- [For Recipients](../recipients/creating-an-account.md) — recipient-side flows.
- [For Organizations](../organizations/creating-an-organization.md) — issuer-side flows.
- [API Reference](../api/overview.md) — programmatic integration.
