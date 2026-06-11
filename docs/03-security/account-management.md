# Account & Access Management — Claude Teams

**Version:** 2.0 | **Date:** June 2026 | **Owner:** Lead DevOps & AI Ethics Officer
**Classification:** Internal Use Only | **Review Cycle:** Quarterly

---

## 1. Overview

We use **Claude Teams** — a subscription-based product accessed via email login at [claude.ai](https://claude.ai) and the Claude Desktop application. There are no API keys to manage for daily use. Access is controlled through the Teams admin console.

This document covers: account provisioning, shared/group accounts, offboarding, and what to do when an account is compromised.

---

## 2. Account Types on Our Team

| Account Type | Who | Login Method |
|---|---|---|
| **Individual account** | Developer who works exclusively on one team | Personal company email (e.g., `dev@company.com`) |
| **Group / shared account** | Team or role that is shared across developers | Group email (e.g., `devops-team@company.com`) |

### Guidance on Group Accounts

Group accounts create accountability gaps because activity cannot be attributed to a specific individual. Apply these controls when using group accounts:

- **Never use a group account for client-sensitive work** — if something goes wrong, you cannot trace who sent what
- **Do not use a group account for conversations that may contain customer data, credentials, or confidential business logic** — use individual accounts for those tasks
- **Rotate the login password** whenever someone leaves the team that used the group account
- **Document which individuals have access** to each group account — maintain this list in the AI Ethics Officer's access register

---

## 3. Provisioning a New Developer

When a new developer joins, the Team Admin must:

1. **Log in to the Claude Teams admin console** at [claude.ai/settings/team](https://claude.ai/settings/team)
2. **Invite the developer** via their company email address
3. **Assign the appropriate role** — Member (standard) or Admin (team leads and AI Ethics Officer only)
4. The developer will receive an email invitation and create their password on first login

**Developer first-login steps:**
1. Accept the invitation from the email
2. Set a strong password — minimum 16 characters
3. Enable two-factor authentication (2FA) — mandatory for all team members (see Section 4)
4. Install Claude Desktop if working on macOS or Windows
5. Follow the [new-developer.md](../04-onboarding/new-developer.md) onboarding guide for Claude Code setup

---

## 4. Offboarding a Developer

When a developer leaves the team, the Team Admin must act **on their last working day**:

1. **Log in to the Teams admin console**
2. **Remove the departing developer** from the team — this immediately revokes their access to Claude Web, Desktop, and Claude Code using the company account
3. **For group accounts:** Change the group email password immediately if the departing developer had access
4. **Audit shared Projects:** Review any Projects the departing developer had created or shared — ensure team access is maintained and nothing sensitive is left unreviewed

> **Do not delay offboarding.** A former employee retaining Claude access means they retain access to your Projects (which may contain system prompts describing your internal architecture), uploaded documents, and any conversations they shared with the team.

---

## 5. Two-Factor Authentication (2FA) — Mandatory

All team members must enable 2FA on their Claude account:

1. Log in to [claude.ai](https://claude.ai)
2. Go to **Settings → Account → Security**
3. Enable **Two-factor authentication**
4. Use an authenticator app (Google Authenticator, Authy, 1Password TOTP) — not SMS

**Team Leads:** Verify 2FA is enabled for your squad members during the monthly governance review. The AI Ethics Officer checks this for the full team quarterly.

---

## 6. Password Policy

| Requirement | Rule |
|---|---|
| Minimum length | 16 characters |
| Complexity | Mix of upper, lower, numbers, symbols |
| Unique | Not reused from any other service |
| Storage | Password manager only — never written down or in Slack/email |
| Rotation | Individual accounts: rotate annually or on suspected compromise |
| Group accounts | Rotate on every team member change |

---

## 7. What to Do If an Account Is Compromised

Signs of compromise: unexpected sessions, password reset emails you didn't request, conversations you didn't start.

**Immediate steps:**

1. **Change the password immediately** via [claude.ai → Settings → Account](https://claude.ai/settings)
2. **Sign out all other sessions**: Settings → Security → Sign out all devices
3. **Notify the AI Ethics Officer** — they will review recent activity and assess if any sensitive data was exposed
4. **If a group account:** Notify all members and rotate password for everyone
5. **File an AI incident report** — see [incident-response.md](../02-ai-ethics/incident-response.md)
6. **Review Projects:** Check that no unauthorised changes were made to Project instructions or shared documents

---

## 8. Claude Code Authentication

Claude Code (the IDE/CLI integration) authenticates using your Claude Teams account — no API key required.

**First-time setup:**

```bash
# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Start Claude Code — it will open a browser login
claude

# Follow the browser authentication flow:
# → Opens claude.ai/login
# → Log in with your company email and password
# → Approve the authentication request
# → Return to terminal — you're now authenticated
```

Authentication is stored in your local keychain. You do not need to re-authenticate on every session.

**If your authentication expires or stops working:**

```bash
# Re-authenticate
claude auth login

# Or reset authentication entirely
claude auth logout
claude auth login
```

---

## 9. Admin Console — What Admins Can See

Team Admins (AI Ethics Officer, Lead DevOps) have access to the Teams admin console at [claude.ai/settings/team](https://claude.ai/settings/team).

Available in the admin console:
- List of all team members and their roles
- Usage statistics (conversations, models used — depending on Teams plan tier)
- Seat management (add/remove members)
- Billing and subscription management

---

## 10. Access Register

Maintain a simple access register (not in this repo — in the AI Ethics Officer's private documentation):

| Name | Email | Account Type | 2FA | Date Added | Last Verified |
|---|---|---|---|---|---|
| Developer Name | email@company.com | Individual | ✅ | 2026-04-01 | 2026-06-01 |
| DevOps Team | devops@company.com | Group | N/A | 2026-04-01 | 2026-06-01 |

Review this register:
- **Monthly:** during the governance review, verify no stale accounts exist
- **Immediately:** when anyone joins or leaves the team

---

*See also: [data-classification.md](./data-classification.md) | [incident-response.md](../02-ai-ethics/incident-response.md)*
