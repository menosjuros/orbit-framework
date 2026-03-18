<purpose>
Coordinate a cross-project build: create CONTRACT.md, spawn a receiver agent, and track completion.
</purpose>

<when_to_use>
- Initiator project needs another project to implement an interface (endpoint, event, service)
- Running /orbit:cross-build {receiver}
</when_to_use>

<process>

<step name="detect_mode" priority="first">
1. Read `.orbit/config.md` → find `linked_projects`
2. Locate receiver project entry:
   ```yaml
   linked_projects:
     back:
       path: /abs/path   # absolute → multi-repo
       path: ../../back  # relative → monorepo
   ```
3. Determine mode:
   - Absolute path → **multi-repo**
     - Receiver root = path as-is
   - Relative path → **monorepo**
     - Receiver root = `git rev-parse --show-toplevel` + relative path
4. Validate receiver path exists and has `.orbit/` directory
5. If missing: error "Receiver project not found or ORBIT not initialized at {path}"
</step>

<step name="read_context">
1. Read initiator's current LOOP.md (from STATE.md)
2. Identify what needs to be built in the receiver project
3. Read prior INTEGRATE.md (if any) for deferred issues
</step>

<step name="create_contract">
1. Determine contract ID: `{NN}-{receiver}`
2. Copy template from `~/.claude/orbit-framework/templates/CONTRACT.md`
3. Fill placeholders:
   - INITIATOR_PROJECT: current project name
   - DATE: today
   - INITIATOR_REFINE_PATH: current LOOP.md path
   - CONTRACT_ITEMS: what receiver must implement
   - ACCEPTANCE_CRITERIA: testable ACs
   - DEPENDENCY_NOTE: what blocks what
4. Save to: `.orbit/projects/{NN}-{name}/CONTRACT-{receiver}.md`
5. Display contract for user confirmation before proceeding
</step>

<step name="create_coordination">
1. Check if `.orbit/COORDINATION.md` exists
   - If yes: append new row to Active Cross-Builds table
   - If no: create from template `~/.claude/orbit-framework/templates/COORDINATION.md`
2. Fill:
   - INITIATOR_PROJECT, DATE, MODE
   - CONTRACT_ID, RECEIVER_PROJECT, Status: PENDING
</step>

<step name="spawn_receiver_agent">
**For multi-repo:**
```
Spawn Claude Code team agent at receiver path:
  cd {receiver_path}
  Read .orbit/projects/ (find latest project)
  Read CONTRACT.md at initiator path (shared reference)
  Run /orbit:refine
  Run /orbit:build
  Update CONTRACT.md Response section → BUILD_COMPLETE
```

**For monorepo:**
```
Same as multi-repo but paths are relative.
Git operations happen in same repo — no separate push needed.
```

Agent instructions to pass:
- Path to CONTRACT.md
- Receiver project name (for /orbit:refine targeting)
- "After BUILD complete: update CONTRACT.md status to BUILD_COMPLETE and fill Response section"
</step>

<step name="track_completion">
1. Poll or wait for receiver agent completion signal
2. When CONTRACT.md shows `BUILD_COMPLETE`:
   - Update COORDINATION.md status
   - Notify user:
     ```
     ════════════════════════════════════════
     CROSS-BUILD COMPLETE
     ════════════════════════════════════════

     Receiver: {receiver_project}
     Contract: {contract_path}
     Status:   BUILD_COMPLETE

     Receiver refine: {receiver_refine_path}

     ---
     Ready to integrate. Run:
     /orbit:integrate {initiator_loop_path}
     ════════════════════════════════════════
     ```
</step>

</process>

<error_handling>
**linked_projects not configured:**
- "No linked_projects in config.md. Add receiver project path first."
- Show config.md example

**Receiver has no ORBIT:**
- "Receiver path exists but .orbit/ not found. Run /orbit:init in {path} first."

**Contract already exists:**
- "Contract {id} already exists. Check COORDINATION.md for status."
- Offer: resume tracking or create new contract

**Receiver agent fails:**
- Log failure in COORDINATION.md
- Notify user with last known receiver state
</error_handling>

<output>
- `.orbit/projects/{NN}-{name}/CONTRACT-{receiver}.md`
- `.orbit/COORDINATION.md` (created or updated)
- Receiver project: LOOP.md + INTEGRATE.md (created by receiver agent)
</output>
