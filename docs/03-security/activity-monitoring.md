# Activity Monitoring & Accountability

**Version:** 2.0 | **Date:** June 2026 | **Owner:** Lead DevOps & AI Ethics Officer
**Classification:** Internal Use Only | **Review Cycle:** Quarterly

---

## 1. Overview

Since we use Claude Teams as an interactive tool (not via API integrations), our monitoring approach is different from automated logging. Accountability comes from:

1. **Account-level access controls** — each person uses their own login
2. **PR disclosure** — all AI-assisted code is documented in the PR
3. **Admin console visibility** — Team Admins can see usage summaries
4. **Governance reviews** — monthly team review of usage patterns

---

## 2. What the Teams Admin Console Provides

Team Admins (AI Ethics Officer, Lead DevOps) can access the admin console at [claude.ai/settings/team](https://claude.ai/settings/team).

Available information:
- **Member list** — all accounts on the team, their roles, last login
- **Usage overview** — aggregate conversation and model usage across the team (availability depends on Teams plan tier)
- **Seat management** — who has access, when accounts were added or removed

The admin console does **not** give visibility into the contents of individual conversations. Claude does not expose conversation logs to admins — this is by design for user privacy.

---

## 3. The PR Disclosure System — Your Primary Accountability Trail

Since conversation logs are private, the primary accountability mechanism for AI-assisted work is the **PR Assistance disclosure**. Every PR where Claude was used for more than trivial auto-complete must include:

```markdown
## AI Assistance
- Tool used: Claude [model] via [Web / Desktop / Claude Code]
- Scope: [Specific description — e.g., "Generated the observer class, reviewed payment logic"]
- Human review: Confirmed — reviewed output, tested locally, validated against standards
```

This creates a traceable record of:
- Which features/fixes used AI assistance
- What Claude was asked to do (scope)
- That a human reviewed the output before it was merged

Team Leads **must** review this section when approving PRs. See [standards.md](../governance/standards.md) for review checklist.

---

## 4. What to Do When Something Needs Investigation

There is no automated log to query. If an incident occurs (unexpected output, suspected data leak, harmful content), the investigation trail comes from:

1. **The affected developer's conversation history** — each developer can see their own history at [claude.ai](https://claude.ai) → conversation list in the left sidebar
2. **PR descriptions** — the AI Assistance section links AI activity to specific code changes
3. **Git history** — when commits were made, by whom, what was changed
4. **Slack #ai-governance channel** — if the developer followed procedure and flagged anything unusual

Ask the developer to share the relevant conversation link (Web/Desktop has a "Share" option per conversation). They can share a link that makes the conversation visible without giving admin access to all their history.

See [incident-response.md](../02-ai-ethics/incident-response.md) for the full incident response procedure.

---

## 5. Monitoring Usage Patterns — Monthly Review

The monthly governance review (first Friday of each month) should include a review of Claude usage patterns. Without automated cost dashboards, gather this information manually:

**Questions to answer each month:**

| Question | How to Answer |
|---|---|
| Is Opus being used appropriately, or for tasks Sonnet handles? | Ask team leads — are there complaints about hitting Opus rate limits? |
| Are there any known cases where sensitive data was sent? | Review incident log. Count: 0 is the target. |
| Are developers using CLAUDE.md and Projects, or re-explaining context every session? | Quick team survey or 1:1 check-in |
| Is the PR disclosure requirement being followed? | Team Lead reviews: check last 10 merged PRs for AI Assistance sections |
| Any new use cases that weren't approved through the proposal process? | Team leads report anything new from their squads |

Document findings and actions in `/governance/meeting-notes/YYYY-MM.md`.

---

## 6. Data Retention — Conversations

**Claude Web / Desktop:** Conversation history is stored in your Claude account. Anthropic's data retention policy for Teams accounts applies (refer to [Anthropic's Privacy Policy](https://www.anthropic.com/privacy) for current retention periods). Users can delete individual conversations or clear all history from Settings.

**Claude Code:** Sessions are not automatically stored beyond the current session. Output that matters should be saved manually (copy to a file, create a PR, etc.).

> **Important:** Do not rely on Claude's conversation history as a documentation or audit trail. If something matters, document it properly in your codebase, PR description, or incident report.

---

## 7. Reporting a Usage Concern

If a team member observes concerning AI usage (potential data leak, inappropriate content, policy violation):

| Severity | Action |
|---|---|
| **Potential data leak** (PII, credentials sent to Claude) | Contact AI Ethics Officer immediately — same day |
| **Policy violation** (using Claude for prohibited purposes) | Report in Slack #ai-governance — Ethics Officer responds within 24h |
| **Suspicious account activity** (account compromise suspected) | Follow [account-management.md Account Compromise](./account-management.md#6-what-to-do-if-an-account-is-compromised) steps immediately |
| **Harmful or biased output** | File GitHub issue tagged `ai-bias-report` |

---

*See also: [account-management.md](./account-management.md) | [incident-response.md](../02-ai-ethics/incident-response.md) | [standards.md](../governance/standards.md)*
