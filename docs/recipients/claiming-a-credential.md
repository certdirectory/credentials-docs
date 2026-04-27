# Claiming a Credential

When an organization issues a credential to your email address, it becomes immediately public and verifiable — but **claiming** it permanently links it to your account. Claiming takes two clicks.

## You'll receive an email

The email comes from the organization that issued the credential and includes:

- The badge image and name.
- The issuing organization.
- The credential ID (e.g. `CRD-ZBUYQMTA`).
- A primary call-to-action button: **View & Verify Credential** → `https://credentials.certdirectory.io/verify/{credential-id}`.

For renewals, the email is differentiated to make the context obvious — for example "Your credential has been renewed 🔄" instead of "You've earned a new credential 🏆".

## Claim flow

1. Click **View & Verify** in the email — this opens the public verification page.
2. The page shows the credential as **Pending** (unclaimed) at first.
3. Click **Sign In to Claim** (or **Create an Account** if you're new).
    - The login flow preserves where you came from with `?redirect=/verify/{credential-id}`, so you'll land back on the same page after signing in.
4. After signing in, click **Claim This Credential**.
5. The page updates: status becomes **Valid** and shows a "Claimed on" date.

The credential now also appears in your **Dashboard** under "Your Credentials" and (if marked public by the issuer) on your public profile.

## What happens if…

### …you sign in with a different email?

Claiming requires your **signed-in email** to exactly match the **recipient email** the issuer entered. If they differ, you'll see an `EMAIL_MISMATCH` error.

**Fix it by either:**

- Signing out and signing in with the email the credential was sent to.
- Asking the issuer to re-send it to your correct email.

### …the credential has already been claimed?

If someone else (or you, on a previous device) already claimed it, you'll see `ALREADY_CLAIMED`. Each credential can only be claimed once. If this happens to you in error, contact the issuing organization.

### …the credential is private?

If the issuer marked the credential as private, the verification page shows a "Private credential" message and you'll need to be signed in (and own it) to view full details.

### …you don't have an account yet?

The verification page detects this and offers **Create an Account**. After registration and email verification, you'll automatically return to the credential page to claim it.

### …the credential has expired or been revoked before you claimed it?

You can still claim valid (non-revoked, non-expired) credentials. Expired or revoked credentials remain visible and verifiable, but cannot be newly claimed.

## After claiming

- The credential appears on your **Dashboard** and on your **Public Profile** (if the issuer made it public).
- Use the **Share** button on the verification page to post to LinkedIn or X, or to grab embed code. See [Sharing Your Credentials](sharing-credentials.md).
- Download the **JSON** (raw OB3 payload), the **baked PNG** (badge image with embedded credential data), or a **QR code** (paid-tier issuers only).

## Next

- [Sharing Your Credentials](sharing-credentials.md)
- [Public Profile](public-profile.md)
