# MERN Stack — Claude Workflow

**Version:** 2.0 | **Date:** June 2026 | **Classification:** Internal Use Only

---

## System Prompt

Store this in `/ai-prompts/mern/system-prompt-claude.md` and add it to your CLAUDE.md or Project instructions so it loads every session automatically.

```
You are a senior MERN stack developer at Krish TechnoLabs.
You have deep expertise in:
- React 18+ (hooks, context, suspense, server components)
- Node.js 20+ with Express.js and middleware patterns
- MongoDB with Mongoose ODM and aggregation pipelines
- REST API design and JWT/OAuth2 authentication
- State management: Zustand, Redux Toolkit, React Query
- TypeScript across the full stack
- Testing: Jest, React Testing Library, Supertest

Code style: ESLint Airbnb config, Prettier, TypeScript strict mode.
Return only the code requested. No explanatory prose unless asked.
Always use async/await — never raw Promise chains.
```

---

## Task Patterns

### React Component

```xml
<task>Create a reusable ProductCard component that displays product image, title, price with sale badge, and an Add to Cart button with loading state.</task>

<constraints>
  - TypeScript, functional component with hooks
  - Props interface must be explicit — no 'any'
  - Use React Query for cart mutation
  - Tailwind CSS for styling
  - Return: component file + types file only
  - No test file
</constraints>
```

### MongoDB Aggregation

```xml
<task>Write a MongoDB aggregation pipeline that returns monthly revenue by product category for the last 12 months, including zero-revenue months.</task>

<context>
  Collection: orders
  Schema: { createdAt: Date, items: [{productId, category, qty, price}], status: String }
  Only include orders with status: 'completed'
</context>

<constraints>
  - Return pipeline as TypeScript const
  - Use $facet for category breakdown + total
  - No Mongoose model boilerplate
</constraints>
```

### Express Middleware

```xml
<task>Create an Express middleware that validates incoming Shopify webhook signatures using HMAC-SHA256.</task>

<constraints>
  - TypeScript
  - Node.js built-in crypto only — no external packages
  - Return 401 with {error: 'Invalid signature'} on failure
  - Export as named middleware function
</constraints>
```

### Service Class

```xml
<task>Write a Node.js service class that wraps the Stripe payment intent API for creating, confirming, and refunding payments.</task>

<constraints>
  - TypeScript class with typed method signatures
  - All methods async, throw typed errors on failure
  - Return service class only — no Express route wiring
</constraints>
```

### Code Review

```xml
<task>Review this React component for: performance issues, accessibility violations, and TypeScript strictness problems.</task>

<constraints>
  - Return JSON: [{file, line, severity, category, issue, fix}]
  - severity: CRITICAL | HIGH | MEDIUM | LOW
  - category: performance | accessibility | typescript | security | patterns
  - Max 10 findings, ordered by severity
</constraints>

<data>
  [Paste component here]
</data>
```

---

## Bulk Tasks

One session per file, consistent output format, combine at the end. See [bulk-work.md](../01-prompt-efficiency/bulk-work.md) for the full pattern.

| Task | Approach |
|---|---|
| Review all React components in /components | One Claude Code session per component — same JSON schema, merge results |
| Generate JSDoc for all Express routes | One session per route file, or Claude Code subagents |
| Audit all Mongoose schemas for missing indexes | One session per model file |
| Generate unit tests for utility functions | One session per utility — define expected test format upfront |

---

## CLAUDE.md Template

Place this at the root of your MERN project.

```markdown
# [Project Name] — MERN

## Stack
React 18.3 | Node.js 20 LTS | Express 4.x | MongoDB 7 | TypeScript 5.4

## Structure
/client   → React/Vite frontend
/server   → Express API
/shared   → Shared TypeScript types
/tests    → Jest test suites

## Standards
- TypeScript strict: true — no 'any', no @ts-ignore
- ESLint Airbnb config + custom rules in .eslintrc
- Functional components only, hooks only — no class components
- API responses: always {data, error, meta} envelope
- AppError class for errors — never throw raw strings
- No console.log in committed code — use logger utility

## Forbidden
- Direct DOM manipulation — use React refs only
- Raw MongoDB driver — use Mongoose models only
- Synchronous file operations in Express handlers

## Key Commands
npm run dev
npm test
npm run lint
```

---

## Claude Code Project Setup

The full project structure for a MERN project with Claude Code configured:

```
my-mern-project/
├── CLAUDE.md                        ← System prompt (template above)
├── .claude/
│   ├── settings.json                ← Permissions, hooks, env vars
│   └── commands/
│       ├── review.md                ← /review    — pre-PR code review
│       ├── component.md             ← /component — scaffold React component
│       └── route.md                 ← /route     — scaffold Express route + controller
└── .mcp.json                        ← MCP server integrations
```

