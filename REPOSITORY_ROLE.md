# Repository Role Declaration — ArgAtlas

Declaration version: 1.0
Effective status: FROZEN AUTHORITY BOUNDARY
Repository: Kelziejordan/ArgAtlas

## Architectural classification
- Tier: 1 — Continuity/provenance system
- Lifecycle: Active
- Source of truth: Yes, for canonical project history, architecture records, provenance, evidence classification, and continuity records
- Historical/reference status: Preserves historical evidence without automatically promoting it

## Authority
- Identity: NONE / DOCUMENTARY
- State: NONE / DOCUMENTARY
- Governance: DOCUMENTARY
- Provenance: OWNER for project-level engineering records
- Execution: NONE

## Dependencies
ArgAtlas is an engineering continuity and evidence system. ArgCore MUST NOT depend on it at runtime. ArgAtlas records runtime truth; it does not replace runtime governance.

## Owned records
ArgAtlas owns canonical architecture records, project state records, session continuity, historical reconciliation, evidence classification, unresolved questions, promotion/deprecation records, and provenance of engineering decisions.

## Not owned
ArgAtlas does not own executable identity, state, governance, or execution contracts. It must not become an executable replacement for ArgCore or Arg.

## Permitted modifications
ArgAtlas may record and reconcile evidence, architecture, and project state. It may document proposed contract changes but cannot enact them by documentation alone.

## Contents
- Original implementation: Documentation/continuity mechanisms
- Derived implementation: Yes, where records encode recovered architecture
- Documentation: Primary role
- Packaging/deployment: No runtime authority
- Historical/provenance: Primary role
- Experimental: Only as explicitly marked records

## Promotion rules
A claim becomes canonical project knowledge only when its evidence/provenance is identified and contradictions are recorded rather than silently erased.

## Authority boundary
ArgAtlas records what the ecosystem knows, what it can prove, what remains uncertain, and what changed. ArgCore and Arg remain the executable authorities for their respective boundaries.

FINAL RULE: ArgAtlas remembers and reconciles; it does not execute.
