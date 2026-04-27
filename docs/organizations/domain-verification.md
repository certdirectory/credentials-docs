# Domain Verification

Verify ownership of your organization's domain via a DNS TXT record. Verified organizations display a **Verified** badge on their public profile and on credentials, signalling to recipients and viewers that the issuer is genuine.

!!! info "Plan requirement"
    Domain verification is available on the **Professional** and **Enterprise** plans. Free-tier organizations see an upsell.

## Prerequisites

- Your organization must have a **website** set in its profile (e.g. `https://acme.example.com`). The host portion is the domain we'll verify.
- You must be on Professional or Enterprise.
- Only **owners** can start, run, or remove verification.

## How it works

1. CertDirectory generates a unique **verification token** for your organization.
2. You add a `TXT` record on your DNS containing that token.
3. CertDirectory queries DNS, finds the record, and marks your domain verified.

## Step-by-step

1. Open your organization → **Domain verification** (`/organizations/{id}/domain-verification`).
2. Click **Start verification**. This generates the token and shows the DNS record you need to add.
3. In your DNS provider, add a record on the domain that matches your organization's website:

    | Field | Value |
    |---|---|
    | **Type** | `TXT` |
    | **Host / Name** | `_certdirectory` (or `_certdirectory.your-domain.com` depending on your provider) |
    | **Value** | The token shown in the UI (e.g. `certdirectory-verification=abc123...`) |
    | **TTL** | 300 (or your provider's lowest) |

    Use the **Copy** buttons in the UI to avoid typos.

4. Wait a few minutes for DNS propagation (typically 1–5 minutes; up to a few hours for some providers).
5. Click **Verify**. CertDirectory queries the DNS record:
    - **Match found** → status changes to **Verified**, the verified domain is recorded, and the **Verified** badge appears on your public profile.
    - **No match yet** → message tells you the record wasn't found; try again after another minute.

## After verification

- Your **public profile** displays a small Verified checkmark next to your organization name.
- The verified domain is also part of your public OB3 issuer profile, so anyone fetching `/issuers/{slug}` can see it.
- You can leave the TXT record in place permanently (recommended) or remove it after verification — but if you remove it, periodic re-checks may fail.

## Removing verification

If you ever change domains:

1. Go to **Domain verification**.
2. Click **Remove verification**.
3. Update your organization's website.
4. Restart the flow with the new domain.

## Troubleshooting

**"Website URL is required"** — set your organization's website on the org detail page first.

**"Feature not available"** — your organization is on Free. [Upgrade to Professional](billing.md).

**TXT record not found, even though I added it** — DNS can take time. Try `dig TXT _certdirectory.your-domain.com` from a terminal to confirm propagation. If you see the record but the platform doesn't, double-check that the **value** matches exactly (extra quotes or whitespace will fail).

**Multiple TXT records** — you can have multiple TXT records on the same hostname; CertDirectory looks for one matching the expected token, so other records are safely ignored.

## Next

- [Members & Roles](members-and-roles.md)
- [Issuing Credentials](issuing-a-credential.md)
