---
name: orbit:build-bg
description: Execute an approved REFINE refine autonomously in the background
argument-hint: "[refine-path]"
allowed-tools: [Read, Write, Edit, Bash, Glob, Grep, Task]
---

<model>sonnet</model>

<objective>
Run an approved REFINE.md as a background agent — execution happens unattended and you are notified on completion.

**Requires:** Refine must have `autonomous: true`. Refines with checkpoints must use `/orbit:build` (foreground).
</objective>

<execution_context>
@~/.claude/orbit-framework/workflows/build.md
</execution_context>

<context>
Refine path: $ARGUMENTS

@.orbit/STATE.md
</context>

<process>
Follow workflow: @~/.claude/orbit-framework/workflows/build.md — route to `background_build` step.
</process>

<success_criteria>
- [ ] Refine has autonomous: true (blocking if not)
- [ ] Background agent spawned
- [ ] User notified on completion
- [ ] STATE.md updated with BUILD complete
- [ ] Next action clear: /orbit:integrate
</success_criteria>
