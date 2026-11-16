# Engineering Session — 2026-11-16 — ArgAtlas Continuity Activation

Status: RECORDED IN ARGATLAS BRANCH; PR #4 OPEN — READY FOR REVIEW
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
10. Independent human review is optional, never a mandatory project/repository/PR/release gate. For important or consequential decisions, require an independent second-AI audit before acceptance; prefer a different provider/model family, record the audited artifact/model/findings/disposition, and resolve material disagreements. If no second auditor is available, mark the audit UNVERIFIED and defer acceptance unless the operator explicitly accepts the documented risk. Routine low-risk reversible work uses proportionate verification. The second AI advises but does not authorize. Operator authorization remains mandatory for protected actions and is distinct from audit.

## Review policy decision — independent AI cross-audit

The operating policy now distinguishes ordinary verification from an independent second opinion. Important/consequential decisions require a second-AI audit before acceptance; independent human review is optional and is not a default dependency. Prefer a different provider or model family, but record when only a same-provider/different-version auditor is available. The auditor must receive the actual decision, acceptance criteria, source/evidence and failure scenarios, and must challenge rather than merely endorse the originating AI's work. Record model/provider, date, exact commit/artifact, scope, findings and disposition. Unavailable audit means UNVERIFIED; defer acceptance unless the operator explicitly accepts the documented risk. The auditor does not grant execution or merge authority.

This policy was adopted by the operator in this session and is being added to the controlling protocol. The current PR's earlier Codex review completed against `001b8442f216afec7bb15beb1608d7dd8576b576` with no findings; that review predates this policy addition and does not audit the new text. A fresh Codex audit is to be requested against the updated head.

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

- Original runtime: Kelziejordan/Arg; G2 source trace is pinned to commit 9158fdec5f7cad33727b443dcb110ed958b8ab6f. This is a recorded trace baseline, not an assertion that it remains the latest Arg main. Reconcile current branch/head before source changes.
- ArgCore 008 remains frozen.
- ArgAtlas remains the engineering continuity authority.
- ArgLearn is analytical/proposal only.
- VTM is not required to be operational before ArgAtlas continuity can function.
- PR #140 remains draft and unmerged.
- Production and deployment remain untouched.
- Vercel remains intentionally guarded and is not the active investigation target.
- Experimental organization: ARG-Experimental with ARG-X, ArgCore-X, ArgAtlas-X, ArgLearn-X. The ArgAtlas-X source snapshot is tree-verified; the other source imports are pending/empty per the import ledger.

## G2 carry-forward findings and source support

The following route-selection and journal-boundary findings were rechecked against exact source files at Arg commit 9158fdec5f7cad33727b443dcb110ed958b8ab6f:

- FACT — `api/execute.js` creates TCS with `createTraceChronicle({ storagePath })` and does not read `ARGOS_TCS_DATABASE_URL`. File blob: `cbff226ccef10f5f5846a5aa837354dd70cea1c9`.
- FACT — `api/conversation.js` uses `createPostgresTcs` when `ARGOS_TCS_DATABASE_URL` is present; otherwise it requires `ARGOS_EXECUTION_TCS_PATH` and uses `createTraceChronicle`. PostgreSQL mode is explicitly fail-closed. File blob: `0234873d24b7fc95f88a22f3445dda50308168f3`.
- FACT — `api/execute.js` creates a separate Knowledge Vault journal at `${storagePath}.knowledge-vault.jsonl`; the TCS and Knowledge Vault stores are distinct. Supporting source: `api/execute.js` above and `src/memory/knowledge-vault.js`, blob `bcebbc924f2660cc1b157693f8a19d82cc7646ee`.
- FACT — `api/conversation.js` does not import or instantiate Knowledge Vault or the ArgLearn memory cycle. This means this handler does not itself ingest its TCS events into that memory lifecycle; it does not prove that no other integration path exists. Supporting source: `api/conversation.js` above.
- OBSERVATION — The recorded Neon Preview integration fixture contained 5 claims and 35 TCS events; read-only chain checks found no broken parent links and the journal head matched the final event. This is Preview fixture evidence only, not production persistence proof.
- INTERPRETATION — The two route handlers have divergent TCS adapter-selection contracts. The smallest candidate correction remains a shared TCS factory, subject to current-source reconciliation and tests.
- UNVERIFIED — Production TCS target, production persistence/restart behavior, and production deployment are not established by these findings.

The source-trace baseline and file blob identities are evidence references, not proof of current-main status. Recheck before implementation. The detailed ledger `ARG_TCS_Runtime_Findings_2026-11-16.csv` remains a working Library artifact and has not yet been imported into ArgAtlas.

