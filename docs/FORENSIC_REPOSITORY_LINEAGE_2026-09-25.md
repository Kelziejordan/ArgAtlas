# Forensic Repository Lineage Inventory — 2026-09-25

Status: FORENSIC INVENTORY / NOT AN ARCHITECTURAL CHANGE

Purpose
Identify remaining Arg/ArgOS/Omega/Anchor-adjacent repositories, inspect actual repository contents and declared dependencies, distinguish historical material from potentially unique mechanisms, and preserve provenance leads for later reconciliation.

Authority rule
This inventory is documentary. It does not promote any historical repository into current runtime authority. Historical repositories may inform the current system; only the designated current authority may define current behavior.

## 1. High-priority historical ArgOS lineage

### Kelziejordan/ArgOS-Omega-v1
Classification: historical ArgOS/Omega runtime family; private; non-archived; approximately 2,014 KB.

Observed contents include:
- full application source structure;
- `core/`, `server/`, `experiments/`, `data/`, `argos-omega/`;
- Dockerfile and deployment material;
- `SYSTEM_MAP.md`;
- `IP-STATUS.md` marking the repository as private/proprietary/historical core.

Material of forensic interest:
- GBE, VCE, ARRE, DO, TCS, Governor, LightweightLEO, ICM, Metrics, orchestration, nervous-system/event-bus, app-builder organisms, Digital Hands, trading workflow, validation suites.
- `SYSTEM_MAP.md` describes a seven-stage governance/self-healing pipeline and explicitly identifies the platform services above.
- The repository contains the Omega continuity/recovery implementation family previously identified in the ArgOS forensic corpus.

Disposition:
HISTORICAL / HIGH-VALUE REFERENCE. Do not treat as current runtime authority. Preserve separately from ArgosEvolved and current Arg/ArgCore lineage.

### Kelziejordan/ArgosEvolved
Classification: historical/current-transition ArgOS implementation family; private; non-archived; approximately 6,040 KB.

Observed contents include:
- executable TypeScript/React/Express/Vite implementation;
- canonical baseline and state records;
- salvage matrix;
- continuity doctrine;
- tests and deployment material.

Forensic significance:
The repository contains direct executable implementations and an explicit salvage matrix distinguishing VERIFIED, VERIFIED DESIGN, CANDIDATE, and NOT EVIDENCED material. It also records concrete lineage differences between the June 9 remixed corpus, Omega, and the later ArgosEvolved implementation.

Known high-value mechanisms requiring provenance reconciliation:
- ICM;
- GBE;
- DO;
- VCE;
- ARRE;
- Governor;
- LEO;
- TCS;
- Communication Bus;
- Knowledge Vault;
- continuity/recovery references;
- canonical event definitions.

Important boundary:
Its frozen 2026-08-18 baseline explicitly excludes back-projecting Omega/V12 concepts into the v1 runtime boundary and excludes an unproven alternate registry-backed ICM.

Disposition:
HISTORICAL / HIGH-VALUE IMPLEMENTATION EVIDENCE. Treat its salvage matrix and canonical baseline as evidence about historical selection, not as authority over current ArgCore or Arg.

### Kelziejordan/Remixed-Argos-Evolved-2nd-Edition
Classification: historical remixed ArgOS implementation snapshot; private; non-archived; approximately 1,562 KB.

Observed contents and prior forensic record establish:
- June 9 historical implementation family;
- concrete ICM, GBE, DO, TCS, Communication Bus, and Knowledge Vault variants;
- PipelineOrchestrator is materially more skeleton-like/TODO-heavy than the later ArgosEvolved executable path;
- GBE contains a role-quarantine behavior that materially diverged from the later current branch;
- Communication Bus and Knowledge Vault contracts evolved materially in later generations.

Disposition:
HISTORICAL SOURCE ARTIFACT. Retain for behavioral comparison and provenance. Do not use as a runtime dependency.

## 2. Other evolution repositories requiring classification

### Kelziejordan/Argos-Evolution-
Private, non-archived, approximately 279 KB. AI Studio application scaffold with Gemini dependency, Firebase, CCXT, Express, Vite/React, and no demonstrated ArgCore dependency in the inspected manifest.

Disposition:
HISTORICAL/EXPERIMENTAL APPLICATION ARTIFACT. Requires source-level comparison before any claim of unique mechanism.

### Kelziejordan/ArgOS_Evolution
Public, non-archived, approximately 545 KB. Repository identity indicates an evolution branch; full source reconciliation remains pending.

Disposition:
UNRESOLVED HISTORICAL/EXPERIMENTAL. Requires deeper inspection before classification is frozen.

### Kelziejordan/ArgOS-Evolution
Public, non-archived, size 0.

Disposition:
EMPTY/PLACEHOLDER unless additional history or non-default refs reveal content.

