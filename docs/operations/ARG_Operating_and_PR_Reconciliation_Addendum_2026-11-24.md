# ARG Operating and PR Reconciliation Addendum — 2026-11-24

**Repository authority:** ArgAtlas continuity record  
**ARG runtime baseline:** `Kelziejordan/Arg@9158fdec5f7cad33727b443dcb110ed958b8ab6f`  
**ArgCore dependency:** `@kelziejordan/argcore@1.0.0-argcore-008` — FROZEN  
**Architecture:** FROZEN  
**Scope:** Read-only source/PR/CI reconciliation plus persistence of the 2026-11-16 operating records. No ARG runtime, ArgCore, Vercel, production, database, secret, or ARG-X changes.

## 1. Canonical persisted operating records

The following 2026-11-16 source artifacts have now been committed to this ArgAtlas branch without rewriting their historical contents:

- [G1-G10 minimum operational completion gates](./ARG_Minimum_Operational_Completion_Gates_2026-11-16.csv)
- [Detailed TCS runtime findings ledger](./ARG_TCS_Runtime_Findings_2026-11-16.csv)
- [TCS runtime source and environment trace](./ARG_TCS_Runtime_Trace_2026-11-16.md)

These are historical, dated records. This addendum records later findings and does not silently rewrite the November 16 evidence.

## 2. September benchmark checkpoint versus November 16 operating decision

The September benchmark work was not inherently the wrong direction. The Governance Benchmark Specification v1 remains a valid, separately versioned empirical research plan. It defines controlled baselines, paired governed comparisons, repeat runs, failure injection, raw evidence, reproducibility, and governance-overhead measurement.

However, treating the September 15 benchmark-next-gate checkpoint as the immediate next engineering gate was no longer the right sequence by November 16. The current runtime still had unresolved operational proof obligations: production TCS target and role, route-to-storage selection, full governed execution, recovery, and final phone acceptance. Benchmark results would be difficult to interpret responsibly before those foundations were reconciled.

**Disposition:** the November 16 G1-G10 operating decision correctly supersedes the September benchmark-next-gate checkpoint as the immediate operating priority. It does not cancel or invalidate the benchmark specification. Resume benchmark execution only when the relevant runtime baseline, evidence path, environment manifest, and reproducible harness are stable. G2 remains the active technical gate; the other completion gates remain visible so they can progress independently where safe.

## 3. ArgCore 008 admission boundary — source finding

The pinned ArgCore 008 `admitGovernedExecution()` checks principal/request identity, authority principal, action, resource, expiry, replay state, budget, and `policy.allow === true`. It then issues and consumes an execution grant before invoking the supplied executor.

**Important boundary:** ArgCore 008 validates the policy value supplied by its caller; it does not independently prove that the value was produced by an authoritative ARG Governor or durably recorded. That is the caller's responsibility. ArgCore 008 remains frozen and is not to be changed to compensate for an ARG application-layer gap.

### Mainline route findings

At the inspected ARG main SHA:

- `api/execute.js` authenticates a shared action API key, accepts request-provided task/session/principal/action/payload fields, and passes literal `policy: { allow: true }` to the governed orchestrator. No durable ARG Governor decision or proposal-bound approval is recorded by this route. It dispatches to the configured webhook target. This is a source-level governance gap that must be resolved or the route must remain disabled before consequential use.
- `api/conversation.js` has a durable proposal/approval token and an execution-intent claim, but also passes literal `policy: { allow: true }` and does not persist a separate Governor decision. The approval flow is not equivalent to a Governor decision.
- Production environment metadata in the November 16 TCS trace showed the required action/conversation credentials and both TCS backend settings absent. The inspected production configuration therefore failed closed before these execution paths could run. That does not remove the source-level gap.

### PR #140 and PR #142

- PR #140's Companion bootstrap branch also passes literal `policy: { allow: true }`. It is stacked on PR #129 and is not the correct candidate to merge as-is.
- PR #142 introduces a Companion-specific policy evaluator, persists the Governor decision before governed orchestration, and persists a grant-bound execution-start checkpoint before provider invocation. Its current head is `1b4619a16c6f40730e5c2eaf16d09e431896a51c`, based directly on current ARG main. All four GitHub Actions workflows have successful runs on that exact head.
- PR #142 changes the Companion conversation route and operator bootstrap route, but it does not change `api/execute.js`. Therefore it does not close the generic webhook route gap above. It remains draft/unmerged and is not a complete governance certification.
- The previously observed proof-spine failure was on an earlier PR #142 commit (`07061e369093b1534f49d5c9d1acd463c8366dcd`). It is not the current head. The current head's four exact-SHA workflow runs are green. Do not carry forward the earlier failure as current status.

**Required correction before enabling or merging the generic action route:** bind the authenticated service identity server-side (never trust a request-supplied principal), apply an explicit ARG policy decision to the exact action/resource/payload hash, durably append that decision, and fail closed if the decision or pre-provider execution checkpoint cannot be persisted. Do not pass a literal allow flag as a substitute for a Governor decision. Do not change ArgCore 008. This is an ARG application-boundary repair, not permission to add a parallel constitutional runtime.

