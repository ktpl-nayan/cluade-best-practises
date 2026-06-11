# AI Acceptable Use Policy — Claude & Generative AI Tools

**Version:** 1.0 | **Date:** June 2026 | **Owner:** Lead DevOps & AI Ethics Officer  
**Classification:** Internal Use Only | **Review Cycle:** Quarterly

---

## 1. Purpose & Scope

This policy governs the use of Claude (Anthropic) and any other generative AI tools by all members of the development, DevOps, and operations teams. It applies to:

- Claude Web and Desktop (claude.ai — Sonnet, Opus, Haiku models)
- Claude Code (CLI/IDE-integrated assistant)
- Any future AI tools adopted by the organization

All team members who use AI tools in their workflows are bound by this policy from their first day of access.

---

## 2. Guiding Principles

| Principle | What It Means in Practice |
|---|---|
| **Human Accountability** | A human is always responsible for AI-generated output. "Claude wrote it" is not an excuse for defective code or a data breach. |
| **Minimal Data Exposure** | Send only the minimum data necessary for the task. Never send data that isn't required. |
| **Transparency** | Disclose when AI is used in deliverables, documentation, or customer-facing content. |
| **Least Privilege** | API keys should have only the scopes needed. No shared keys. No keys in source code. |
| **Continuous Review** | AI tool usage is reviewed monthly. This policy is reviewed quarterly. |

---

## 3. Approved Use Cases

The following uses are **approved without requiring additional sign-off**:

### ✅ Code Development
- Writing, reviewing, and refactoring code in supported stacks (Magento, MERN, Shopify)
- Generating unit tests and test data (non-production data only)
- Debugging and root cause analysis using error logs (anonymised)
- Generating boilerplate, configuration files, and documentation

### ✅ DevOps & Operations
- Writing CI/CD pipeline definitions (GitHub Actions, GitLab CI)
- Drafting infrastructure-as-code templates
- Analysing anonymised performance metrics and logs
- Summarising system incident reports (after PII scrubbing)

### ✅ Documentation
- Drafting technical documentation, API references, and runbooks
- Translating technical specs into plain-language summaries
- Generating changelog entries from commit history

---

## 4. Restricted Use Cases

The following require **written approval from the AI Ethics Officer** before proceeding:

| Use Case | Risk | Approval Required From |
|---|---|---|
| Sending client project code to Claude | IP exposure | Team Lead + Ethics Officer |
| Using AI in customer-facing features (chatbots, recommendations) | Trust & accuracy | Product Owner + Ethics Officer |
| Using AI to generate marketing or legal content | Accuracy, compliance | Marketing/Legal + Ethics Officer |
| Processing any real customer data through Claude | PII, GDPR | Ethics Officer + DPO |
| Using AI models not listed in this policy | Unknown risk profile | Ethics Officer |

**How to request approval:** Raise a GitHub issue tagged `ai-ethics-approval` with a description of the use case, data involved, and expected output.

---

## 5. Prohibited Use Cases

The following are **strictly forbidden** under any circumstances:

- ❌ Sending actual customer PII (names, emails, payment data, addresses) to any AI tool or service
- ❌ Using AI to circumvent code review, testing, or security gates
- ❌ Generating content intended to deceive, manipulate, or harm users
- ❌ Using AI to make autonomous decisions on hiring, firing, or performance evaluation
- ❌ Sharing API keys with third parties or storing them in source code repositories
- ❌ Using personal AI accounts (personal Anthropic console, ChatGPT Plus, etc.) for company work
- ❌ Deploying AI-generated code to production without human review

---

## 6. Approval Workflow for New AI Use Cases

```
Developer identifies a new potential AI use case
          ↓
Is it in the "Approved" list above?
    ├── YES → Proceed. Document in PR description.
    └── NO  → Raise ai-ethics-approval issue on GitHub
                    ↓
          Ethics Officer reviews within 48 hours
                    ↓
          APPROVED → Use case added to policy + proceed
          REJECTED → Documented with reasoning
          MORE INFO → Developer responds within 5 business days
```

---

## 7. Disclosure Requirements

### In Code Pull Requests
All PRs containing AI-generated code **must** include in the PR description:

```markdown
## AI Assistance
- Tool used: Claude Sonnet 4.6 / Claude Code
- Scope: [e.g., "Generated the MongoDB aggregation pipeline in analytics.service.ts"]
- Human review: [Confirm: reviewed, tested, and validated by developer]
```

### In Documentation
Add the following footer to any document substantially written with AI assistance:

```
*This document was drafted with AI assistance (Claude, Anthropic) and reviewed by [Name], [Role], on [Date].*
```

### In Client Deliverables
Consult the AI Ethics Officer before submitting any client-facing deliverable that was substantially AI-generated. Client agreements may require disclosure.

---

## 8. Consequences of Policy Violations

| Severity | Example | Consequence |
|---|---|---|
| **Low** | Forgot to add AI disclosure to PR | Verbal reminder, PR updated |
| **Medium** | Used personal AI account for company code | Written warning, access audit |
| **High** | Sent client data to AI API without approval | Formal disciplinary process, incident report |
| **Critical** | Sent PII to AI API, potential data breach | Immediate escalation, legal/DPO involvement |

---

## 9. Policy Review & Maintenance

- **Quarterly review:** First Monday of each quarter
- **Owner:** Lead DevOps & AI Ethics Officer
- **Contributors:** Team Leads, Security, Legal
- **Change process:** Any team member may propose changes via PR against this file

---

*Policy version history is tracked in git. For questions, contact the AI Ethics Officer via Slack #ai-governance.*
