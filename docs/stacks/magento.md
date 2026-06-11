# Adobe Commerce / Magento — Claude Workflow

**Version:** 2.0 | **Date:** June 2026 | **Classification:** Internal Use Only

---

## System Prompt

Store this in `/ai-prompts/magento/system-prompt-claude.md` and add it to your CLAUDE.md or Project instructions so it loads every session automatically.

```
You are a senior Adobe Commerce (Magento 2.4.x) developer at Krish TechnoLabs.
You have deep expertise in:
- Magento module architecture (app/code structure, di.xml, events/observers)
- EAV attribute system, resource models, collections
- GraphQL schema, resolvers, and mutations
- REST and SOAP API integration
- Adobe Commerce Cloud deployment (ece-tools, .magento.env.yaml)
- Performance: Varnish, Redis, OpenSearch configuration
- Payment and shipping method integration (Stripe, PayPal, UPS, FedEx)

Coding standards: PSR-12, Magento coding standard v2.
Always use constructor injection — never ObjectManager directly.
Return only requested code. Do not regenerate unchanged files.
```

---

## Task Patterns

### Module Development

```xml
<task>Create a Magento 2 observer for the sales_order_place_after event that logs order ID, customer email, and grand total to a custom log file.</task>

<constraints>
  - Return only: Observer PHP class + events.xml + di.xml entries
  - Namespace: Krish\OrderLogger
  - No unit test boilerplate
  - Format: separate fenced code blocks per file, each with file path as comment
</constraints>
```

### GraphQL Resolver

```xml
<task>Write a Magento 2 GraphQL resolver that returns configurable product variants filtered by in-stock status only.</task>

<context>
  Store: B2B catalogue, ~50K SKUs, OpenSearch 2.x backend.
  Current resolver at: Krish/Catalogue/Model/Resolver/ProductVariants.php
</context>

<constraints>
  - Return resolver PHP class + schema.graphqls fragment only
  - Use service contracts, not resource models directly
</constraints>
```

### Performance Debugging

```xml
<task>Identify slow queries from this New Relic trace and suggest Magento-specific fixes.</task>

<constraints>
  - Return JSON: [{query_pattern, affected_model, fix_type, estimated_improvement}]
  - fix_type: add_index | add_cache | refactor_collection | use_flat_table
</constraints>

<data>
  [Paste New Relic slow query log here — scrub customer data first]
</data>
```

### Data Patch / Migration

```xml
<task>Generate an InstallData migration script that creates a custom EAV attribute 'warranty_period' for catalog_product, type: int, visible on frontend, searchable.</task>

<constraints>
  - Magento 2.4.6+ compatible (use AttributeRepositoryInterface)
  - Return Setup/Patch/Data/AddWarrantyPeriodAttribute.php only
  - No comments explaining standard Magento boilerplate
</constraints>
```

### Code Review

```xml
<task>Review this Magento module for: security issues, performance anti-patterns, and violations of Magento coding standards.</task>

<constraints>
  - Return JSON: [{file, line, severity, category, issue, fix}]
  - severity: CRITICAL | HIGH | MEDIUM | LOW
  - category: security | performance | standards | architecture
  - Top 10 issues by severity only
</constraints>

<data>
  [Paste code here]
</data>
```

---

## Bulk Tasks

One session per module, consistent output format, combine at the end. See [bulk-work.md](../01-prompt-efficiency/bulk-work.md) for the full pattern.

| Task | Approach |
|---|---|
| Review all custom modules in app/code | One Claude Code session per module — same JSON schema, merge at end |
| Generate PHPDoc for all classes | One session per class, or use Claude Code subagents for parallel work |
| Audit all GraphQL resolvers for N+1 | One session per resolver |
| Monthly security review | One session per module, combine for final report |

---

## CLAUDE.md Template

Place this at the root of your Magento project.

```markdown
# [Project Name] — Magento

## Stack
Adobe Commerce 2.4.7-p3 | Cloud Pro | PHP 8.2 | OpenSearch 2.11

## Structure
Custom modules: app/code/Krish/
Theme: app/design/frontend/Krish/default/
Config: app/etc/ (never edit env.php directly)

## Standards
- PSR-12 + Magento coding standard — run: vendor/bin/phpcs
- Constructor injection only — never ObjectManager::getInstance()
- All new attributes via Data Patch, never InstallSchema
- Cache types to flush after module changes: config, full_page, layout

## Forbidden
- Direct SQL — use resource models and collections
- Plugin on __construct — use afterCreate or virtual types
- Modifying core files — use preferences, plugins, or observers

## Key Commands
bin/magento setup:upgrade && bin/magento cache:flush
bin/magento setup:di:compile
bin/magento dev:query-log:enable
vendor/bin/phpcs --standard=Magento2 app/code/Krish/
```

---

## Claude Code Project Setup

The full project structure for a Magento project with Claude Code configured:

```
my-magento-project/
├── CLAUDE.md                        ← System prompt (template above)
├── .claude/
│   ├── settings.json                ← Permissions, hooks, env vars
│   └── commands/
│       ├── review.md                ← /review  — pre-PR code review
│       ├── module.md                ← /module  — scaffold new module
│       └── patch.md                 ← /patch   — generate data patch
└── .mcp.json                        ← MCP server integrations
```

### `.claude/settings.json`

