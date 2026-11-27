# ArgAtlas

ArgAtlas is the engineering continuity, provenance, canonical lifecycle, and architectural-memory authority for the Arg ecosystem.

It preserves what is known, what is evidenced, what remains uncertain, and what must be proven before an implementation or claim is promoted.

## Current project state

The current cross-repository checkpoint is [PROJECT_STATE_2026-11-26.md](./docs/PROJECT_STATE_2026-11-26.md). It records the reconciled Arg main baseline, the exact ARG Functional v1 branch/commit, acceptance matrix, and exact-head CI evidence. Earlier checkpoints remain immutable historical records.

The active implementation is [ARG Functional v1 draft PR #143](https://github.com/Kelziejordan/Arg/pull/143). The 2026-11-09 PR #140 continuity addendum remains historical and is not the current implementation target.

Current executable Arg dependency: `@kelziejordan/argcore@1.0.0-argcore-008`. ArgCore 008 and both accepted architectures remain frozen.

Current next gates:
- Review PR #143 and merge only with explicit operator authorization.
- Complete post-merge verification; production migration/deployment remains a separate authorization gate.
- After ARG Functional v1 is shipped, the next user-selected milestone is LinkedIn profile and professional positioning.
- The Governance Benchmark and other engineering tracks remain preserved secondary work; overall canonical lifecycle completion remains NOT_PROVEN.

## Canonical lifecycle

[ACL-001](./docs/ACL-001-CANONICAL-INTELLIGENCE-LIFECYCLE.md) defines:

Intent -> Translation (ATL) -> Planning -> Execution -> Validation -> Observation -> Memory -> Next Intent

Recovery is a controlled exception path that rejoins through Observation. Learning is a governed feedback loop. Semantic ATL is the translation layer between Intent and Planning. The older `argos-atl` C-ABI/interface use remains a distinct forensic item.

## Continuity rule

ArgAtlas records evidence without promoting unsupported claims. Historical artifacts do not automatically override canonical architecture or current executable evidence. CONNECTED, IMPLEMENTED, and EXERCISED are not synonyms for PROVEN; each test proves only its exercised boundary.

See [ATL reconciliation](./docs/ATL-RECONCILIATION.md) and the project-state checkpoint for authority, proof boundaries, risks, and next steps.
