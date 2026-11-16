# ArgAtlas Continuity Operating Protocol v1

Status: ACTIVE OPERATING RULE — HUMAN-ENFORCED
Effective: 2026-11-16
Authority: ArgAtlas engineering continuity
Scope: The original ARG ecosystem only. ARG-X remains a separately governed experimental workspace.

## 1. Purpose

ArgAtlas is the continuously maintained engineering continuity and provenance record for ARG. It must prevent loss of decisions, accepted work, evidence, defects, open questions, and next actions across conversations and engineering sessions.

This protocol is active immediately. It is not optional, and it must not depend on the operator remembering to request a checkpoint.

The current process is human-enforced through the engineering assistant and repository workflow. It is not yet an autonomous ARG capability. No claim of automatic capture on conversation close, application exit, timeout, or disconnection is permitted until a working implementation has been built and tested.

## 2. Mandatory operating lifecycle

### Session start
1. Read this protocol and the latest authoritative ArgAtlas project-state/checkpoint.
2. Read the active master completion-gates register and the current active-gate task packet.
3. Reconcile repository, branch, commit, environment, open PRs, and frozen boundaries against live source evidence where relevant.
4. State the current objective, acceptance criteria, baseline, active gate, blockers, risks, and next action.
5. Do not ask the operator to repeat information already recorded.

### During work
Capture material engineering events as they occur; do not wait until the end of a conversation.

Record:
- Decisions and their authority/status.
- Findings, including contradictory and negative findings.
- Source references, exact commits, files, tests, and evidence.
- Implementations and the precise verification boundary.
- Defects, blockers, deferred work, and why they are deferred.
- Scope changes, frozen boundaries, and invalidation triggers.
- The next concrete action.

Classify each assertion as FACT, OBSERVATION, INTERPRETATION, HYPOTHESIS, DECISION, or UNVERIFIED. Never promote an inference to a fact. Preserve failed tests and negative results.

### Milestones and session close
At each meaningful milestone, and at the end of every available work session:
1. Persist a dated session record or update the active task packet.
2. Update the active gate's status and evidence links.
3. Update the current project-state checkpoint and next action when changed.
4. Record what was implemented, tested, CI-verified, deployed, and proven as separate states.
5. Record remaining work and exact resumption instructions.
6. Verify the persisted files/commit and report the resulting reference.

A clean close is not required for continuity. The system must eventually preserve events incrementally so abrupt closure, timeout, battery loss, reset, or lost connection does not erase the work since the last checkpoint.

## 3. Evidence and provenance

Preserve the chain:

Decision -> ADR/decision record -> Session -> Repository/branch/commit -> Changed files -> Tests/CI -> Resulting evidence -> Gate disposition.

A commit, PR, successful test, or deployment proves only its actual scope. CONNECTED, IMPLEMENTED, EXERCISED, CI-PASS, DEPLOYED, and PROVEN are distinct classifications.

Do not overwrite historical records to make them agree with current status. Publish a new dated checkpoint that explicitly supersedes older status where necessary.

## 4. Canonical information boundaries

- ArgAtlas: engineering continuity, decisions, provenance, gate status, and resumption state.
- TCS: authoritative execution evidence produced by the running ARG system.
- Knowledge Vault: runtime operational knowledge under its governed lifecycle.
- VTM: analytical temporal reconstruction from authoritative evidence; not a competing source of truth.
- ArgLearn: analysis and proposals; not execution authority.
- ArgCore 008: frozen constitutional foundation.

Engineering development history does not automatically belong in ARG's runtime knowledge. Only runtime-relevant identity, authority, capabilities, dependencies, state, recovery, integrity, and operational evidence should be exposed to the running product. ArgAtlas retains the engineering detail needed to maintain and evolve it.

## 5. Degradation and retention principles

Degradation must reduce noise without destroying rare, novel, contradictory, or minority-held information that may later prove important.

Popularity or frequency is not a sufficient measure of value. A signal supported by two independent observations must not be discarded merely because another signal appears in thirty observations. A low-frequency finding may be highly valuable if it is novel, falsifiable, consequential, technically specific, independently reproducible, or later corroborated.

Evaluate candidate information across separate dimensions:
- Provenance and evidence quality.
- Relevance to current objectives and known architecture.
- Novelty and information gain.
- Consequence if true or if missed.
- Independent corroboration and reproducibility.
- Contradiction with existing beliefs or evidence.
- Recency and rate of change.
- Frequency/popularity, treated as one signal only.
- Cost of retaining versus cost of losing it.

Do not collapse these dimensions into a simple majority vote or a single popularity score.

Use differentiated retention:
1. Immutable evidence/history: decisions, source references, test results, negative results, and provenance are append-only; corrections are new linked records.
2. Active knowledge: current, actionable, well-supported information is promoted into concise working summaries.
3. Candidate signals: uncertain, novel, minority, contradictory, or low-frequency information is retained for evaluation rather than silently discarded.
4. Superseded summaries: may be compacted or replaced only while preserving their source references and history.

