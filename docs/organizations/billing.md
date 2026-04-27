# Billing & Plans

CertDirectory Credentials offers three subscription tiers. Each organization has its own subscription — a user can own multiple organizations on different tiers.

## Plan comparison

| | **Free** | **Professional** | **Enterprise** |
|---|---|---|---|
| Price (monthly) | **$0** | **$49** | **$150** |
| Unique recipients per month | 1,000 | 1,000 | 5,000 |
| Badge templates | 100 | Unlimited | Unlimited |
| Owners per organization | 1 | 2 | Unlimited |
| API keys (active) | 0 | 1 | Unlimited |
| Single issuance | ✅ | ✅ | ✅ |
| Skills on credentials | ✅ | ✅ | ✅ |
| Bulk CSV issuance | – | ✅ | ✅ |
| QR codes | – | ✅ | ✅ |
| API access | – | ✅ | ✅ |
| Domain verification | – | ✅ | ✅ |
| Advanced analytics | – | ✅ | ✅ |
| Branded emails | – | – | ✅ |
| Webhooks | – | – | ✅ |
| Advanced permissions | – | – | ✅ |
| SSO integration | – | – | ✅ |

The "monthly recipients" metric counts **unique recipient emails per organization per calendar month**. Re-issuing or revoking-and-reissuing the same email in the same month does **not** count again.

## What "Unlimited owners" means on Enterprise

You can promote any number of members to **Owner**. Useful when multiple co-leads need full administrative control (billing, signing keys, member management).

## Upgrading

1. Go to **Pricing** (`/pricing`) or click **Upgrade** in the **Billing** section of your organization page.
2. The page lists organizations you **own** that are eligible for upgrade.
3. Click **Upgrade to Professional** or **Upgrade to Enterprise**.
4. You're redirected to a Stripe Checkout session.
5. Enter payment details and confirm. Stripe redirects you back; the upgrade takes effect within a few seconds.

Only an organization's **owner** can change its subscription.

## Managing your subscription

On the organization detail page, the **Billing** section shows:

- Your **current plan** with a coloured badge.
- Your **subscription status** (Active, Trialing, Past due, Canceled, etc.).
- A **cancel-at-period-end** warning if the subscription is set to end.
- A **Manage Billing** button (owners only) that opens the Stripe Customer Portal — change card, view invoices, cancel.

Non-owner members see a read-only view that reads "Contact an owner to change billing."

## Subscription statuses

| Status | What it means |
|---|---|
| **`FREE`** | The organization is on the Free plan. |
| **`TRIALING`** | A free trial is active. Full access during the trial. |
| **`ACTIVE`** | Subscription is healthy and current. |
| **`PAST_DUE`** | Last payment failed. Stripe will retry; if it fails repeatedly, the subscription transitions to canceled. |
| **`CANCELED`** | The subscription has ended. Access reverts to Free at the end of the paid period. |
| **`UNPAID`** | Payment failures exhausted retries. Access has been suspended. |

When the period ends after cancellation, the org auto-reverts to **Free** and any over-Free-limit features (bulk, API, domain verification) become unavailable until you re-subscribe.

## Downgrading

To downgrade, open the **Stripe Customer Portal** via **Manage Billing** and cancel or switch plans. Downgrades take effect at the end of the current billing period — you keep paid features until then.

If your post-downgrade tier doesn't support a feature you're using:

- Existing **API keys** beyond your tier's allotment are deactivated.
- Existing **badge templates** beyond your tier's allotment are kept; you just can't create new ones above the cap.
- **Domain verification** stays in place but the management UI becomes read-only.

## Granted plans (partner / sponsored)

Subscriptions can also be **granted** by the platform (e.g. for partners or trial extensions). Granted subscriptions show a `subscriptionSource` of `GRANTED` or `PARTNER` and may have a `grantExpiresAt` date. Manage-billing actions are disabled for granted plans — contact the granting party to extend or modify.

## Receipts & invoices

All receipts and invoices are issued by Stripe. Access them through the Stripe Customer Portal:

1. Click **Manage Billing**.
2. Open **Invoice history**.
3. Download PDFs for any past invoice.

## Tax & VAT

Stripe automatically calculates and collects applicable taxes based on the billing address you provide.

## Cancellation

Cancel any time via the Stripe Customer Portal. The cancellation:

- Takes effect at the **end of the current billing period** by default (you keep paid features until then).
- Can be set to take effect immediately if you toggle that option in the portal.
- Reverts your organization to the Free plan automatically.

## Need help?

- Plan questions: see the [pricing page](https://credentials.certdirectory.io/pricing).
- Billing problems: use **Contact us** on the platform, or email support directly.
- Custom enterprise terms (volume discounts, invoicing, SSO setup): contact us via the website.

## Next

- [Members & Roles](members-and-roles.md)
- [API Authentication](../api/authentication.md)