### Kelziejordan/Argos-Evolution-
The repository contains an explicit IP audit hold and identifies itself as historical implementation material under containment.

Disposition:
HISTORICAL / CONTAINED. No promotion.

### Kelziejordan/ArgusEvolution
Public, non-archived, approximately 15 KB. Next.js/React application with minimal package dependencies. Repository contents include generated/opaque artifact-like filenames and require source archaeology before assigning a substantive architectural role.

Disposition:
HISTORICAL/EXPERIMENTAL LEAD. Not current authority.

### Kelziejordan/argos-organism
Public, non-archived, approximately 4 KB. README describes an ArgOS Organism Runtime v1 focused on survivability, controlled evolution, contradiction detection, replay archaeology, mutation trust scoring, lineage inheritance, rollback safety, and reality-aligned adaptation. Package manifest contains TypeScript tooling but no executable test suite.

Disposition:
EXPERIMENTAL / POTENTIALLY UNIQUE MECHANISM LEAD. Requires direct source inspection before promotion or dismissal.

### Kelziejordan/argos-organism-evolution
Public, non-archived, size 0.

Disposition:
EMPTY/PLACEHOLDER unless history reveals otherwise.

## 3. Anchor lineage

### Kelziejordan/Arg_Anchor
Active product/workspace repository and already role-declared as a Tier 2 consumer. It is not being treated as historical merely because it is downstream.

Forensic boundary:
Any mechanism inside Anchor that appears to redefine constitutional identity, state, governance, provenance, recovery, or execution authority must be compared against ArgCore/Arg before reuse. Product/workspace behavior remains downstream unless explicitly promoted through the authority process.

No architectural modification is made by this inventory.

## 4. Additional repositories discovered during owner-wide inventory

The owner-wide repository search also identified:
- ArgFoundation — explicit historical evidence/foundation repository; preserves historical PDFs, reconstruction records, constitutional artifacts, and provenance.
- ArgBeyond — proprietary review repository; contents require classification.
- ArgLearn — active learning/discovery system; explicitly declares epistemic autonomy without constitutional authority.
- ArgGovernance- — empty/near-empty identity requiring inspection if it contains historical refs.
- ArgOS-Workspace-Dev-Edition — development workspace; downstream.
- argus101 — ARGUS V10 workspace variant; historical/reference lead.
- Arg-Studios — separate studio artifact with ARGOS/ARGUS naming; requires comparison with ArgOS-Argus-Studio.

These are not automatically part of the ArgOS/Omega historical lineage. Their inclusion here is a discovery result only.

## 5. Provenance leads requiring next-pass inspection

Priority A:
1. ArgOS-Omega-v1 — inspect actual source modules and package/dependency graph.
2. ArgosEvolved — reconcile all salvage-matrix claims against source.
3. Remixed-Argos-Evolved-2nd-Edition — compare concrete implementations against ArgosEvolved and Omega.
4. Argos-Evolution- / ArgOS_Evolution — determine whether either contains unique mechanisms absent from the above.
5. argos-organism — inspect source for unique evolution/rollback/replay mechanisms.

Priority B:
6. ArgusEvolution / argus101 — determine whether they are distinct ARGUS generations or duplicate packaging.
7. ArgFoundation — use as provenance/evidence boundary, not runtime source.
8. ArgBeyond / ArgGovernance- / Arg-Studios — classify only if contents establish Arg lineage relevance.

## 6. Current dependency conclusion

The inspected historical repositories do not establish a current runtime dependency into ArgCore or the current Arg runtime.

The strongest observed historical dependency relationship is conceptual and implementation-lineage based:
historical ArgOS/Omega/ArgosEvolved artifacts -> forensic comparison -> current Arg/ArgCore decisions.

That is provenance, not runtime dependency.

## 7. Preservation rule

No historical repository is to be:
- imported into current Arg;
- made a runtime dependency;
- used to silently replace a current contract;
- promoted because its implementation appears newer or more sophisticated;
- treated as proven solely because its README claims production readiness.

A unique mechanism becomes eligible for current consideration only after:
1. exact source provenance is established;
2. behavior is described;
3. current ArgCore contracts are compared;
4. compatibility tests are defined;
5. a new current implementation is created in the authoritative repository;
6. the historical source remains preserved as evidence.

## Current forensic state

Historical inventory: STARTED / SIGNIFICANT LINEAGE IDENTIFIED
Unique-mechanism reconciliation: NOT COMPLETE
Runtime authority: UNCHANGED
ArgCore authority: UNCHANGED
Arg authority: UNCHANGED
ArgAtlas role: DOCUMENTARY / PROVENANCE OWNER

Next gate:
Deep source comparison of Omega -> ArgosEvolved -> Remixed corpus, followed by explicit identification of mechanisms that are unique, duplicated, superseded, or already represented in current Arg/ArgCore.
