# FORENSIC OMEGA / v6.66 PROVENANCE AND ARGCORE-005 CONTRACT COMPARISON — 2026-09-25

Status: FORENSIC / NON-AUTHORITATIVE
Scope: Omega recovery lineage, v6.66 recovery lineage, ArgosEvolved recovery lineage, ArgCore-005 exact contract/test evidence.
Disposition: No production changes. No ArgCore changes. No current ArgOS changes.

## 1. Question

Determine whether the Omega recovery family and the later v6.66 recovery substrate share a demonstrable deeper predecessor, then compare both against the exact ArgCore-005 implementation and contract/test evidence.

## 2. Provenance result

No direct shared predecessor is established by the inspected source evidence.

Omega's recovery family is concretely identified by:
- argos-omega/src/recovery/SnapshotCheckpoint.ts
- argos-omega/src/recovery/ContinuityRecovery.ts
- argos-omega/src/recovery/CorruptionTolerance.ts
- related MemoryStore / ContinuityManager components and Phase 5 recovery tests.

Its defining mechanisms are adaptive checkpoint thresholds (500 events / 5 MB / 15 minutes), SHA-256 checkpoint integrity, snapshot-plus-delta replay, full-replay fallback, stream sanitization, and recovery evidence counters.

The v6.66 family is structurally different:
- persistent event logger / JSONL ledger,
- sequence-bounded replay,
- checkpoint sequence anchoring,
- application-state projection,
- rollback from known-good snapshots,
- explicit event persistence and replay behavior.

The current forensic corpus contains no inspected source path, shared commit ancestry, shared class/function identifiers, shared schema, or explicit provenance statement connecting Omega's SnapshotCheckpoint/ContinuityRecovery family to v6.66's EventLogger/ReplayEngine/StateEngine/RollbackEngine family.

Therefore the correct classification is:
OMEGA = DISTINCT HISTORICAL RECOVERY FAMILY.
v6.66 = DISTINCT LATER RECOVERY FAMILY.
COMMON DEEPER PREDECESSOR = NOT ESTABLISHED.

This is an evidence boundary, not a claim that the two families could not have conceptual ancestry. A deeper predecessor would require additional source/commit evidence.

## 3. ArgosEvolved position

ArgosEvolved preserves recovery/state-persistence mechanisms in a later implementation family, including state persistence, KnowledgeVault persistence/recovery language, and validation infrastructure. It also independently preserves/evolves the Omega synaptic loopback mechanism family.

The inspected evidence does not establish that ArgosEvolved is the missing common predecessor between Omega and v6.66. Its recovery/state mechanisms are better classified as a later evolutionary implementation family than as proven ancestry for both historical lines.

## 4. Exact ArgCore-005 contract/test evidence

ArgCore repository:
@kelziejordan/argcore 1.0.0-argcore-005.

Exact inspected contract/test files include:
- tests/contracts/architecture-boundary.test.mjs
- tests/contracts/authority-governance-admission.contract.test.mjs
- tests/contracts/execution-grant-provenance.contract.test.mjs
- tests/contracts/execution-lifecycle.contract.test.mjs
- tests/contracts/governed-execution.contract.test.mjs
- tests/contracts/integrated-governed-execution.contract.test.mjs
- tests/contracts/provenance-audit-record.contract.test.mjs
- tests/integration/state-continuity-vertical-slice.mjs
- src/state/index.js
- src/runtime/lifecycle.js
- src/runtime/provenance.js
- src/recovery/.gitkeep

The package scripts are:
test:contracts = node tests/contracts/architecture-boundary.test.mjs
test:state-continuity = node tests/integration/state-continuity-vertical-slice.mjs
test:all = node tests/run-all-contracts.mjs && npm run test:state-continuity

## 5. What ArgCore-005 actually contracts

