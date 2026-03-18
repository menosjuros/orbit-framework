# CONTRACT REQUEST

**From:** {{INITIATOR_PROJECT}}
**Date:** {{DATE}}
**Cross-refine:** {{INITIATOR_REFINE_PATH}}
**Coordinator:** {{COORDINATOR_PROJECT}}

---

## What I need you to implement

{{CONTRACT_ITEMS}}

<!-- Example:
- POST /api/inadimplencia/calcular
  Body: { contrato_id: string, data_ref: string }
  Response: { status: string, parcelas: Parcela[], valor_total: number }

- Webhook: inadimplencia-detected
  Payload: { contrato_id, tipo, data_referencia }
-->

## Acceptance Criteria expected from you

{{ACCEPTANCE_CRITERIA}}

<!-- Example:
- AC-1: endpoint returns 200 with correct payload shape
- AC-2: returns 404 if contrato not found
- AC-3: webhook fires within 500ms of status change
-->

## Dependency

{{DEPENDENCY_NOTE}}

<!-- Example:
My REFINE enters BUILD only after your AC-1 is integrated.
If your AC changes the contract shape, update this file and notify coordinator.
-->

---

## Response

<!-- The receiving project fills this section when their REFINE is created -->

**Status:** PENDING | REFINE_CREATED | BUILD_COMPLETE | INTEGRATED
**Refine path:** {{RECEIVER_REFINE_PATH}}
**Notes:** {{RECEIVER_NOTES}}
