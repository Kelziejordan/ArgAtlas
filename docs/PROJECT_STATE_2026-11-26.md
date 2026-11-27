# ARG Functional v1 — Persistence Integration Checkpoint

Date: 2026-11-26 (America/Edmonton)
Status: CI-VERIFIED / DRAFT PR / NOT MERGED / NOT DEPLOYED

## Operating context

This checkpoint records a bounded ARG Functional v1 persistence deliverable within the active G2 technical workstream. It does not close G2 as a whole or replace the G1-G10 completion register. The G1-G10 register and 2026-11-24 operating/PR reconciliation addendum remain the controlling records for all remaining G2 work.

## Canonical repository and exact source state

- Repository: `Kelziejordan/Arg`
- Canonical main baseline: `9158fdec5f7cad33727b443dcb110ed958b8ab6f`
- Working branch: `work/arg-functional-v1-persistence-001`
- Exact CI-verified branch head: `41a8989ca87ad1bcce770ac90d473e9ccd4847ab`
- Draft PR: [#143 — ARG Functional v1: unify durable TCS and Knowledge Vault persistence](https://github.com/Kelziejordan/Arg/pull/143)
- PR base: `main` at `9158fdec5f7cad33727b443dcb110ed958b8ab6f`
- ArgCore dependency: `@kelziejordan/argcore@1.0.0-argcore-008`
- ArgLearn dependency pin: `d129803e6ef09920c47b8a3d8550e84bb97c1ff7`

PR #143 is open, draft, and unmerged. Its current head is the exact commit listed above.

## Protected boundaries and decisions

- ArgCore 008 remains frozen and unchanged.
- Accepted ARG and ARG-X architectures remain frozen and separate.
- No ArgCore source or dependency upgrade was included in PR #143.
- No production configuration, Vercel configuration, or deployment settings were changed.
- No production database migration or production deployment occurred. The new migration was applied only to disposable PostgreSQL in CI.
- No protected merge occurred.
- PR #142 is not the implementation target or a prerequisite for this milestone.
- The previously resolved Preview deployment concern is accepted as resolved per the operator's instruction and is not an acceptance gate. No further evidence request is authorized or needed.

## Implemented boundary

1. Added an append-only PostgreSQL Knowledge Vault journal in the same database as PostgreSQL TCS.
2. Added a shared asynchronous persistence factory for PostgreSQL production mode and file-backed regression mode.
3. Converted the ArgLearn memory-cycle and learning-enabled orchestrator boundary to await asynchronous TCS/Vault operations.
4. Connected the Companion conversation and operator-bootstrap entrypoints to the shared persistence factory and ArgLearn cycle; the execution API uses the same factory.
5. Captured one deterministic Vault observation per completed correlation, keyed as `tcs-observation:<correlationId>`.
6. Bound observation provenance to authoritative TCS records using `tcs:<sequence>:<sourceEventId>` references through the durable `VERIFIED_RESULT` boundary.
7. Added restart reconciliation that derives missing observations from completed TCS records. Recovery does not call the execution provider.
8. Made unavailable/corrupt Vault storage fail closed before provider invocation. If a completed TCS outcome exists but Vault capture is interrupted, recovery completes the observation without replaying the external action.
9. Preserved the existing file-backed implementation path for regression tests.

TCS remains the authoritative execution history. The Knowledge Vault is derived memory. The two journals are not represented as one atomic transaction; interruption is handled through deterministic reconciliation.

## Acceptance matrix

| Acceptance criterion | Status | Evidence |
|---|---|---|
| PostgreSQL TCS is authoritative across Companion entrypoints | CI-VERIFIED | Companion PostgreSQL E2E and operator-bootstrap integration |
| Knowledge Vault uses the same PostgreSQL backend | CI-VERIFIED | PostgreSQL migration and adapter integration |
| Affected TCS and memory operations are awaited | CI-VERIFIED | Orchestration and memory-lifecycle suites |
| Companion completion creates exactly one correlated observation | CI-VERIFIED | Both Companion entrypoint tests |
| Observation refs identify authoritative TCS records through VERIFIED_RESULT | CI-VERIFIED | Capture-boundary provenance assertions |
| Fresh-process restart recovers TCS/Vault state | CI-VERIFIED | Separate Node-process recovery tests and Companion evidence verification |
| Replay and concurrent capture do not duplicate execution or observations | CI-VERIFIED | Replay rejection and concurrent idempotency tests |
| Vault/database failures fail closed without provider invocation | CI-VERIFIED | Unavailable-storage provider-invocation guard |
| Existing file-backed regression behavior remains valid | CI-VERIFIED | Memory lifecycle, conversation bridge, and orchestration suites |

## Exact-head CI evidence

All four workflows completed successfully against `41a8989ca87ad1bcce770ac90d473e9ccd4847ab`:

- [Arg B1 Denial Boundary — run 45918518442](https://github.com/Kelziejordan/Arg/actions/runs/45918518442) — SUCCESS
- [ArgOS Proof Spine — run 45918518622](https://github.com/Kelziejordan/Arg/actions/runs/45918518622) — SUCCESS
- [ArgOS Memory Lifecycle — run 45918518412](https://github.com/Kelziejordan/Arg/actions/runs/45918518412) — SUCCESS
- [Arg Orchestration CI — run 45918518696](https://github.com/Kelziejordan/Arg/actions/runs/45918518696) — SUCCESS
  - orchestration job — SUCCESS
  - PostgreSQL TCS integration job — SUCCESS

The PostgreSQL job emitted these acceptance results:

- PostgreSQL TCS integration: claims, transitions, concurrency, fresh-process recovery, hash-chain validation, and unavailable-database fail-closed — PASS.
- PostgreSQL Knowledge Vault: hash chain, exactly-once observation identity, concurrent idempotency, fresh-process recovery, and fail-closed storage — PASS.
- Companion -> ARG orchestrator -> ArgCore 008 -> PostgreSQL TCS + Knowledge Vault: explicit approval, exactly-once observation, replay hold, and fresh-process recovery — PASS.
- Actual Companion UI -> explicit approval -> governed Playwright action -> PostgreSQL TCS + Knowledge Vault -> exactly-once observation -> replay rejection -> fresh-process evidence verification — PASS.

The memory-lifecycle and orchestration workflows also passed the existing file-backed lifecycle, ArgLearn, Companion identity, Governor, conversation-bridge, and replay regression suites.

## Status classification

- IMPLEMENTED: yes.
- TESTED: yes.
- CI-VERIFIED: yes, at exact head `41a8989ca87ad1bcce770ac90d473e9ccd4847ab`.
- MERGED: no.
- DEPLOYED: no.
- PROVEN IN PRODUCTION: no.

CI verification is not production proof. PR #143 remains draft and unmerged pending review and explicit merge authorization.

## Failure budget and recovery rules

- PostgreSQL or Vault unavailable before execution: fail closed; provider invocation is blocked.
- Vault journal corruption or hash-chain mismatch: reject the memory operation; do not silently repair or bypass.
- TCS outcome committed but Vault observation missing: reconcile from TCS after restart; never repeat the provider call.
- Duplicate observation identity with matching content: return the existing durable observation.
- Duplicate identity with conflicting content: reject with an idempotency conflict.
- Ambiguous database commit: reconcile durable state before any retry; do not repeat an external effect.

## Relationship to G2 and next gate

This integration proves the ARG Functional v1 persistence slice. It does not declare the full G2 gate or the complete canonical intelligence lifecycle complete. Remaining G2 work continues to be governed by the G1-G10 register and the 2026-11-24 operating/PR reconciliation addendum.

Immediate next actions:
1. Review PR #143 and its exact-head evidence.
2. Merge only after explicit operator authorization.
3. Complete post-merge verification. Production migration/deployment remains a separate, explicitly authorized gate.
4. After ARG Functional v1 is shipped, the next user-selected milestone is LinkedIn profile and professional positioning.
5. Resume remaining G2 work from the existing register when the user selects the next technical gate. Preserve the Governance Benchmark as a separate secondary track.

## Continuity rule

Preserve this checkpoint as an immutable record of the 2026-11-26 state. Update the master/project navigation files when the PR state or acceptance evidence changes. Do not retroactively rewrite earlier checkpoints.