## Master completion register status

The sole G1-G10 register is `ARG_Minimum_Operational_Completion_Gates_2026-11-16.csv`, currently held as a working Library artifact. It has not yet been imported into the ArgAtlas repository. No competing checklist is to be created. Import/reconcile this same register as a separate continuity-record task; until then, do not describe it as repository-persisted.

## Work completed and verification

- Retrieved prior ArgAtlas continuity and degradation design context.
- Reconciled original ArgAtlas repository identity and main baseline `aa5e771c146472de1876773c8f46cd3ee2f99547`.
- Created branch `ops/argatlas-continuity-v1-2026-11-16` from ArgAtlas main.
- Added the operating protocol and this session record.
- Verified protocol file blob after evidence-reference reconciliation: `d4407cb4ad475958810b0c09d3629ae8fe18b76d`.
- Original session-record blob before this correction: `73b66fd5bfc2ad0eb67aafb019c2e97a695df48b`.
- PR #4: [Establish mandatory ArgAtlas continuity protocol v1](https://github.com/Kelziejordan/ArgAtlas/pull/4).
- After the documentation/evidence reconciliation, PR #4 was marked READY FOR REVIEW at head `91ce664fdba2b6a2a079c79a61d4d6ba95d868c3`; it remains OPEN and UNMERGED.
- No CODEOWNERS file was found at the standard repository paths. Independent human review is now explicitly optional and is not a merge or project gate. No external reviewer is expected or required.
- PR metadata reports `mergeable: true`. Automated Codex review completed against final head `c511214bbea55552238f334ec08f49f48947a45b` and reported one P2 finding: the older `MASTER_PROJECT_STATE.md` checkpoint conflicts with the later G2 gate decision. This finding is being addressed in this branch by explicitly recording checkpoint supersession. No workflow runs or commit statuses were reported; this is not a passing CI result.
- PR #4 at the start of this review reconciliation: OPEN, DRAFT, UNMERGED; base `main` at `aa5e771c146472de1876773c8f46cd3ee2f99547`; head branch `ops/argatlas-continuity-v1-2026-11-16`; head commit `40327558f849576a616c3f46934512f5b949ce87`; 2 commits, 2 changed files, 241 additions, 0 deletions.
- Changed-file review confirmed the intended documentation-only scope: the continuity protocol and this session record only.
- At that PR snapshot, no review submissions were present, no PR-triggered workflow runs were returned, and the head commit had no reported commit statuses. These are NO CHECKS REPORTED, not passing checks.
- No runtime test or CI pass is claimed for this documentation-only PR.
- Original Arg runtime, ArgCore 008, PR #140, production, databases, secrets, deployments, and ARG-X repositories were not changed.

### Review-policy implementation evidence

- Updated controlling protocol at commit `f3feb7ed36896a374de6fd8d5372e76948ff1114`; protocol blob `722fcd39cdf5091d3e106b979edd94c5059955c8`.
- Recorded this operator decision in the session record; session blob at the time of this update: `f54df407c6e2cd5788b4c93fb237e5fe3446e882`.
- PR #4 description now states the second-AI cross-audit policy and preserves operator authorization as a separate control.
- Codex review of prior head `001b8442f216afec7bb15beb1608d7dd8576b576` completed with no findings. It predates the new cross-audit policy and is not evidence that the new policy text has been audited.
- A fresh Codex audit was requested against head `6990fbb66cad8212798478e9faddd09fcd561f23`; no response had appeared at the time of this checkpoint. This is PENDING, not passed.
- No CI workflow runs or commit statuses were reported for the policy-update head. No CI pass is claimed.

## Next actions

1. Verify the updated session record, final branch head, and two-file PR diff after this reconciliation commit.
2. PR #4 has no mandatory human-review dependency. Use automated review and risk-based engineering verification where available; human review is optional. Do not treat missing CI/status reports as passing checks. Merge authorization remains with the operator.
3. Import/reconcile the existing G1-G10 register and TCS findings ledger into ArgAtlas working records as a separate continuity task, without creating competing master records.
4. Continue G2 only: inspect current Arg source branch/tests, then implement the smallest shared TCS factory boundary on an authorized non-main work branch.
5. Add a verified always-on continuity mechanism to the engineering roadmap as a future capability; do not claim it exists yet.

## Disposition

IN PROGRESS — ArgAtlas continuity protocol activated in a documentation branch; PR #4 is open and ready for review, unmerged. Source-level G2 findings are supported at the recorded Arg commit. The G1-G10 and TCS findings CSVs remain working Library artifacts pending import. Automatic app-close/inactivity capture is not implemented. G2 remains the sole active technical workstream.
