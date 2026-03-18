# COORDINATION

**Initiated by:** {{INITIATOR_PROJECT}}
**Date:** {{DATE}}
**Mode:** {{MODE}} <!-- multi-repo | monorepo -->

---

## Active Cross-Builds

| Contract | Receiver | Status | Refine |
|----------|----------|--------|--------|
| {{CONTRACT_ID}} | {{RECEIVER_PROJECT}} | PENDING | — |

<!-- Statuses: PENDING | REFINE_CREATED | BUILD_RUNNING | BUILD_COMPLETE | INTEGRATED | BLOCKED -->

---

## Timeline

```
{{DATE}} — Cross-build initiated by {{INITIATOR_PROJECT}}
```

---

## Blocked / Notes

<!-- Any blockers, interface changes, or coordination notes go here -->

---

## How to Update

When receiver project creates their REFINE:
1. Update Status → REFINE_CREATED
2. Fill Refine column with path

When receiver BUILD completes:
1. Update Status → BUILD_COMPLETE

When initiator integrates the contract:
1. Update Status → INTEGRATED

---

*Last updated: {{DATE}}*
