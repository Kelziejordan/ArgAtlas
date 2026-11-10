# DigitalHands → Memory Continuity Trace — 2026-11-09

Status: SOURCE TRACE COMPLETE; INTEGRATION GAP CONFIRMED
Repository: Kelziejordan/Arg
Branch: checkpoint/digitalhands-memory-trace-2026-11-09
Scope: Read-only source reconciliation; no implementation or runtime mutation.

## Reconciled authority and baseline

- ArgCore 008 remains FROZEN.
- ACL-001 remains FROZEN.
- Four-repository authority assignments remain unchanged.
- ArgAtlas project-state checkpoint: docs/PROJECT_STATE_2026-10-09.md.
- PR #140 remains OPEN, DRAFT, UNMERGED under the 2026-11-09 continuity reconciliation. No PR or deployment action was taken in this task.
- Complete canonical lifecycle remains NOT_PROVEN.

## Four-repository documentation branch review

All four documentation branches compare cleanly against their recorded main baselines; none is behind main.

| Repository | Branch head | Ahead / behind | Changed files |
|---|---|---:|---|
| Arg | cf89b0be9868b24e25dd601da52943c2abf47ad6 | 2 / 0 | README.md; REPOSITORY_ROLE_DECLARATION.md |
| ArgCore | d1165baaad73ae4d72aff37baebdf5d7811fd203 | 1 / 0 | README.md |
| ArgAtlas | cb8209b7cbb66217b0eaa5ab2c28d44c197117ba | 1 / 0 | README.md |
| ArgLearn | 4f4c65fc0146a7e650f8de74a23c79914c3de4e2 | 3 / 0 | README.md; REPOSITORY_ROLE_DECLARATION.md |

The diffs remain limited to the requested documentation alignment. No PRs were opened, no branches merged, and no CI or tests were run. The role links and role distinctions were previously inspected on the branches. Main remains unchanged by this documentation work.

## Actual browser observation trace

Source files and inspected blob SHAs:

- Arg api/conversation.js — 0234873d24b7fc95f88a22f3445dda50308168f3
- Arg src/digital-hands/digital-hands.js — 2f932c12e00fdc85049752f0d45950a69d396e9a
- Arg src/digital-hands/playwright-actuator.js — e32c52cd90eda0fa430f0a7d35134b0352ea6b13
- Arg src/digital-hands/governed-browser-execution.js — 928e886f8db8c4040993f1f469efef5326faa077
- ArgCore src/runtime/lifecycle.js — inspected at ArgCore 008 package source
- Arg src/tcs/postgres-adapter.js — ef93999f13fcf64f9c5d89ef57de9db825bfe5fd

Observed source path:

1. api/conversation.js validates the approved intent and correlation, claims EXECUTION_INTENT in TCS before invoking DigitalHands, then calls createGovernedBrowserExecution.
2. DigitalHands returns the Playwright result. On success it includes observation {url,title,text,links,hash}, executed steps, observedAt, durationMs, browser identity and provenance. The observation hash is SHA-256 over the observation object.
3. ArgCore 008 lifecycle awaits the executor and places its returned object in the EXECUTION_OUTCOME event's outcome field.
4. governed-browser-execution.js iterates ArgCore events in order and awaits tcs.append for each event. Its TCS state includes {status, outcome:event.outcome, grantId}; therefore the EXECUTION_OUTCOME event carries the full DigitalHands result, including the browser observation, into TCS.
5. PostgreSQL TCS serializes the supplied state as canonical JSON and inserts it into the JSONB payload field inside a transaction. It returns the persisted record. The wrapper awaits the append promises.
6. api/conversation.js returns the governed outcome and TCS evidence references to the caller.

Finding: the browser observation is present in the source-level TCS persistence path. This is stronger than a mere event-count record: the full browser executor result is included in the EXECUTION_OUTCOME payload. Actual live database row contents were not queried in this trace. Runtime persistence remains conditional on the configured durable TCS backend. PostgreSQL is selected when ARGOS_TCS_DATABASE_URL is configured; otherwise ARGOS_EXECUTION_TCS_PATH is required and non-test ephemeral paths trigger a warning.

Inherited evidence: ArgAtlas project-state checkpoint records a bounded real Playwright/Chromium DigitalHands integration through observation and TCS. That historical test result was not rerun here.

