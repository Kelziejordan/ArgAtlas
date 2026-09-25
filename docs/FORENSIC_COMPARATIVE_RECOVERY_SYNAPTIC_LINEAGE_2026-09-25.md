# FORENSIC COMPARATIVE RECOVERY AND SYNAPTIC LINEAGE 2026-09-25

Status: FORENSIC / NON-AUTHORITATIVE
Scope: ArgCore, current Arg, ArgosEvolved, and Omega recovery/synaptic lineage
Purpose: compare historical mechanisms and provenance without changing current authority or implementation.

## 1. Recovery comparison result

The comparison resolves an important distinction: Omega recovery is a real historical implementation family, but it is not the direct historical source of every recovery mechanism now discussed in current ArgOS.

Omega contains:
- adaptive snapshot thresholds;
- SHA-256 snapshot integrity;
- corruption sanitization;
- snapshot-plus-delta replay;
- full replay fallback;
- JSONL event persistence;
- continuity reconstruction;
- dedicated snapshot/recovery tests.

ArgosEvolved explicitly freezes Omega continuity/recovery as an Omega-layer artifact and states that it must not be back-projected onto the historical ArgosEvolved v1 runtime.

Therefore:
OMEGA RECOVERY = HISTORICAL IMPLEMENTATION FAMILY / PRESERVE AS PROVENANCE.

## 2. ArgCore-005 comparison

Current ArgCore was inspected at the repository level and at its recovery path.

The authoritative repository contains `src/recovery/.gitkeep` but no recovered recovery implementation under that boundary.

The current ArgOS evidence record separately establishes that the final ArgCore-005 forensic search found no authoritative recovery/resume contract covering recovery, continuity, checkpoint, resume, restore, replay, snapshot, rollback, interruption, or related authority concepts.

This is an absence finding. It does not mean recovery can never be added.

Result:
ARGCORE-005 RECOVERY AUTHORITY = NOT ESTABLISHED.

This preserves the existing authority boundary. Omega cannot fill the missing Core contract by historical inheritance.

## 3. Current Arg comparison

Current Arg contains substantial recovery archaeology, but its own documents deliberately separate historical execution evidence from current capability.

The current forensic mapping identifies an independently verified v6.66 recovery substrate containing:
- persistent event logging;
- sequence identity;
- checkpoint anchors;
- known-good snapshots;
- versioned snapshots;
- replay;
- rollback;
- replay isolation;
- startup recovery;
- corruption detection.

The current mapping explicitly classifies these as mechanisms to RECOVER, ADAPT, REPLACE, or PRESERVE CONCEPT rather than current capability.

It also explicitly rejects several historical weaknesses:
- buffered asynchronous persistence with crash-loss windows;
- permissive corruption skipping;
- duplicate tolerance;
- non-atomic checkpoint publication;
- historical StateEngine as a parallel state authority.

The minimum current recovery boundary is defined as:

ArgCore authoritative event
-> durable TCS evidence
-> validated checkpoint
-> sequence/identity validation
-> replay
-> ArgCore state reconstruction
-> integrity/provenance verification
-> explicit RESUME or RECOVERY_FAILURE

That boundary remains NOT PROVEN.

## 4. Important lineage finding

There are therefore at least two distinct historical recovery lineages in the current forensic corpus:

A. Omega lineage:
SnapshotCheckpoint -> ContinuityRecovery -> MemoryStore/ContinuityManager

B. v6.66 lineage:
Persistent Event Logger -> checkpoint sequence boundary -> ReplayEngine -> StateEngine -> RollbackEngine

The current Arg forensic records explicitly identify the v6.66 lineage as executionally confirmed and map it toward the frozen ArgCore/ArgOS/TCS/VTM boundary.

The current records also explicitly preserve Omega recovery separately.

Therefore we should NOT collapse these into a single provenance chain without further evidence.

Classification:
OMEGA = separate historical recovery family.
V6.66 = separate historically verified recovery substrate.
CURRENT ARG = reconciliation/mapping layer, not yet authoritative recovery implementation.
ARGCORE = authority boundary currently lacking recovery contract.

