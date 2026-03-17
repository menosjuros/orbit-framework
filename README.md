<div align="center">

# ORBIT

**Observe, Refine, Build, Integrate, Test** — A structured development loop for Claude Code.

[![npm version](https://img.shields.io/npm/v/orbit-framework?style=for-the-badge&logo=npm&logoColor=white&color=CB3837)](https://www.npmjs.com/package/orbit-framework)
[![License](https://img.shields.io/badge/license-MIT-blue?style=for-the-badge)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/menosjuros/orbit-framework?style=for-the-badge&logo=github&color=181717)](https://github.com/menosjuros/orbit-framework)

<br>

```bash
npx github:menosjuros/orbit-framework
```

</div>

---

## What is ORBIT

ORBIT is a framework for Claude Code that keeps AI-assisted development **predictable and reliable** across sessions.

Most AI workflows break down over time: sessions fill up, context degrades, plans get started but never closed, state drifts. ORBIT solves this with a **mandatory closed loop** — every unit of work must be planned, executed, and reconciled before moving on.

### How ORBIT stands out

| | ORBIT | Generic AI workflows |
|--|-------|----------------------|
| **State** | Tracked in `STATE.md` — always knows where you are | Lost between sessions |
| **Planning** | Acceptance criteria before a single line of code | Vague prompts |
| **Execution** | Tasks run sequentially, deviations logged | Freeform, unpredictable |
| **Closure** | INTEGRATE is mandatory — loop can't skip | Plans abandoned mid-way |
| **Research** | Subagents for research only, never implementation | Mixed concerns |
| **Context** | Rich in-session context preserved via pause/resume | Restarts from zero |

---

## Getting Started

**Prerequisites:** [Node.js](https://nodejs.org) 18+ and [Claude Code](https://claude.ai/code).

### Install

```bash
npx github:menosjuros/orbit-framework
```

Choose **global** (`~/.claude/`) to use in all projects, or **local** (`./.claude/`) for this project only.

<details>
<summary>Non-interactive install</summary>

```bash
npx github:menosjuros/orbit-framework --global
npx github:menosjuros/orbit-framework --local
```

</details>

### Initialize

Restart Claude Code, then run:

```
/orbit:init
```

This creates `.orbit/` with `PROJECT.md`, `ROADMAP.md`, and `STATE.md` through a short conversational setup.

Verify with `/orbit:help` — you should see the full command reference.

---

## The Main Loop

The core of ORBIT. Every unit of work follows this cycle — no shortcuts.

```
┌──────────────────────────────────────────┐
│  REFINE ──▶ BUILD ──▶ INTEGRATE          │
│  Define     Execute    Reconcile & close  │
└──────────────────────────────────────────┘
```

```
/orbit:refine     # define what you're building — get plan approved
/orbit:build      # execute the approved plan
/orbit:integrate  # reconcile and close the loop — never skip
```

Repeat for each piece of work.

### REFINE

Creates a `LOOP.md` — the contract for what gets built. Defines:
- **Objective** — what and why
- **Acceptance Criteria** — Given/When/Then definitions of done
- **Tasks** — each with `files`, `action`, `verify`, and `done`
- **Boundaries** — what must NOT change

If you can't fill all four fields per task, the task is too vague to execute.

### BUILD

Executes the approved plan sequentially:
- Each task has a built-in verification step
- Deviations are logged with reason and impact
- Checkpoints pause for human decisions, visual verification, or manual steps

### INTEGRATE

Closes the loop — **never skip this**:
- Compares plan vs what was actually built
- Records decisions and deferred issues
- Updates `STATE.md`
- If it's the last plan in a project: triggers project transition and git commit automatically

---

## Going Deeper

These commands add structure before and around the main loop. Use them for complex projects, unclear scope, or multi-session work.

### Before planning a project

Use these to align before writing a single line of plan:

```
/orbit:cocreate "feature"    # articulate what you want to build
/orbit:observe "topic"       # research technical unknowns before deciding
/orbit:assumptions           # surface Claude's understanding — catch misalignments early
```

**`/orbit:cocreate`** — Conversational exploration of the feature. Helps you articulate goals, scope, and constraints before committing to a plan. Avoids building the wrong thing.

**`/orbit:observe "topic"`** — Deploys research subagents to compare libraries, patterns, or approaches. Produces an `OBSERVE.md` with a recommendation and confidence level. Use when you have a technical unknown that could change the plan.

**`/orbit:assumptions`** — Surfaces what Claude *intends* to do before planning. Run this after cocreate and observe to catch misalignments early, not mid-build.

---

### The full loop

```
/orbit:refine      # plan informed by cocreate + observe
/orbit:build       # execute
/orbit:test        # verify against acceptance criteria
/orbit:integrate   # close the loop
```

**`/orbit:test`** — Auto-detects the project's test runner (Jest, Vitest, Pytest, Go test, Cargo), writes integration tests for any ACs without coverage, runs them, and maps results to AC-1, AC-2... Falls back to guided manual UAT if no test runner is found.

Flags:
- `--e2e` — also run Playwright CLI browser tests (requires setup below)
- `--manual` — skip auto-detection, go straight to manual UAT

---

### E2E testing with Playwright CLI

ORBIT integrates with [Playwright CLI](https://github.com/microsoft/playwright-cli) — an agent-optimized browser automation tool built for coding agents like Claude Code.

#### Install

```bash
npm install -g @playwright/cli@latest
playwright-cli install --skills
```

#### Enable in ORBIT

```
/orbit:enable-e2e
```

This installs Playwright CLI (if not already installed), installs your chosen browser, and configures ORBIT to run E2E tests by default on every `/orbit:test`.

Once enabled, set your app URL in `.orbit/config.md`:

```yaml
e2e:
  enabled: true
  runner: playwright-cli
  browser: chromium
  default: true
  base_url: "http://localhost:3000"
```

#### How it works

When `default: true` or `--e2e` is passed:
1. Integration tests run first (native test runner)
2. After integration tests pass, Playwright CLI navigates to `base_url`
3. Claude writes and runs browser scenarios for each user-facing AC
4. Results mapped to ACs — E2E failures are **warnings** (not blockers), integration test failures **block** INTEGRATE

> E2E tests are intentionally non-blocking because browser tests are inherently flakier than integration tests. Failures are logged and routed to `/orbit:plan-fix`.

---

### Session management

Work across multiple sessions without losing context:

```
/orbit:pause    # safe stop — creates a handoff with full context
/orbit:resume   # restore context and get exactly ONE next action
```

**`/orbit:pause`** — Captures current loop position, open decisions, and next steps into a handoff file. Safe to close Claude Code after this.

**`/orbit:resume`** — Reads `STATE.md` and the latest handoff. Gives you exactly one next action — no decision fatigue when returning to work.

---

### Milestones

Group related projects into milestones for larger initiatives:

```
/orbit:cocreate-milestone    # align on milestone vision before starting
/orbit:milestone "name"      # create milestone and add projects to ROADMAP.md
...                          # work through projects with the main loop
/orbit:complete-milestone    # archive, tag, and close the milestone
```

**`/orbit:cocreate-milestone`** — Like cocreate, but for the full milestone. Define the vision, success criteria, and scope before creating any projects.

**`/orbit:complete-milestone`** — Archives completed milestone to `.orbit/milestones/`, creates a git tag, and prepares `STATE.md` for the next milestone.

---

## How It Works

### File structure

```
.orbit/
├── PROJECT.md           # Project context and goals
├── ROADMAP.md           # Milestones and project breakdown
├── STATE.md             # Loop position, session state, decisions
├── config.md            # Optional integrations
├── SPECIAL-FLOWS.md     # Optional skill requirements per project
├── milestones/          # Archived completed milestones
│   └── v1.0-launch.md
└── projects/            # Flat numbering — never restarts at 01
    ├── 01-auth/
    │   ├── 01-01-LOOP.md
    │   └── 01-01-INTEGRATE.md
    └── 02-dashboard/
        ├── 02-01-LOOP.md
        └── 02-01-INTEGRATE.md
```

Projects are numbered continuously across milestones. Milestone grouping lives in `ROADMAP.md`, not in the folder structure.

### State management

`STATE.md` always knows where you are: current project, loop position (REFINE / BUILD / INTEGRATE), accumulated decisions, and blockers. `/orbit:resume` reads it and gives exactly ONE next action.

### LOOP.md structure

```markdown
---
project: 01-auth
plan: 01
type: execute
autonomous: true
---

<objective>
Goal, Purpose, Output
</objective>

<context>
@-references to relevant files
</context>

<acceptance_criteria>
## AC-1: Login works
Given a valid user
When they submit credentials
Then they receive a JWT token
</acceptance_criteria>

<tasks>
<task type="auto">
  <name>Create login endpoint</name>
  <files>src/api/auth/login.ts</files>
  <action>Implementation details...</action>
  <verify>curl returns 200 with token</verify>
  <done>AC-1 satisfied</done>
</task>
</tasks>

<boundaries>
## DO NOT CHANGE
- database/migrations/*
- src/lib/auth.ts
</boundaries>
```

### Task types

| Type | Use for |
|------|---------|
| `auto` | Fully autonomous execution |
| `checkpoint:decision` | Choices requiring human input |
| `checkpoint:human-verify` | Visual or functional verification |
| `checkpoint:human-action` | Manual steps (deploy, external service) |

---

## Troubleshooting

**Commands not found after install?**
Restart Claude Code. Verify files exist in `~/.claude/commands/orbit/` (global) or `./.claude/commands/orbit/` (local).

**Commands not working as expected?**
Run `/orbit:help` to verify installation. Re-run `npx github:menosjuros/orbit-framework` to reinstall.

**Loop position seems wrong?**
Check `.orbit/STATE.md` directly. Run `/orbit:progress` for guided next action.

**Resuming after a break?**
Run `/orbit:resume` — reads STATE.md and handoffs automatically.

---

## License

MIT. See [LICENSE](LICENSE).

This project is a fork of [CARL](https://github.com/ChristopherKahler/carl) by Chris Kahler.

---

<div align="center">

**Claude Code is powerful. ORBIT makes it reliable.**

</div>
