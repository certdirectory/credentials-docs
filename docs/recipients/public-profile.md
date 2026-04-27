# Public Profile

Every signed-in user gets a public profile at `https://credentials.certdirectory.io/u/{profile-slug}` — a single page showcasing all your claimed credentials.

## Set up your profile

Go to **Profile → Edit** (or `/profile/edit`) to configure:

| Field | What it does |
|---|---|
| **First name** & **last name** | Displayed as the page title. |
| **Profile slug** | The URL-safe identifier in your profile URL. Must be unique. |
| **LinkedIn URL** | Adds a "LinkedIn Profile" link button to your public profile. |
| **Avatar** | Profile picture (PNG/JPEG/WebP, max 5 MB). Upload from this page; click **Remove avatar** to delete. |

Pick your profile slug carefully — it's part of every link you share. You can change it later, but old links stop working.

## What's shown on your profile

Your public profile includes:

- Your **name**, **avatar**, and **LinkedIn link** (if set).
- **Quick stats** — total credentials, total issuers, etc.
- A grid of **platform credentials** (issued through CertDirectory) that have been claimed and marked public by the issuer.
- A grid of **imported credentials** (Open Badges from other platforms you've imported into your wallet).
- Owner-only actions: **Edit profile** and **Export all credentials (JSON)**.

## What controls visibility

A credential appears on your public profile when **all** of these are true:

1. It's been **claimed** by you.
2. The issuer marked it **public** when issuing.
3. It hasn't been deleted from your wallet.

Private credentials — those the issuer marked non-public — are still visible to you on your **dashboard**, but never on your public profile, and the verification URL won't render to logged-out viewers.

!!! note "Public/private is set at issue time"
    The public/private flag is currently set by the **issuer** when the credential is created. There is no recipient-side toggle to switch a credential between public and private after issuance.

## Sharing your profile URL

Once your slug is set, share your profile anywhere:

```
https://credentials.certdirectory.io/u/your-slug
```

A great place to put it: email signature, resume, LinkedIn "Contact info → Website", personal site.

## Importing external credentials

Got Open Badges 3.0 credentials from other platforms? Use **Import credentials** (`/credentials/import`) to add them to your wallet:

- Upload a **baked PNG** (badge image with embedded OB3 data).
- Upload a **JSON file** (raw OB3 credential).
- Or **paste** the JSON directly.

Imported credentials appear on your public profile alongside platform-issued credentials, in a separate "Imported" section.

## Exporting your wallet

The **Export All (JSON)** button on your profile downloads a Verifiable Presentation (VP) containing every credential in your wallet. Useful for:

- Archiving.
- Migrating to another OB3-compatible wallet.
- Programmatic processing.

## Next

- [Sharing Your Credentials](sharing-credentials.md)
- [Verifying a Credential](../verification/verifying-a-credential.md)
