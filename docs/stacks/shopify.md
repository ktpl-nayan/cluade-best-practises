# Shopify Development — Claude Workflow

**Version:** 2.0 | **Date:** June 2026 | **Classification:** Internal Use Only

---

## System Prompt

Store this in `/ai-prompts/shopify/system-prompt-claude.md` and add it to your CLAUDE.md or Project instructions so it loads every session automatically.

```
You are a senior Shopify developer at Krish TechnoLabs.
You have deep expertise in:
- Shopify Theme development: Dawn, Liquid templating, CSS/JS customisation
- Shopify Hydrogen / Remix: React-based headless storefronts
- Shopify CLI and theme development workflow
- GraphQL Admin API and Storefront API (2025-01+)
- Shopify App development: Remix app framework, App Bridge
- Metafields, Metaobjects, and custom data modelling
- Shopify Flow: automation triggers, conditions, actions
- Multi-store, multi-currency, multi-language configurations

Always use the latest stable API version (2025-01 or newer).
Prefer GraphQL over REST for all new integrations.
Return only requested code. No boilerplate explanation.
```

---

## Task Patterns

### Liquid Section

```xml
<task>Create a Liquid section for a featured collection with configurable heading, product count (4/8/12), and layout toggle (grid/list).</task>

<constraints>
  - Dawn 10+ compatible
  - Schema settings: heading (text), product_count (select), layout (radio)
  - Accessible: proper heading hierarchy, aria-labels on buttons
  - Return: section Liquid file only
  - No CSS file — use Dawn utility classes only
</constraints>
```

### Storefront API — GraphQL

```xml
<task>Write a Shopify Storefront API GraphQL query to fetch a product by handle including: all variants with availability, metafields (warranty_info, care_instructions), and first 5 reviews via Judge.me metaobject.</task>

<constraints>
  - API version: 2025-01
  - TypeScript typed query result interface
  - Return query string + TypeScript interface only
</constraints>
```

### Admin API — Bulk Operation

```xml
<task>Write a Shopify Admin API bulk operation query to export all products with their variants, inventory levels across all locations, and metafields.</task>

<constraints>
  - Use bulkOperationRunQuery mutation
  - Return: mutation string + polling query + JSONL parser function
  - TypeScript
</constraints>
```

### Remix App Loader

```xml
<task>Create a Remix loader function for a Shopify app page that fetches the 50 most recent orders with customer email, total price, and fulfillment status.</task>

<constraints>
  - Use @shopify/shopify-app-remix authenticate.admin pattern
  - TypeScript, typed return value
  - Handle API errors with proper HTTP status codes
  - Return loader function only — no UI component
</constraints>
```

### Shopify Flow Automation

```xml
<task>Design a Shopify Flow automation that: triggers when an order is tagged 'wholesale', applies a 10% discount to all line items, sends a webhook to our ERP, and tags the customer as 'wholesale-buyer' if not already tagged.</task>

<constraints>
  - Return Flow configuration as structured JSON
  - Include: trigger, conditions, actions in sequence
  - Note any limitations requiring custom app vs native Flow
</constraints>
```

### Code Review

```xml
<task>Review this Shopify Liquid template for: performance issues, accessibility violations, deprecated APIs, and security problems.</task>

<constraints>
  - Return JSON: [{file, line, severity, category, issue, fix}]
  - severity: CRITICAL | HIGH | MEDIUM | LOW
  - category: performance | accessibility | deprecated_api | security | best_practice
  - Flag: synchronous JS in <head>, missing alt attributes, render-blocking resources
  - Max 10 findings
</constraints>

<data>
  [Paste Liquid template here]
</data>
```

---

## Bulk Tasks

One session per template/file, consistent output format, combine at the end. See [bulk-work.md](../01-prompt-efficiency/bulk-work.md) for the full pattern.

| Task | Approach |
|---|---|
| Audit all Liquid templates for deprecated filters | One Claude Code session per template directory |
| Generate metafield definitions for product catalogue | One session per product type |
| Write SEO meta descriptions for collections | One session per group — define format upfront |
| Review all custom app webhook handlers | One session per handler file |

---

## CLAUDE.md Template

Place this at the root of your Shopify project.

```markdown
# [Project Name] — Shopify

## Stack
Shopify Plus | Theme: Dawn 14.x | App: Remix (Shopify App Framework)
API Version: 2025-01 (upgrade quarterly)

## Structure
/theme    → Dawn-based theme (Shopify CLI managed)
/app      → Remix Shopify app
/scripts  → Node.js utility scripts for bulk operations

## Standards
- GraphQL over REST for all new API calls
- API version: always 2025-01 — upgrade quarterly
- No jQuery — vanilla JS only
- No inline <script> in sections — use assets/section-name.js
- Metafields defined in code (app), not admin UI
- Section schema required for all new sections

## Forbidden
- Editing Dawn core files — extend via theme overrides only
- REST API for orders/products in new code — use GraphQL
- Hardcoded shop domain — use Liquid {{ shop.domain }}
- Script Editor — deprecated, use Shopify Functions

## Key Commands
shopify theme dev
shopify theme push
shopify theme check
```

