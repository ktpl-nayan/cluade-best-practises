# Claude AI — Technical Guide for Development Teams

**Version:** 1.1 | **Date:** June 2026 | **Audience:** Developers & Team Leads | **Classification:** Internal Use Only

This guide is organized into modular, stack-specific sections for Adobe Commerce (Magento), MERN Stack, and Shopify Development.

## 📚 Documentation Structure

```
/docs
  ├── 00-core-principles/         ← Token economics, model selection
  ├── 01-token-optimization/      ← Caching, batch API, output constraints
  ├── 02-prompt-engineering/      ← XML templates, best practices
  ├── stacks/
  │   ├── magento/               ← Magento-specific workflows
  │   ├── mern/                  ← MERN stack workflows
  │   └── shopify/               ← Shopify development workflows
  ├── governance/                 ← Team standards, checklists
  └── reference/                  ← Quick refs, links, commands
```

## 🚀 Quick Start

1. **New to Claude?** → Read [`00-core-principles/economics.md`](./docs/00-core-principles/economics.md)
2. **Building a Magento feature?** → Start with [`stacks/magento/README.md`](./docs/stacks/magento/README.md)
3. **Working on MERN?** → Check [`stacks/mern/README.md`](./docs/stacks/mern/README.md)
4. **Shopify development?** → See [`stacks/shopify/README.md`](./docs/stacks/shopify/README.md)
5. **Setting up Claude Code?** → Read [`reference/claude-code-setup.md`](./docs/reference/claude-code-setup.md)

## 📋 Table of Contents

- [Core Principles](./docs/00-core-principles/)
  - [Token Economics](./docs/00-core-principles/economics.md)
  - [Model Selection Strategy](./docs/00-core-principles/model-selection.md)

- [Token Optimization](./docs/01-token-optimization/)
  - [Prompt Caching Fundamentals](./docs/01-token-optimization/caching.md)
  - [Output Constraints](./docs/01-token-optimization/output-constraints.md)
  - [Batch API Guide](./docs/01-token-optimization/batch-api.md)
  - [Context Window Management](./docs/01-token-optimization/context-management.md)

- [Prompt Engineering](./docs/02-prompt-engineering/)
  - [Standard Prompt Template](./docs/02-prompt-engineering/template.md)
  - [XML Tag Reference](./docs/02-prompt-engineering/xml-tags.md)
  - [Multi-Shot Examples](./docs/02-prompt-engineering/examples.md)

- [Stack-Specific Workflows](./docs/stacks/)
  - [Magento/Adobe Commerce](./docs/stacks/magento/README.md)
  - [MERN Stack](./docs/stacks/mern/README.md)
  - [Shopify Development](./docs/stacks/shopify/README.md)

- [Governance & Standards](./docs/governance/)
  - [Team Standards](./docs/governance/standards.md)
  - [Code Review Checklist](./docs/governance/code-review-checklist.md)
  - [Prompt Library Structure](./docs/governance/prompt-library.md)
  - [Monthly Cost Review](./docs/governance/cost-review.md)

- [Quick Reference](./docs/reference/)
  - [Cheat Sheet](./docs/reference/cheat-sheet.md)
  - [Claude Code Commands](./docs/reference/claude-code-commands.md)
  - [Model Pricing & Selection](./docs/reference/pricing.md)
  - [Helpful Links](./docs/reference/links.md)

## 🎯 Core Takeaways

| Principle | Action |
|---|---|
| **Output tokens cost 5× more than input** | Always constrain output with `max_tokens` |
| **Cache everything reusable** | System prompts, static context, coding standards |
| **Match model to task** | Don't use Opus for tasks Haiku can handle |
| **One task per session** | Start fresh chat for each distinct problem |
| **Use structured XML prompts** | Enables consistent, parseable outputs |

## 📞 Support & Maintenance

- **Questions?** Check the relevant stack guide first
- **Found an issue?** Create a GitHub issue with the label `claude-guide`
- **Have a better practice?** Submit a PR with improvements
- **Monthly reviews:** First Friday of each month at 10 AM

---

*Maintained by: Lead DevOps Engineer | Review cycle: Quarterly | Last updated: June 2026*
