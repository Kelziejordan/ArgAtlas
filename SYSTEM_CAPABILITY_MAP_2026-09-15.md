# Arg Ecosystem System Capability Map — 2026-09-15

Status: EVIDENCE SNAPSHOT
Snapshot ID: ARG-SYS-2026-09-15-01
Purpose: Cross-repository continuity and benchmark guardrail.

## Authority Model

This map records verified repository and architectural evidence. It does not redefine the ArgOS Governance Benchmark Specification v1 and does not promote preserved capabilities into benchmark scope without direct evidence.

Evidence hierarchy:

Current frozen canonical implementation > verified repository state > documented current decision > historical architectural decision > historical conversation/proposal.

## Repository Nodes

| Repository | Observed main SHA | Role | State | Benchmark relevance |
|---|---|---|---|---|
| Kelziejordan/ArgCore | `fcce0cb49f4218d2f35c92dc9704e0efd571ad0c` | constitutional/runtime foundation | constitutional_frozen | dependency authority |
| Kelziejordan/Arg | `f4665f887042244eab126996aeea789f56680457` | ArgOS runtime/execution/orchestration | active | direct |
| Kelziejordan/ArgLearn | `e047322324b71f61445783d4e4be7c129d329e63` | learning/discovery/evaluation | active_not_exercised | not_exercised |
| Kelziejordan/ArgAtlas | `8648387247aba56fc3db68793b41256f8b95ca82` | continuity/provenance | active | indirect/evidence |
| Kelziejordan/ArgosEvolved | `8740046c04100ea90baa0e200010b54db8460eb4` | historical ArgOS lineage | historical | out_of_scope |

## Capability Nodes

### Constitutional and Runtime Authority

ArgCore owns the frozen constitutional/runtime foundation and governed execution contracts. ArgOS consumes that authority through the package boundary; it does not replace or redefine it.

### ArgOS Runtime

Arg is the current executable home of ArgOS. Current evidence includes ArgCore-owned state lineage integration into VTM, TCS as chronological trace authority, runtime/test infrastructure, and removal of the production mock execution adapter.

### ArgLearn

ArgLearn is a separate repository with a V1 contract and a GREEN/FROZEN checkpoint. Its V1 lifecycle is `observe -> knowledge gap -> hypothesis -> experiment -> result -> finding -> learning candidate`. The V1 checkpoint explicitly excludes LLM calls, persistence, ArgOS runtime integration, ArgAtlas writes, autonomous experimentation, model profiling, routing, and production policy changes.

Classification: active learning/discovery infrastructure; not currently benchmark-exercised.

### ATL

ATL is a canonical architectural component, not a missing repository. ArgAtlas records it as the Translation Layer between Intent and Planning. A historical `argos-atl` C-ABI/shared-memory implementation creates a naming collision that remains an explicit forensic item.

Classification: canonical component; relationship to the benchmark path unresolved unless direct evidence establishes relevance.

### ArgAtlas

ArgAtlas is the engineering continuity and provenance system. It records authoritative project state, verified evidence, decisions, uncertainty, and next justified engineering gates.

### Knowledge Vault

Knowledge Vault is an intended canonical knowledge substrate in current ArgAtlas state, while historical ArgosEvolved contains concrete Knowledge Vault implementation evidence. Current Arg repository evidence does not, in this snapshot, establish that historical Knowledge Vault code is a current ArgOS runtime dependency.

Classification: preserved architectural capability; current implementation status unresolved; benchmark effect non-blocking unless directly exercised.

### VTM/CSRE, 7S/ARRE, Experience, DigitalHands

These are preserved governed expansion/evaluation boundaries identified in current ArgAtlas state. They are not automatically benchmark scope merely because they exist in the ecosystem.

### Application and Vertical Branches

Discovery, Trading, InspectionOS, LifeOS, and related application/vertical branches remain preserved ecosystem capabilities. Their presence does not establish current benchmark execution relevance.

### Multi-LLM Orchestrator

The Multi-LLM Orchestrator is recorded by ArgAtlas as infrastructure within the ecosystem rather than the destination. Its shipped boundary is preserved; future changes require independent justification.

## Independent Benchmark Infrastructure

The Governance Benchmark is intentionally independent from ArgOS internals.

Required benchmark infrastructure:

1. independent harness;
2. standardized adapter contract;
3. controlled workload suite WL-01 through WL-07;
4. substrate adapters for the frozen comparison systems;
5. deterministic reset/isolation;
6. fault injection and failure-budget accounting;
7. raw run-level evidence retention;
8. reconciliation and report generation.

The benchmark infrastructure is a separate research/evaluation boundary. It must not become an ArgOS runtime dependency merely to measure ArgOS.

## Execution Substrates

The frozen benchmark compares ArgOS native execution/governance against the declared substrate systems and governed integrations. Temporal, Akka, and conventional microservice orchestration are execution substrates/comparison systems, not ArgOS-owned repositories.

The benchmark must treat them through the independent adapter contract.

## Relationship States

Use these states for every cross-repository edge:

- VERIFIED: explicit repository, artifact, contract, import, package, runtime, or benchmark evidence establishes the relationship.
- UNRESOLVED: a plausible relationship exists but current evidence is insufficient.
- HISTORICAL: evidence belongs to a prior implementation generation.
- NON_BLOCKING: unresolved state does not affect the benchmark's validity.
- BLOCKING: the unresolved state affects benchmark target, measured path, correctness, constitutional dependency identity, or evidence integrity.

No relationship is inferred from naming alone.

## Current Tree

ARG
|
+-- ArgCore (repository)
|   +-- constitutional/runtime authority
|   +-- governed execution contracts
|
+-- Arg (repository)
|   +-- ArgOS runtime
|   +-- TCS trace authority
|   +-- VTM analytical path
|   +-- governed ArgCore integration
|
+-- ArgLearn (repository)
|   +-- learning/discovery/evaluation
|   +-- independent epistemic capability
|
+-- ATL (architectural component)
|   +-- translation layer
|
+-- ArgAtlas (repository)
|   +-- continuity
|   +-- provenance
|   +-- system evidence map
|
+-- Independent Governance Benchmark (research infrastructure)
|   +-- harness
|   +-- adapters
|   +-- workloads WL-01 -> WL-07
|   +-- evidence
|   +-- reconciliation
|
+-- External Execution Substrates
|   +-- Temporal
|   +-- Akka
|   +-- conventional microservice orchestration
|
+-- Historical Lineage
    +-- ArgosEvolved
        +-- historical implementation evidence
        +-- forensic recovery source

## Continuity Gate

At the start of a future session:

1. verify the current heads of ArgCore, Arg, ArgLearn, and ArgAtlas;
2. identify any SHA changes from snapshot `ARG-SYS-2026-09-15-01`;
3. read the latest ArgAtlas master state and relevant ArgOS continuity state;
4. determine whether the change affects the benchmark measured path;
5. continue from the smallest justified atomic action.

Do not silently merge historical architecture into current implementation. Do not reopen the frozen benchmark because a preserved capability is discovered.
