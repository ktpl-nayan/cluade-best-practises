# New Developer Onboarding Guide — Claude AI

**Version:** 2.0 | **Date:** June 2026 | **Classification:** Internal Use Only

---

## Welcome

Get set up and using Claude safely within your first day. Read in order.

**By the end:** You have access to all three Claude tools, understand data safety rules, can write effective prompts, and know what to do if something goes wrong.

---

## Step 1: Read the Policy First (10 min)

1. **[AI Acceptable Use Policy](../02-ai-ethics/policy.md)** — what you can and cannot use Claude for. Non-compliance has consequences.
2. **[Data Handling Guide](../02-ai-ethics/data-handling.md)** — what data is safe to send. This is where most mistakes happen.

> **Critical rule: Never send real customer data, API keys, passwords, or PII to Claude. If in doubt, anonymise first. Still unsure — ask the AI Ethics Officer.**

---

## Step 2: Get Account Access (5 min)

1. Request access from your Team Lead or AI Ethics Officer
2. Accept the email invitation and create your password (minimum 16 characters, use a password manager)
3. **Enable 2FA immediately:** [claude.ai](https://claude.ai) → Settings → Account → Security → Enable 2FA (use an authenticator app — not SMS)
4. Log in to confirm access

Your account is personal. Do not share it.

---

## Step 3: Set Up Your Tools (10 min)

| Tool | Setup |
|---|---|
| **Claude Web** | Go to [claude.ai](https://claude.ai) — ready to use |
| **Claude Desktop** | Download at [claude.ai/download](https://claude.ai/download) → install → log in |
| **Claude Code** | See below |

**Claude Code (recommended for daily development):**

```bash
npm install -g @anthropic-ai/claude-code
claude --version                        # verify
cd /path/to/your-project
claude                                  # first run opens browser login
```

Log in with your company email. Authentication is stored locally — no re-auth each session.

**VS Code / JetBrains:** Install the Claude Code extension from the marketplace, sign in when prompted.

---

## Step 4: Your First Session (10 min)

```bash
cd /path/to/your-project
claude

# Test connection
"What is the purpose of CLAUDE.md in this project?"

# Check context usage
/cost

# Test a scoped task for your stack:
# Magento: "List custom modules in app/code/ as a markdown table. Max 300 tokens."
# MERN:    "List Express routes in server/src/routes/ as a markdown table. Max 300 tokens."
# Shopify: "List custom sections in theme/sections/ as a markdown table. Max 300 tokens."

# Compact heavy context
/compact

# Start fresh for a new task
/clear
```

**Build this habit from day one:** use `/clear` between different tasks. Never pile unrelated problems into one session.

---

## Step 5: Five Rules to Internalise

```
1. ONE TASK PER CONVERSATION
   /clear (Code) or new chat (Web/Desktop) for each new problem.

2. MATCH MODEL TO TASK
   Simple → Haiku.  Standard dev → Sonnet.  Complex architecture → Opus.

3. USE PROJECTS AND CLAUDE.md
   Set project context once. Don't re-explain the stack every session.

4. PASTE ONLY WHAT IS RELEVANT
   Fixing one function? Paste the function, not the whole file.

5. NEVER SEND SENSITIVE DATA
   PII, passwords, API keys, .env files, customer data — never.
```

**PR disclosure rule** — every PR where Claude helped must include:

```markdown
## AI Assistance
- Tool used: Claude Sonnet 4.6 via Claude Code
- Scope: [What Claude helped with]
- Human review: Confirmed — reviewed, tested, validated
```

---

## Step 6: Set Up Project Context

**Claude Code:** Check if your project has a `CLAUDE.md`:
- If yes → read it carefully; it has project-specific rules
- If no → create one using the template from your stack guide

**Claude Web/Desktop:** Ask your Team Lead if a Project exists for the workstream. If yes, open it before starting work.

See [context-persistence.md](../01-prompt-efficiency/context-persistence.md) for templates.

---

## Step 7: When Something Goes Wrong

**Accidentally sent sensitive data:**
1. Stop immediately — do not try to fix it quietly
2. Note what was sent and when
3. Contact AI Ethics Officer immediately → Slack #ai-governance
4. See [Incident Response Playbook](../02-ai-ethics/incident-response.md)

**Claude gave wrong or dangerous output:** Do not deploy it. File GitHub issue tagged `ai-bias-report`. If security-related: notify Tech Lead immediately.

**Unsure if data is safe to send:** Ask in #ai-governance. The cost of asking is zero.

---

## Readiness Checklist

```
[ ] Read Acceptable Use Policy
[ ] Read Data Handling Guide
[ ] Claude account active — can log into claude.ai
[ ] 2FA enabled
[ ] Claude Code installed and authenticated (claude --version works)
[ ] Ran test session: /cost, /model, /compact, /clear
[ ] Know if project has CLAUDE.md or Team Project and have read it
[ ] Know what AI Assistance section in a PR must contain
[ ] Know what to do if sensitive data is accidentally sent
```

---

*Questions: Slack #ai-governance (policy) | #dev-tools (technical)*
