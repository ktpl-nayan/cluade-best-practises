# Threat Model — LLM-Specific Security Risks

**Version:** 1.0 | **Date:** June 2026 | **Owner:** Lead DevOps & AI Ethics Officer
**Classification:** Internal Use Only | **Review Cycle:** Quarterly

---

## 1. Threat Summary

| # | Threat | Likelihood | Impact | Risk | Primary Mitigation |
|---|---|---|---|---|---|
| T-01 | Prompt Injection | Medium | Medium | 🟡 Medium | `<data>` tag separation, sanitise inputs |
| T-02 | Data Exfiltration via Prompt | Low | Medium | 🟢 Low | No secrets in prompts |
| T-03 | Account Credential Compromise | Low-Med | High | 🔴 High | Mandatory 2FA, unique passwords |
| T-04 | Security Code Hallucination | High | Critical | 🔴 Critical | Mandatory human review, SAST |
| T-05 | Malicious AI-Suggested Package | Low-Med | High | 🟡 Medium | Verify packages before installing |
| T-06 | Shared Session Data Exposure | Low | Medium | 🟢 Low | Individual accounts, `/clear` on shared machines |
| T-07 | Rate Limit Exhaustion via Scripts | Low | Medium | 🟢 Low | Use bulk work pattern — no session loops |

---

## 2. Critical Threats — Detail

### T-03: Account Credential Compromise

Attacker gains access to a developer's Claude Teams account via phishing or reused passwords. They can read all conversation history including internal code and architecture context.

**Mitigations:**
- Mandatory 2FA on every account — no exceptions (see [account-management.md](./account-management.md))
- Unique strong password per account — use a password manager
- Compromised? Change password immediately → sign out all sessions → notify AI Ethics Officer
- Offboarding: remove account in Teams admin console on the developer's last day

### T-04: Security Code Hallucination

Claude generates code with security vulnerabilities — missing auth checks, weak crypto, SQL injection — that looks correct but isn't.

```javascript
// Example: Claude generates JWT validation that looks right but doesn't verify signature
const decoded = jwt.decode(token);  // Missing: jwt.verify(token, secret)
```

**Mitigations:**
- Security-critical code requires human review — no exceptions
- Use Opus for security-sensitive tasks
- Run SAST tools (Semgrep, Snyk) on all PRs
- Security code gets additional senior developer review before merge

### T-05: Malicious AI-Suggested Package

Claude recommends a package that doesn't exist or is malicious — attackers register packages matching names Claude commonly hallucinates.

**Mitigations:**
- Always verify packages at npmjs.com / packagist.org before installing
- Check download counts, last publish date, maintainer reputation
- Run `npm audit` / `composer audit` after adding new dependencies
- Add to prompts: "Only recommend packages with >100K weekly downloads"

---

## 3. Lower-Risk Threats

**T-01 Prompt Injection:** Use `<data>` tags to separate instructions from data; treat Claude's output from external data with scepticism.

**T-02 Data Exfiltration:** System prompts must contain only context/standards — no secrets. Restrict Claude Code scope; no `--dangerously-skip-permissions` in workflows.

**T-06 Shared Sessions:** Each developer uses their own account. Use `/clear` at the end of any session on a shared machine.

**T-07 Rate Exhaustion:** Never script a loop of `claude` calls. Use the one-session-per-item bulk work pattern.

---

## 4. Security Review Cadence

| Review | Frequency | Owner |
|---|---|---|
| Pre-commit secret scanning | Every commit | Developer (automated) |
| CI/CD secret scanning | Every pipeline run | DevOps (automated) |
| Claude Teams account access review | Monthly | AI Ethics Officer |
| Threat model review | Quarterly | AI Ethics Officer |

---

*See also: [account-management.md](./account-management.md) | [data-classification.md](./data-classification.md) | [incident-response.md](../02-ai-ethics/incident-response.md)*