## 4. Open ARG PR reconciliation — snapshot 2026-11-24

All workflow statuses below are for the exact PR head SHA shown in the GitHub PR snapshot. A green run proves only that workflow on that commit; it does not prove phone acceptance, production readiness, or deployment safety.

| PR | Head branch / head SHA (short) | Base relation | Exact-head CI | Disposition |
|---|---|---|---|---|
| #142 | `verify/arg-personal-operator-v1-2026-11-11` / `1b4619a` | Current `main`; 25 commits ahead, 0 behind | 4/4 workflows pass | Keep draft. Best current Companion candidate, but generic `api/execute.js` governance gap remains; phone acceptance and Preview containment remain unproven. |
| #139 | `ops/pr-reconciliation-register-2026-11-04` / `0e32c25` | Current `main`; 4 ahead, 0 behind | 4/4 pass | Keep draft until its Nov 4 register is replaced by this current reconciliation. |
| #140 | `companion/arg-bootstrap` / `1c77db8` | Stacked on PR #129 | 2/2 pass | Do not merge as-is: literal allow policy; older Companion route; stacked base; Vercel Preview-disable condition not verified. |
| #129 | `work/provider-independent-companion-001` / `7aef918` | Stacked on PR #126 | 2/2 pass | Review overlap with #142 before deciding whether to supersede or retain. Do not merge independently before base-chain reconciliation. |
| #138 | `fix/argcore-008-authority-continuity` / `49f5af9` | Main base is 3 commits behind | 4/4 pass | Relevant authority-continuity work; update against current main and rerun exact-head checks before disposition. |
| #137 | `docs/repository-role-declaration-refresh` / `69bfb0a` | Main base is 3 commits behind | 4/4 pass | Documentation-only; update against current main and review for conflict with canonical ArgAtlas role records. |
| #136 | `governance/argos-identity-coherency-gate-v1` / `c24c594` | Main base is 3 commits behind | 4/4 pass | Keep separate pending overlap review with the Nov 16 ArgAtlas continuity protocol. |
| #128 | `integration/communication-bus-001` / `3afcaa7` | Main base is 7 commits behind; GitHub reports not mergeable | 4/4 pass | Keep draft. Preview POST and durable end-to-end evidence were not completed in its recorded scope. |
| #126 | `companion-operational-ui-001` / `dae3294` | Main base is 7 commits behind | 4/4 pass | Companion UI work; candidate #142 may supersede it. Compare changed files and acceptance coverage before closing or merging. |
| #125 | `tcs-service-role-permissions-gate` / `b851578` | Main base is 7 commits behind | 0/4 pass | Blocked. All four current recorded workflows failed; isolated permission evidence does not replace full CI or role-mapping proof. |
| #115 | `feat/branching-discovery-001` / `db404b5` | Main base is 42 commits behind; GitHub reports not mergeable | 2/3 pass; proof spine failed | Experimental/non-production work. Do not merge; require fresh reconciliation and proof. |
| #114 | `fix/creator-recovery-admission` / `2d3e640` | Main base is 42 commits behind; GitHub reports not mergeable | 3/3 pass | Recovery work remains separate; stale base and no current end-to-end recovery authority proof. |
| #121 | `fix/learning-eval-evidence-retention` / `26196b6` | Stacked on PR #120 | 2/2 pass | Review only after #119/#120 chain is reconciled; not independently mergeable to main. |
| #120 | `feat/argos-memory-recovery-scoring-evidence` / `36fe1af` | Stacked on PR #119 | 2/2 pass | Review after #119 is updated and dispositioned; retain its evidence scope. |
| #119 | `fix/protected-memory-identity-and-evidence` / `18e404a` | Main base is 39 commits behind | 4/4 pass | Potentially relevant memory integrity work; update to current main and rerun before merge decision. |

**Snapshot count:** 15 open ARG PRs. Several are intentionally stacked. A green historical run is not a reason to merge a stale branch.

## 5. Repeated CI work and token/time waste

The current ARG workflow definitions run broad suites for pull requests without meaningful file-path scoping. In particular, the orchestration and memory-lifecycle workflows trigger for any pull request; proof-spine and B1 workflows also run on main-targeted PRs. Consequently, documentation-only PR #139 ran all four suites.

This is avoidable repetition. The smallest safe improvement to evaluate is:
1. Add path-based change detection for source, tests, package/dependency files, and workflow files.
2. Run only the relevant focused suite for a code area.
3. Retain a lightweight documentation validation check for docs-only changes.
4. Keep a required aggregate status that always completes, so skipped suites do not leave branch protection pending.
5. Verify branch protection semantics before changing triggers.

Do not make this workflow change until the Vercel Preview deployment freeze is independently verified, because a branch push to the ARG repository can trigger an automatic Preview deployment. Do not change Vercel settings under this record.

The PR readiness gate should also be automated: reconcile base SHA; record exact head SHA; require all relevant checks on that exact SHA; check for stacked/duplicate PRs; verify acceptance evidence; and verify deployment side effects before marking a PR ready. This moves predictable failures to PR entry rather than late-stage review.