## Downstream continuity trace

The browser conversation route does not import or instantiate:
- createKnowledgeVault
- createArgLearnEngine
- createArgLearnMemoryCycle
- createLearningEnabledOrchestrator

There is therefore no call from the browser route that captures the browser correlation into Knowledge Vault, no ArgLearn analysis of that browser evidence, and no browser-derived lesson retrieval on a subsequent governed browser cycle.

Existing general orchestration integration is separate:
- api/execute.js creates the file-backed TraceChronicle, Knowledge Vault, ArgLearn engine, memory cycle, and learning-enabled orchestrator.
- src/learning/arglearn-memory-cycle.js requires TCS evidence for a correlation, converts records into evidence references, observes through ArgLearn, and persists an episodic observation in Knowledge Vault.
- src/orchestration/learning-enabled-orchestrator.js retrieves applicable lessons before execution and captures post-execution observations through the memory cycle.
- This is not invoked by api/conversation.js and does not prove a DigitalHands browser-to-memory-to-next-cycle path.

Additional compatibility gap:
- The current conversation route's PostgreSQL TCS adapter exposes asynchronous getByCorrelation and append operations.
- The existing arglearn-memory-cycle calls tcs.getByCorrelation synchronously and immediately reads records.length/maps it.
- learning-enabled-orchestrator also treats retrieveApplicable, TCS append, and related learning calls synchronously.
- A direct wiring of the existing learning cycle into the PostgreSQL browser route would therefore be incompatible without an async-compatible integration boundary. Do not simply instantiate the current cycle in conversation.js and assume it works.

## Disposition

- DigitalHands actuation and observation creation: IMPLEMENTED; bounded real-browser proof inherited from ArgAtlas.
- Full browser observation in TCS event payload: VERIFIED BY SOURCE TRACE; inherited bounded integration evidence exists; live DB payload not independently read in this task.
- TCS durable persistence: IMPLEMENTED; actual backend durability depends on configured PostgreSQL or durable file storage.
- Browser observation → Knowledge Vault: NOT IMPLEMENTED on the inspected conversation route.
- Browser observation → ArgLearn: NOT IMPLEMENTED on the inspected conversation route.
- Browser-derived lesson → next governed browser cycle: NOT PROVEN / no route-level integration found.
- Full Memory Lifecycle V1: NOT PROVEN.

## Smallest next engineering task

Keep architecture and authority assignments frozen. Define and implement only the minimum async-compatible bridge that:
1. reads the just-persisted browser TCS records for the same correlationId;
2. captures an episodic Knowledge Vault observation with exact TCS sequence/source-event references and a bounded task class/outcome;
3. rehydrates/updates ArgLearn from that durable observation without giving ArgLearn persistence or promotion authority;
4. makes only already validated/governed lessons available to the next browser execution through the existing pre-execution learning guard;
5. fails closed or explicitly reports capture failure without replaying the browser action.

Before coding, inspect the relevant existing tests and Knowledge Vault storage configuration to choose the smallest compatible change. Do not alter ArgCore 008, ACL-001, repository roles, Vercel, deployment, or PR #140. Do not rerun the inherited browser/PG suite merely to rediscover existing evidence. Add only targeted tests for the newly connected boundary when implementation is authorized.

## QA / execution record

- Source inspection: COMPLETE for the traced files.
- Documentation branch comparison: COMPLETE; all four branches ahead of main and zero behind.
- Code changes: NONE.
- Tests: NOT RUN.
- CI: NOT RUN.
- Database reads/writes: NONE.
- PRs, merges, deployments, Vercel changes: NONE.
- ArgAtlas handoff persistence: this checkpoint document only; no application code or runtime state changed.

Next checkpoint: inspect current tests and Knowledge Vault storage/configuration, then implement the smallest async-compatible DigitalHands-to-memory bridge and targeted tests if the existing contract confirms the boundary.

Status: IN PROGRESS.


## Follow-up implementation — 2026-11-09

Status: IMPLEMENTED ON ISOLATED ARG BRANCH; TARGETED TEST AUTHORED; EXECUTION UNVERIFIED.

