# Prompting for Concise, Structured Output

**Version:** 2.0 | **Date:** June 2026 | **Classification:** Internal Use Only

---

## 1. The Problem

Without constraints, Claude chooses its own format and length — verbose, inconsistent, hard to use in downstream tasks.

```
Wrong:  "Review this code and tell me what's wrong."

Right:  "Review this code. Return JSON only: [{file, line, severity, issue, fix}]
         Max 5 findings. severity: CRITICAL | HIGH | MEDIUM | LOW. No prose."
```

---

## 2. Always Specify Three Things

1. **Format** — JSON, PHP, markdown table, unified diff, numbered list
2. **Scope** — max N findings, this function only, top 3 issues
3. **Exclusions** — no explanation, no preamble, no boilerplate

---

## 3. Format Directives

```
JSON:
  "Return JSON only: [{file, line, severity, issue, fix}]
   No markdown fencing. No explanation."

Code:
  "Return only the PHP class.
   No comments on standard patterns.
   Fenced block with file path as first comment."

Table:
  "Return a markdown table only. Columns: [Module, Purpose, Status]
   No introductory text."

Diff:
  "Return a unified diff only.
   Changed lines ± 3 context lines. No explanation."
```

---

## 4. Length Control

```
"Max 5 findings"
"In 3 bullet points"
"One sentence summary only"
"Return only the modified function, not the full class"
```

---

## 5. Anti-Patterns

| Instead of… | Use… |
|---|---|
| "Tell me what you think about..." | "Return JSON: [{issue, severity, fix}]" |
| "Explain how to fix this error" | "Return only the corrected code. No explanation." |
| "Review my code" | "Return JSON findings. Top 5 by severity." |
| "What would you do differently?" | "List max 3 changes: [{change, reason, impact}]" |

---

## 6. Session-Level Constraints

Set output rules at the start of a conversation and they apply for the whole session:

```
"For this session:
 - Return code only, never explain what it does
 - All findings in JSON format
 - No 'Here is...' preambles
 - Ask for clarification instead of assuming"
```

---

*See also: [context-persistence.md](./context-persistence.md) | [bulk-work.md](./bulk-work.md)*
