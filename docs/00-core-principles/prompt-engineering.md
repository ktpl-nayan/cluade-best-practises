# Prompt Engineering Standards

**Version:** 2.0 | **Date:** June 2026 | **Classification:** Internal Use Only

---

## 1. The Standard Prompt Template

Use this structure for all Claude prompts across all three stacks. Consistent structure produces predictable, reusable outputs.

```xml
<system_context>
  You are a senior [Magento/MERN/Shopify] developer at Krish TechnoLabs.
  [Stack-specific expertise — see docs/stacks/ for each stack's full template]
  Always respond in the format specified in <constraints>.
  Never add explanations unless explicitly asked.
</system_context>

<task>
  [Single, specific task. One task per prompt — never combine multiple jobs.]
</task>

<context>
  [Relevant background: project name, relevant files, current behaviour]
</context>

<constraints>
  - Output format: [JSON / PHP / JSX / Liquid / YAML]
  - Scope: [specific files or modules only]
  - Max: [N findings / N lines]
  - Do not: [regenerate unchanged code / explain / add boilerplate]
</constraints>

<examples>
  [2–5 input → output pairs showing the exact format you want]
</examples>

<data>
  [Runtime data: code snippet, error log, API response — paste here]
</data>
```

> Put `<system_context>`, `<constraints>`, and `<examples>` in your CLAUDE.md or Project instructions — they stay the same across sessions. Only `<task>` and `<data>` change per conversation.

---

## 2. XML Tag Reference

| Tag | Changes how often | Purpose |
|---|---|---|
| `<system_context>` | Once per project | Role, expertise, output style |
| `<task>` | Every prompt | The single specific job |
| `<context>` | Rarely | Project background, architecture notes |
| `<constraints>` | Rarely | Output format, scope, length limits |
| `<examples>` | Rarely | Input/output demonstrations |
| `<data>` | Every prompt | Runtime data: code, errors, logs |
| `<thinking>` | As needed | Extended reasoning — Opus only |

---

## 3. Always Include Examples for Structured Output

Without examples, Claude decides the format. With 2–5 examples, outputs are consistent and parseable every time.

```xml
<examples>
  <example>
    <input>Function: calculateTax(price, rate) throws TypeError on null input</input>
    <output>{"file": "utils/tax.js", "line": 12, "issue": "Missing null guard", "fix": "if (!price || !rate) return 0;"}</output>
  </example>
  <example>
    <input>SQL query runs in 8s on products table (2M rows)</input>
    <output>{"file": "Model/ResourceModel/Product.php", "line": 45, "issue": "Missing index on sku", "fix": "ALTER TABLE catalog_product_entity ADD INDEX idx_sku (sku);"}</output>
  </example>
</examples>
```

---

## 4. Scope Constraints by Task Type

| Task | What to specify |
|---|---|
| Classification / lookup | `"Return a one-word answer only"` |
| Single function fix | `"Return the modified function only. No surrounding code."` |
| Full class / component | `"Return the full class. No explanation."` |
| Code review | `"Return JSON: [{file, line, severity, issue, fix}]. Max 10 items."` |
| Architecture review | `"Return JSON: [{concern, recommendation, priority}]. Max 5 items."` |

---

## 5. Universal Pre-PR Review Prompt

Use this before every PR regardless of stack:

```xml
<task>
Pre-PR code review. Check for:
1. Security: injection vulnerabilities, exposed credentials, missing auth checks
2. Performance: N+1 queries, missing caches, unoptimised loops
3. Standards: violations of our coding standards
4. Edge cases: null handling, empty arrays, network failure scenarios
</task>

<constraints>
  Return JSON: [{severity, category, file, line, issue, recommended_fix}]
  severity: CRITICAL | HIGH | MEDIUM | LOW
  Only CRITICAL and HIGH block merge
  Max 15 findings
</constraints>
```

---

*Stack-specific prompt templates: [magento.md](../stacks/magento.md) | [mern.md](../stacks/mern.md) | [shopify.md](../stacks/shopify.md)*  
*See also: [model-selection.md](./model-selection.md) | [output-constraints.md](../01-prompt-efficiency/output-constraints.md)*
