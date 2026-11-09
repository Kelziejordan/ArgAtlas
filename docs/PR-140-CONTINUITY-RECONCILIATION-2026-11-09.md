# PR #140 Continuity Reconciliation — 2026-11-09

## Authority and repository identity

This record corrects the repository identity for the ARG Companion operator bootstrap work.

- Canonical repository: `Kelziejordan/Arg`
- Pull request: [#140 — Connect operator workspace to ARG orchestrator](https://github.com/Kelziejordan/Arg/pull/140)
- Head branch: `companion/arg-bootstrap`
- Base branch: `work/provider-independent-companion-001`
- Reconciled head commit: `1c77db8d4f94fd4c8b4ebafb378b3dc7f5a2fb69`
- Base commit: `7aef918949441685cc992260cf2a8a28885e61d3`
- PR state at reconciliation: OPEN, DRAFT, UNMERGED.

**Repository correction:** `Atech31/activity-boost` PR #140 is an unrelated repository and unrelated pull request. Its head is `b93a1485e9aaf78a525d45a5a049d0c3fd13fd94`; it is not the ARG Companion PR. Do not use that repository or commit as ARG project evidence. The earlier continuity reference to `Atech31/activity-boost` was an identity mismatch and is superseded by this record.

## Objective and frozen boundaries

Ship the first usable ARG Companion cockpit through a phone-operable workflow: submit a task, inspect a proposed plan, explicitly approve it, observe ARG-governed execution, and retrieve durable evidence.

The Companion architecture retains two distinct experiences:
1. ARG operator-authority Companion.
2. General-user Companion.

Frozen boundaries:
- ArgCore 008 remains frozen.
- Architecture remains frozen.
- PR #140 remains draft and unmerged.
- Vercel configuration, deployment, and production remain frozen.
- No merge, deployment, production activation, or architecture expansion is authorized by this reconciliation.

## Accepted work carried forward

The following evidence is inherited from the exact PR head and its recorded CI/review evidence. It was not rerun during this continuity correction.

- Exact-head CI: PASS for commit `1c77db8d4f94fd4c8b4ebafb378b3dc7f5a2fb69`.
- Arg Orchestration CI: run `42507418209` (run 854), completed successfully.
- ArgOS Memory Lifecycle CI: run `42507418318` (run 236), completed successfully.
- PostgreSQL integration job: `128812946968`, successful.
- PostgreSQL 16 integration exercised migrations, claims, transitions, concurrency, fresh-process recovery, hash-chain verification, and fail-closed behavior when the database is unavailable.
- Companion bootstrap integration exercised the Companion-to-orchestrator-to-ArgCore 008-to-PostgreSQL TCS path, explicit approval, bounded local execution, replay hold, and fresh-process recovery.
- Companion PostgreSQL end-to-end test exercised the UI, explicit approval, governed read-only Playwright action, durable PostgreSQL TCS, replay rejection, and fresh-process evidence verification.
- The recorded end-to-end run persisted nine ordered TCS events and independently read them in a fresh process.
- Exact-head Preview evidence recorded a successful read-only Chromium workflow and valid nine-event TCS hash chain. This is historical Preview evidence only; it does not authorize further Preview or Production activity and does not prove Vercel Preview deployments are disabled.
- The CI workflow successfully installed its dependencies. This proves package access in that CI environment; local package access is not thereby proven.

## Proof boundaries and outstanding gates

The successful tests prove only their exercised boundaries. They do not establish arbitrary task planning, broad tool/branch control, production identity and permissions, live provider behavior, production DigitalHands through the central orchestrator, screenshot handling, or complete in-flight external-effect recovery.

The recorded isolated recovery limitation remains separate: ArgCore 008's accepted recovery contract is bounded to its existing action/principal/session rules; the observed interrupted `external.webhook.dispatch` case did not carry the execution binding required to prove recovery authorization for that action. ArgCore 008 is frozen and is not to be modified as part of this PR reconciliation.

- Private package access: CI install PASS; local access remains UNVERIFIED.
- Integration test in this reconciliation: NOT RUN. Existing exact-head PostgreSQL and Companion integration CI evidence: PASS.
- Disposable Neon branch: `pr140-integration-2026-11-09` (`br-broad-resonance-b8d567lu`) was previously created and observed READY. It is separate from the PostgreSQL 16 CI service container. No TCS test records were written to this Neon branch.
- CI: exact-head PASS as listed above.
- Deployment: no new deployment performed in this reconciliation. Previously recorded Preview evidence remains historical; Production remains untouched. Vercel stays frozen.

## Invalidation triggers

Recheck only if the PR head or relevant test/workflow source changes, CI evidence is superseded or revoked, the PostgreSQL/TCS contract changes, the ArgCore 008 adapter boundary changes, or new evidence contradicts the recorded results. A new session or commit elsewhere does not by itself invalidate these results.

## Next bounded action

Resume from the current PR #140 acceptance-gate review using the existing exact-head evidence. Do not rerun the successful PostgreSQL integration suite merely to rediscover its result. Do not merge, deploy, alter Vercel, or modify ArgCore 008 without separate authorization.

## Status

TASK STATUS: IN PROGRESS  
FREEZE: architecture, PR merge, deployment, production, ArgCore 008.  
Reconciliation write: continuity record only; no application code, tests, database contents, PR state, or deployment changed.
