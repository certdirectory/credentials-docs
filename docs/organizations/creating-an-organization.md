# Creating an Organization

Organizations are the **issuers** in CertDirectory. Every credential is signed by an organization — never by an individual user — so before you can issue, you need to create one.

## Prerequisites

- A signed-in account with a **verified email address**. Email verification is required before you can create an organization.

## Create the organization

1. From your dashboard, click **Create Organization** (or go to `/organizations/new`).
2. Fill in:
    - **Name** — your organization's display name. Shown on credentials, the public profile, and emails.
    - **Slug** — auto-generated from the name; appears in your public URL `/o/{slug}`. Edit if needed.
    - **Website** — your organization's primary website. Required if you later want to verify your domain.
    - **Description** — a short blurb shown on your public profile.
3. Click **Create**.

You'll be taken to the organization detail page (`/organizations/{id}`).

## Approval flow

New organizations start with status **`pending_review`**. While in this state you can:

- Generate signing keys.
- Create badge templates.
- Invite team members.
- Configure billing & domain verification.

You **cannot** issue credentials until you're **approved**. Approval is typically completed within one business day; you'll get an email when the status changes.

If your organization is **rejected** (e.g. because the website doesn't load or the description is unclear), you'll get an email with the reason. You can update the org details and **resubmit** for review.

If your organization is **suspended** later (for policy violations), issuance is paused but existing credentials remain verifiable.

## Generate signing keys

Every credential is signed with your organization's Ed25519 keypair. Keys are generated **once per organization**.

1. On the organization page, find the **Signing keys** section.
2. Click **Generate signing keys**. (Owner only.)
3. Your public key is now published at:
    ```
    https://credentials.certdirectory.io/issuers/{slug}/did.json
    ```

The private key is encrypted and stored on the server — it never leaves it.

!!! warning "Generate keys before you start issuing"
    The Issue and Bulk Issue pages show a warning until keys are generated. You can't issue credentials without them.

## Public profile

Your organization automatically gets:

| URL | What it serves |
|---|---|
| `/o/{slug}` | Public marketing page with stats and badge gallery. |
| `/issuers/{slug}` | OB3 issuer profile (JSON-LD). |
| `/issuers/{slug}/did.json` | DID document with your public key. |
| `/issuers/{slug}/achievements` | Public catalogue of your badges (JSON-LD). |

Customize the page by setting your **logo** (uploaded from the organization detail page) and keeping your **description** and **website** up to date.

## Subscription tier

Every new organization starts on the **Free** plan. You can upgrade any time — see [Billing & Plans](billing.md).

Each organization is billed independently. A user can own multiple organizations on different tiers.

## Next

- [Domain Verification](domain-verification.md)
- [Creating a Badge](creating-a-badge.md)
- [Members & Roles](members-and-roles.md)
- [Issuing Credentials](issuing-a-credential.md)
