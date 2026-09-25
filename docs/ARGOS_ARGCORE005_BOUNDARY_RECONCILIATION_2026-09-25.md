# ARGOS -> ARGCORE-005 BOUNDARY RECONCILIATION — 2026-09-25

Status: COMPLETE / NON-DESTRUCTIVE
Scope: Determine the exact evidence ArgOS must demonstrate at the frozen ArgCore-005 boundary.
Rule: No ArgCore-005 source or tests modified. No ArgOS runtime behavior modified by this reconciliation.

## 1. Boundary principle

ArgCore-005 is the frozen foundational boundary.

ArgOS does not need to reproduce ArgCore internals. It must demonstrate that its orchestration and continuity behavior supplies valid inputs to the ArgCore contracts, preserves the identities/provenance those contracts require, and does not claim recovery authority that ArgCore-005 has not established.

ArgCore remains separate. The boundary is an interface/adaptation boundary, not an architectural merge.

## 2. What ArgCore-005 actually requires from a caller

At the inspected contract level, an ArgOS caller must provide and preserve:

1. Request identity
   - sessionId
   - principalId
   - correlationId
   - prompt
   - one to four providers
   - positive call/token budget

2. Authority binding
   - principal matches request principal
   - authority resource matches session
   - authority action is the permitted action
   - authority is not consumed
   - authority has not expired

3. Governance/resource admission
   - policy explicitly allows
   - call budget remains
   - token budget remains
   - execution is not invoked when admission denies

4. Grant provenance
   - ArgCore-issued grant remains bound to correlationId, principalId, action, and resource
   - a grant cannot be consumed twice

5. Consequential event provenance
   - ArgCore events carry the same correlationId
   - grant-linked events carry grantId
   - event types remain within the ArgCore contract
   - sensitive credentials are not placed into provenance

6. State continuity
   - state has stateId, version, value, and valid SHA-256 integrity
   - transitions use the current state version
   - transitions carry correlationId, principalId, action, and sourceEventIds
   - state mutation remains distinguishable from evidence export

## 3. What ArgOS currently demonstrates

The inspected ArgOS adapter and tests already demonstrate:

- ArgCore package/version binding.
- Request construction through ArgCore.
- Task-to-authority binding before governed execution.
- Correlation preservation through ArgOS lifecycle transitions.
- Correlation preservation into consequential ArgCore events.
- ArgCore-owned state transition provenance.
- Source-event linkage from state transition to ArgCore event IDs.
- TCS durability and correlation-indexed evidence.
- VTM reconstruction from supplied sequential evidence.
- Explicit recovery-resume gate remaining closed.

These are useful boundary proofs.

## 4. Required boundary evidence that is still missing

### B1 — Denial-to-no-mutation invariant

ArgOS must demonstrate that when ArgCore admission denies execution, ArgOS does not perform a consequential execution and does not commit a state transition representing that denied execution.

This is important because the current ArgOS state-lineage adapter calls commitTransition after execute() returns. The existing state-lineage test covers successful execution only.

Required proof:

DENIED admission
-> executor not called
-> no consequential ArgCore execution events
-> no ArgCore state commit attributed to the denied execution.

Classification: OPEN.

### B2 — Failed-execution state semantics

ArgOS must explicitly establish what state transition is permitted after an ArgCore-governed execution returns FAILED.

A failed execution must not silently appear as a successful state transition.

The boundary must distinguish at least:

- execution outcome;
- last verified state;
- terminal failure evidence;
- any state mutation that is intentionally allowed after failure.

Classification: OPEN.

### B3 — Event-to-state causal linkage

The state transition already records sourceEventIds. ArgOS must prove that those IDs identify the exact ArgCore events belonging to the same governed execution and correlationId, and that no unrelated event can be substituted.

Classification: PARTIALLY VERIFIED; explicit adversarial boundary proof OPEN.

### B4 — Lifecycle mapping

ArgOS has its own lifecycle:

CREATED -> PLANNING -> AUTHORIZING -> AUTHORIZED -> EXECUTING -> OBSERVING -> VERIFYING -> COMPLETED/FAILED/RECOVERING.

