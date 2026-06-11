# Model Selection Strategy

**Version:** 2.0 | **Date:** June 2026 | **Classification:** Internal Use Only

---

## 1. Decision Framework

```
1. Deep reasoning, multi-step analysis, complex architecture?
   YES → Opus 4.6

2. Standard development work (code, debug, review, generate)?
   YES → Sonnet 4.6  ← default

3. Simple, repetitive, or lookup task?
   YES → Haiku 4.5
```

**How to switch:**
- **Web / Desktop:** Model picker top-left of the chat
- **Claude Code:** `/model claude-opus-4-6` (or haiku/sonnet)

---

## 2. Decision Matrix

| Task | Model |
|---|---|
| Simple lookups, classification, boilerplate | **Haiku 4.5** |
| Feature development, bug fixing, code review | **Sonnet 4.6** |
| GraphQL / complex queries, debugging | **Sonnet 4.6** |
| Writing technical documentation | **Sonnet 4.6** |
| Architecture design, security audit | **Opus 4.6** |
| Complex multi-module debugging | **Opus 4.6** |
| Upgrade planning, migration strategy | **Opus 4.6** |

---

## 3. Rules

**Don't use Opus "just to be safe."** For well-defined coding tasks, Sonnet produces equivalent quality and has more generous daily limits. Only use Opus when the task genuinely requires deep reasoning.

**If Sonnet "got confused," fix the prompt first.** Confusion is usually a prompt engineering problem, not a model capability problem. Add more context or clearer constraints before escalating models.

**Switch models mid-session** without losing context (Claude Code):

```bash
/model claude-opus-4-6    # For architecture planning
/model claude-sonnet-4-6  # Drop back for implementation
/model claude-haiku-4-5   # Quick lookups
```

Use `/cost` to check session token usage — a high count is a signal to start fresh with `/clear`.

---

*See also: [subscription.md](./subscription.md) | [output-constraints.md](../01-prompt-efficiency/output-constraints.md)*
