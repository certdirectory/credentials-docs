# Members & Roles

Invite team members to your organization and assign roles. Roles control who can issue, revoke, manage members, and access billing.

## Roles overview

| Role | Typical use |
|---|---|
| **Owner** | The buck stops here. Owners can do everything — including managing billing, generating signing keys, promoting/demoting other members, and deleting the organization. |
| **Admin** | Day-to-day management. Can manage badges, issue and revoke credentials, and invite/remove non-owner members. Cannot touch billing or other owners. |
| **Issuer** | Day-to-day issuance. Can create badges and issue credentials. Cannot revoke or manage members. |
| **Viewer** | Read-only. Sees the org, badges, credentials, and analytics but cannot make changes. |

## Permission matrix

| Action | Owner | Admin | Issuer | Viewer |
|---|:-:|:-:|:-:|:-:|
| Update organization details | ✅ | ✅ | – | – |
| Generate signing keys | ✅ | – | – | – |
| Invite owners | ✅ | – | – | – |
| Invite admins / issuers / viewers | ✅ | ✅ | – | – |
| Change member roles (non-owners) | ✅ | – | – | – |
| Promote a member to owner | ✅ | – | – | – |
| Remove members | ✅ | ✅ | – | – |
| Leave the organization (self) | ✅* | ✅ | ✅ | ✅ |
| Create / edit / delete badges | ✅ | ✅ | partial† | – |
| Upload badge images | ✅ | ✅ | ✅ | – |
| Issue credentials (single & bulk) | ✅ | ✅ | ✅ | – |
| Revoke credentials | ✅ | ✅ | – | – |
| View credentials | ✅ | ✅ | ✅ | ✅ |
| View analytics | ✅ | ✅ | ✅ | ✅ |
| Manage API keys | ✅ | – | – | – |
| Manage billing | ✅ | – | – | – |
| Manage domain verification | ✅ | – | – | – |

*An owner can leave only if at least one other owner remains. The last owner cannot leave; promote someone else first.
†Issuers can **create** badges but not **edit** or **delete** them.

## Invite members

1. Go to **Team** (`/organizations/{orgId}/team`).
2. Click **Invite member**.
3. Enter their **email address** and select a **role**.
4. Click **Send invite**.

The invitee receives an email. If they don't have an account yet, the email links them to sign up; their invitation will be pending until they accept it from their dashboard.

## Owner limits per plan

The number of **owners** per organization is capped by your subscription tier:

| Plan | Max owners |
|---|---|
| **Free** | 1 |
| **Professional** | 2 |
| **Enterprise** | Unlimited |

There's no hard limit on **admins**, **issuers**, or **viewers** at any tier.

If you try to invite or promote beyond the cap, you'll see an error pointing you to upgrade.

## Accepting invitations

The invited person:

1. Signs in to their dashboard.
2. Sees a **Pending invitation** card.
3. Clicks **Accept** (or **Decline**).
4. Once accepted, they appear in the org's **Team** page with their assigned role.

## Changing roles

On the **Team** page (Owner only), click the role next to a member to change it. Notes:

- You cannot change another **owner's** role directly. To remove an owner, promote someone else first, then ask them to step down.
- **Promoting to owner** counts toward your tier's owner cap and triggers an email to the promoted member.

## Removing a member

Click **Remove** next to a member (Owners and Admins only).

- Owners can remove anyone except other owners.
- Admins can remove issuers and viewers.
- The removed member receives an email notification (unless self-removal).

## Leaving an organization

Any member can leave their own organization via **Leave organization** on the Team page. The catch: the **last** owner cannot leave (would orphan the org). Either promote someone first, or contact support if the org should be deleted.

## Notifications

Members receive email notifications for:

- Being **invited** (with accept link).
- Having their **role changed**.
- Being **promoted to owner**.
- Being **removed** by another member.
- Another **owner leaving** the organization (sent to remaining owners).

Emails are sent from the platform, not from your organization's address. They're suppressible by disabling **Email notifications** at the organization level (this disables *all* org emails, including credential issuance — use with caution).

## Next

- [Billing & Plans](billing.md)
- [API Keys](../api/authentication.md)
