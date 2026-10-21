# Domain Application Repository Boundary — 2026-10-21

Status: ACTIVE ARCHITECTURAL DECISION
Authority: ArgAtlas continuity and provenance record
Scope: New and existing ARG ecosystem domain applications

## Decision

New domain applications belong in the Branches / Domain Applications layer. Each application has its own dedicated application repository and consumes shared ecosystem capabilities through explicit approved interfaces.

The architecture is:

ARG (parent ecosystem)
-> ArgCore (frozen constitutional foundation)
-> ArgOS (governed operating/runtime layer)
-> Branches / Domain Applications (separate application repositories)

The arrows describe architectural dependency and layering, not permission to place application code inside a foundational repository.

## Non-negotiable placement rules

- Do not place trading application code, strategy implementations, paper-trading brokers, market-data adapters, trading UI, or trading-specific ledgers inside `Kelziejordan/Arg` (the active ArgOS runtime repository).
- Do not place domain application code inside ArgCore or modify the frozen ArgCore boundary for an application.
- Do not treat ArgAtlas as an application implementation repository. It records architecture, provenance, evidence, and project state.
- Do not use ArgOS Evolved or other historical repositories as current implementation destinations without an explicit governed promotion decision.
- A repository name, apparent similarity, or prior misplaced pull request is not proof that a repository is the approved application home.
- AegisKraken is explicitly excluded from this project and must not be used as its source, destination, dependency, or implementation reference.
- Existing work misplaced in a foundational repository must be preserved as evidence, not blindly copied or deleted. Its disposition must be recorded and any future transfer reviewed for provenance, interface compatibility, and tests.

## Trading application disposition

The trading application is a separate domain application. Its implementation must live in its approved dedicated application repository, outside ARG, ArgCore, and ArgOS runtime source.

PR #130 in `Kelziejordan/Arg` (live-market-data and paper-trading foundation) is not an approved application home and must not be merged into the ArgOS runtime. Preserve its branch and commits as evidence pending a controlled disposition. Do not transplant its contents until the intended destination repository is verified against the application registry/creation record.

No real-money order path, credential use, cloud spend, or production deployment is authorized by this decision.

## Mandatory preflight for every new application

Before implementation or transfer, record and verify:

1. Application ID and purpose.
2. Dedicated repository URL and default branch.
3. The authoritative registry/creation record that assigns that repository to the application.
4. Shared capabilities consumed and exact approved interfaces/versions.
5. Explicitly prohibited dependencies and historical repositories.
6. CI/test command and acceptance evidence.
7. Data, secrets, external-effect, and deployment boundaries.

If any item is missing or conflicts with authoritative evidence, stop implementation and resolve that item only. Do not reopen the already-settled architectural placement rule.

## Current disposition

- Architectural layer: SETTLED — Branches / Domain Applications.
- AegisKraken: EXCLUDED.
- Trading code in Arg runtime PR #130: WRONG DESTINATION; DO NOT MERGE.
- Exact canonical trading application repository: NOT YET VERIFIED by the available authoritative registry record.
- ArgCore: FROZEN; NO APPLICATION CHANGES.
- ArgOS runtime: NO TRADING APPLICATION IMPLEMENTATION.