## 6. Forward operating model — parallel lanes, one active technical blocker

Avoid serializing the whole project behind one gate. Keep independent work moving while preserving the evidence boundary:

- **Lane A — G2 TCS (active technical gate):** resolve canonical TCS factory/route selection, verify runtime role privileges on disposable infrastructure, then prove append/reopen/restart. No production changes.
- **Lane B — Companion governance:** use PR #142 as the current candidate for review, but close the generic action-route policy gap and reconcile the final acceptance tests before merge consideration.
- **Lane C — PR portfolio:** update/reconcile relevant branches in dependency order; close only demonstrably superseded work; merge only after exact-head CI, acceptance, and deployment containment pass.
- **Lane D — MCP interoperability:** continue read-only source comparison. It does not block G2 and does not authorize architecture changes.
- **Lane E — memory/recovery and communication bus:** retain as independent workstreams; do not force them into the Companion/G2 critical path.

## 7. Protected boundaries and disposition

- ArgCore 008: FROZEN; no modifications.
- ARG architecture: FROZEN; no new runtime layer.
- ARG-X: separate; no access or integration in this review.
- Vercel Preview/Production settings: FROZEN; `previewDeploymentsDisabled` was not present in the project metadata returned during this inspection, so the disable state remains UNVERIFIED.
- No ARG code, branch, workflow, database, secret, production, or deployment changes were made.
- The three dated operating records were persisted to this ArgAtlas PR branch. They are committed on the branch, not merged to ArgAtlas main.

**Disposition:** IN PROGRESS — operating records persisted on PR #4 branch; governance source gap identified; PR portfolio reconciled; code fixes and merges remain gated by deployment containment and exact acceptance proof.


## 8. Post-action reconciliation — 2026-11-24

The following PR housekeeping actions were completed after the 15-PR snapshot above:

- ARG PR #140 closed as superseded by consolidated PR #142.
- ARG PR #129 closed as superseded by consolidated PR #142.
- ARG PR #126 closed as superseded by consolidated PR #142.
- ARG PR #139 closed because this ArgAtlas addendum now carries the current PR reconciliation; the Nov 4 register remains preserved in its branch history.
- ARG PR #136 closed because its proposed single-objective-only rule conflicts with the operator's explicit requirement to allow independent non-blocking lanes to progress in parallel. Its history is preserved; the proposed gate was not adopted.
- ArgAtlas PR #3 (repository role declaration) merged to ArgAtlas main as documentation-only commit `a45c6aa6ed8f661ae4976c85cba74b722f28d82f`.
- ArgAtlas PR #4 remains draft/unmerged. Its branch predates the PR #3 merge and must be reconciled with current ArgAtlas main before merge review.

**Current ARG open PR count after closures:** 10 — #142, #138, #137, #128, #125, #115, #114, #121, #120, and #119.

No ARG runtime PR was merged. No ARG main commit, Vercel setting, Preview/Production deployment, database, secret, or ArgCore package was changed.

**Current disposition:** PR #142 remains the single Companion integration candidate, but generic `api/execute.js` governance, Preview-disable verification, and final phone acceptance remain blockers. The closed PRs remain available as historical branches and records; no work was deleted.


## 9. CI and TCS role clarifications — 2026-11-24

### CI failure root cause and repeat cost

- PR #125's inspected Proof Spine failure occurred during dependency installation, before tests ran: npm attempted `git ls-remote ssh://git@github.com/Kelziejordan/ArgLearn.git` and failed with `Permission denied (publickey)`. This is an install/authentication failure, not a failing TCS assertion.
- PR #142's current workflows configure read-only ArgLearn access and all four exact-head workflows pass. This is the current reusable CI repair evidence.
- The inspected ARG main tree has no `package-lock.json` or other npm lockfile. Workflows use `npm install`, not `npm ci`. Dependency resolution is therefore not pinned by a committed lockfile.
- Broad pull-request triggers run all four suites for documentation-only changes. PR #139 is an observed example: a one-file documentation PR ran all four workflows.

**Smallest CI efficiency/reproducibility repair to prepare:** commit a lockfile after reconciling the pinned ArgLearn Git dependency; switch to `npm ci`; use path-aware suite selection plus an always-completing aggregate required check; centralize the same read-only ArgLearn authentication setup. Do not push this workflow repair to ARG until Vercel Preview disablement is verified.

### TCS runtime role identity

PR #125's additive migration grants privileges to PostgreSQL role `service_role`. The November 16 TCS findings also recorded that the Neon integration role named `argos_companion_runtime` could `SET ROLE neon_superuser`. The mapping between the Vercel connection secret and the Neon database role was intentionally not decrypted or independently verified.

Therefore, the PR #125 migration must not be assumed to solve the actual runtime-role privilege issue. First verify the exact database username/role mapping through non-secret metadata or an approved secure check on the disposable integration branch. Then grant only the minimum required permissions to the verified runtime role. No role or database mutation is authorized by this finding.

**Disposition:** CI root cause identified; lockfile and trigger optimization remain proposed; TCS runtime-role mapping remains UNVERIFIED.