Implementation branch: `Kelziejordan/Arg:integration/digitalhands-memory-bridge-2026-11-09`
Head commit: `81b719e35287f6ab92e13e36d3728f954db807d0`
Base: `main` at `9158fdec5f7cad33727b443dcb110ed958b8ab6f`
Comparison: 2 commits ahead, 0 behind.

Changed files:
- `src/learning/arglearn-memory-cycle.js`: additive `captureObservedRunAsync` and `recordLessonUseAsync`; synchronous APIs retained for existing callers.
- `src/digital-hands/browser-memory-bridge.js`: new browser-specific async bridge. It captures post-execution TCS references into a durable episodic Knowledge Vault observation; before a later browser execution, it retrieves only validated lessons with the existing `ARGOS_PRE_EXECUTION_GUARD_V1` consumption contract and applies only the existing typed `REJECT_IF_PAYLOAD_FIELD_PRESENT_V1` effect.
- `api/conversation.js`: browser route now initializes the existing ArgLearn/Knowledge Vault integration, uses the same correlation identity, records learning consumption before any browser invocation, and captures the result after TCS append.
- `tests/digital-hands-memory-continuity.mjs`: targeted test for async TCS read/write, persisted Knowledge Vault evidence references, restart/reopen, missing-evidence refusal, and validated lesson consumption before the next browser invocation.
- `package.json`: adds `test:digital-hands-memory`.

Storage boundary:
- Browser execution requires a durable Knowledge Vault path from `ARGOS_KNOWLEDGE_VAULT_PATH`, or the established `ARGOS_EXECUTION_TCS_PATH` plus `.knowledge-vault.jsonl` suffix.
- PostgreSQL TCS deployments without a file-backed TCS path must configure `ARGOS_KNOWLEDGE_VAULT_PATH`.
- Outside test mode, relative and ephemeral paths are rejected. The parent directory and existing journal file must be writable before the browser executor is invoked.
- Missing/corrupt/unwritable vault storage fails closed before the browser execution claim/provider boundary.
- If post-execution memory capture fails, the already-completed browser action is not replayed; response reports `CAPTURE_FAILED` with `retrySafe: false`.

Governance boundary:
- No ArgCore 008, ACL-001, repository role, authority, or TCS ownership changes.
- ArgLearn still cannot persist runtime memory or promote lessons.
- Only validated lessons returned by the existing Knowledge Vault retrieval contract can affect a subsequent browser action.
- No raw page text is duplicated into Knowledge Vault. The episodic memory stores browser observation hash/metadata and exact TCS sequence/source-event references; TCS remains the source for the full browser observation payload.

Verification:
- GitHub branch comparison: verified 2 ahead / 0 behind.
- JavaScript syntax compilation (source text with module declarations stripped for parser check): PASS for all four changed/new JavaScript files.
- Targeted test: authored and registered, NOT RUN.
- Existing memory lifecycle/runtime/repeat-task tests: NOT RUN.
- CI: NOT RUN. No matching automatic push workflow for this branch was invoked.
- Runtime PostgreSQL/Knowledge Vault storage: NOT exercised.
- Browser execution: NOT repeated.
- No PR opened, no merge, no deployment, no Vercel changes, no database access.

Execution limitation: the working container cannot resolve GitHub to clone the repository, and the GitHub connector exposes source and commit operations but no arbitrary test execution or workflow-dispatch action. Syntax compilation is not a substitute for executing the targeted test. The new implementation and test therefore remain UNVERIFIED until `npm run test:digital-hands-memory` is executed in a repository workspace with the pinned dependencies installed.

Next checkpoint:
1. Run `npm run test:digital-hands-memory`.
2. Run existing `npm run test:arglearn-memory-lifecycle`, `npm run test:arglearn-memory-runtime-proof`, `npm run test:arglearn-memory-repeat-task-effect`, and `npm run test:conversation-bridge` as regression checks.
3. Confirm deployed runtime has a durable writable `ARGOS_KNOWLEDGE_VAULT_PATH` when PostgreSQL TCS is selected.
4. Do not merge or deploy without separate authorization.

Status: IN PROGRESS; implementation is isolated and not CI-verified or runtime-proven.


## Targeted verification checkpoint — 2026-11-09

Status: TARGETED BRIDGE TEST PASS IN ISOLATED SOURCE SNAPSHOT; FULL REPOSITORY REGRESSION AND RUNTIME CONFIGURATION UNVERIFIED.

