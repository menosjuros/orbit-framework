---
name: orbit:cross-build
description: Trigger a build in a linked project and coordinate via CONTRACT
argument-hint: "<receiver-project>"
allowed-tools: [Read, Write, Bash, Glob, Grep, Task, AskUserQuestion]
---

<model>sonnet</model>

<objective>
Coordinate a cross-project build: create CONTRACT.md in the initiator project,
spawn an ORBIT agent in the receiver project, and track completion.

**When to use:** When your current build depends on another project implementing
an interface (endpoint, event, schema, service).

**Modes:**
- multi-repo: receiver path is absolute → each project has its own git history
- monorepo: receiver path is relative → single git repo, shared history
</objective>

<execution_context>
@~/.claude/orbit-framework/workflows/cross-build.md
@~/.claude/orbit-framework/templates/CONTRACT.md
@~/.claude/orbit-framework/templates/COORDINATION.md
@~/.claude/orbit-framework/references/cross-build.md
</execution_context>

<context>
Receiver: $ARGUMENTS

@.orbit/STATE.md
@.orbit/config.md
</context>

<process>

<step name="validate">
1. Confirm $ARGUMENTS is set (receiver project name)
   - If empty: "Usage: /orbit:cross-build <receiver>" — list available linked_projects
2. Confirm receiver exists in config.md linked_projects
3. Run workflow: @~/.claude/orbit-framework/workflows/cross-build.md
</step>

</process>

<success_criteria>
- [ ] Mode detected (multi-repo or monorepo)
- [ ] CONTRACT.md created and confirmed by user
- [ ] COORDINATION.md created or updated
- [ ] Receiver agent spawned
- [ ] User notified on completion
</success_criteria>