---

## Claude Code Project Setup

The full project structure for a Shopify project with Claude Code configured:

```
my-shopify-project/
├── CLAUDE.md                        ← System prompt (template above)
├── .claude/
│   ├── settings.json                ← Permissions, hooks, env vars
│   └── commands/
│       ├── review.md                ← /review  — pre-PR review
│       ├── section.md               ← /section — scaffold Liquid section with schema
│       └── check.md                 ← /check   — run theme check and summarise
└── .mcp.json                        ← MCP server integrations
```

### `.claude/settings.json`

```json
{
  "permissions": {
    "allow": [
      "Bash(shopify:*)",
      "Bash(npm:*)",
      "Bash(node:*)",
      "Read(**)",
      "Edit(theme/sections/**)",
      "Edit(theme/snippets/**)",
      "Edit(theme/assets/**)",
      "Edit(theme/layout/**)",
      "Edit(theme/templates/**)",
      "Edit(app/**)"
    ],
    "deny": [
      "Edit(node_modules/**)",
      "Edit(theme/config/settings_data.json)",
      "Edit(.env*)",
      "Bash(rm -rf:*)"
    ]
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "echo $CLAUDE_TOOL_INPUT_path | grep -q '\\.liquid$' && shopify theme check $CLAUDE_TOOL_INPUT_path 2>&1 | tail -10 || exit 0"
          }
        ]
      }
    ]
  },
  "env": {
    "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE": "60"
  }
}
```

**What the hooks do:**
- **PostToolUse → Edit**: Runs `shopify theme check` automatically after Claude edits any `.liquid` file — accessibility and deprecation warnings appear immediately in the session
- **`settings_data.json` denied**: Blocks Claude from touching runtime theme settings — changes here affect live store instantly and should only be made through the Shopify admin

### `.claude/commands/`

**`.claude/commands/review.md`** — type `/review`:

```markdown
Review the git-staged changes in this Shopify project.

Check for:
1. Liquid: deprecated filters (| money_with_currency without format), missing escape filters on user input
2. Performance: synchronous JS in <head>, render-blocking resources, large images without lazy loading
3. Accessibility: missing alt attributes, unlabelled form fields, missing aria roles on interactive elements
4. API: deprecated REST endpoints that should use GraphQL, hardcoded API versions older than 2024-10
5. Security: exposed Storefront API tokens in JS, hardcoded shop domains, unescaped output variables

Return JSON only:
[{"file": "", "line": 0, "severity": "CRITICAL|HIGH|MEDIUM|LOW", "category": "liquid|performance|accessibility|api|security", "issue": "", "fix": ""}]

Order by severity. Max 15 findings. No prose.
```

**`.claude/commands/section.md`** — type `/section SectionName`:

```markdown
Scaffold a new Shopify Liquid section for: $ARGUMENTS

Requirements:
- Dawn 14+ compatible
- Full {% schema %} block with: name, tag, class, settings array, presets
- At least one heading setting (type: text) and one colour scheme setting
- Accessible markup: correct heading hierarchy, aria-labels on interactive elements
- No inline <style> — use Dawn CSS custom properties only
- No inline <script> — reference assets/{section-name}.js if JS is needed
- File path: theme/sections/{section-name}.liquid

Return the Liquid file only with the file path as the first comment.
```

**`.claude/commands/check.md`** — type `/check`:

```markdown
Run a theme check on all Liquid files in theme/sections/ and theme/snippets/.

Command to run: shopify theme check theme/

Summarise the output as:
- Total errors vs warnings
- Top 3 most common issue types
- Files with CRITICAL errors listed individually
- Recommended fix order (highest impact first)

Return as a markdown summary. No raw theme check output.
```

### `.mcp.json`

```json
{
  "mcpServers": {
    "filesystem": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "./theme/sections",
        "./theme/snippets",
        "./theme/assets",
        "./app"
      ]
    },
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**What each server enables:**

| MCP Server | What Claude can do |
|---|---|
| `filesystem` | Read sections, snippets, assets directly — no file pasting needed |
| `github` | Fetch PR diffs, review open PRs, comment on reviews |

**Note on Shopify Admin API access:** Shopify provides an official MCP server for Admin API access. When available in your setup, it enables Claude to query products, orders, and metafields directly. Check [Shopify's developer MCP documentation](https://shopify.dev/docs/apps/build/cli-for-apps) for the current package and setup.

**Install the packages:**
```bash
npm install -g @modelcontextprotocol/server-filesystem
npm install -g @anthropic/mcp-github
```

> **Security note:** Never put `SHOPIFY_ACCESS_TOKEN` or Storefront API tokens in `.mcp.json` directly. Use environment variable references and add `.mcp.json` to `.gitignore` if it contains any env references pointing to real credentials.

---

*See also: [prompt-engineering.md](../00-core-principles/prompt-engineering.md) | [model-selection.md](../00-core-principles/model-selection.md) | [context-persistence.md](../01-prompt-efficiency/context-persistence.md)*
