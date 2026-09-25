# Forensic Source Comparison — Omega -> ArgosEvolved -> Remixed

Status: FORENSIC / SOURCE COMPARISON
Date: 2026-09-25

Authority
This record is documentary only. It does not alter ArgCore, Arg, or any current authority boundary.

Repositories inspected

1. Kelziejordan/ArgOS-Omega-v1
Observed default-head commit: 279b94f4373b426e5b35a5ec8b3857cc42afe863
Latest commit observed: 2026-09-23, "REPAIR: establish repository IP status".
Important source paths include server/gbe, server/icm, server/do, server/tcs, server/nervous-system, server/orchestrator, server/governor, server/leo, server/memory, server/runtime, server/services, and argos-omega/.

2. Kelziejordan/ArgosEvolved
Observed default-head commit: 6d877badd3c259b7fc67afdb0a71eccb9f7f3fff
Latest commit observed: 2026-09-23, "REPAIR: add proprietary repository notice".

3. Kelziejordan/Remixed-Argos-Evolved-2nd-Edition
Observed default-head commit: 4632b5f8ffce6bee17e337d8d390c5ce86c71f8f
Latest commit observed: 2026-06-09, "feat: integrate Firestore for telemetry and audit".

## 1. GBE

Omega:
The executable GBE checks role quarantine through context.identity.status === "quarantined", checks allowed domains, and requires admin permission for production evolution actions. Artifact evaluation also blocks code containing eval()/exec().

ArgosEvolved:
The current implementation was selected as the frozen v1 executable implementation. Its forensic records explicitly state that the historical role-quarantine behavior exists in the earlier corpus but is absent from the later current implementation.

Remixed:
The historical corpus contains the concrete role-quarantine gate.

Classification:
EVOLVED / BEHAVIORAL DIVERGENCE.

The quarantine gate is not merely a naming difference. It is a real behavior difference between generations. Therefore it cannot be labeled "superseded" solely by chronology. The current ArgosEvolved baseline intentionally froze the later behavior while retaining the earlier gate as evidence.

Current Arg:
No corresponding historical GBE implementation was found in the current runtime repository by conceptual source search. Current Arg instead treats governance and canonical authority as boundaries above the old ArgOS service architecture.

Disposition:
PRESERVE AS HISTORICAL MECHANISM. Do not transplant automatically.

## 2. ICM

Omega:
The repository contains a concrete ICM service in the same platform-service family as GBE, VCE, ARRE, DO, TCS, Governor, and LEO.

ArgosEvolved:
The direct executable implementation hard-codes platform identity to argos-core/1.0.0, supplies a default experimental role identity, and leaves setRoleStatus() as a no-op.

Remixed:
Prior forensic evidence establishes a concrete but thin ICM with the same important limitation: role-status mutation is not actually implemented.

Classification:
DUPLICATED / INCOMPLETE ACROSS GENERATIONS.

The historical ICM family is important as evidence that identity/context was treated as a first-class platform service, but the inspected implementations do not establish a mature authoritative identity-state mechanism.

Current ArgCore:
Identity and state authority are now explicitly owned by ArgCore. Therefore these historical ICM implementations are historical design evidence, not candidates for direct reuse.

Disposition:
HISTORICAL DESIGN/IMPLEMENTATION EVIDENCE. No direct promotion.

## 3. Deterministic Optimizer

Omega:
Concrete artifact analysis detects large string payloads and oversized metadata and produces optimization suggestions. Application may trim metadata to ten properties and add an optimization timestamp.

ArgosEvolved:
The current implementation was frozen with a known type-safety defect involving unsafe any casts.

Remixed:
The forensic corpus establishes a materially similar executable optimizer.

Classification:
DUPLICATED / STABLE MECHANISM FAMILY.

The mechanism is not unique to one historical repository. It appears to have persisted across generations with relatively small implementation changes.

Disposition:
HISTORICAL IMPLEMENTATION FAMILY. Current relevance is an existing known concept, not a newly discovered unique mechanism.

## 4. TCS

Omega:
TCS keeps trace entries in an instance-local in-memory array. It constructs chronicles and additionally ingests a summary into KnowledgeVault.

ArgosEvolved:
Later TCS evolution moved toward static/shared in-memory trace collection and explicit retrieval. The frozen baseline records a persistence limitation.

Remixed:
Earlier TCS was instance-local and did not establish durable standalone trace storage.

Classification:
EVOLVED / PERSISTENCE-LIMITED.

The historical progression is clear: trace capture remained important, while storage semantics evolved. None of the inspected historical TCS implementations independently establishes the durable canonical event-history authority now required by current Arg.

Disposition:
PRESERVE EVOLUTION HISTORY. Do not treat historical TCS persistence as current authority.

## 5. Communication Bus

Omega:
Concrete singleton bus using EventEmitter, bounded in-memory history, UUID normalization, TCS logging, typed routing, and wildcard broadcast. BranchMessage includes source, target, timestamp, type, content, confidence, and priority metadata.

ArgosEvolved:
The later implementation expands the message contract with stronger causal/trajectory semantics and priority-aware routing. Current source imports show direct use by cognitive, trading, LifeOS, and reflex services.

Remixed:
Earlier forensic comparison found executable typed routing, wildcard broadcast, TCS logging, and bounded history, but a materially smaller message contract.

Classification:
EVOLVED MECHANISM FAMILY.

The bus is not unique to Omega. The later ArgosEvolved branch represents a substantive contract expansion rather than a wholly new subsystem.

Current Arg:
Current architecture treats Communication Bus as an existing canonical interface that VTM and other analytical layers must consume rather than redefine.

Disposition:
HISTORICAL PREDECESSOR / ALREADY RECONCILED CONCEPT. No transplant.

