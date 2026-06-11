# Training Plan — Claude AI for Development Teams

**Version:** 2.0 | **Date:** June 2026 | **Classification:** Internal Use Only

---

## Overview

Self-paced training. Estimated total: **4–5 hours** over two weeks.

---

## Week 1 — Foundations (Days 1–5)

### Day 1: Policy & Setup (1 hour)

| Task | Resource |
|---|---|
| Read Acceptable Use Policy | [policy.md](../02-ai-ethics/policy.md) |
| Read Data Handling Guide | [data-handling.md](../02-ai-ethics/data-handling.md) |
| Activate account + enable 2FA | [new-developer.md](./new-developer.md) Steps 2–3 |
| Install Claude Code + authenticate | [new-developer.md](./new-developer.md) Step 3 |
| Run first guided session | [new-developer.md](./new-developer.md) Step 4 |

**Checkpoint:** Claude Code runs in terminal, 2FA enabled. You can state what data you must never send to Claude.

---

### Day 2: Teams Subscription & Model Selection (45 min)

| Task | Resource |
|---|---|
| Read Teams Subscription guide | [subscription.md](../00-core-principles/subscription.md) |
| Read Model Selection | [model-selection.md](../00-core-principles/model-selection.md) |
| Practice: Run same task on Haiku vs Sonnet. Compare output quality using `/cost` | Your dev environment |

**Checkpoint:** You can explain when to use each model and why Opus should be used selectively.

---

### Day 3: Prompt Engineering (1 hour)

| Task | Resource |
|---|---|
| Read Output Constraints guide | [output-constraints.md](../01-prompt-efficiency/output-constraints.md) |
| Review standard XML template | [standards.md](../governance/standards.md#5-code-review-checklist) |
| Practice: Write a prompt for a real task using `<task>`, `<constraints>`, `<examples>` | Your dev environment |

**Checkpoint:** Your practice prompt produces structured output with no unwanted preamble.

---

### Day 4: Context Persistence (45 min)

| Task | Resource |
|---|---|
| Read Projects & CLAUDE.md guide | [context-persistence.md](../01-prompt-efficiency/context-persistence.md) |
| Review or create CLAUDE.md for your project | Current project |
| Find or create a Project for your workstream | claude.ai |

**Checkpoint:** Your project has a CLAUDE.md. Claude Code confirms loading it: `"What project context do you have loaded?"`

---

### Day 5: Stack Deep Dive (1 hour)

Read the stack guide for your primary stack. Then complete one practical task using the stack's prompt template:

- **Magento:** Generate an observer for an event in your project
- **MERN:** Write a MongoDB aggregation for a real reporting need
- **Shopify:** Create a Liquid section for your theme

**Checkpoint:** Task completed with structured output, using CLAUDE.md or a Project. AI Assistance section drafted for a practice PR.

---

## Week 2 — Integration & Governance (Days 6–10)

### Day 6: Claude Code Mastery (30 min)

| Task | Resource |
|---|---|
| Read Claude Code Commands reference | [claude-code-commands.md](../reference/claude-code-commands.md) |
| Complete a real debugging session using Claude Code | Current sprint task |

Practice: use `/compact`, `/clear`, `/cost`, and pipe a log file for error analysis.

---

### Day 7: Security & Data Classification (30 min)

| Task | Resource |
|---|---|
| Read Data Classification | [data-classification.md](../03-security/data-classification.md) |
| Read Account & Access Management | [account-management.md](../03-security/account-management.md) |
| Practice: Classify 3 real data items from your project into Tier 0/1/2/3 | — |

**Checkpoint:** Can classify data in under 30 seconds. Know what to do if account is compromised.

---

### Day 8: Ethics & Transparency (30 min)

| Task | Resource |
|---|---|
| Read Bias & Fairness guide | [bias-and-fairness.md](../02-ai-ethics/bias-and-fairness.md) |
| Read Transparency guide | [transparency.md](../02-ai-ethics/transparency.md) |
| Skim Incident Response | [incident-response.md](../02-ai-ethics/incident-response.md) |

**Checkpoint:** Can apply the bias checklist. Know what the AI Assistance PR section must contain.

---

### Day 9: Governance Standards (20 min)

| Task | Resource |
|---|---|
| Read Governance Standards | [governance/standards.md](../governance/standards.md) |
| Read Activity Monitoring | [activity-monitoring.md](../03-security/activity-monitoring.md) |

---

### Day 10: Assessment & Sign-off (30 min)

**Written quiz — answer without looking up:**
1. What three data categories must never be sent to Claude?
2. Where in a PR must AI assistance be disclosed, and what must it contain?
3. What do you do if you accidentally send customer data to Claude?
4. When should you use Opus instead of Sonnet?
5. What is the purpose of CLAUDE.md?

**Practical task:** Complete a real sprint task using Claude. Write 5 bullet notes:
- Model chosen and why
- How the prompt was structured
- Data checks performed before pasting
- Human review applied to the output
- Session token count (`/cost` result)

**Prompt contribution:** Add one reusable prompt to `/ai-prompts/[your-stack]/` following [standards.md](../governance/standards.md#4-shared-prompt-library) quality criteria.

**Sign-off:** Team Lead marks complete in your onboarding checklist.

---

## Ongoing

| Activity | Frequency |
|---|---|
| Monthly governance review meeting notes | Monthly |
| Anthropic changelog | Monthly — [anthropic.com/news](https://www.anthropic.com/news) |
| Attend governance meeting | Monthly |
| Share useful prompts with team | As discovered → PR to `/ai-prompts/` |

---

*Questions: Slack #ai-governance*
