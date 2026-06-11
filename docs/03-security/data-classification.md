# Data Classification — AI Usage Security

**Version:** 1.0 | **Date:** June 2026 | **Owner:** Lead DevOps & AI Ethics Officer
**Classification:** Internal Use Only | **Review Cycle:** Quarterly

---

## 1. Classification Tiers

| Tier | Label | Examples | AI Controls |
|---|---|---|---|
| **Tier 0** | Public | Open-source code, public docs | ✅ Send freely |
| **Tier 1** | Internal | Your own code, anonymised logs, config files (no secrets) | ✅ Verify no secrets embedded |
| **Tier 2** | Confidential | Client project code, business logic, unreleased features | ⚠️ Team Lead approval required |
| **Tier 3** | Restricted | Customer PII, payment data, credentials, auth tokens | ❌ Never send — no exceptions |

---

## 2. Classification Decision Tree

```
Is the data publicly available or already published?
    → YES → Tier 0 — send freely

Does it identify a real person?
(name, email, phone, address, ID, IP linked to a person)
    → YES → Tier 3 — DO NOT SEND

Does it contain credentials, keys, or passwords?
    → YES → Tier 3 — DO NOT SEND

Does it contain payment card data?
    → YES → Tier 3 — DO NOT SEND

Is it specific to a client project?
    → YES → Tier 2 — requires Team Lead approval

Is it internal but not client-specific?
    → YES → Tier 1 — check for embedded secrets then send
```

---

## 3. Technical Enforcement

### Pre-commit Secret Detection

```bash
# .git/hooks/pre-commit
#!/bin/bash
set -e
gitleaks protect --staged --verbose
if [ $? -ne 0 ]; then
  echo "❌ Potential secrets detected. Commit blocked."
  exit 1
fi
```

### Pre-Paste Check (Before Copying Logs to Claude)

Run a quick grep scan before pasting any log or data snippet:

```bash
grep -Eo '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' yourfile.txt   # emails
grep -Eo 'sk-[a-zA-Z0-9]{20,}' yourfile.txt                                # API keys
grep -iE 'password\s*[:=]\s*\S+' yourfile.txt                              # inline passwords
```

If any match: scrub before pasting using the checklist in [data-handling.md](../02-ai-ethics/data-handling.md#4-scrubbing-checklist).

### Approved Outbound Domains

For managed/shared workstations, restrict AI tool access to approved domains only:

```
claude.ai         (443/HTTPS)
api.anthropic.com (443/HTTPS)
```

Block all other external AI endpoints (OpenAI, Gemini, Copilot) unless approved by the AI Ethics Officer.

---

## 4. Compliance Mapping

| Regulation | Relevant Data | Control |
|---|---|---|
| **GDPR** | EU customer PII | Tier 3 — never sent to AI |
| **PCI-DSS** | Payment card data | Tier 3 — never sent to AI |
| **HIPAA** | Health records | Tier 3 — never sent to AI |
| **COPPA** | Under-13 user data | Tier 3 — never sent to AI |

---

*See also: [data-handling.md](../02-ai-ethics/data-handling.md) | [activity-monitoring.md](./activity-monitoring.md)*
