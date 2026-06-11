# Claude Teams — Subscription & Usage Limits

**Version:** 2.0 | **Date:** June 2026 | **Classification:** Internal Use Only

---

## 1. How We Access Claude

We use **Claude Teams** — a flat per-seat subscription. No per-message charges, no API tokens, no bills that spike. Every developer logs in at [claude.ai](https://claude.ai) or via the Desktop app with their company email.

| Access Method | Use For |
|---|---|
| Claude Web — [claude.ai](https://claude.ai) | General tasks, Projects, team collaboration |
| Claude Desktop — Mac/Windows app | Same as Web with better OS integration |
| Claude Code — CLI/IDE (VS Code, JetBrains) | In-project development sessions |

All three use the same Teams account. No API key required for any of them.

---

## 2. Usage Limits

Claude Teams has message rate limits that vary by model — not token counts. They reset on a rolling basis.

| Model | Limit Rate | Use For |
|---|---|---|
| **Haiku 4.5** | Least restricted | Simple lookups, classification, quick edits |
| **Sonnet 4.6** | Standard | All standard development work |
| **Opus 4.6** | Most restricted | Architecture decisions, complex debugging only |

If you hit a rate limit, Claude tells you when it resets (typically a few hours). Switch to a less-restricted model in the meantime.

---

## 3. Staying Effective Within Limits

**Preserve Opus quota.** Opus has the tightest limits. Default to Sonnet — escalate to Opus only when the task genuinely needs deep reasoning.

**One task per conversation.** Long conversations accumulate context and degrade quality. Use `/clear` in Claude Code, or start a new chat in Web/Desktop, for each distinct problem.

**Paste only what is relevant.** Fixing one function? Paste the function, not the whole file.

**Set context once.** Use Projects (Web/Desktop) or `CLAUDE.md` (Claude Code) instead of re-explaining the project every session. See [context-persistence.md](../01-prompt-efficiency/context-persistence.md).

---

## 4. Token Scale Reference

Even without per-token billing, this helps you scope prompts and avoid context limit surprises:

| Content | Approximate Tokens |
|---|---|
| 100-line function | 300–500 |
| 500-line PHP class | 1,500–2,500 |
| A full Magento module (5 files) | 3,000–8,000 |
| 100-line error log | 500–1,000 |

Use `/cost` in Claude Code to see session token usage.

---

*See also: [model-selection.md](./model-selection.md) | [context-persistence.md](../01-prompt-efficiency/context-persistence.md)*
