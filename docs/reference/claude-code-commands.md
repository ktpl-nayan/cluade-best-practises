# Claude Code Commands Reference

**Version:** 2.0 | **Date:** June 2026 | **Classification:** Internal Use Only

---

## Installation & Auth

```bash
npm install -g @anthropic-ai/claude-code
claude --version

# First run — opens browser login automatically
cd /path/to/project && claude
# Log in with company email → approve → return to terminal

# Re-authenticate
claude auth login

# Sign out
claude auth logout
```

No API key required. Auth stored in local keychain.

---

## Session Commands

| Command | Description | When to Use |
|---|---|---|
| `/cost` | Session token usage | Before heavy operations; check context level |
| `/compact` | Compress conversation history | When context hits 50–75% |
| `/clear` | Wipe all context — fresh session | Between unrelated tasks |
| `/model [name]` | Switch model mid-session | Escalate to Opus; drop to Haiku for lookups |
| `# text` | Add a note without triggering Claude | Bookmark info in the session |

---

## Model Names

```bash
/model claude-haiku-4-5      # Fast — simple tasks, classification
/model claude-sonnet-4-6     # Default — all standard dev work
/model claude-opus-4-6       # Deep reasoning — complex architecture, security audits
```

---

## Scoped File Work

Always scope to the relevant area — reading entire codebases fills context and degrades quality:

```bash
# Correct
"Review app/code/Krish/Payment/Observer/ for security issues. JSON findings."

# Wrong
"Review our entire codebase for security issues."
```

---

## Piping Data

Scrub before piping — see [data-handling.md](../02-ai-ethics/data-handling.md).

```bash
# Pipe log file for error analysis
tail -n 100 var/log/exception.log | claude \
  "Classify errors by type and frequency.
   Return JSON: [{error_type, count, fix}]"

# Pipe test output
npm test 2>&1 | claude \
  "Identify failing tests and root causes.
   Return JSON: [{test_name, file, cause}]"

# Pre-PR review
git diff main | claude \
  "Review for security issues and breaking changes.
   Return JSON: [{severity, file, line, issue, fix}]"
```

---

## CLAUDE.md

`CLAUDE.md` in the project root loads automatically every session.

```bash
ls CLAUDE.md                              # check if it exists
claude && "What project context do you have loaded?"  # verify it loaded
/cost                                     # token count includes CLAUDE.md
```

**Minimal template:**

```markdown
# [Project Name]

## Stack
[Magento 2.4.7-p3 | PHP 8.2 | OpenSearch 2.11]

## Structure
[Key directories and their purpose]

## Standards
[Coding rules]

## Forbidden
[Anti-patterns]

## Useful Commands
[Key CLI commands]
```

---

## Context Management

```bash
/cost                     # check level at any point
/compact && "Continue"    # quality dropping — compact first
/clear                    # starting a different task — always clear
```

---

## Subagents for Parallel Work

```bash
"Use subagents to simultaneously:
 1. Review all PHP observers in app/code/Krish/ for security issues
 2. Review all GraphQL resolvers for N+1 patterns
 3. Check Data Patch files for correct versioning

 Each subagent: JSON [{file, line, severity, issue, fix}]
 Merge into one prioritised report by severity."
```

---

## Troubleshooting

| Issue | Likely Cause | Fix |
|---|---|---|
| Claude ignores earlier instructions | Context too full | `/compact` or `/clear` |
| Outputs getting shorter / vaguer | Context degradation | `/compact` immediately |
| Auth error on startup | Session expired | `claude auth login` |
| `rate_limit_error` | Model quota hit | Switch to Sonnet if on Opus; wait for reset |
| `overloaded_error` | Anthropic service busy | Check status.anthropic.com; retry in minutes |
| High context from start | Large CLAUDE.md | Trim CLAUDE.md to essentials |

---

*Full docs: [docs.anthropic.com/en/docs/claude-code](https://docs.anthropic.com/en/docs/claude-code/overview)*
