# What Makes ORBIT Different

> **ORBIT** (Observe, Refine, Build, Integrate, Test) builds on the foundation of GSD (Get Shit Done) while addressing key friction points that emerge during real-world AI-assisted development.

---

## The Core Insight

GSD pioneered the concept of treating plans as executable prompts. ORBIT takes this further by recognizing that **execution without reconciliation creates drift** — and drift compounds across sessions.

Where GSD focuses on *getting work done*, ORBIT focuses on *getting work done correctly, consistently, and with full traceability*.

---

## Key Differentiators

### 1. Explicit Loop Discipline

**GSD:** PLAN → EXECUTE → (implicit review)

**ORBIT:** PLAN → APPLY → UNIFY (enforced)

The UNIFY phase isn't optional. Every plan produces a INTEGRATE.md that captures:
- What actually happened vs. what was planned
- Decisions made during execution
- Issues deferred for later
- Acceptance criteria results (pass/fail)

*Why it matters:* Without explicit reconciliation, learnings evaporate between sessions. UNIFY creates an audit trail that makes context resumption reliable.

---

### 2. Context-Aware Execution (Token Economics)

**GSD:** Spawns subagents for plan execution, pursuing speed through parallelization.

**ORBIT:** Executes in the current context by default, optimizing for token-to-value efficiency.

The parallel-subagent approach sounds fast on paper. In practice, it's expensive and wasteful:
- Each subagent starts cold, requiring context injection
- Subagents duplicate work the main session already understands
- Coordination overhead compounds across agent boundaries
- Output quality degrades without full project context

ORBIT takes a different stance: **AI is already the speed enhancement.** We don't need to optimize speed at the cost of quality. What we need is to make every token count.

Subagents in ORBIT are reserved for their optimal use case: **parallel research and discovery** — bounded, well-defined information gathering where cold-start overhead is acceptable. Execution stays in-session where the context lives.

*Why it matters:* Parallel execution subagents generate more output faster, but "more garbage faster" isn't progress. ORBIT optimizes for exceptional value per token, consistently, from project start to finish. Token efficiency > speed to done.

---

### 3. Single Next Action

**GSD:** Presents multiple options after each step ("Would you like to: A, B, C, or D?")

**ORBIT:** Suggests ONE best path based on current state.

Decision fatigue is real. ORBIT analyzes project state and recommends the most logical next step. Users can always redirect, but the default is momentum, not menu navigation.

*Why it matters:* Every decision point is a potential context switch. Reducing decisions preserves focus.

---

### 4. Structured Session Continuity

**GSD:** Implicit state via `.continue-here.md` in phase directories.

**ORBIT:** Explicit `HANDOFF-{date}.md` files with loop position, decisions, and prioritized next actions.

ORBIT handoffs are designed for zero-context resumption. They capture not just *where* you stopped, but *why* you were doing what you were doing and *what* decisions led there.

*Why it matters:* Sessions end unexpectedly. Context windows reset. ORBIT handoffs make "resume from cold" a first-class operation.

---

### 5. Acceptance Criteria as First-Class Citizens

**GSD:** Tasks describe what to do.

**ORBIT:** Tasks link to numbered acceptance criteria (AC-1, AC-2, AC-3) with Given/When/Then format.

Every REFINE.md includes explicit acceptance criteria. Every INTEGRATE.md reports pass/fail against those criteria. This creates verifiable quality gates, not just completion checkboxes.

*Why it matters:* "Done" is ambiguous. "AC-3: PASS" is not.

---

### 6. Boundaries That Stick

**GSD:** Scope guidance in plans.

**ORBIT:** Explicit `## Boundaries` section in every REFINE.md with DO NOT CHANGE declarations.

When you specify boundaries, ORBIT treats them as hard constraints, not suggestions. Modifications to protected items require explicit confirmation.

*Why it matters:* Scope creep happens subtly. Explicit boundaries make violations visible before they cascade.

---

### 7. Skill Tracking and Verification

**GSD:** No mechanism for tracking specialized workflow usage.

**ORBIT:** `SPECIAL-FLOWS.md` declares required skills per project. UNIFY audits whether they were invoked.

If your project requires `/commit` or `/review-pr` or custom skills, ORBIT tracks whether they were actually used — preventing "forgot to run the linter" moments.

*Why it matters:* Manual checklists get skipped. Automated audits don't.

---

### 8. Decimal Phases for Interruptions

**GSD:** Integer phases only. Urgent work requires phase insertion and renumbering.

**ORBIT:** Decimal phases (8.1, 8.2) for urgent interruptions without disrupting the roadmap.

When something urgent lands mid-milestone, ORBIT slots it as a decimal phase. The original plan stays intact, and the interruption is clearly marked as an insertion.

*Why it matters:* Real projects have interruptions. The planning system should accommodate them, not fight them.

---

## Philosophy Comparison

| Aspect | GSD | ORBIT |
|--------|-----|------|
| Primary goal | Ship fast | Ship correctly |
| Optimization target | Speed to done | Token-to-value efficiency |
| Loop closure | Flexible | Mandatory |
| Subagent role | Execution (parallel speed) | Discovery only (bounded research) |
| Decision flow | Multiple options | Single best path |
| Session handoff | Implicit | Explicit + dated |
| Quality gates | Completion-based | Acceptance-based |
| Scope control | Guidance | Enforcement |

---

## When to Use Which

**Use GSD when:**
- Speed is the primary constraint
- Project scope is small and well-understood
- Single-session completion is likely
- You don't need audit trails

**Use ORBIT when:**
- Quality and traceability matter
- Work spans multiple sessions
- You need verifiable acceptance criteria
- Scope creep is a concern
- You want explicit reconciliation of plan vs. reality

---

## The Bottom Line

GSD answers: *"How do I get Claude to execute a plan fast?"*

ORBIT answers: *"How do I get maximum value from every token spent?"*

The insight: AI development is already fast. Using Claude at all *is* the speed advantage. The marginal gains from parallel subagent execution come at significant cost — more tokens, more coordination, more garbage output, less coherent results.

ORBIT doesn't try to enhance the enhancement. It takes what AI gives us and makes it **as valuable as possible** — through explicit reconciliation, preserved context, verifiable quality gates, and disciplined loop closure.

ORBIT isn't a replacement for GSD — it's an evolution for projects where **sustainable quality** matters more than **raw throughput**.

---

*ORBIT Framework — Plan. Apply. Unify. Repeat.*
