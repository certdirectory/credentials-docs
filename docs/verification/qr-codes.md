# QR Codes

QR codes encode the credential's verification URL, so anyone can scan a printed certificate, conference badge, or screen and land directly on the verification page.

!!! info "Plan requirement"
    QR code download is enabled when the issuing organization is on the **Professional** or **Enterprise** plan. Free-tier issuers can still issue verifiable credentials — recipients just won't see the QR download button.

## Where the QR button appears

On the public verification page (`/verify/{credential-id}`):

- The **Download QR** button shows up when `features.qrCodeEnabled` is `true` for the credential's issuer.
- Anyone visiting the page can download the QR — issuer, recipient, or third-party verifier — no sign-in required (for public credentials).

## What the QR encodes

The QR encodes the **current verification URL**:

```
https://credentials.certdirectory.io/verify/CRD-ZBUYQMTA
```

Scanning it from any phone camera opens the verification page in the user's browser. No special app or wallet is required.

## What you get

Clicking **Download QR** generates a PNG containing the QR code. The image is square, with sufficient quiet space for printing, and uses a standard error-correction level (medium-high) so the QR remains scannable even when scaled or printed on coloured backgrounds.

## Recommended uses

- **Printed certificates** — embed the QR next to the credential ID. Anyone holding the paper can verify it.
- **Conference badges & ID cards** — print a small QR with the wearer's primary credential.
- **Email signatures** — though most signatures use the verification URL directly, a QR is helpful when sending PDFs that may be printed.
- **LinkedIn cover images / portfolios** — visual confirmation that scales well.

## Generating QR codes programmatically

If you need to generate QR codes for many credentials at once, build them yourself from the verification URL pattern. Any QR library will do:

```bash
# Example with `qrencode` (Linux/macOS, install via brew/apt)
qrencode -o cred.png "https://credentials.certdirectory.io/verify/CRD-ZBUYQMTA"
```

Or in Node.js:

```javascript
import QRCode from 'qrcode';
await QRCode.toFile('cred.png', `https://credentials.certdirectory.io/verify/${credentialId}`);
```

## Limits

- The QR encodes the verification URL only — not the full credential payload. This keeps the QR small and ensures the recipient always sees the most up-to-date status (including expiry/revocation), since scanning re-fetches.
- For **private** credentials, the QR still works — but the resulting page will require sign-in (and only the recipient can view the full credential).

## Next

- [Verifying a Credential](verifying-a-credential.md)
- [Open Badges 3.0](open-badges.md)
