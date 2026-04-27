# Changelog

A high-level log of platform releases and notable changes. The most recent entries are at the top.

!!! note
    This changelog is curated for end-users. For day-to-day commit history, see the source repository (private).

## 2026

### Q1 2026

- **Differentiated email templates for renewals.** Renewal credential emails now use distinct subjects and copy ("Your credential has been renewed 🔄") to avoid confusing recipients with a generic "you've earned" message when they were actually receiving a renewal.
- **Revoked tagline on public profile and dashboard.** Revoked credentials now display a clear `Revoked <date>` tagline under the credential card, mirroring the existing `Expired <date>` UI for expired credentials.
- **Credential linking via supersession.** Renewing a credential now creates explicit OB3 supersession links (`supersedesCredentialId` / `supersededByCredentialId`) between the old and new credentials. Both verification pages display callouts pointing to the related credential.
- **Phase 4 of the credential renewal plan complete.** All four credential lifecycle states — expired, active time-bound, revoked, active non-expiring — are now handled correctly during issuance with appropriate UI banners and back-end behaviour.
- **Pre-issue recipient check.** The single-issue and bulk-issuance forms now run a real-time check against existing credentials for the recipient + badge combination, with coloured banners (green/blue/amber/red) and clear behaviour for each state.
- **Bulk issuance enhancements.** The bulk validation table now shows per-recipient status (new / renewal / skipped / invalid) before submission, so issuers can review exactly what will happen.
- **Documentation site launched.** This site (`docs.credentials.certdirectory.io`) is now live, built with [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) and deployed via GitHub Actions.

## 2025

### Earlier in 2025

- **Open Badges 3.0 issuance.** All credentials are issued as fully-signed OB3 JSON-LD with Ed25519 + `eddsa-rdfc-2022`.
- **Issuer DID documents.** Each organization publishes a `did:web` DID document at `/issuers/{slug}/did.json`.
- **Domain verification (Pro+).** Verify ownership of your organization's domain via DNS TXT record.
- **QR codes (Pro+).** Verification pages display a QR download button for paid-tier issuers.
- **Embed widgets.** Recipients of public credentials can embed a small verification widget on any web page.
- **Stripe billing.** Subscriptions for Free / Professional / Enterprise plans, with self-service upgrade and the Stripe Customer Portal for management.
- **Public organization profiles.** Each org has a public marketing-style page at `/o/{slug}` with their badge gallery.
- **Public user profiles.** Each user has a public profile at `/u/{slug}` showcasing their claimed credentials.
- **Imported credentials.** Users can import OB3 credentials from external platforms (Credly, Badgr, etc.) into their wallet.

---

## Roadmap

Things we're actively building or considering. (No commitments on dates.)

- **Outbound webhooks.** Real-time event notifications to customer-controlled URLs.
- **First-class bulk issuance API.** A dedicated `POST /api/v1/credentials/bulk` endpoint.
- **Credential revocation via v1 API.** Currently revocation is session-only.
- **JS and Python SDKs.** Wrappers around the v1 API.
- **Branded emails.** Custom-domain sending for Enterprise customers.
- **OB3 JWT-VC format.** Alternative to the current Data Integrity (Ed25519 / `eddsa-rdfc-2022`) format.
- **Recipient-side public/private toggle.** Recipients controlling visibility of their own claimed credentials.

Suggestions or requests? Use **Contact us** in the [main app](https://credentials.certdirectory.io/contact).
