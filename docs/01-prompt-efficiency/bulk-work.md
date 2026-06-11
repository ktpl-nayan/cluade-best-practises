# Handling Bulk & Repetitive Work with Claude

**Version:** 2.0 | **Date:** June 2026 | **Classification:** Internal Use Only

---

## 1. The Core Rule

One session per item. Never try to process everything at once — long sessions degrade quality as context fills up.

```
Task: Review all 12 custom Magento modules for security issues

Wrong:
  "Review all 12 modules and give me a security report"
  → Context overflows, quality degrades, items get skipped

Right:
  Session 1: "Review Krish_Payment — security issues only. JSON output."
  Session 2: "Review Krish_Checkout — security issues only. JSON output."
  ...
  Final:     Paste all results → "Merge these into a prioritised report"
```

Use `/clear` between sessions. Same JSON schema every time so results merge cleanly.

---

## 2. Define Your Output Schema First

Set the format once at the start of the first session — it becomes the contract for all sessions:

```bash
"For all reviews in this bulk task, return JSON in this format:
 [{file, line, severity, category, issue, recommended_fix}]
 severity: CRITICAL | HIGH | MEDIUM | LOW
 category: security | performance | standards | maintainability"
```

This makes it possible to combine all results into a single report at the end.

---

## 3. Claude Code for File-Based Bulk Work

Claude Code can read files directly — no manual copy-paste needed:

```bash
# Review files one at a time, waiting for your "next" signal
"Review each PHP file in app/code/Krish/Payment/ one at a time.
 For each: list security issues only.
 Return JSON: [{file, line, severity, issue, fix}]
 After each file, wait for me to say 'next' before continuing."
```

The `wait for next` pattern keeps you in control — you review each output before proceeding.

---

## 4. Subagents for Parallel Work

Claude Code can run parallel reviews in one session:

```bash
"Use subagents to simultaneously:
 1. Review all PHP observers in app/code/Krish/ for security issues
 2. Review all GraphQL resolvers in app/code/Krish/ for N+1 query patterns
 3. Check all Data Patch files for correct versioning

 Each subagent: JSON [{file, line, severity, issue, fix}]
 Merge results into one prioritised report sorted by severity."
```

---

## 5. Projects for Recurring Bulk Tasks

For tasks you run monthly (security reviews, Liquid template audits), create a dedicated Project so Claude has the right context every time without re-explaining:

1. Create a Project: e.g., `Monthly Security Review — Magento`
2. Set Project instructions: stack, what to check, expected JSON schema
3. Each month: open the Project, paste that month's files

---

## 6. Common Mistakes

| Mistake | Effect | Fix |
|---|---|---|
| Pasting all files at once | Context overflows, items skipped | One file per session or use subagents |
| Different schema per session | Can't merge results | Define JSON format upfront |
| Not saving output between sessions | Work lost on close | Copy to file before closing |
| Starting new task mid-session | Mixed context degrades quality | Always `/clear` before a new bulk task |

---

*See also: [context-persistence.md](./context-persistence.md) | [output-constraints.md](./output-constraints.md) | [claude-code-commands.md](../reference/claude-code-commands.md)*