Controls what Claude Code is allowed to do, runs automated checks after edits, and blocks dangerous operations.

```json
{
  "permissions": {
    "allow": [
      "Bash(bin/magento:*)",
      "Bash(composer:*)",
      "Bash(vendor/bin/phpcs:*)",
      "Bash(vendor/bin/phpunit:*)",
      "Read(**)",
      "Edit(app/code/Krish/**)",
      "Edit(app/design/frontend/Krish/**)"
    ],
    "deny": [
      "Edit(vendor/**)",
      "Edit(app/code/Magento/**)",
      "Edit(app/etc/env.php)",
      "Bash(rm -rf:*)",
      "Bash(bin/magento setup:uninstall:*)"
    ]
  },
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "vendor/bin/phpcs --standard=Magento2 --report=summary $CLAUDE_TOOL_INPUT_path 2>&1 | tail -5"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "echo $CLAUDE_TOOL_INPUT_command | grep -qE 'DROP TABLE|TRUNCATE|DELETE FROM' && echo 'BLOCKED: Destructive DB command — run manually if intentional' && exit 1 || exit 0"
          }
        ]
      }
    ]
  },
  "env": {
    "MAGENTO_ROOT": "/var/www/html",
    "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE": "60"
  }
}
```

**What the hooks do:**
- **PostToolUse → Edit**: Runs PHPCS on every file Claude edits — violations appear immediately in the session, not in CI
- **PreToolUse → Bash**: Blocks any command containing DROP TABLE, TRUNCATE, or DELETE FROM — must be run manually

### `.claude/commands/`

Each `.md` file becomes a `/commandname` slash command inside Claude Code. The file content is the prompt that runs.

**`.claude/commands/review.md`** — type `/review` before `git commit`:

This runs **locally on staged changes only** (`git diff --staged`). Claude sees only the files in your current commit — not the full codebase. Token usage is proportional to the size of your change, not the project.

This replaces the need for a Claude-based CI review step. CI only runs static tools (PHPCS, PHPStan) as a safety net — no duplicate token spend.

```markdown
Run: git diff --staged

Review only the staged changes shown above in this Magento project.
Do not review unstaged or untracked files.

Check for:
1. Security: SQL injection, mass assignment, XSS, path traversal, exposed credentials
2. Performance: N+1 collection loads, missing cache tags, unoptimised EAV queries
3. Standards: PSR-12 violations, ObjectManager direct use, plugin on __construct
4. Architecture: InstallSchema usage, core file modification, hardcoded strings that should use config

Return JSON only:
[{"file": "", "line": 0, "severity": "CRITICAL|HIGH|MEDIUM|LOW", "category": "security|performance|standards|architecture", "issue": "", "fix": ""}]

Order by severity. Max 15 findings. No prose.
If staged diff is empty, respond: "Nothing staged. Run git add first."
```

**`.claude/commands/module.md`** — type `/module Krish_ModuleName`:

```markdown
Scaffold a new Magento 2 module for: $ARGUMENTS

Generate the minimum required files:
- registration.php
- etc/module.xml
- composer.json (with Krish namespace, version 1.0.0)

Namespace: Krish\{ModuleName}
Path: app/code/Krish/{ModuleName}/

Return one fenced code block per file with the file path as the first comment.
No boilerplate beyond the minimum required files.
```

**`.claude/commands/patch.md`** — type `/patch PatchClassName`:

```markdown
Generate a Magento 2 Data Patch class for: $ARGUMENTS

Requirements:
- Implements DataPatchInterface
- Namespace: Krish\{InferModuleFromContext}\Setup\Patch\Data
- Uses DependencyData::getDependencies() returning []
- Uses getAliases() returning []
- Constructor injection only — no ObjectManager
- Compatible with Magento 2.4.6+

Return Setup/Patch/Data/{ClassName}.php only.
No comments explaining standard boilerplate.
```

### `.mcp.json`

MCP servers give Claude Code direct access to tools. Install each server package before adding it here.

```json
{
  "mcpServers": {
    "filesystem": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/var/www/html/app/code/Krish"]
    },
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "mysql": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@benborla29/mcp-server-mysql"],
      "env": {
        "MYSQL_HOST": "localhost",
        "MYSQL_USER": "${DB_USER}",
        "MYSQL_PASS": "${DB_PASS}",
        "MYSQL_DB": "${DB_NAME}"
      }
    }
  }
}
```

**What each server enables:**

| MCP Server | What Claude can do |
|---|---|
| `filesystem` | Read/navigate files in `app/code/Krish/` without you pasting them |
| `github` | List PRs, create issues, fetch PR diff, comment on reviews |
| `mysql` | Query Magento DB — check EAV attributes, catalog rules, order status — without writing raw queries yourself |

**Install the packages first:**
```bash
npm install -g @modelcontextprotocol/server-filesystem
npm install -g @anthropic/mcp-github
npm install -g @benborla29/mcp-server-mysql
```

> **Security note:** Never put real credentials in `.mcp.json`. Use environment variable references (`${VAR_NAME}`) and store values in your shell profile or a `.env` file that is `.gitignore`d.

---

*See also: [prompt-engineering.md](../00-core-principles/prompt-engineering.md) | [model-selection.md](../00-core-principles/model-selection.md) | [context-persistence.md](../01-prompt-efficiency/context-persistence.md)*
