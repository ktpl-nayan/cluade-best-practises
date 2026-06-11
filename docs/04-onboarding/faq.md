# Frequently Asked Questions — Claude AI Usage

**Version:** 2.0 | **Date:** June 2026 | **Maintained by:** AI Ethics Officer
**Classification:** Internal Use Only

---

## General

### What tools do we use?

**Claude Teams** — a subscription from Anthropic. Three access methods, all using the same company account login:

| Tool | When to Use |
|---|---|
| **Claude Web** (claude.ai) | General tasks, Projects, team collaboration |
| **Claude Desktop** | Same as Web with native OS integration |
| **Claude Code** (CLI/IDE) | Daily development work in-project |

No API key needed.

### Which model should I use?

```
Simple / repetitive / lookup  → Haiku 4.5   (fast, sufficient)
Standard dev (code, debug)    → Sonnet 4.6  ← default
Complex architecture / audit  → Opus 4.6    (tighter limits — use selectively)
```

Start with Sonnet. Only escalate to Opus if Sonnet can't handle the complexity.

### Does Anthropic use my prompts to train models?

No. Claude Teams conversations are not used to train Anthropic's models. Data is processed on Anthropic's servers and may be retained for trust & safety review. See [Anthropic's Privacy Policy](https://www.anthropic.com/privacy).

---

## Data & Privacy

### Can I paste client code into Claude?

- **Your own internal code** → Yes, scrub embedded secrets first
- **Generic client code** → Yes, with Team Lead approval
- **Client business logic** → Requires written client approval + Team Lead sign-off
- **Code containing customer data** → Never

### I have a log file with errors. Can I paste it?

Yes — after scrubbing:

```
Remove before pasting:
✂  Customer email addresses
✂  IP addresses (replace with 192.0.2.x)
✂  Order IDs / customer IDs
✂  Session tokens / auth tokens
```

See [data-handling.md](../02-ai-ethics/data-handling.md) for the full checklist.

### Can I paste my .env or env.php?

**Never.** These contain API keys, DB passwords, and encryption keys.

Share the structure without values if you need Claude to understand your setup:
```bash
SOME_API_KEY=[set]
DB_PASSWORD=[set]
```

---

## Prompts & Quality

### Why is Claude giving inconsistent outputs?

Without constraints, Claude decides the format. Fix it:

1. **Specify format:** "Return JSON: [{field1, field2}]"
2. **Specify scope:** "Max 5 findings", "This function only"
3. **Add examples:** 2–3 input→output pairs for structured responses

See the [standard prompt template in governance/standards.md](../governance/standards.md#5-code-review-checklist).

### How do I stop Claude from adding explanations I didn't ask for?

```xml
<constraints>
  - Return ONLY [JSON/PHP/code] — no prose
  - No "Here is the code:" preamble
  - Do not restate the task
</constraints>
```

### How do I make Claude change only what I asked, not rewrite everything?

```xml
<constraints>
  - Return ONLY the modified [function/class/section]
  - Do NOT return unchanged code
  - Return only the diff if the change is small
</constraints>
```

### Context feels slow and Claude is "forgetting" instructions. What do I do?

Your context window is filling up:

```
50–75% full → Tighten requests, avoid large file reads
75–90% full → Run /compact
90%+        → /clear and start fresh
```

Check with `/cost` in Claude Code.

### I'm hitting Opus rate limits. What should I do?

Opus has the tightest daily limits. Check if your tasks actually need Opus — most development work (feature dev, bug fixes, code review) runs fine on Sonnet. Save Opus for architecture decisions, deep multi-module debugging, and security audits.

---

## Claude Code

### What do the slash commands do?

| Command | What It Does |
|---|---|
| `/cost` | Session token usage |
| `/compact` | Compress context history |
| `/clear` | Start completely fresh session |
| `/model` | Switch model mid-session |
| `# text` | Add a session note without triggering Claude |

### Can Claude Code edit my files directly?

Yes. Always review changes before accepting. Commit before starting a complex session — easy rollback if needed.

### How do I authenticate? Do I need an API key?

No API key. Claude Code authenticates via your Claude Teams account:

```bash
claude            # first run opens browser login
claude auth login # re-authenticate if expired
```

---

## Getting Help

| Issue | Where to Go |
|---|---|
| Policy question | Slack #ai-governance → AI Ethics Officer |
| Technical question | Slack #dev-tools → Tech Lead |
| Accidentally sent sensitive data | AI Ethics Officer immediately |
| Biased/harmful output | GitHub issue: `ai-bias-report` |
| Account access issue | AI Ethics Officer |
| Propose new AI use case | GitHub issue: `ai-use-case-proposal` |

---

*This FAQ is a living document. Ask in #ai-governance if your question isn't here.*