## 6. Knowledge Vault

Omega:
The inspected Omega source tree contains a dedicated memory/vault/service family and the Omega lineage includes continuity/recovery work.

ArgosEvolved:
KnowledgeVault directly persists JSONL, maintains an index, performs integrity/audit work, supports heuristic compilation, local recovery, and optional Firestore synchronization. The forensic record explicitly notes malformed-line recovery and hash-chain healing in the later branch.

Remixed:
Historical source inspection established a local JSONL/hash-chain/indexing implementation.

Classification:
EVOLVED MECHANISM FAMILY WITH MATERIAL HARDENING.

The later implementation is not simply a renamed copy: persistence, malformed-line recovery, hash-chain handling, and Firestore synchronization were added or strengthened.

Current Arg:
Current Arg references KnowledgeVault as an authority that must remain canonical and exposes it through read/verification adapters for higher-level analysis. Current recovery authority remains deliberately unproven at the ArgCore boundary.

Disposition:
ALREADY REPRESENTED IN CURRENT LINEAGE. Historical versions remain useful for provenance only.

## 7. Governor / LEO

Omega:
Governor and LightweightLEO form an integrated governance/evolution mechanism. The Governor handles guardrail, validation, repair-failure, evolution, and freeze triggers; LEO carries lineage state. The inspected Governor also consults KnowledgeVault integrity and heuristics before promotion.

ArgosEvolved:
The verified v1 repair/evolution path is VCE -> Governor notification -> ARRE proposal/application -> VCE re-validation, with Governor integrated with InMemoryLEO.

Remixed:
Historical source family contains the same broad platform-service architecture.

Classification:
EVOLVED / INTEGRATED MECHANISM FAMILY.

Important limitation:
Historical claims about LEO validation, SURE, confidence thresholds, or other more elaborate promotion logic are not automatically supported merely because they appear in documentation. The inspected current forensic record explicitly rejects unsupported variants.

Current Arg:
Governor remains authoritative for governed decisions. Current work is testing the actual ArgOS -> ArgCore governance boundary rather than transplanting historical Governor code.

Disposition:
HISTORICAL PREDECESSOR / CURRENT CONCEPT RECONCILED. No direct reuse.

## 8. Pipeline Orchestrator

Omega:
The inspected implementation is concrete. It executes pre-GBE gating, artifact production, post-GBE gating, VCE validation, ARRE repair, re-validation, deterministic optimization, and TCS logging, with Governor escalation on blocking conditions.

ArgosEvolved:
The forensic record identifies the later current branch as the stronger executable implementation compared with the June 9 remix skeleton.

Remixed:
The archived PipelineOrchestrator is materially TODO-heavy and is weaker evidence of executable capability.

Classification:
EVOLVED EXECUTION PIPELINE.

This is significant: the historical orchestration architecture was not merely speculative. Omega contains a real executable closed-loop pipeline. However, this does not prove compatibility with the current ArgCore governed execution contract.

Current Arg:
The current runtime has its own orchestration implementation and explicitly consumes ArgCore rather than copying the historical Omega pipeline.

Disposition:
HIGH-VALUE HISTORICAL IMPLEMENTATION EVIDENCE. Compare semantics when useful; do not transplant architecture.

## 9. Omega-specific continuity/recovery

The Omega repository contains a distinct continuity/recovery implementation family, including dedicated phase-validation and snapshot-related tests under argos-omega.

The ArgosEvolved forensic record explicitly classifies this as Omega-layer material and states that it must not be retroactively attributed to ArgosEvolvedv1.

Current Arg evidence independently records recovery authority as NOT PROVEN at the ArgCore-005 boundary.

Classification:
UNIQUE OMEGA-LAYER MECHANISM FAMILY.

This is the strongest genuinely unique historical mechanism identified in this pass.

Disposition:
PRESERVE AS OMEGA PROVENANCE. Do not import. Future recovery work must prove its relationship to current ArgCore contracts independently.

## 10. Omega experimental mechanism lead

Omega contains an explicit `experiments/synaptic_loopback.ts` artifact.

Classification:
UNRESOLVED EXPERIMENTAL MECHANISM.

No current authority is assigned. It requires a separate source inspection before deciding whether it is novel, derivative, or obsolete.

## Summary classification

Genuinely unique or high-value historical mechanisms identified:
- Omega continuity/recovery implementation family.
- Omega-specific phase validation/snapshot test lineage.
- Omega experimental synaptic-loopback artifact.
- Concrete historical closed-loop PipelineOrchestrator implementation as evidence of an earlier executable integration model.

Repeated/evolved rather than unique:
- GBE.
- ICM.
- Deterministic Optimizer.
- TCS.
- Communication Bus.
- Knowledge Vault.
- Governor/LEO.

Already represented or reconciled in current lineage:
- Knowledge Vault as a canonical current interface.
- Communication Bus as a protected canonical interface.
- Governor as current governance authority.
- Orchestration as current Arg runtime responsibility.
- Recovery deliberately remains NOT PROVEN rather than inherited from Omega.

## Forensic conclusion

The historical corpus is not one monolithic predecessor. It contains at least three meaningful implementation generations:

Remixed:
earlier executable/design snapshot with several concrete mechanisms and several skeletons.

Omega:
broader integrated runtime with concrete governance, orchestration, self-healing, lineage, continuity/recovery, and experimental subsystems.

ArgosEvolved:
later recovery/selection branch that explicitly reconciled competing historical mechanisms and froze a narrower v1 boundary.

The current Arg/ArgCore lineage is therefore not missing an obvious wholesale predecessor. The strongest historical value is provenance and behavioral comparison. The one area that remains materially distinct is Omega continuity/recovery, which must stay quarantined from current authority until independently proven.

No current repository was modified by this comparison.
