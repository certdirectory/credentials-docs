# Sharing Your Credentials

Every credential has a unique public verification URL — `https://credentials.certdirectory.io/verify/{credential-id}` — that anyone can visit to confirm authenticity. From the verification page, you have several built-in sharing options.

## LinkedIn

Two options, both available when you're viewing a credential you own:

### Add to your LinkedIn profile

The **Add to Profile** button uses LinkedIn's official "Add Credential" URL. It pre-fills:

- Credential **name** and **issuing organization**.
- **Issue month** and **year**.
- **Credential URL** (the verification page) and **Credential ID**.
- **Expiration date** (if applicable).

LinkedIn will add it to the **Licenses & certifications** section of your profile, with a clickable **Show credential** link that points back to your verification page.

### Share as a post

The **Share to LinkedIn** button opens LinkedIn's share dialog with the verification URL pre-loaded, so you can write a custom post.

## X (Twitter)

The **Share to X** button opens a pre-filled tweet with a link to your verification page.

## Direct link

Use **Copy link** to copy the verification URL — paste it anywhere: email signatures, resumes, portfolios, Slack messages, or your personal site.

## Embed code

Click **Embed** on the verification page (when you own the credential) to get a copy-pastable HTML snippet. The embed renders a small verification widget on any web page, with a link back to the full verification page.

The widget is served from `https://credentials.certdirectory.io/api/embed/{credential-id}` and only works for credentials marked as **public**.

## QR code

If the issuing organization is on a paid plan ([Professional or Enterprise](../organizations/billing.md)), you'll see a **Download QR** button on the verification page. The QR code encodes the verification URL — scan it from any phone to view the credential. Useful for printed certificates and conference badges.

## Baked badge image

Click **Download Badge** to get a **baked PNG** — a high-resolution copy of the badge image with the full Open Badges 3.0 credential data embedded as PNG metadata. This file:

- Renders as a normal image anywhere.
- Can be **uploaded back** to the [verification page](../verification/verifying-a-credential.md) to verify it offline-of-our-platform.
- Travels with you — anyone who has the file can verify it via any OB3-compatible tool.

## Raw JSON

Click **Download JSON** to get the full signed Open Badges 3.0 credential as JSON-LD. Useful for:

- Importing into other Open Badges 3.0 wallets.
- Programmatic verification.
- Long-term archival.

## Public profile

Your public profile at `/u/{profile-slug}` shows all the credentials you've claimed (that the issuer marked public). Set up your profile slug at **Profile → Edit** so you have a clean URL to share. See [Public Profile](public-profile.md).

## What about private credentials?

If the issuer marked your credential **private**, the verification page is only visible when you're signed in. Sharing options are limited — embed code and public LinkedIn shares won't work, since the URL doesn't render for unauthenticated viewers.