Implementation branch: `Kelziejordan/Arg:integration/digitalhands-memory-bridge-2026-11-09`
Implementation head: `81b719e35287f6ab92e13e36d3728f954db807d0`
Comparison rechecked: 2 commits ahead of main, 0 behind.
Commit status: no status checks reported. Commit workflow-run lookup returned no associated PR-triggered runs.

Exact branch source retrieved from GitHub and assembled in an isolated local test directory:
- `src/memory/knowledge-vault.js` blob `bcebbc924f2660cc1b157693f8a19d82cc7646ee`
- `src/learning/arglearn-memory-cycle.js` blob `62b17137340096ceef704cbde092b72aa7f0f1a1`
- `src/digital-hands/browser-memory-bridge.js` blob `186ac9484b48e00b88ebde0eb7175214db8e88c4`
- `tests/digital-hands-memory-continuity.mjs` blob `85469c6db064e7e95cdd8679caa054f35f0bc05a`

Execution:
- Command: `node tests/digital-hands-memory-continuity.mjs`
- Runtime: Node v22.16.0.
- Result: PASS.
- Assertions exercised: async TCS-style read; browser observation capture into Knowledge Vault; exact TCS evidence references; journal reopen/recovery; refusal when TCS evidence is absent; validated lesson retrieval; TCS learning-consumption event; lesson-use evaluation; pre-execution block; no-match behavior.
- Both test success messages emitted.
- This is a real execution of the exact committed test and exact retrieved module sources, but in an isolated minimal snapshot with `package.json` set to ESM. It is not a full Arg checkout.
- The test uses an asynchronous in-memory TCS mock and a stub ArgLearn engine. It does not connect to PostgreSQL or load the pinned ArgLearn package.

Storage configuration source review:
- TCS selects PostgreSQL when `ARGOS_TCS_DATABASE_URL` is configured; otherwise the file TCS path `ARGOS_EXECUTION_TCS_PATH` is required.
- Knowledge Vault uses `ARGOS_KNOWLEDGE_VAULT_PATH`, falling back to `ARGOS_EXECUTION_TCS_PATH` plus `.knowledge-vault.jsonl`.
- Therefore PostgreSQL TCS without a file TCS path requires an explicit `ARGOS_KNOWLEDGE_VAULT_PATH`.
- Outside test mode the vault path must be absolute and non-ephemeral; its parent directory must already exist and be writable, and an existing journal must be writable. The route does not provision a missing parent directory.
- Actual deployment environment values, mounted-volume durability, and PostgreSQL connectivity were not inspected. No Vercel environment/configuration read or write was performed.

Remaining verification:
- `npm run test:arglearn-memory-lifecycle`: NOT RUN.
- `npm run test:arglearn-memory-runtime-proof`: NOT RUN.
- `npm run test:arglearn-memory-repeat-task-effect`: NOT RUN.
- `npm run test:conversation-bridge`: NOT RUN.
- Full-repository `npm run test:digital-hands-memory`: NOT RUN from a full checkout; the exact test passed in the isolated snapshot described above.
- CI: NOT VERIFIED; no workflow run was found for this commit.
- Live PostgreSQL, production storage path, and route-level execution: NOT VERIFIED.
- No browser execution repeated; no PR opened; no merge, deployment, Vercel change, or database access.

Workspace constraint:
- The working container has Node v22.16.0 and npm 10.9.2 but no Arg repository checkout. GitHub hostname resolution failed from the container.
- The GitHub connector provides source/commit/status retrieval but no workflow-dispatch or arbitrary repository test-run action.
- No cloud sandbox was provisioned because cloud spending and Vercel infrastructure remain frozen.

Next checkpoint:
1. Obtain an authorized full Arg repository workspace with the exact branch head and pinned dependencies.
2. Run the four outstanding regression tests listed above, plus the targeted test from that full checkout.
3. Verify the runtime Knowledge Vault path and mounted storage without changing Vercel configuration or deploying.
4. Fix only demonstrated blockers, maximum two repair passes. Keep branch isolated; do not open/merge a PR or deploy without separate authorization.

Status: IN PROGRESS. Targeted test PASS in isolated exact-source snapshot; full regression, CI, PostgreSQL and deployed storage remain UNVERIFIED.
