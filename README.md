# Claude AI — Best Practices for Development Teams

**Version:** 2.0 | **Date:** June 2026 | **Classification:** Internal Use Only  
**Owner:** Lead DevOps & AI Ethics Officer | **Review Cycle:** Quarterly

> Practical guide for using Claude AI effectively across Adobe Commerce (Magento), MERN Stack, and Shopify development.

---

## Start Here — Choose Your Path

| I am… | Start with… |
|---|---|
| **New to this team / first day** | [New Developer Onboarding →](./docs/04-onboarding/new-developer.md) |
| **A developer needing a quick answer** | [Quick Reference Cheat Sheet →](./docs/reference/cheat-sheet.md) |
| **Working on Magento** | [Magento Workflow & Prompt Templates →](./docs/stacks/magento.md) |
| **Working on MERN** | [MERN Workflow & Prompt Templates →](./docs/stacks/mern.md) |
| **Working on Shopify** | [Shopify Workflow & Prompt Templates →](./docs/stacks/shopify.md) |
| **Setting up Claude for a new project** | [Projects & CLAUDE.md Setup →](./docs/01-prompt-efficiency/context-persistence.md) |
| **Setting up Claude Code for Magento** | [Magento — Full Project Setup →](./docs/stacks/magento.md#claude-code-project-setup) |
| **Setting up Claude Code for MERN** | [MERN — Full Project Setup →](./docs/stacks/mern.md#claude-code-project-setup) |
| **Setting up Claude Code for Shopify** | [Shopify — Full Project Setup →](./docs/stacks/shopify.md#claude-code-project-setup) |
| **Writing better prompts** | [Prompt Engineering Standards →](./docs/00-core-principles/prompt-engineering.md) |
| **Handling an AI incident right now** | [Incident Response Playbook →](./docs/02-ai-ethics/incident-response.md) |

---

## What We Use

**Claude Teams** — a subscription product from Anthropic. Login: [claude.ai](https://claude.ai) with your company email.

| Tool | Purpose |
|---|---|
| **Claude Web** | Chat at claude.ai — general tasks, Projects, collaboration |
| **Claude Desktop** | Native Mac/Windows app — same as Web, better OS integration |
| **Claude Code** | CLI + IDE extension (VS Code, JetBrains) — in-project development sessions |

No API keys. No per-token billing. All three tools use the same Teams account login.

---

## Documentation Structure

```
/docs
├── 00-core-principles/          Core rules and strategy
│   ├── subscription.md          Teams license, model tiers, usage limits
│   ├── model-selection.md       When to use Haiku / Sonnet / Opus
│   └── prompt-engineering.md    Prompt templates, XML structure, examples
│
├── 01-prompt-efficiency/        Prompt quality and context techniques
│   ├── context-persistence.md   Projects & CLAUDE.md — set context once, reuse always
│   ├── output-constraints.md    Prompting for concise, structured output
│   └── bulk-work.md             Bulk work strategies (one session per item)
│
├── 02-ai-ethics/                AI Ethics Officer's section
│   ├── policy.md                Acceptable use policy
│   ├── data-handling.md         What data is safe to send to Claude
│   ├── bias-and-fairness.md     Output review, human-in-the-loop checkpoints
│   ├── transparency.md          PR disclosure, client disclosure rules
│   └── incident-response.md     Playbook for AI incidents
│
├── 03-security/                 Security for Claude Teams usage
│   ├── account-management.md    Account & access management (login, 2FA, offboarding)
│   ├── data-classification.md   Tier 0–3 data classification
│   ├── activity-monitoring.md   Activity monitoring and accountability
│   └── threat-model.md          LLM-specific threats and mitigations
│
├── 04-onboarding/               Developer enablement
│   ├── new-developer.md         30-minute getting-started guide
│   ├── training-plan.md         10-day self-paced training programme
│   └── faq.md                   Common questions answered
│
├── stacks/                      Stack-specific developer guides
│   ├── magento.md               System prompt, task patterns, CLAUDE.md for Magento
│   ├── mern.md                  System prompt, task patterns, CLAUDE.md for MERN
│   └── shopify.md               System prompt, task patterns, CLAUDE.md for Shopify
│
├── governance/                  Team standards
│   └── standards.md             Developer standards, prompt library, review checklist
│
└── reference/                   Quick lookup
    ├── cheat-sheet.md           One-page quick reference card
    └── claude-code-commands.md  Slash commands, piping, CLAUDE.md guide
```

---

## Core Principles

| Principle | Rule |
|---|---|
| **Match model to complexity** | Haiku → simple, Sonnet → standard, Opus → complex (use selectively) |
| **One task per conversation** | Use `/clear` between distinct problems |
| **Set context once** | Use Projects (Web/Desktop) or `CLAUDE.md` (Claude Code) |
| **Constrain your output** | Specify exact format, scope, and length in every prompt |
| **Never send sensitive data** | PII, passwords, credentials, customer data → never to Claude |
| **Disclose AI use in PRs** | Every AI-assisted PR must have an AI Assistance section |
| **Human is always accountable** | Review, test, and validate all Claude output before use |

---

## Data Safety — Quick Reference

```
SEND:    Your own code, anonymised logs, scrubbed errors, generic schemas

SCRUB:   Production logs (remove emails/IPs/IDs), client code (get approval)

NEVER:   Customer PII · passwords · credentials · .env files · payment data
```

---

## PR Disclosure — Required

```markdown
## AI Assistance
- Tool used: Claude Sonnet 4.6 via Claude Code
- Scope: [What Claude helped with]
- Human review: Confirmed — reviewed, tested locally, validated
```

---

## Governance Calendar

| Event | Frequency | Owner |
|---|---|---|
| Monthly AI Governance Review | First Friday of each month | AI Ethics Officer |
| Account access review | Monthly | AI Ethics Officer |
| Threat model + policy review | Quarterly | AI Ethics Officer |
| Prompt library audit | Quarterly | Tech Leads |

---

## Support & Contacts

| Need | Channel |
|---|---|
| Policy or safety question | Slack: **#ai-governance** |
| Technical question about Claude | Slack: **#dev-tools** |
| Incident — sensitive data sent to Claude | AI Ethics Officer directly (urgent) |
| Propose a new AI use case | GitHub Issue: `ai-use-case-proposal` |
| Report biased/harmful AI output | GitHub Issue: `ai-bias-report` |

---

## Key Links

| Resource | URL |
|---|---|
| Claude Web (login) | https://claude.ai |
| Claude Desktop download | https://claude.ai/download |
| Claude Code documentation | https://docs.anthropic.com/en/docs/claude-code/overview |
| Prompt Engineering Guide | https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview |
| Anthropic Status Page | https://status.anthropic.com |
| Magento Developer Docs | https://developer.adobe.com/commerce/docs/ |
| Shopify Developer Docs | https://shopify.dev/docs |

---

*Maintained by: Lead DevOps & AI Ethics Officer | Review cycle: Quarterly*  
*Questions: Slack #ai-governance*
