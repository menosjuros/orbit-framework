---
name: orbit:observe
description: Research technical options before planning a phase
argument-hint: "<phase or topic>"
allowed-tools: [Read, Bash, Glob, Grep, WebSearch, WebFetch, Task, AskUserQuestion]
---

<objective>
Execute discovery to inform planning decisions. Produces OBSERVE.md with findings, recommendation, and confidence level.

**When to use:** Before planning a phase with technical unknowns (library selection, architecture decisions, integration approaches).

**Distinct from /orbit:research:** Research is for gathering documentation/information. Discover is for making technical decisions.
</objective>

<execution_context>
@~/.claude/orbit-framework/workflows/discovery.md
@~/.claude/orbit-framework/templates/OBSERVE.md
</execution_context>

<context>
$ARGUMENTS (phase number or topic)

@.orbit/STATE.md
@.orbit/ROADMAP.md
</context>

<process>
**Follow workflow: @~/.claude/orbit-framework/workflows/discovery.md**

The workflow implements:
1. Determine depth level (quick/standard/deep)
2. Identify unknowns for the phase
3. Research options using subagents
4. Cross-verify findings
5. Create OBSERVE.md with recommendation
6. Assign confidence level
7. Route to planning when complete
</process>

<success_criteria>
- [ ] Discovery depth determined
- [ ] Unknowns identified
- [ ] Options researched with sources
- [ ] OBSERVE.md created (for standard/deep)
- [ ] Recommendation provided with confidence
- [ ] Ready for /orbit:refine
</success_criteria>
