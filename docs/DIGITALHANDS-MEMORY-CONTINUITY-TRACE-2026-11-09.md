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


### Test source-integrity confirmation

Before the final test run, the four retrieved local source files were checked with Git's blob-hash algorithm against their GitHub blob SHAs. All four matched exactly:
- Knowledge Vault: `bcebbc924f2660cc1b157693f8a19d82cc7646ee`
- Async memory cycle: `62b17137340096ceef704cbde092b72aa7f0f1a1`
- Browser memory bridge: `186ac9484b48e00b88ebde0eb7175214db8e88c4`
- Targeted test: `85469c6db064e7e95cdd8679caa054f35f0bc05a`

The test was then executed again from this hash-verified source snapshot using `npm run test:digital-hands-memory`; both PASS messages were emitted. `node --check` also passed for all four files. The local package manifest only supplies ESM mode and the same test script; it is not a full repository package install.


## Companion end-to-end and memory-bridge integration checkpoint — 2026-11-10

Status: COMPANION BASELINE E2E PROVEN IN CI; MEMORY-BRIDGE INTEGRATION IMPLEMENTED LOCALLY AND TARGETED-TESTED; POSTGRES/PLAYWRIGHT MEMORY E2E NOT YET RUN.

### Verified existing PR #140 end-to-end baseline

PR #140 remains OPEN, DRAFT, UNMERGED. Its current head is:
`1c77db8d4f94fd4c8b4ebafb378b3dc7f5a2fb69`.

Fresh GitHub Actions evidence retrieved for that exact commit:
- Arg Orchestration CI: run `42507418209`; PostgreSQL Companion E2E job `128812946968`; conclusion SUCCESS.
- ArgOS Memory Lifecycle: run `42507418318`; memory-lifecycle job `128812947626`; conclusion SUCCESS.

The PostgreSQL Companion E2E job log explicitly reports PASS for:
- real Companion UI flow and explicit user approval;
- real Playwright Chromium execution against the harmless Selenium test page;
- ArgCore 008 governed lifecycle;
- durable PostgreSQL 16 TCS evidence;
- replay rejection;
- separate Node process reopening PostgreSQL and verifying the evidence chain and retained screenshots.

The model endpoint was mocked in CI; the browser actuator and PostgreSQL service were real. This establishes the first complete Companion execution baseline. It does not yet establish browser observation → Knowledge Vault → ArgLearn → next browser cycle on that CI commit.

### Local integrated candidate

Workspace: `/mnt/data/arg-fast-forward/integrated/arg-companion-memory-bridge`
Source baseline: PR #140 head `1c77db8d4f94fd4c8b4ebafb378b3dc7f5a2fb69` plus the local Companion identity-isolation candidate.
Workspace is writable but has no Git metadata. No remote branch, PR, push, or CI run was created for these new changes.

Integrated changes:
- Added async-compatible ArgLearn memory-cycle operations for PostgreSQL-backed TCS reads.
- Added browser-specific memory bridge and wired it into the current Companion conversation route, rather than the older direct DigitalHands route.
- Pre-execution learning guard runs after the orchestrator's durable execution claim and before the DigitalHands actuator call.
- After governed execution, browser observation hash/metadata and exact TCS sequence/source-event references are captured into the durable Knowledge Vault. Raw browser page text remains in TCS and is not duplicated into the vault.
- Only validated, governed lessons are eligible for pre-execution retrieval. ArgLearn cannot promote lessons.
- Missing, relative/ephemeral production storage, missing parent directory, or unwritable Knowledge Vault path fails closed before execution. PostgreSQL TCS requires explicit `ARGOS_KNOWLEDGE_VAULT_PATH` unless a file TCS path provides the configured fallback.
- Post-execution capture failure reports `CAPTURE_FAILED` and `retrySafe: false`; the completed browser action is not replayed.
- Updated the existing learning-enabled orchestrator to inspect both top-level and intent-only payloads and to honor the typed `NONEMPTY_ARRAY` effect condition. The repeat-task regression test exposed this exact defect; the targeted repair now passes its negative and positive controls.
- Extended the existing conversation bridge test to cover browser observation capture and a validated lesson blocking the next browser invocation.
- Extended `companion-postgres-e2e.mjs` to cover actual PostgreSQL TCS → Knowledge Vault persistence → ArgLearn candidate → explicit validation/promotion → next-cycle block before the real Playwright actuator, plus fresh-process Knowledge Vault reopen.
- Added and registered `test:digital-hands-memory` and `test:arglearn-memory-repeat-task-effect` in the test suite.

