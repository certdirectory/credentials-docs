# Creating a Badge

A **badge** (a.k.a. an *Achievement* in Open Badges 3.0 terms) is a reusable template that defines *what* you award. Once created, you can issue many credentials from the same badge.

## Prerequisites

- You're a member of an approved organization with at least the **Issuer** role.
- Your plan allows another badge template (Free: 1,000 max; Professional & Enterprise: unlimited).

## Create a badge

1. Go to your organization → **Badge templates → New badge** (`/organizations/{orgId}/badges/new`).
2. Fill in the form:

| Field | Notes |
|---|---|
| **Image** | Required. PNG, JPEG, WebP, or SVG. Max 5 MB. Square images work best (recommended 512×512). |
| **Name** | The credential title. Appears in emails, on the verification page, and on LinkedIn. |
| **Slug** | Auto-generated from the name. Forms part of the public badge URL `/o/{org-slug}/badges/{badge-slug}`. |
| **Description** | What this badge represents. Markdown is *not* rendered — keep to plain text. |
| **Criteria** | What a recipient did to earn it. Shown on the public verification page. |
| **External URL** *(optional)* | A link to your own page about this badge. |
| **Expires in days** *(optional)* | If set, every credential issued from this badge will expire that many days after issuance (e.g. `730` for a two-year cert). Leave blank for non-expiring credentials. |
| **Skills** | Tags representing the skills the badge covers (e.g. `kubernetes`, `cloud-native`). Shown on the verification page and the public profile. |

3. Click **Create badge**.

## Where badges show up publicly

Once created and your organization is approved, the badge has these public URLs:

| URL | Purpose |
|---|---|
| `/o/{org-slug}/badges/{badge-slug}` | Marketing-style badge page with description, criteria, skills, and stats. |
| `/achievements/{org-slug}/{badge-slug}` | Open Badges 3.0 JSON-LD definition (machine-readable). |

The badge image is uploaded to Google Cloud Storage and served from a stable URL.

## Editing & deactivating

- **Edit** (`/organizations/{orgId}/badges/{badgeId}/edit`) — Owner or Admin only. You can change name, description, criteria, image, skills, and `expiresInDays`. Existing credentials issued from this badge are *not* re-signed; their content stays as it was at issue time.
- **Delete** — If no credentials have been issued from the badge yet, deletion is permanent. If credentials exist, the badge is **deactivated** instead, hiding it from the gallery and preventing new issuances while leaving existing credentials intact and verifiable.

## Plan limits

| Tier | Max badge templates per organization |
|---|---|
| **Free** | 1,000 |
| **Professional** | Unlimited |
| **Enterprise** | Unlimited |

If you hit the limit, you'll see an error when creating a new badge with a link to upgrade.

## Tips

- **Use a recognizable image** — the badge image is what recipients share to LinkedIn and Twitter. Make it on-brand.
- **Set `expiresInDays` thoughtfully** — recipients can renew expired credentials, but a credential that never expires can also never need renewal. Pick what fits your program.
- **Add specific skills** — recruiters and ATS systems pick up the skills tags from the OB3 JSON. Be precise.

## Next

- [Issuing Credentials](issuing-a-credential.md)
- [Bulk Issuance](bulk-issuance.md)