### `.claude/settings.json`

```json
{
  "permissions": {
    "allow": [
      "Bash(npm:*)",
      "Bash(npx:*)",
      "Bash(node:*)",
      "Read(**)",
      "Edit(client/src/**)",
      "Edit(server/src/**)",
      "Edit(shared/**)",
      "Edit(tests/**)"
    ],
    "deny": [
      "Edit(node_modules/**)",
      "Edit(dist/**)",
      "Edit(build/**)",
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
            "command": "npx eslint $CLAUDE_TOOL_INPUT_path --max-warnings 0 2>&1 | tail -10"
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
            "command": "echo $CLAUDE_TOOL_INPUT_command | grep -qE 'db\\.drop|dropDatabase|dropCollection' && echo 'BLOCKED: Destructive DB operation — run manually if intentional' && exit 1 || exit 0"
          }
        ]
      }
    ]
  },
  "env": {
    "NODE_ENV": "development",
    "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE": "60"
  }
}
```

**What the hooks do:**
- **PostToolUse → Edit**: Runs ESLint on every file Claude edits — linting errors surface immediately in the session
- **PreToolUse → Bash**: Blocks `db.drop`, `dropDatabase`, `dropCollection` — must be run manually

### `.claude/commands/`

**`.claude/commands/review.md`** — type `/review`:

```markdown
Review the git-staged changes in this MERN project.

Check for:
1. TypeScript: 'any' usage, missing return types, @ts-ignore without justification
2. React: missing keys, unnecessary re-renders, useEffect dependency issues, missing error boundaries
3. Express: missing input validation, unhandled promise rejections, missing auth middleware
4. MongoDB: missing indexes for query fields, unbounded queries without limit(), N+1 population patterns
5. Security: JWT stored in localStorage, exposed secrets, missing rate limiting, unvalidated user input

Return JSON only:
[{"file": "", "line": 0, "severity": "CRITICAL|HIGH|MEDIUM|LOW", "category": "typescript|react|express|mongodb|security", "issue": "", "fix": ""}]

Order by severity. Max 15 findings. No prose.
```

**`.claude/commands/component.md`** — type `/component ComponentName`:

```markdown
Scaffold a new React component for: $ARGUMENTS

Requirements:
- TypeScript functional component
- Explicit Props interface — no 'any'
- Export: named export (not default)
- Path: client/src/components/{ComponentName}/{ComponentName}.tsx
- Include: component file + index.ts barrel export

Return one fenced code block per file with file path as the first comment.
No test file. No Storybook file. No explanatory comments.
```

**`.claude/commands/route.md`** — type `/route resourceName`:

```markdown
Scaffold an Express route + controller for: $ARGUMENTS

Requirements:
- TypeScript
- Route file: server/src/routes/{resource}.routes.ts — register CRUD endpoints
- Controller file: server/src/controllers/{resource}.controller.ts — handler functions
- Use async/await with try/catch, AppError for errors
- Input validation placeholder comments (Zod schema to be added separately)
- No Mongoose model — just the route and controller skeleton

Return one fenced code block per file with file path as the first comment.
```

### `.mcp.json`

```json
{
  "mcpServers": {
    "filesystem": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "./client/src", "./server/src", "./shared"]
    },
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    },
    "mongodb": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@mongodb-js/mongodb-mcp-server"],
      "env": {
        "MDB_MCP_CONNECTION_STRING": "${MONGODB_URI}"
      }
    }
  }
}
```

**What each server enables:**

| MCP Server | What Claude can do |
|---|---|
| `filesystem` | Read `client/src/`, `server/src/`, `shared/` directly — no manual file pasting |
| `github` | Fetch PR diffs, list open PRs, post review comments |
| `mongodb` | Query collections directly — check indexes, inspect documents, debug aggregation pipelines |

**MongoDB MCP in practice:**
- "Why is this aggregation slow?" → Claude checks collection stats and existing indexes
- "Does the users collection have an index on email?" → Claude queries directly
- "Show me a sample document from the orders collection" → Claude fetches one (no real customer data — use dev DB only)

**Install the packages:**
```bash
npm install -g @modelcontextprotocol/server-filesystem
npm install -g @anthropic/mcp-github
npm install -g @mongodb-js/mongodb-mcp-server
```

> **Security note:** Point `MONGODB_URI` to your **development database only** — never a production connection string in `.mcp.json`.

---

*See also: [prompt-engineering.md](../00-core-principles/prompt-engineering.md) | [model-selection.md](../00-core-principles/model-selection.md) | [context-persistence.md](../01-prompt-efficiency/context-persistence.md)*
