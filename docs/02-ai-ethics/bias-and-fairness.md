# Bias, Fairness & Output Review Guidelines

**Version:** 1.0 | **Date:** June 2026 | **Owner:** Lead DevOps & AI Ethics Officer
**Classification:** Internal Use Only | **Review Cycle:** Quarterly

---

## 1. Bias Categories to Watch For

### Technical Bias

| Manifestation | Example | Fix |
|---|---|---|
| Stack preference | Suggests React when Vue is your standard | Always specify stack in `<system_context>` |
| Recency bias | Recommends deprecated APIs | Always pin API versions in prompts |
| Western defaults | Assumes USD, LTR text, US date format | State locale requirements explicitly |

### Language & Inclusivity

Review all generated documentation and comments for:
- Non-inclusive terms → use `allowlist/denylist`, `primary/replica`, `main` branch, "they/their"
- Gender-specific pronouns in generic examples → use "they" or rephrase

| Avoid | Use Instead |
|---|---|
| `master` branch | `main` |
| `whitelist` / `blacklist` | `allowlist` / `denylist` |
| `master` / `slave` (DB) | `primary` / `replica` |
| `sanity check` | `confidence check` / `validation` |

### Hallucination (Confident Errors)

Claude produces incorrect outputs with high confidence. Most dangerous in: security recommendations, API version-specific code, performance benchmarks. Always verify independently.

---

## 2. Mandatory Human Review Gates

These output types **must** have explicit human review before use:

| Output Type | Required Reviewer |
|---|---|
| Security code (auth, encryption) | Senior Dev + Tech Lead |
| Architecture recommendations | Tech Lead |
| Data migration scripts | Senior Dev + DevOps |
| Customer-facing copy | Developer + Product Owner |
| Performance benchmarks | Developer — run actual benchmarks |
| Legal or compliance statements | Ethics Officer + Legal |

---

## 3. Output Review Checklist

```
[ ] ACCURACY:    Independently verified key facts?
[ ] COMPLETENESS: Covers edge cases, not just happy path?
[ ] ASSUMPTIONS: Any unvalidated assumptions?
[ ] LANGUAGE:    Inclusive, professional language?
[ ] TECHNOLOGY:  Correct API versions for our stack?
[ ] SECURITY:    Follows security best practices?
[ ] ATTRIBUTION: AI assistance noted in PR?
```

---

## 4. Known Failure Modes

| Stack | Task | Known Issue | Mitigation |
|---|---|---|---|
| Magento | DB schema generation | Generates pre-2.4 InstallSchema instead of Data Patch | Specify "Magento 2.4.6+ Data Patch pattern" |
| MERN | Auth middleware | JWT handling that misses refresh token invalidation | Add "include refresh token invalidation" to constraints |
| Shopify | Liquid templating | References deprecated pre-2022 filters | Specify "Dawn 10+, 2025-01 API" in system prompt |
| All | Performance estimates | AI-stated numbers are illustrative, not measured | Never use without running actual benchmarks |

---

## 5. Reporting Biased or Harmful Output

1. **Do not deploy it**
2. Save the full prompt and output (scrub sensitive data first)
3. Report via GitHub Issue tagged `ai-bias-report`
4. AI Ethics Officer reviews within 24 hours
5. Severe cases: pattern added to Known Failure Modes above; system prompts updated

---

*See also: [incident-response.md](./incident-response.md) | [transparency.md](./transparency.md)*
