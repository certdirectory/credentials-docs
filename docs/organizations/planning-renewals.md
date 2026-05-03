# Planning for Renewal & Re-issuance

How you set up a badge when you first create it decides what you can do later. Can the same person receive this badge more than once? Can recipients renew it when it expires? Pick the right pattern up front and everything else falls into place. Pick the wrong one and you'll be stuck working around it for months.

This page walks through the trade-offs, with concrete patterns for the two most common use cases on the platform: **meetups** and **exams/certifications**.

!!! tip "Running a meetup group? Start here."
    Most of our customers are meetup organizers, and meetups are where people get this wrong most often. Jump straight to [Meetup scenarios](#meetup-scenarios) for the exact pattern we recommend.

## How renewal behaviour works

When you create a badge the single most important choice is whether to set **"Expires in days"**. It changes everything downstream.

| Badge setup | Can the same recipient receive it more than once? | Recipient can renew? | Best for |
|---|---|---|---|
| **No expiration** (`expiresInDays` left blank) | ❌ No — the platform blocks issuing the same badge to the same recipient twice. | No renewal needed. | One-time achievements — a passed exam, a completed course, a conference attended. |
| **With expiration** (e.g. `expiresInDays: 365`) | ✅ Yes, but the new credential **supersedes** (replaces) the old one via Open Badges 3.0 linking. The old credential is no longer the "latest" one on their profile. | Yes. Either you or the recipient can re-issue once the credential is near or past expiry. | Time-sensitive qualifications — security certs, role-based credentials, annually-renewed memberships. |

!!! note "The platform enforces this automatically"
    If you try to issue a no-expiry badge to a recipient who already has one, the issue form stops you with a clear *"already issued"* message. You don't have to police it yourself.

For the mechanics of the supersession UX (what recipients and organizers see), check [Renewing Credentials](renewing-credentials.md) and [Issuing Credentials](issuing-a-credential.md).

## Meetup scenarios

Meetups generate the **most re-issuance traffic** on the platform — people come back month after month, speakers return, organizers recur every year. A bad template design falls apart quickly at this volume. Here's what works.

### Attendee badges — one template per event

The tempting shortcut is to create a single "Meetup Attendee" template with no expiry and issue it for every meetup. **Don't.** Once someone has attended even one event, the platform will block you from issuing the same template to them again.

Setting an expiry date doesn't rescue you either — the *next* month's issuance would **supersede** the earlier one, so the attendee would lose their standalone proof of attending the previous event.

!!! note "About the examples below"
    We use **"CertDirectory"** throughout the examples on this page as a stand-in for your own organization or meetup group. Substitute your own name — e.g. *"YourOrg Meetup Attendee"*, slug `yourorg-meetup-attendee-april-2026`, and so on.

**Recommended pattern:**

- Use the **same display name** for every event's attendee badge — e.g. `CertDirectory Meetup Attendee`. Names are not required to be unique, so this is perfectly fine and keeps recipients' profiles looking consistent.
- Give each badge a **unique slug**, like `certdirectory-meetup-attendee-april-2026`, `certdirectory-meetup-attendee-may-2026`, and so on. The slug is what makes two templates distinct in the system.
- Give each badge a **unique image** — design a base template and overlay the event's month and year. Recipients end up with a clean collage of per-event badges on their public profile.
- Leave **expiry blank** (no expiration). Each event's attendance is a permanent record.

With this pattern:

- A regular attendee can have 12+ distinct attendee badges per year, all visible on their profile.
- No "already issued" errors for returning attendees.
- Each event stays as a separate, verifiable record.

![Example: a monthly attendee badge with the event month on the image](../images/meetup-attendee-badge-example.png){ width=220 }

*Example: same name, unique slug and image per month.*

### Speaker badges — same pattern as attendees

Speakers who return to speak again shouldn't lose their badge from the previous event. Apply the exact same pattern:

- Shared display name such as `CertDirectory Meetup Speaker`.
- Unique slug and image per event (e.g. `certdirectory-meetup-speaker-april-2026`).
- No expiry.

### Organizer & volunteer badges — yearly is usually enough

Organizer and volunteer roles are stable across a year, and the same person rarely holds the role multiple times in the same year. A **yearly template** works well here and keeps your badge list tidy.

- Template name: `CertDirectory 2026 Organizer`, slug `certdirectory-2026-organizer`.
- No expiration (or set expiry to 31 December 2026 if you want the year-specific meaning to lapse formally).
- The following year, create a new template: `CertDirectory 2027 Organizer`.

Recipients accumulate one organizer badge per year on their profile — a clean timeline of contributions.

## Exam & certification scenarios

### One-time achievement — no expiry

If your badge represents passing a specific exam, finishing a specific course, or attending a specific conference, **and** the content of the achievement doesn't get outdated, leave expiry blank. The credential becomes a permanent record.

!!! warning "Remember the one-per-recipient rule"
    With no expiry set, a recipient can receive this badge **only once**. That's usually exactly what you want for a one-time exam — the same exam shouldn't be taken twice, and the credential should be a permanent proof.

### Time-sensitive certification — with expiry

If your badge represents a certification whose content changes over time (e.g. a Kubernetes admin exam that gets revised every two years, or an annual workplace-safety recertification), set `expiresInDays` accordingly — for example, `730` for two years.

Then:

- Recipients can retake the updated exam and earn a fresh credential.
- The fresh credential automatically **supersedes** the old one, with a linked chain visible on the verification page.
- Employers verifying the credential can see both the current credential and the full history.

## Quick decision helper

Still unsure? Ask yourself these questions in order:

1. **Can the same person legitimately earn this more than once?**
    - *Yes* → set an expiry (with supersession) **or** create a separate template per instance (the attendee pattern).
    - *No* → leave expiry blank.
2. **If they can earn it multiple times, is each earning a distinct event?**
    - *Yes, distinct events (meetups, conferences)* → follow the attendee pattern: one template per event, shared name, unique slug + image.
    - *No, it's the same qualification being renewed* → use a single template with an expiry.
3. **Does the content become outdated over time?**
    - *Yes* → set `expiresInDays` so holders are prompted to renew.
    - *No* → leave blank; no renewals needed.

## Changing your mind later

Badge templates are **deliberately strict** about what can be edited. This is to protect the cryptographic integrity of credentials already issued from them — the whole reason the platform exists.

### Before any credential is issued

You can freely edit anything *except* the template's **name** and **slug** — those are always locked so that a template can't be silently repurposed to mean something different, and so that public URLs don't change underneath anyone.

### After the first credential is issued

Once the template has been used to issue **even one credential**, almost everything becomes immutable:

| Field | After first issuance | Why |
|---|---|---|
| **Name** | 🔒 Locked | Prevents silent repurposing. |
| **Slug** | 🔒 Locked | Preserves existing public URLs like `/o/{org-slug}/badges/{badge-slug}`. |
| **Image** | 🔒 Locked | Part of the signed OB3 credential payload. |
| **Description** | 🔒 Locked | Part of the signed OB3 credential payload. |
| **Criteria** | 🔒 Locked | Part of the signed OB3 credential payload. |
| **Skills** | 🔒 Locked | Part of the signed OB3 credential payload. |
| **Expires in days** | 🔒 Locked | Protects the expiry terms of existing credentials. |
| **External URL** | ✅ Editable | Not part of any signed credential — purely a display field on the badge page. |

!!! info "Why the lock exists"
    Each credential is signed at issuance time with an Ed25519 signature over a canonical form of the credential data — which *includes* the achievement's image, description, criteria, and skills. The signature is stored on the credential row, but the signed JSON itself is never archived — it's rebuilt from the current template every time a credential is verified or a baked badge image is downloaded.

    If those fields could be changed, the regenerated JSON would no longer match what was originally signed, and **every existing credential's signature check would fail** — the verification page's *"✓ Signature"* check would flip to *"✕ Signature"*. Recipients and verifiers would see the credential as cryptographically untrusted, even though nothing malicious happened.

    The platform closes this door entirely: you can't edit fields that affect the signed payload once anything has been issued, so credentials you issued years ago stay verifiable forever.

### If you realise a template is set up wrong

The clean path is always the same: **deactivate the old template and create a new one** with the right setup. Deactivation hides a template from new issuances while leaving existing credentials intact and fully verifiable — signatures included.

This is exactly why the meetup patterns above (one template per event, with a shared display name but unique slug and image) exist: they embrace the immutability rather than fighting it.

## Next

- [Creating a Badge](creating-a-badge.md) — the mechanics of the form itself.
- [Issuing Credentials](issuing-a-credential.md) — what the pre-issue recipient check looks like.
- [Renewing Credentials](renewing-credentials.md) — what recipients see when they renew.
