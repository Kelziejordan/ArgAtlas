# Repository Role Declaration — ArgAtlas

**Repository:** `Kelziejordan/ArgAtlas`  
**Default branch:** `main`  
**Role:** Engineering continuity, provenance, architecture records, and evidence ledger  
**Authority status:** Record-keeping and architectural-memory authority; not executable runtime authority

## 1. Purpose

ArgAtlas preserves the ecosystem's engineering memory: canonical contracts, repository roles, architectural decisions, checkpoints, provenance, evidence, unresolved questions, and the proof required before a capability or claim can be promoted. It is intended to prevent context loss and prevent historical or aspirational descriptions from being mistaken for current executable truth.

ArgAtlas is authoritative for what its records say and for the provenance of those records. It does not become the executable source of truth for code in Arg, ArgCore, or domain repositories.

## 2. Ownership

ArgAtlas owns:
- The documented canonical lifecycle and its versioned contract records, including ACL-001.
- Cross-repository role maps, dependency/authority maps, project checkpoints, and decision records.
- Evidence-ledger organization, provenance references, proof status, and known gaps.
- Explicit distinctions between current, historical, proposed, frozen, and unproven material.
- Recording what changed, why it changed, what evidence supports the change, and what remains to be verified.

ArgAtlas does not own:
- Runtime execution, governance enforcement, provider calls, or external actions.
- ArgCore's executable constitutional contracts or package release authority.
- ArgOS's implementation or the runtime truth of a feature.
- ArgLearn's analytical engine or the authority to approve its own proposals.
- The power to make a capability proven merely by documenting it.

## 3. Relationship to other repositories

- **ArgCore:** executable constitutional foundation; ArgAtlas records its version and evidence but cannot override it.
- **Arg:** current ArgOS runtime/orchestration implementation; implementation and CI evidence must be checked in Arg.
- **ArgLearn:** evidence analysis, experiments, confidence assessments, and improvement proposals; ArgAtlas preserves findings and their provenance without converting them into mandates.
- **Domain branches/apps:** implement domain behavior. Their own source and tests determine implementation status; ArgAtlas records their role, evidence, dependencies, and boundaries.
- **Historical Argus/ArgOS/Omega repositories:** preserve lineage and historical experiments. They do not become current implementation authority without an explicit promotion decision and evidence.

## 4. Canonical lifecycle record

ACL-001 is documented as:

`Intent → Translation (ATL) → Planning → Execution → Validation → Observation → Memory → Next Intent`

Recovery is a controlled exception path that rejoins through Observation. Learning is a governed feedback loop. Semantic ATL is the translation layer between Intent and Planning. Similar names in historical repositories, including older C-ABI or foreign-language interface artifacts, are separate until contract and implementation evidence proves equivalence.

The lifecycle definition is not a claim that every stage is fully implemented or proven in the current runtime. The end-to-end status must be derived from current executable evidence, not from the existence of this document.

## 5. Evidence semantics

Use explicit labels consistently:
- **PROPOSED:** design or recommendation not yet accepted.
- **DOCUMENTED:** described in a record.
- **IMPLEMENTED:** code exists.
- **INTEGRATED:** components are connected.
- **EXERCISED:** a test or run has invoked the path.
- **PROVEN:** evidence establishes the specifically stated boundary.
- **FROZEN:** a defined contract or baseline is held stable.
- **NOT PROVEN:** evidence is absent, incomplete, or does not cover the claim.
- **HISTORICAL:** useful provenance, not current authority.

A CI pass proves only the tested commit, configuration, assertions, and path. A deployed service, dependency, screenshot, or document is not independently sufficient to establish full capability. Evidence records should include source revision, test/run identifier, date, scope, outcome, limitations, and links where available.

## 6. Conflict and correction rules

When records conflict:
1. Identify the claim, affected repository, and relevant revisions.
2. Check executable source, package/release data, tests, CI, and operational evidence.
3. Preserve the old record as historical where it explains lineage; do not rewrite history to make the current state appear cleaner.
4. Update the current checkpoint with the verified disposition and remaining uncertainty.
5. Do not promote a claim until the relevant evidence exists.

ArgAtlas may recommend reconciliation but cannot silently replace a runtime contract or package authority.

## 7. Current cross-repository baseline

The current known ArgOS dependency baseline is `@kelziejordan/argcore@1.0.0-argcore-008`; R8 recovery/resume proof is bounded to the exercised ArgOS/ArgCore-008 boundary. The full ACL-001 lifecycle and complete ecosystem operation remain **NOT PROVEN** unless a newer evidence-backed checkpoint explicitly establishes otherwise.

Economic validation and runtime engineering are separate tracks. A commercial experiment does not certify architecture; architecture certification does not establish revenue.

## 8. Change management

Every material architecture or status update should identify:
- The affected contract/repository and its authority owner.
- The previous state and new state.
- Evidence and exact revision(s).
- Compatibility and downstream implications.
- What remains unproven and the next verification gate.

## 9. Decision rule

**ArgAtlas preserves the map and evidence of the ecosystem; it does not operate the ecosystem or create authority by narration.** When in doubt, preserve uncertainty explicitly and point to the source that can settle it.
