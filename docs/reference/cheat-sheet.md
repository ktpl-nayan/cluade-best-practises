# Quick Reference Cheat Sheet

**Claude AI — Claude Teams | All Stacks | Version 2.0 | June 2026**

---

## Model Selection — 30 Seconds

```
Simple / repetitive / lookup?          → Haiku 4.5
Standard dev task (code, debug)?       → Sonnet 4.6   ← default
Complex architecture / deep analysis?  → Opus 4.6     (use selectively — tighter limits)
```

**How to switch:**
- Web / Desktop: model picker top-left of the chat
- Claude Code: `/model claude-sonnet-4-6` (or haiku/opus)

---

## Context Traffic Light

```
0–50%   🟢 Work normally
50–75%  🟡 Tighten requests, avoid large file reads
75–90%  🟠 Run /compact immediately
90%+    🔴 Start a new chat — DO NOT continue
```

---

## Standard Prompt Template

```xml
<system_context>[From CLAUDE.md or Project instructions — set once]</system_context>
<task>[Single specific task — never combine multiple jobs]</task>
<context>[Relevant background for this specific task]</context>
<constraints>
  - Output format: [JSON/PHP/JSX/Liquid/YAML]
  - Return only: [specific files/objects/fields]
  - Max: [N findings / N tokens / N lines]
  - Do not: [add boilerplate / explain / regenerate unchanged code]
</constraints>
<examples>[2–5 input→output pairs for consistent format]</examples>
<data>[Dynamic runtime data: code, errors, logs — scrubbed of PII]</data>
```

---

## Claude Code Commands

```
/cost      → Check session token usage (use before heavy operations)
/compact   → Compress context history (use at 75% full)
/clear     → Fresh context for a new, unrelated task
/model     → Switch model mid-session
/review    → Code review current working files
# note     → Add a session note without triggering Claude
```

---

## Do / Don't

| DO | DON'T |
|---|---|
| One task per conversation | Pile unrelated problems into one session |
| Use Projects and CLAUDE.md for context | Re-explain the project every session |
| Use Haiku for simple/repetitive tasks | Default everything to Opus |
| Scope requests to one file at a time | Ask Claude to review entire codebases |
| Add AI disclosure to PR description | Commit AI-generated code without disclosure |
| Scrub data before pasting logs | Send raw logs with customer emails or IPs |
| Use structured output constraints | Expect consistent format without asking for it |
| Include 2–5 examples for structured output | Ask for JSON without showing the expected structure |
| Start new chat when context hits 75% | Keep building context until quality degrades |

---

## Data — Quick Go / No-Go

```
SEND:    Your own code, anonymised logs, error messages (scrubbed),
         generic schemas, open-source patterns

SCRUB:   Production logs (remove IPs, emails, IDs), client code (approval needed)

NEVER:   Customer PII, passwords, API keys, .env files,
         database dumps with real data, session tokens
```

---

## Context Persistence — Set Once, Reuse Always

| Tool | Mechanism | How |
|---|---|---|
| Claude Code | `CLAUDE.md` in project root | Create file → auto-loaded every session |
| Claude Web / Desktop | Project instructions | New Project → Set project instructions |

Never re-explain the stack in every chat. Set it up in CLAUDE.md or a Project — then it is always there.

---

## Bulk Work Pattern

```
Wrong:  "Review all 15 modules and give me a security report"
Right:  One module per session. Same output format every time.
        Combine at the end: paste all results → "Merge these into a prioritised report"
```

---

## PR Disclosure — Required

```markdown
## AI Assistance
- Tool used: Claude [model] via [Web / Desktop / Claude Code]
- Scope: [What Claude helped with]
- Human review: Confirmed — reviewed, tested, validated
```

---

## Account & Access

```
Login:          claude.ai with company email
2FA:            Mandatory — set up in Settings → Account → Security
Compromised?:   Change password → sign out all devices → notify AI Ethics Officer
Leaving team:   AI Ethics Officer removes account on last day
```

---

## If Something Goes Wrong

```
Sent sensitive data →    Stop. Contact AI Ethics Officer immediately. #ai-governance
Harmful AI output  →     Don't deploy. GitHub issue: ai-bias-report
Account issues     →     Change password, sign out all sessions, contact Ethics Officer
Service down       →     Check status.anthropic.com. Defer AI tasks until restored.
```

---

*Full documentation: see /docs/ | Questions: Slack #ai-governance*
