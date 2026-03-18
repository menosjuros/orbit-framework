# Cross-Build Reference

Cross-build enables ORBIT projects to trigger and coordinate builds in other projects — across repositories (multi-repo) or within the same repository (monorepo).

---

## Configuration

In `.orbit/config.md`, add `linked_projects`:

```yaml
# Multi-repo
linked_projects:
  back:
    path: /absolute/path/to/crm-back
  agents:
    path: /absolute/path/to/crm-agents

# Monorepo (relative from current project root)
linked_projects:
  back:
    path: ../../packages/back
  agents:
    path: ../../packages/agents
```

ORBIT auto-detects mode:
- Absolute path → **multi-repo** (separate git roots)
- Relative path → **monorepo** (single git root)

---

## How It Works

```
[Front ORBIT]                    [Back ORBIT]
     │                                │
     ├─ /orbit:cross-build back ──→   │
     │                                ├─ reads CONTRACT.md
     │                                ├─ /orbit:refine
     │                                ├─ /orbit:build
     │                                └─ updates CONTRACT.md → BUILD_COMPLETE
     │
     ├─ detects BUILD_COMPLETE
     └─ /orbit:integrate (proceeds)
```

### Key Files

| File | Location | Purpose |
|------|----------|---------|
| `CONTRACT.md` | `.orbit/projects/{NN}-{name}/CONTRACT-{receiver}.md` | Interface contract request |
| `COORDINATION.md` | `.orbit/COORDINATION.md` | Cross-project status board |

---

## CONTRACT.md

Created by initiator project. Defines:
- What to implement (endpoints, webhooks, events)
- Acceptance criteria
- Dependency note (what blocks what)

Receiver fills in the `## Response` section when their REFINE is created.

### Status lifecycle

```
PENDING → REFINE_CREATED → BUILD_COMPLETE → INTEGRATED
```

---

## Coordination Protocol

1. **Initiator** runs `/orbit:cross-build back` (or similar)
2. ORBIT creates `CONTRACT.md` in initiator's project folder
3. ORBIT writes `COORDINATION.md` at root of initiator `.orbit/`
4. ORBIT opens a Claude Code team agent in the receiver project
5. Receiver agent reads CONTRACT.md → runs `/orbit:refine` → `/orbit:build`
6. Receiver updates CONTRACT.md status → `BUILD_COMPLETE`
7. Initiator polls / is notified → proceeds to `/orbit:integrate`

---

## Monorepo Specifics

- Paths resolved relative to project root (`git rev-parse --show-toplevel`)
- Single git history — no cross-repo commits needed
- COORDINATION.md lives at monorepo root `.orbit/` (shared)

## Multi-Repo Specifics

- Each repo has its own `.orbit/`
- COORDINATION.md lives in each repo's `.orbit/`
- CONTRACT.md is the communication artifact between repos
- Agent uses absolute path to spawn in receiver project

---

## Model Routing

Cross-build coordinator uses **sonnet** (orchestration + decisions).
Receiver agents follow their own project model routing.
