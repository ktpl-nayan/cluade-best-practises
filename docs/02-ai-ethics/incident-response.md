# AI Incident Response Playbook

**Version:** 1.0 | **Date:** June 2026 | **Owner:** Lead DevOps & AI Ethics Officer
**Classification:** Internal Use Only | **Review Cycle:** Quarterly

---

## 1. What Is an AI Incident?

Any event where AI tool use results in — or has significant potential to result in:
- A data breach or unauthorised exposure of sensitive data
- A security vulnerability introduced into production
- Harmful, biased, or materially incorrect output delivered to a client or user
- Misuse of Claude that violates team policy or causes reputational damage

---

## 2. Incident Classification

| Severity | Criteria | Response Time | Escalation |
|---|---|---|---|
| **P1 — Critical** | PII sent to AI; security breach in prod via AI-generated code | Within 30 min | Ethics Officer + DPO + CTO |
| **P2 — High** | AI security vulnerability found pre-prod; harmful output to client | Within 2 hours | Ethics Officer + Tech Lead |
| **P3 — Medium** | Biased output caught in review; policy violation without data exposure | Within 24 hours | Ethics Officer |
| **P4 — Low** | Missed PR disclosure; wrong model used | Within 1 week | Team Lead |

---

## 3. Response Steps

### Step 1: STOP

```
[ ] Stop using Claude for this task
[ ] Do NOT delete any prompts, outputs, or logs — preserve evidence
[ ] Do NOT attempt to "fix it quietly"
[ ] Note the exact time of discovery
```

### Step 2: ASSESS

Answer within 30 minutes:
1. What data was involved? (describe type, not actual data)
2. Was it sent to Claude? (Yes = processed on Anthropic's servers)
3. Data tier? → Tier 3 = P1 | Tier 2 = P2 | Tier 1 = P3/P4
4. Was AI output deployed to production? → Yes = P1/P2 | No = P3
5. Clients or users affected? → Yes = escalate one level higher

### Step 3: CONTAIN

**Data exposure (P1/P2):**
```
[ ] Identify every conversation that contained the sensitive data
[ ] Review conversation history at claude.ai for the affected account
    Claude Code: check ~/.claude/projects/ local session logs
[ ] Change account password, sign out all sessions at claude.ai/settings
[ ] If client data: notify AI Ethics Officer → begin GDPR/breach notification
```

**Security vulnerability in production (P1/P2):**
```
[ ] Roll back the affected deployment immediately
[ ] Identify users potentially affected
[ ] Notify Tech Lead and Security lead
[ ] Preserve original AI-generated code as evidence
```

**Harmful output to client (P2):**
```
[ ] Contact client to retract or correct
[ ] Preserve original prompt and output
[ ] Prepare human-generated correction for immediate delivery
```

### Step 4: INVESTIGATE

Complete the Incident Report (Section 5) within 24h (P1/P2) or 72h (P3).

Key questions: Was the prompt following team standards? Was the output reviewed? What data classification step was skipped? Was policy unclear?

### Step 5: NOTIFY

| Incident Type | Who | When |
|---|---|---|
| PII sent to Claude | Ethics Officer → DPO → CTO | Within 1 hour |
| GDPR-reportable breach | DPO → supervisory authority | Within 72 hours (legal requirement) |
| Client impacted | Ethics Officer → Account Manager → Client | P1: 4h | P2: 24h |
| Security vuln in prod | Ethics Officer → Tech Lead → DevOps | Immediately |
| Internal violation only | Team Lead → Ethics Officer | Within 24 hours |

### Step 6: REMEDIATE & CLOSE

```
[ ] Fix root cause (patch, remove code, update prompt/policy)
[ ] Add to Known Failure Modes in bias-and-fairness.md
[ ] Update CLAUDE.md / system prompts to prevent recurrence
[ ] Blameless post-mortem within 5 business days (P1) or 2 weeks (P2/P3)
[ ] Close GitHub issue with resolution summary
[ ] Present findings at next monthly governance meeting
```

---

## 4. Escalation Contacts

| Role | Contact | Availability |
|---|---|---|
| AI Ethics Officer | [Your name / contact] | Business hours + emergency Slack |
| Data Protection Officer | [DPO contact] | Business hours |
| CTO | [CTO contact] | Emergency only |
| Anthropic Trust & Safety | privacy@anthropic.com | For confirmed data processing issues |

> Update this table with real names before distributing.

---

## 5. Incident Report Template

```markdown
# AI Incident Report

**Incident ID:** AI-YYYY-MM-DD-NNN
**Date/Time Discovered:**
**Reported By:**
**Severity:** P1 / P2 / P3 / P4
**Status:** Open / Investigating / Contained / Closed

## Summary
[1–2 sentences]

## Timeline
| Time | Event |
|---|---|
| HH:MM | First indication |
| HH:MM | Discovery |
| HH:MM | Containment |

## Data Involved
- Data type (NOT actual data):
- Data tier: Tier 1 / 2 / 3
- Sent to Claude: Yes / No / Unknown
- Estimated records affected:

## AI Tool & Prompt
- Tool: Claude Web / Desktop / Code
- Model: claude-sonnet-4-6 / opus / haiku
- Prompt preserved: Yes (stored securely) / No
- Output reviewed before use: Yes / No

## Impact
- Systems affected:
- Users/clients affected:

## Root Cause
## Containment Actions
## Remediation
## Prevention
## Lessons Learned

## Sign-off
- Ethics Officer: [Name, Date]
- Tech Lead: [Name, Date]
```

---

## 6. Anthropic Data Retention

Claude Teams conversations may be retained by Anthropic for trust & safety review. Claude Teams does **not** use conversations for model training. To request data deletion: privacy@anthropic.com. Document all communications as part of the incident record.

---

*See also: [policy.md](./policy.md) | [data-handling.md](./data-handling.md) | [activity-monitoring.md](../03-security/activity-monitoring.md)*