A candidate may be deprioritized, but evidence and provenance must not be silently erased. Degradation must be explainable, reversible where possible, and auditable. A human-approved policy change is required before destructive retention behavior is enabled.

Historical proposal from earlier ArgAtlas design work included weighted event classes (architecture 100, ADR 95, feature 90, bug 85, roadmap 80, research 40, casual 0). Treat these as historical starting values, not validated thresholds. They must not override the multidimensional and minority-signal protections above.

## 6. Automation target and proof boundary

The eventual target is an Arg/ArgAtlas continuity capability that:
- Captures engineering events incrementally.
- Persists them durably without relying on conversation-end hooks.
- Recovers the newest valid state after abrupt interruption.
- Reconstructs project state, decisions, evidence, and next task.
- Applies explainable, non-destructive degradation.
- Preserves rare and contradictory signals for later reassessment.
- Produces verifiable audit trails.

Potential implementation layers previously explored include an immutable event history, curated knowledge objects, and a compact machine-readable state (historically proposed as SQLite plus published Markdown views). These are design references, not proof of an implemented service.

Automatic triggers such as application close, conversation X, ten-minute inactivity, or network disconnect are NOT IMPLEMENTED/NOT_PROVEN in the current ChatGPT workflow. They must not be promised. Until a real event-capture service is implemented and tested, this protocol is enforced by the assistant's engineering workflow with incremental persistence at available milestones.

## 7. Current active execution

Controlling working register: `ARG_Minimum_Operational_Completion_Gates_2026-11-16.csv` (the current working Library artifact). It remains the sole G1-G10 master register; it has not yet been imported into the ArgAtlas repository. Do not create a competing checklist. Import/reconcile this same register as a separate continuity-record task.

Sole active technical workstream: G2 — shared TCS persistence/adapter boundary.

Current source-trace baseline: `Kelziejordan/Arg@9158fdec5f7cad33727b443dcb110ed958b8ab6f`. This is the recorded source-trace commit, not an assertion that it remains the latest Arg main. Recheck branch/head before any source change.

Key TCS findings to carry forward:
- `api/execute.js` selects JSONL TCS via `createTraceChronicle({ storagePath })` and does not read `ARGOS_TCS_DATABASE_URL`.
- `api/conversation.js` selects PostgreSQL when `ARGOS_TCS_DATABASE_URL` is set; otherwise it requires `ARGOS_EXECUTION_TCS_PATH` and selects JSONL. PostgreSQL configuration is fail-closed rather than silently falling back.
- The two routes therefore have divergent adapter-selection contracts.
- `api/execute.js` initializes a separate Knowledge Vault journal at `${storagePath}.knowledge-vault.jsonl`; TCS and Knowledge Vault journals must remain distinct.
- `api/conversation.js` does not instantiate the Knowledge Vault or ArgLearn memory cycle; Companion TCS events are not automatically ingested by that handler.
- Preview database observations are bounded integration-fixture evidence, not production proof.
- Vercel is intentionally guarded and is not the target runtime for this work; do not reopen Vercel investigations absent a specific acceptance requirement.
- No production target, production activation, or deployment is authorized by this protocol.

Source provenance for the route-selection and journal-boundary findings:
- `api/execute.js` at the source-trace commit; blob `cbff226ccef10f5f5846a5aa837354dd70cea1c9`.
- `api/conversation.js` at the source-trace commit; blob `0234873d24b7fc95f88a22f3445dda50308168f3`.
- `src/tcs/trace-chronicle.js` at the source-trace commit; blob `eda00e49c7d0f1ee871ebf890fa0a7e6dd37981b`.
- `src/memory/knowledge-vault.js` at the source-trace commit; blob `bcebbc924f2660cc1b157693f8a19d82cc7646ee`.

The detailed G2 findings ledger `ARG_TCS_Runtime_Findings_2026-11-16.csv` is currently a working Library artifact and has not yet been imported into ArgAtlas. Its TCS-01/TCS-02 source claims are corroborated by the exact source files above. Its environment and database observations retain their individual evidence classes and must not be promoted to production proof.

Next action: reconcile the latest Arg main/working branch and tests, then design the smallest shared TCS factory change on an authorized non-main work branch. Preserve ArgCore 008, architecture, PR #140 draft/unmerged status, production, and ARG-X separation.

## 8. Session closeout contract

Every completed engineering session must leave:
- Session record.
- Updated active-gate task state.
- Updated evidence references.
- Decision record(s), when a decision was made.
- Explicit next action and resumption baseline.
- Persistence status: COMMITTED, PR-OPEN, LOCAL-ONLY, or UNPERSISTED.

A session is not described as fully recorded unless the persistence status and exact repository reference are verified.
