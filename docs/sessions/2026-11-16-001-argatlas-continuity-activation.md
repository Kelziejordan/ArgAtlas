# Engineering Session — 2026-11-16 — ArgAtlas Continuity Activation

Status: RECORDED IN ARGATLAS BRANCH; PR NOT YET OPEN
Session type: Engineering governance / continuity
Active gate: G2 — TCS shared persistence/adapter boundary
Architecture: FROZEN
Repository scope: FROZEN

## Objective

Activate ArgAtlas as the continuously maintained engineering continuity record immediately, rather than treating it as a future capability or an optional end-of-session task.

## Decisions and operating rules

1. ArgAtlas continuity is mandatory from this session onward. It must not depend on the operator remembering to ask for documentation.
2. Material findings, decisions, evidence, changes, and next actions are to be persisted incrementally during work, not only at conversation end.
3. The existing G1-G10 Minimum Operational Completion Gates register remains the sole master checklist. G2 remains the sole active technical workstream.
4. ArgAtlas is the engineering continuity record; TCS is execution evidence; Knowledge Vault is runtime knowledge; VTM is analytical reconstruction. These responsibilities remain distinct.
5. ARG-X and its four repositories remain isolated from the original ARG ecosystem. No experimental source, package, credential, database, or deployment is promoted into original ARG without separate governed acceptance.
6. Runtime ARG should not carry unnecessary development history. It receives only the operational information needed for identity, authority, capabilities, dependencies, state, integrity, safe execution, recovery, and operational verification.
7. Degradation must not equate usefulness with popularity. Low-frequency, minority, novel, contradictory, and potentially high-consequence signals must be retained as candidates and assessed on evidence, novelty, consequence, corroboration, reproducibility, and provenance. Popularity is one input, never the deciding vote.
8. Historical weighted event scores are not validated thresholds. No destructive retention/deletion policy is authorized.
9. The eventual goal is for ARG to enforce this discipline itself, but that capability is not currently proven.

## Automation limitation — explicitly not claimed

The current ChatGPT conversation environment does not provide a guaranteed callback when the user presses X, closes the app, or becomes inactive for ten minutes. It cannot promise background writes after the conversation has ended. The continuity protocol therefore requires incremental persistence at available engineering milestones and a closeout checkpoint whenever the workflow is still active. A true always-on capture mechanism remains an implementation requirement, not an existing capability.

Abrupt-end resilience is a design requirement: continuity must eventually recover from the newest valid durable event/checkpoint without requiring a clean shutdown.

## Prior design elements recovered

Earlier ArgAtlas work established:
- State Refresh at session start: retrieve current project state, decisions, evidence, and next task rather than asking the user to remember.
- Continuous engineering-event capture and a session shutdown sequence.
- Separate layers for immutable history, curated knowledge, and compact machine-readable state.
- A proposed provenance chain: Decision -> ADR -> Session -> Commit -> Files -> Test Results.
- A proposed atlas_state.json with project, milestone, completion, focus, next task, branch, blockers, risks, and last session.
- A historical SQLite canonical store with Markdown published views.
- A degradation concept that preserves engineering signal rather than entire raw conversations.
- A warning that checkpointing is an optimization; crash/closure recovery must not depend on a clean session ending.

These are recovered design references. Their existence in historical discussions does not prove they were implemented or operational.

## Current project context

- Original runtime: Kelziejordan/Arg; known main baseline from the current working record is 9158fdec5f7cad33727b443dcb110ed958b8ab6f. Recheck before source edits.
- ArgCore 008 remains frozen.
- ArgAtlas remains the engineering continuity authority.
- ArgLearn is analytical/proposal only.
- VTM is not required to be operational before ArgAtlas continuity can function.
- PR #140 remains draft and unmerged.
- Production and deployment remain untouched.
- Vercel remains intentionally guarded and is not the active investigation target.
- Experimental organization: ARG-Experimental with ARG-X, ArgCore-X, ArgAtlas-X, ArgLearn-X. The ArgAtlas-X source snapshot is tree-verified; the other source imports are pending/empty per the import ledger.

## G2 carry-forward findings

- api/execute.js selects JSONL TCS and ignores ARGOS_TCS_DATABASE_URL.
- api/conversation.js can select PostgreSQL when the database URL exists; otherwise it selects JSONL.
- The routes have divergent TCS adapter selection.
- TCS and Knowledge Vault remain separate journals.
- The conversation handler does not instantiate the Knowledge Vault/ArgLearn memory lifecycle.
- Preview database evidence is not production proof.
- No production runtime target has been authorized or established.

## Work completed in this session

- Retrieved prior ArgAtlas continuity and degradation design context.
- Reconciled original ArgAtlas repository identity and current main commit from the available GitHub source.
- Created branch ops/argatlas-continuity-v1-2026-11-16 from ArgAtlas main.
- Added this operating protocol and this session record to that branch.

## Verification and limitations

- Branch creation: completed.
- Documentation commits: to be verified after file writes.
- PR: not yet created.
- Original Arg runtime: unchanged.
- ArgCore 008: unchanged.
- PR #140: unchanged.
- Production, databases, secrets, deployments: untouched.
- ARG-X repositories: untouched.
- No runtime automation has been implemented or tested.

## Next actions

1. Verify both documents and their commit SHAs on the ArgAtlas branch.
2. Open a documentation-only PR for review; do not merge without approval.
3. Reconcile the G1-G10 master register and TCS findings into the ArgAtlas working records without creating a competing checklist.
4. Continue G2 only: inspect current Arg source branch/tests, then implement the smallest shared TCS factory boundary on an authorized work branch.
5. Add a verified always-on continuity mechanism to the engineering roadmap as a future capability; do not claim it exists yet.

## Disposition

IN PROGRESS — ArgAtlas continuity protocol activated in a documentation branch. Automatic app-close/inactivity capture is not implemented. G2 remains the sole active technical workstream.