## 5. State-authority distinction

Omega's ContinuityRecovery reconstructs a compact runtime state from snapshot plus events.

The v6.66 StateEngine reconstructs application/runtime state.

Neither historical state mechanism becomes current constitutional authority.

The current mapping correctly replaces the historical StateEngine dependency with ArgCore state authority.

This is a key architectural-provenance result: recovery mechanisms may be recoverable while historical state authorities are not.

## 6. Synaptic loopback lineage

Omega's `experiments/synaptic_loopback.ts` directly exercises:

DiscoveryBranch
-> CommunicationBus
-> LifeOSBranch / TradingBranch
-> state transition
-> RESULT message
-> TCS logging.

ArgosEvolved preserves this mechanism family.

Its current `server/validation/ValidationRun.ts` imports CommunicationBus, DiscoveryBranch, LifeOSBranch, and TradingBranch and contains a dedicated “Test 15: Synaptic Loopback verification.”

ArgosEvolved's CommunicationBus is also a direct evolution of the Omega bus pattern:
- typed routing remains;
- in-memory history remains;
- TCS logging remains;
- UUID/timestamp normalization remains;
- priority handling was added;
- causalChainId was added;
- branchTrajectory was added;
- high/critical messages bypass the normal queue.

ArgosEvolved therefore did NOT deliberately discard the synaptic loopback mechanism.

Classification:
SYNAPTIC LOOPBACK = PRESERVED AND EVOLVED IN ARGOS EVOLVED.

## 7. Current Arg synaptic status

A source search of current Arg for the distinctive Omega/ArgosEvolved synaptic identifiers did not find an equivalent Discovery -> LifeOS/Trading loopback implementation.

No current Arg match was found for the historical `synaptic`, `branchTrajectory`, or `causalChainId` identifiers, and the historical DISCOVERY branch pattern is not part of the current runtime evidence.

Current Arg instead treats Communication Bus as a canonical interface consumed by higher-level runtime components, with current certification focused on governed task lifecycle, correlation continuity, consequential ArgCore events, and explicit recovery lineage.

Therefore:
ARGOS-EVOLVED = PRESERVED/EVOLVED SYNAPTIC MECHANISM.
CURRENT ARG = NOT EVIDENCED AS CARRYING THE HISTORICAL SYNAPTIC LOOPBACK IMPLEMENTATION.

This is not evidence that the concept was abandoned architecturally; it is only a source-level finding that the concrete historical loopback implementation is not present in the current Arg runtime corpus inspected here.

## 8. Forensic conclusion

The forensic picture is now substantially clearer.

Recovery:
- Omega supplied a concrete snapshot/corruption/replay family.
- v6.66 supplied a separate, independently executionally verified durable-log/checkpoint/replay/rollback family.
- ArgosEvolved explicitly kept Omega recovery quarantined as Omega provenance while preserving a different canonical v1 boundary.
- Current Arg has reconciled historical recovery mechanisms conceptually, but has not promoted them into recovery authority.
- ArgCore-005 does not currently expose the authoritative recovery contract required to close that boundary.

Synaptic loopback:
- Omega introduced a concrete branch-reaction loopback experiment.
- ArgosEvolved retained and expanded it.
- Current Arg does not currently contain the concrete historical branch loopback implementation.

No authority boundary changed.

## 9. Next forensic gate

The remaining useful forensic question is now narrower:

1. Determine the exact provenance of the v6.66 recovery substrate relative to Omega and ArgosEvolved.
2. Identify whether any Omega recovery component was independently reimplemented in v6.66 or whether the two families share a deeper predecessor.
3. Compare the current Arg recovery mapping against the exact ArgCore-005 contract tests once available.
4. Preserve the synaptic loopback as historical/evolved mechanism evidence without importing it into current Arg.

No implementation change is authorized by this record.

## Disposition

SHIP: comparative recovery and synaptic lineage analysis complete.
FREEZE: authority boundaries unchanged.
EXPAND: only into deeper provenance reconciliation between Omega and v6.66, followed by exact ArgCore-005 contract/test comparison.
Risk: LOW.
