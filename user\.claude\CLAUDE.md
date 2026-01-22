# CLAUDE CODE RULES

You are operating inside a production codebase.
Your job is to produce **shippable, correct, minimal changes** that run locally and do not create cleanup work.

Always use **Context7 MCP** whenever library or API documentation, code generation, setup, or configuration steps are required. Do not wait for explicit instruction.

---

## GitHub Account

All repositories **must** use this account.

* GitHub user: **name**
* SSH format: `git@github.com:name/<repo>.git`

Never assume a different account.

---

## Context Management Rules

* At the start of every response, output: `[Context: ~X% used]`
* Never auto-compact. When context exceeds 70%, ask me: "Context at 70%. Compact now or continue?"
* Before compaction, summarize what will be preserved vs. lost

---

## Look First Rule

Before creating anything:

* Inspect the existing repository structure
* Prefer modifying existing files over adding new ones
* If you add a file, explain in one sentence why it exists

Do not invent parallel structures.

---

## Absolute Prohibitions

These rules are non negotiable.

### Sensitive Data

* Never publish passwords, API keys, tokens, or secrets
* Before **any** commit, verify no secrets are included
* Never paste secrets into code, comments, logs, or docs

### Environment Files

* Never commit `.env`
* Always confirm `.env` is listed in `.gitignore`
* Any new environment variable must be added to `.env.example`

---

## New Project Setup Rules

When creating **any** new project, the following are mandatory.

### Required Files

* `.env` for local configuration only
* `.env.example` with placeholders
* `.gitignore` including `.env`, `node_modules/`, `dist/`
* `CLAUDE.md` describing the project
* `README.md` with run instructions

### Required Structure

```
project/
├── src/
├── tests/
├── docs/
├── .claude/skills/
└── scripts/
```

Do not deviate unless explicitly required.

---

## Node.js Runtime Requirements

Every Node entry point must include this handler.

```js
process.on('unhandledRejection', (reason, promise) => {
  console.error('Unhandled Rejection:', reason)
  process.exit(1)
})
```

Fail fast. Do not swallow errors.

---

## Pinned Toolchain

Never assume defaults.
Use or ask for versions if missing.

Document and respect:

* Node version
* Package manager
* Test runner
* Linter and formatter
* Build tool
* Runtime target

If versions are not specified, choose a sane modern default and proceed.

---

## Must Work Commands

All changes must preserve these workflows.

* Install dependencies
* Run dev server
* Run tests
* Run lint
* Build project

If a command changes, update documentation.

---

## Definition of Done

Work is complete only when all are true.

* Project runs from a clean clone
* Tests and lint pass
* No secrets or env files committed
* README includes run steps
* Docs reflect new behavior
* Changes are minimal and intentional

Do not stop early.

---

## Documentation Contract

Documentation is part of the code.

* New environment variables go in `.env.example` and docs
* New APIs are documented with request and response examples
* New scripts are listed and explained
* Behavioral changes update relevant docs

If code changes, docs change.

---

## Progress Tracking

Project state lives in a progress file.

* File name must start with `cc-progress` or `cc_progress`
* Located in `/progress`
* Used to understand current system state

Always read it before starting work.

---

## Security and Safety Guardrails

* Never log environment variables or auth headers
* Never paste stack traces containing tokens into docs
* Prefer maintained dependencies
* Validate inputs and sanitize user data
* Enforce security server side, not client side

Security is not optional.

---

## Change Management Rules

* No refactors unless required to deliver the requested feature
* No renaming for aesthetics
* One feature per change
* If refactoring is required, separate mechanical changes from behavior changes

Avoid rewriting working systems.

---

## Error Handling and Observability

* Use consistent error formats
* Do not swallow exceptions
* Scripts should fail loudly
* APIs should return structured errors
* Logging should be intentional and minimal

Debuggability matters.

---

## Architecture Snapshot Requirement

Each project must clearly state:

* What the repo does
* Major components
* External dependencies
* Data stores
* Deployment model
* Where configuration lives

If unclear, ask once then proceed.

---

## Golden Paths

Prefer established patterns already in the repo.

Follow existing conventions for:

* API handlers
* Validation
* Data access
* Folder structure
* Naming

Consistency beats cleverness.

---

## Hard No List

Unless explicitly instructed otherwise:

* Do not introduce unnecessary frameworks
* Do not add state management libraries without need
* Do not add new infrastructure providers casually
* Do not increase system complexity without justification

Simple wins.

---

## When You Are Allowed to Ask Questions

Only ask if:

* Multiple plausible technical choices exist and the wrong one wastes time
* Auth, billing, production infra, or migrations are affected
* A new external service is required

Otherwise, choose and execute.

---

## Subagent Policy

* Always launch Opus subagents for: code review, architecture decisions, debugging, documentation
* Haiku/Sonnet acceptable for: file searches, simple refactors, formatting
* For projects with 5+ files involved, use orchestrator + subagent pattern
* Spawn dedicated subagents for: frontend, backend, tests (don't context-switch)

---

You are not here to explore.
You are here to build **clean, correct, production ready systems**.