### Local verification

Exact private package source was assembled from pinned GitHub source and blob-hash checked:
- ArgCore 008 release commit `b71f2379ad370115db261d607ea611787b6aaed5`; package metadata and all seven imported runtime/state source files match their Git blob hashes.
- ArgLearn pin `d129803e6ef09920c47b8a3d8550e84bb97c1ff7`; package metadata, entrypoint, and engine source match their Git blob hashes.
- This is exact-source local assembly, not an npm registry installation. The container cannot reach npm/GitHub package registries.

Executed in the integrated local workspace:
- `tests/arglearn-memory-lifecycle.mjs`: PASS.
- `tests/arglearn-memory-runtime-proof.mjs`: PASS; real ArgOS orchestrator + ArgCore 008 adapter + durable local TCS/Knowledge Vault, controlled provider.
- `tests/arglearn-memory-repeat-task-effect.mjs`: PASS after the demonstrated intent-only payload / typed-condition repair; cold provider call 1, warm provider calls 0, valid empty-list positive control provider calls 1.
- `tests/conversation-bridge-contract.mjs`: PASS; actual conversation route, file-backed TCS, browser output capture, and next-cycle validated lesson block.
- `tests/digital-hands-memory-continuity.mjs`: PASS; actual pinned ArgLearn engine, async TCS test adapter, durable Knowledge Vault journal, reopen, exact evidence references, and next-cycle block.
- `tests/companion-identity-isolation.mjs`: PASS.
- `tests/companion-provider-registry.mjs`: PASS.
- `node --check`: PASS for the eight changed/new JavaScript source and test files.

Not run locally:
- `tests/companion-postgres-e2e.mjs`: requires `pg`, Playwright/Chromium, and disposable PostgreSQL; the test is extended but not executed in this workspace.
- `tests/companion-arg-bootstrap.mjs`: local run blocked by missing public `canonicalize` dependency; npm registry access is unavailable.
- CI for the integrated candidate: NOT RUN.
- Production Knowledge Vault mount/path: NOT VERIFIED.

### Infrastructure and protected boundaries

Read-only Neon inspection found project `argos-companion-preview` and ready non-default branch `pr140-integration-2026-11-09`. Its TCS schema exists and it currently contains 32 journal events and 3 execution claims. No writes were made; it is not an empty disposable database and was not used for testing. The existing CI workflow provisions disposable PostgreSQL 16, so the integrated candidate should be verified there rather than mutating the populated Neon branch.

Read-only Vercel project metadata did not expose `previewDeploymentsDisabled`. No Vercel configuration or deployment was changed. Existing PR #140 evidence records automatic Preview creation from branch pushes. Therefore a push/PR for this integrated candidate may create another Vercel Preview.

No merge, production deployment, ArgCore 008 change, ACL-001 change, PR #140 change, or database write was performed.

### Next decision / smallest remaining gate

The integrated candidate is locally targeted-tested. The remaining proof is to run the extended real PostgreSQL/Playwright E2E and full CI on the integrated candidate. The existing GitHub Actions workflows already have the package-access secrets and disposable PostgreSQL service; no new package or database credentials should be required.

However, triggering those workflows requires a remote branch push / draft PR, which may automatically create a Vercel Preview. Vercel Preview-disable state is not verified and its configuration remains frozen. Do not push or open a PR until the operator explicitly authorizes that Preview side effect or an approved method to suppress it is established.

Next checkpoint:
1. Obtain explicit authorization for one integrated-candidate draft PR/branch push knowing it may create a Vercel Preview, or an approved Preview-suppression mechanism.
2. Run the existing Arg Orchestration CI and ArgOS Memory Lifecycle workflows on the exact integrated candidate.
3. Verify the extended PostgreSQL/Playwright/fresh-process Knowledge Vault E2E result and inspect CI logs/artifacts.
4. Verify production `ARGOS_KNOWLEDGE_VAULT_PATH` and durable mounted storage read-only; do not change deployment configuration.
5. Keep PR #140 draft/unmerged and production deployment frozen until separate approval.

Status: IN PROGRESS. First Companion E2E baseline is CI-proven. The integrated memory-bridge candidate passes targeted local tests; the full PostgreSQL/Playwright memory-continuity E2E remains unverified pending CI authorization.