The exact tests establish:
- provider/domain dependency boundary enforcement;
- governed request identity including sessionId, principalId, correlationId;
- execution lifecycle ordering: ADMISSION -> GRANT_ISSUED -> GRANT_CONSUMED -> EXECUTION_STARTED -> EXECUTION_OUTCOME;
- correlationId propagation across lifecycle events;
- provenance event identity and validation;
- sensitive credential rejection;
- governed execution admission/denial behavior;
- state continuity with versioned state;
- SHA-256 state integrity;
- correlationId and sourceEventIds in state-transition provenance;
- stale-state rejection;
- tamper detection;
- exportEvidence() returning state/history/integrity/provenance evidence.

Critically, there is no authoritative recovery/resume/checkpoint/replay/restore/rollback contract in ArgCore-005. src/recovery contains only .gitkeep.

The state-continuity test is NOT a recovery test. Its exportEvidence snapshot is evidence export from current state continuity, not a restart/recovery authority.

## 6. Omega against ArgCore-005

Omega matches conceptually on:
- SHA-256 integrity;
- event-derived continuity;
- explicit provenance/evidence counters;
- checkpointed state representation;
- replay of event history.

Omega does NOT satisfy an ArgCore-005 recovery contract because none exists.

Omega also does not directly satisfy the exact ArgCore state-continuity contract:
- its SnapshotState schema is timestamp / lastEvaluationId / driftScore / totalEvents / stateHash;
- it does not implement ArgCore stateId/version/value/history/provenance.authority semantics;
- its recovery reconstructs Omega runtime drift state, not ArgCore constitutional state;
- its CorruptionTolerance can skip invalid records, which is not equivalent to an ArgCore-authoritative integrity policy;
- checkpoint writing is not concurrency-locked.

Classification:
HISTORICAL IMPLEMENTATION EVIDENCE; CONCEPTUAL OVERLAP; NOT ARGCORE-005 CONTRACT IMPLEMENTATION.

## 7. v6.66 against ArgCore-005

v6.66 matches conceptually on:
- durable event evidence;
- sequence-bounded replay;
- checkpoint anchoring;
- state reconstruction;
- rollback;
- event persistence;
- historical integrity/corruption detection.

It does not satisfy an ArgCore-005 recovery contract because none exists.

Its reconstructed StateEngine state is application/runtime state, not demonstrated ArgCore constitutional state. Its historical permissive corruption and sequence-gap behavior are explicitly not accepted as the current recovery standard.

The strongest exact ArgCore overlap is with state continuity concepts: versioned state, source-event linkage, provenance, integrity verification. But v6.66 was not shown to implement the exact ArgCore-005 state object or commitTransition/verifyState/exportEvidence contract.

Classification:
HISTORICAL EXECUTIONALLY VERIFIED RECOVERY EVIDENCE; CONCEPTUAL OVERLAP; NOT ARGCORE-005 CONTRACT IMPLEMENTATION.

## 8. Comparative finding

The important distinction is now clear:

Omega and v6.66 independently demonstrate two historical approaches to continuity/recovery:
1. Snapshot + integrity hash + delta/full replay.
2. Durable event log + sequence checkpoint + replay + rollback.

ArgCore-005 deliberately establishes a different authority boundary:
state identity/version/integrity/provenance are Core concerns, while recovery authority is not yet contracted.

Therefore neither historical recovery system should be promoted into ArgCore merely because it predates or resembles current state-continuity contracts.

The minimum future recovery boundary remains:
ArgCore-authoritative event
-> durable TCS evidence
-> validated checkpoint
-> sequence/identity validation
-> replay
-> ArgCore state reconstruction
-> integrity/provenance verification
-> explicit RESUME or RECOVERY_FAILURE.

That boundary remains NOT PROVEN.

## 9. Authority decision

SHIP: deeper provenance comparison complete to the currently accessible source/commit evidence.

FREEZE:
- ArgCore authority unchanged.
- Current Arg authority unchanged.
- Omega remains historical evidence.
- v6.66 remains separate historical evidence.
- ArgosEvolved remains later evolutionary evidence.
- No historical recovery implementation is promoted.

EXPAND:
Only deeper provenance if new primary evidence becomes available: earlier repository commits, archive source, or explicit lineage documentation capable of establishing common ancestry.

Risk: LOW.
