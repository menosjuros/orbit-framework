<purpose>
Initialize ORBTI structure in a new project. Creates .orbti/ directory with PROJECT.md, ROADMAP.md, STATE.md, and phases/ directory. Gathers project context conversationally before routing to planning.
</purpose>

<when_to_use>
- Starting ORBTI in a project that doesn't have .orbti/ directory
- User explicitly requests project initialization
- Beginning a new project from scratch
</when_to_use>

<loop_context>
N/A - This is a setup workflow, not a loop phase.
After init, project is ready for first REFINE.
</loop_context>

<philosophy>
**Flow and momentum:** Init should feel like the natural start of work, not a chore.
- Ask questions conversationally
- Populate files from answers (user doesn't edit templates)
- End with ONE next action
- Build momentum into planning
</philosophy>

<references>
@src/templates/config.md
@src/references/sonarqube-integration.md
</references>

<process>

<step name="check_existing" priority="first">
1. Check if .orbti/ directory exists:
   ```bash
   ls .orbti/ 2>/dev/null
   ```
2. If exists:
   - "ORBTI already initialized in this project."
   - Route to `/orbti:resume` or `/orbti:progress`
   - Exit this workflow
3. If not exists: proceed with initialization
</step>

<step name="create_structure">
Create directories first (gives immediate feedback):
```bash
mkdir -p .orbti/phases
```

Display:
```
ORBTI structure created.

Before planning, I need to understand what you're building.
```
</step>

<step name="gather_core_value">
**Ask ONE question at a time. Wait for response before next question.**

**Question 1: Core Value**
```
What's the core value this project delivers?

(Example: "Users can track expenses and see spending patterns")
```

Wait for user response. Store as `core_value`.
</step>

<step name="gather_description">
**Question 2: What are you building?**
```
What are you building? (1-2 sentences)

(Example: "A CLI tool for managing Docker containers")
```

Wait for user response. Store as `description`.
</step>

<step name="gather_project_name">
**Question 3: Project name**

Infer from:
1. Directory name
2. package.json name field
3. Ask if unclear

If obvious, confirm:
```
Project name: [inferred-name]

Is this correct? (yes/different name)
```

Store as `project_name`.
</step>

<step name="create_project_md">
Create `.orbti/PROJECT.md` with gathered information:

```markdown
# Project: [project_name]

## Description
[description]

## Core Value
[core_value]

## Requirements

### Must Have
- [To be defined during planning]

### Should Have
- [To be defined during planning]

### Nice to Have
- [To be defined during planning]

## Constraints
- [To be identified during planning]

## Success Criteria
- [core_value] is achieved
- [To be refined during planning]

---
*Created: [timestamp]*
```

Note: Requirements and constraints are populated during planning, not init.
</step>

<step name="create_roadmap_md">
Create `.orbti/ROADMAP.md`:

```markdown
# Roadmap: [project_name]

## Overview
[description]

## Current Milestone
**v0.1 Initial Release** (v0.1.0)
Status: Not started
Phases: 0 of TBD complete

## Phases

| Phase | Name | Refines | Status | Completed |
|-------|------|-------|--------|-----------|
| 1 | TBD | TBD | Not started | - |

## Phase Details

Phases will be defined during `/orbti:refine`.

---
*Roadmap created: [timestamp]*
```

Note: Phase details are populated during planning, not init.
</step>

<step name="create_state_md">
Create `.orbti/STATE.md`:

```markdown
# Project State

## Project Reference

See: .orbti/PROJECT.md (updated [timestamp])

**Core value:** [core_value]
**Current focus:** Project initialized — ready for planning

## Current Position

Milestone: v0.1 Initial Release
Phase: Not yet defined
Refine: None yet
Status: Ready to create roadmap and first REFINE
Last activity: [timestamp] — Project initialized

Progress:
- Milestone: [░░░░░░░░░░] 0%

## Loop Position

Current loop state:
```
REFINE ──▶ BUILD ──▶ INTEGRATE
  ○        ○        ○     [Ready for first REFINE]
```

## Accumulated Context

### Decisions
None yet.

### Deferred Issues
None yet.

### Blockers/Concerns
None yet.

## Session Continuity

Last session: [timestamp]
Stopped at: Project initialization complete
Next action: Run /orbti:refine to define phases and first refine
Resume file: .orbti/PROJECT.md

---
*STATE.md — Updated after every significant action*
```
</step>

<step name="prompt_integrations">
**Ask about optional integrations:**

```
Optional integrations:

Would you like to enable SonarQube code quality scanning?
(Requires SonarQube server and MCP server - can enable later)

[1] Yes, enable SonarQube
[2] Skip for now
```

Wait for user response.

**If "1" or "yes" or "enable":**

1. Prompt for project key:
   ```
   SonarQube project key?
   (Press Enter to use: [project_name])
   ```

   - If user presses Enter: use `project_name`
   - Otherwise: use provided key

2. Create `.orbti/config.md`:
   ```markdown
   # Project Config

   **Project:** [project_name]
   **Created:** [timestamp]

   ## Project Settings

   ```yaml
   project:
     name: [project_name]
     version: 0.0.0
   ```

   ## Integrations

   ### SonarQube

   ```yaml
   sonarqube:
     enabled: true
     project_key: [project_key]
   ```

   ## Preferences

   ```yaml
   preferences:
     auto_commit: false
     verbose_output: false
   ```

   ---
   *Config created: [timestamp]*
   ```

3. Store `integrations_enabled = true`

**If "2" or "skip" or "no":**

Store `integrations_enabled = false`
(Don't create config.md - user can add later)
</step>

<step name="check_specialized_flows">
**Ask about specialized skills:**

```
Do you have specialized skills or commands for this project?
(e.g., /revops-expert, /frontend-design, custom workflows)

[1] Yes, configure now
[2] Skip for now (add later via /orbti:skills)
```

Wait for user response.

**If "1" or "yes" or "configure":**

1. Store `specialized_flows_enabled = true`
2. Route to: @workflows/configure-skills.md
3. After completion, return to init confirmation
4. Store `skills_configured_count` from workflow output

**If "2" or "skip" or "no":**

Store `specialized_flows_enabled = false`
(User can add later via /orbti:skills)
</step>

<step name="confirm_and_route">
**Display confirmation with ONE next action:**

**Display based on enabled features:**

```
════════════════════════════════════════
ORBTI INITIALIZED
════════════════════════════════════════

Project: [project_name]
Core value: [core_value]

Created:
  .orbti/PROJECT.md    ✓
  .orbti/ROADMAP.md    ✓
  .orbti/STATE.md      ✓
  .orbti/config.md     ✓  (if integrations_enabled: "SonarQube enabled")
  .orbti/SPECIAL-FLOWS.md  ✓  (if specialized_flows_enabled: "[N] skills configured")
  .orbti/projects/       ✓

────────────────────────────────────────
▶ NEXT: /orbti:refine
  Define your projects and create your first refine.
────────────────────────────────────────

Type "yes" to proceed, or ask questions first.
```

**Note:** Only show config.md and SPECIAL-FLOWS.md lines if those features were enabled.
If neither was enabled, show the minimal version without those lines.

**Do NOT suggest multiple next steps.** ONE action only.
</step>

</process>

<output>
- `.orbti/` directory structure
- `.orbti/PROJECT.md` (populated from conversation)
- `.orbti/ROADMAP.md` (skeleton for planning)
- `.orbti/STATE.md` (initialized state)
- `.orbti/config.md` (if integrations enabled)
- `.orbti/SPECIAL-FLOWS.md` (if specialized flows enabled)
- `.orbti/projects/` (empty directory)
- Clear routing to `/orbti:refine`
</output>

<error_handling>
**Permission denied:**
- Report filesystem error
- Ask user to check permissions

**User declines to answer:**
- Use "[TBD]" placeholder
- Note that planning will ask for this information

**Partial creation failure:**
- Report what was created vs failed
- Offer to retry or clean up
</error_handling>
