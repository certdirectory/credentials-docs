# Introduction

**CertDirectory Credentials** is a digital credentialing platform for issuing, verifying, and managing portable, cryptographically-signed certificates and badges based on the [Open Badges 3.0 (OB3)](https://www.imsglobal.org/spec/ob/v3p0) specification.

## Who it's for

- **Training providers, certification bodies, and educational institutions** issuing certificates of completion, attendance, or achievement.
- **Employers and HR teams** recognizing skills, milestones, and internal certifications.
- **Communities and event organizers** awarding participation or contribution badges.
- **Recipients** of any of the above who want a single home for all their digital credentials.

## What makes a credential "verifiable"

Every credential issued through CertDirectory is:

- **Cryptographically signed** with the issuing organization's Ed25519 keys, so no one can tamper with it without the change being detected.
- **Independently verifiable** — anyone with the verification link can confirm authenticity without contacting the issuer.
- **Standards-based** — credentials follow the [Open Badges 3.0](../verification/open-badges.md) JSON-LD format, which is interoperable with other compliant wallets and verifiers.
- **Linked to its issuer's identity** — the issuer's organization profile and DID document are publicly resolvable.

## Core capabilities

| | |
|---|---|
| **Single & bulk issuance** | Issue one credential at a time, or upload a CSV to issue thousands. |
| **Recipient claiming** | Recipients receive an email with a verification link; logged-in users can permanently claim and add credentials to their profile. |
| **Public verification page** | Every credential has a public URL where anyone can verify it. |
| **Public profiles** | Recipients get a personal profile (`/u/your-slug`) showcasing their claimed credentials. |
| **Renewal & revocation** | Credentials can be renewed (with linkage to the original) or revoked with a public reason. |
| **Domain verification** | Organizations on paid plans can prove ownership of their domain via DNS TXT record. |
| **API access** | Integrate issuance into your own systems on Professional and Enterprise plans. |
| **QR codes & embeds** | Download QR codes and embed verification widgets on your site. |


## Next

- [Quickstart](quickstart.md) — issue your first credential in five minutes.
- [Key Concepts](concepts.md) — understand organizations, badges, credentials, and how they fit together.