ArgCore-005 has its tested event lifecycle:

ADMISSION -> GRANT_ISSUED -> GRANT_CONSUMED -> EXECUTION_STARTED -> EXECUTION_OUTCOME.

ArgCore's repository declaration separately names its canonical conceptual lifecycle:

REQUESTED -> IDENTIFIED -> AUTHORIZED -> GOVERNED -> BUDGETED -> EXECUTED -> VALIDATED -> PROVENANCE CAPTURED -> FINALIZED.

ArgOS must not claim these are identical.

Required proof is an explicit boundary mapping showing which ArgOS states/events correspond to which ArgCore contract events and where the mapping is intentionally one-to-many or many-to-one.

Classification: OPEN.

### B5 — Durable evidence to reconstruction boundary

ArgOS already has durable TCS and VTM reconstruction tests.

The missing boundary proof is that the exact ArgCore consequential event lineage used for a state transition is the same lineage persisted into durable TCS and subsequently supplied to reconstruction.

Classification: PARTIALLY VERIFIED; consequential cross-system lineage proof OPEN.

### B6 — Recovery/resume authority boundary

ArgOS may record recovery intent, preserve durable evidence, and reconstruct a previously verified state.

It must not claim that this establishes ArgCore-authoritative recovery/resume.

Current required disposition:

RECOVERY EVIDENCE: permitted to be demonstrated.
STATE RECONSTRUCTION: permitted to be demonstrated.
AUTOMATIC RESUME AS ARGCORE-AUTHORIZED: NOT PROVEN.
CONSEQUENTIAL RESUME: CLOSED until an authoritative recovery contract exists.

Classification: CLOSED BY BOUNDARY, not a defect in ArgOS.

## 5. Exact minimum boundary proof chain

The next certification target should be one controlled chain:

ArgOS task
-> correlationId assigned
-> ArgCore admission
-> grant issued
-> grant consumed
-> consequential ArgCore event
-> ArgOS lifecycle transition
-> state transition using the exact ArgCore event IDs
-> durable TCS record preserving the same correlation/source-event relationship
-> reconstruction from durable evidence
-> reconstructed state compared with the last verified state
-> no automatic resume claimed.

This chain tests continuity across the boundary without expanding either architecture.

## 6. What is NOT required at this stage

Do not:

- modify ArgCore-005;
- import Omega/v6.66 recovery code into ArgCore;
- redesign ArgOS around ArgCore;
- declare ArgCore recovery authority;
- equate TCS durability with ArgCore recovery;
- equate VTM reconstruction with ArgCore resume authorization;
- expand Domain B into a broader architecture project;
- treat historical recovery implementations as missing code.

## 7. Boundary status

PASS / INTERNALLY VERIFIED:
- request identity propagation;
- authority/resource binding;
- admission gating;
- grant provenance;
- consequential-event correlation;
- basic ArgCore state continuity;
- TCS durability;
- bounded VTM reconstruction.

OPEN:
- denial-to-no-mutation;
- failed-execution state semantics;
- adversarial event-to-state causal linkage;
- explicit lifecycle mapping;
- durable ArgCore-event-to-reconstruction lineage.

CLOSED / NOT PROVEN:
- ArgCore-authorized recovery/resume.

## 8. SHIP / FREEZE / EXPAND

SHIP:
The boundary contract is now explicit.

FREEZE:
No ArgCore-005 change. No historical transplant. No architectural expansion.

NEXT:
Implement only the smallest tests required to close B1-B5, one boundary invariant at a time. B6 remains closed until an authoritative recovery contract exists.

EXPAND:
Blocked until the boundary evidence is complete.

Risk: LOW for the reconciliation itself.

## 9. Bottom line

ArgOS does not currently need to prove that it contains ArgCore.

It needs to prove that it crosses the ArgCore boundary correctly.

The immediate evidence target is therefore continuity of identity, authority, consequential event provenance, state mutation, durable evidence, and reconstruction — with recovery/resume deliberately kept outside the claimed ArgCore authority until it is actually specified and proven.
