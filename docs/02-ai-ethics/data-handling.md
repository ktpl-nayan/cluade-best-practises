# Data Handling & Classification for AI Tools

**Version:** 1.0 | **Date:** June 2026 | **Owner:** Lead DevOps & AI Ethics Officer
**Classification:** Internal Use Only | **Review Cycle:** Quarterly

---

## 1. Overview

Every message sent to Claude — via Web, Desktop, or Claude Code — is processed by Anthropic's servers. This document defines what is safe to send, what must be scrubbed first, and what is strictly forbidden.

> **Sending prohibited data is a policy violation and may constitute a GDPR breach, regardless of intent. When in doubt, anonymise or do not send.**

---

## 2. Data Classification

| Tier | Label | Examples |
|---|---|---|
| **Tier 0** | Public | Open-source code, public docs, marketing copy |
| **Tier 1** | Internal | Your own code snippets, anonymised error logs, stack configs |
| **Tier 2** | Confidential | Client project code, business logic, internal architecture |
| **Tier 3** | Restricted | Customer PII, payment data, credentials, auth tokens |

---

## 3. Go / No-Go Matrix

| Data Type | Send? |
|---|---|
| Generic code you wrote | ✅ Yes |
| Error logs (anonymised) | ✅ Yes — remove IPs, user IDs first |
| Your own module/component code | ✅ Yes |
| Client-specific business logic | ⚠️ Conditional — written client approval required |
| Customer emails, names, addresses | ❌ Never |
| Payment card / financial data | ❌ Never — PCI-DSS violation |
| API keys, passwords, tokens, JWTs | ❌ Never |
| `env.php` / `.env` files | ❌ Never |
| Database dumps with real data | ❌ Never |
| Production IP addresses | ❌ Never — use placeholder |

---

## 4. Scrubbing Checklist

Apply before sending any real data to Claude:

```
[ ] Replace customer names with "Customer_001"
[ ] Replace emails with "user@example.com"
[ ] Replace IPs with "192.0.2.x"
[ ] Replace API keys / tokens with "REDACTED_KEY"
[ ] Replace passwords with "REDACTED_PASSWORD"
[ ] Replace order IDs with fake sequential IDs (ORD-0001)
[ ] Replace real domain names with "example.com" if client-specific
```

---

## 5. Stack-Specific Rules

### Magento

| Data | Safe? |
|---|---|
| Module PHP code (your own) | ✅ |
| GraphQL queries and resolvers | ✅ |
| `exception.log` / `var/log/` files | ⚠️ Scrub IPs, emails, order IDs |
| `env.php` | ❌ Never — contains DB passwords + crypt key |
| Database dump snippets | ❌ Use synthetic data instead |
| Customer order data | ❌ Use Magento sample data |

### MERN

| Data | Safe? |
|---|---|
| React components, Express routes, Mongoose schemas | ✅ |
| MongoDB query examples (anonymised values) | ✅ |
| JWT token values | ❌ Replace with `REDACTED_JWT` |
| `.env` contents | ❌ Never |
| Server logs with user data | ⚠️ Anonymise user IDs, emails |

### Shopify

| Data | Safe? |
|---|---|
| Liquid templates, GraphQL queries, Remix app code | ✅ |
| `config/settings_data.json` | ⚠️ Check for embedded API keys |
| Customer metafield values | ❌ Use synthetic values |
| Shopify Partner API credentials | ❌ Never |
| Order webhook payloads with real data | ❌ Use Shopify's sample payloads |

---

## 6. Production Incidents

Under time pressure, scrubbing often gets skipped. Follow this order:

```
1. CAPTURE — grab the relevant log/data
2. SCRUB   — apply checklist above
3. VERIFY  — manually check before pasting
4. SEND    — paste the scrubbed version
5. LOG     — note AI assistance in the incident record
```

---

## 7. Anthropic's Data Usage

For Claude Teams (as of June 2026):
- Your conversations are **not used to train Anthropic's models**
- Data is processed on Anthropic's servers (US-based by default)
- Conversations may be retained for trust & safety review

See current terms at [claude.ai/settings/team](https://claude.ai/settings/team).

---

*If a data breach occurs: [incident-response.md](./incident-response.md)*
