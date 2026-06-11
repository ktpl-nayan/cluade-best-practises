# Context Persistence — Projects & CLAUDE.md

**Version:** 2.0 | **Date:** June 2026 | **Classification:** Internal Use Only

---

## 1. Overview

Without persistent context, every new Claude conversation starts blank. You re-explain the tech stack, project structure, and coding standards every time.

**Set context once — reuse every session.**

| Tool | Mechanism | Use When |
|---|---|---|
| **Claude Web / Desktop** | Projects with system prompt | Sharing context across the team |
| **Claude Code (IDE/CLI)** | `CLAUDE.md` file in project root | Per-project developer sessions |

---

## 2. Projects — Claude Web & Desktop

A Project is a persistent workspace in Claude Web or Desktop. It has:
- A **system prompt** ("Project instructions") that auto-loads in every conversation
- A **shared document store** for reference files (README, API docs, standards)
- **Team sharing** — add members by email; everyone sees the same context

**What to put in Project instructions:**

```
You are a senior Magento 2 developer working on the Krish B2B Commerce project.

Tech stack: Magento 2.4.7-p3 | PHP 8.2 | OpenSearch 2.11
Custom modules: app/code/Krish/
Standards: PSR-12 + Magento coding standard v2

Rules:
- Constructor injection only — never ObjectManager directly
- No raw SQL — use collection/repository patterns
- All new attributes via Data Patch, never InstallSchema
- Return only requested code — no boilerplate I didn't ask for
```

Write everything that stays the same across conversations. Only task-specific content changes per chat.

---

## 3. CLAUDE.md — Claude Code

`CLAUDE.md` sits in your project root and Claude Code reads it automatically at every session start.

**Place it at:** same level as `package.json` or `composer.json`

**Minimal template:**

```markdown
# [Project Name]

## Stack
[e.g., Magento 2.4.7-p3 | PHP 8.2 | OpenSearch 2.11]

## Structure
[Key directories and what they contain]

## Standards
[Coding rules — PSR-12, ESLint config, etc.]

## Forbidden
- Never use ObjectManager directly
- No raw SQL — use repository/collection
- All new attributes via Data Patch

## Key Commands
bin/magento setup:upgrade
bin/magento cache:flush
```

Stack-specific CLAUDE.md templates: [magento.md](../stacks/magento.md) | [mern.md](../stacks/mern.md) | [shopify.md](../stacks/shopify.md)

---

## 4. What NOT to Include

| Don't include | Why |
|---|---|
| Real customer data or passwords | Never goes into Claude — not even in context |
| Actual secrets or credentials | CLAUDE.md is a plaintext file |
| Entire API documentation files | Bloats context; paste only what's relevant per task |
| Ephemeral task details | Context is for stable project knowledge |

---

## 5. Verifying Context Is Loaded

```bash
# Claude Code — ask directly after starting a session
claude
"What project context do you have loaded?"

/cost   # Token count at session start includes CLAUDE.md
```

In Claude Web/Desktop: open the Project and check Project Settings to confirm the system prompt is set.

---

*See also: [subscription.md](../00-core-principles/subscription.md) | [output-constraints.md](./output-constraints.md)*
