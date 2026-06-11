# Governance Standards & Team AI Policy

**Version:** 2.0 | **Date:** June 2026 | **Owner:** Lead DevOps & AI Ethics Officer
**Classification:** Internal Use Only | **Review Cycle:** Quarterly

---

## 1. Purpose

This document defines team-wide standards for responsible, consistent, and effective Claude usage. Operational companion to the [AI Acceptable Use Policy](../02-ai-ethics/policy.md).

---

## 2. Developer Standards

### Mandatory

```
[ ] Use your company Claude Teams account only — no personal accounts for work
[ ] One task per conversation — /clear between unrelated problems
[ ] Add AI Assistance section to every PR with AI-generated or AI-reviewed code
[ ] Scrub all data before sending — no PII, credentials, or customer data
[ ] Never paste .env, env.php, or any file containing credentials
[ ] Use Projects (Web/Desktop) or CLAUDE.md (Code) for project context
[ ] Match model: Sonnet for standard work, Opus for complex reasoning only
[ ] Review all AI output before use — human review always required
```

### Recommended

- Store reusable prompts in version control under `/ai-prompts/`
- Use the standard XML prompt template for structured tasks
- Include 2–5 examples when expecting parseable output
- Check [bias-and-fairness.md](../02-ai-ethics/bias-and-fairness.md) before deploying AI output to production

---

## 3. Team Lead Standards

### PR Review

When reviewing a PR with AI-assisted code:

```
[ ] AI Assistance section is present and accurately describes scope
[ ] Developer confirmed they tested the output
[ ] No security-sensitive code taken from AI without extra scrutiny
[ ] No hardcoded secrets or PII
[ ] Output quality appropriate — not blindly copied unconstrained AI output
    (sign: overly verbose, unnecessary comments, boilerplate not requested)
```

### Responsibilities

- Approve Tier 2 data sends (see [data-classification.md](../03-security/data-classification.md))
- Escalate Tier 3 requests immediately — these should never happen
- Attend monthly governance review and report squad usage quality

---

## 4. Shared Prompt Library

Maintain prompts in version control alongside code:

```
/ai-prompts/
  /magento/
    system-prompt-claude.md
    module-review.md
    graphql-builder.md
    performance-debug.md
  /mern/
    system-prompt-claude.md
    component-review.md
    aggregation-builder.md
  /shopify/
    system-prompt-claude.md
    liquid-review.md
    graphql-query-builder.md
  /shared/
    pr-review-prompt.md
```

Every prompt in the library must:
- Have a metadata header (purpose, model, author, date)
- Use the standard XML template structure
- Include 2+ examples if requesting structured output
- Specify exact output format and scope
- Be tested before committing

---

## 5. Code Review Checklist

### Standard Pre-PR Review Prompt

```xml
<system_context>[Your stack-specific system prompt from CLAUDE.md or Project]</system_context>

<task>
Pre-PR code review. Check for:
1. Security: injection vulnerabilities, exposed credentials, missing auth checks
2. Performance: N+1 queries, missing caches, unoptimised loops, missing indexes
3. Standards: violations of coding standards in context
4. Edge cases: null handling, empty arrays, network failure scenarios
</task>

<constraints>
  Return JSON: [{severity, category, file, line, issue, recommended_fix}]
  severity: CRITICAL | HIGH | MEDIUM | LOW
  Only CRITICAL and HIGH issues block merge
  Max 15 findings, ordered by severity
</constraints>

<data>[Paste changed files — scrubbed of credentials and customer data]</data>
```

### Human Review Gates

Always reviewed by a human regardless of AI findings:

| Area | Why |
|---|---|
| Authentication & authorisation | AI misses business context for access rules |
| Payment processing | PCI-DSS compliance requires human judgment |
| Data migration scripts | Irreversible — verify rollback exists |
| New external API integrations | Security review of third-party data flows |
| Code touching customer data | Privacy & GDPR implications |

---

## 6. New AI Use Case Approval

```
1. Submit GitHub issue tagged: ai-use-case-proposal
   Include: use case, data tier, business justification, proposed model

2. AI Ethics Officer reviews within 5 business days
   → Approved: added to policy.md Approved list
   → Rejected: documented with reasoning

3. Approved use cases announced in #dev-tools
```

Emergency fast-track: AI Ethics Officer can approve provisionally within 24h for urgent cases.

---

## 7. Monthly Governance Review

**When:** First Friday of each month | **Duration:** 45 min
**Who:** AI Ethics Officer, Tech Leads, DevOps Lead

**Agenda:** Policy compliance → Usage quality → New use cases → Training gaps → Policy updates → Actions

**KPI Targets:**

| Metric | Target |
|---|---|
| PR disclosure compliance | 100% of AI-assisted PRs |
| Data handling incidents | 0 per month |
| New unapproved use cases | 0 per month |
| Developer onboarding completed | 100% within 2 weeks of start |

---

*See also: [policy.md](../02-ai-ethics/policy.md) | [incident-response.md](../02-ai-ethics/incident-response.md)*
