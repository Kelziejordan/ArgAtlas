# ARG Continuity Reconciliation — 2026-11-17

## Disposition and scope

Read-only source and evidence reconciliation of the existing ArgCore-X recovery/resume candidate against explicit written-state, durable-commit, verification, and recovery requirements.

Architecture remains FROZEN. ArgCore 008 and the original ARG repositories remain protected. No runtime code, tests, infrastructure, PR state, deployment, or protected repository was changed by this reconciliation. This record is documentation only.

## Repository and baseline identity

- Experimental repository: `ARG-Experimental/ArgCore-X`
- Frozen ArgCore 008 source commit: `b71f2379ad370115db261d607ea611787b6aaed5`
- Frozen source tree: `e0978ae818b196e25f6fab61a0ef588b593d49d5`
- ArgCore-X baseline branch: `baseline/argcore-008-b71f2379`
- Baseline snapshot commit: `9f2e0ada8310b36f1f6eed5725bc627497c1044c`
- Normal experimental code checkpoint: `09df63cd5787ffc2250cbc52d417c034f880f8f3`
- Recovery code checkpoint: `42e75c05a003648d494020c033a0eca799286c48`
- Recovery branch: `experiment/durable-recovery-resume-v1`
- PR: [ArgCore-X #1](https://github.com/ARG-Experimental/ArgCore-X/pull/1), OPEN, DRAFT, UNMERGED.
- PR head at reconciliation: `4bb976f37b85fa571fddf1ac5a1538f9ae8f89f6`.
- PR head is three commits after the recovery code checkpoint; comparison shows those three commits changed only `README.md` and `docs/ARGCORE_X_EXPERIMENTAL_DELTA_V1.md`. The runtime/test code checkpoint remains `42e75c05...`.

GitHub comparison confirms the recovery branch is 49 commits ahead and zero behind the frozen snapshot, with the frozen snapshot as merge base. The normal experimental branch is 45 ahead and zero behind. These are additive experimental histories, not missing commits from ArgCore 008.

## Existing implementation located

1. `src/state/index.js` — state continuity primitives:
   - SHA-256 state integrity and verification.
   - Versioned transitions with principal, action, correlation and source-event provenance.
   - Recovered-state adoption with expected-current-state checks.
   - Exported state/history/transition evidence.
   - Important boundary: the implementation stores states, histories and transitions in in-process Maps. It is not itself a durable disk/database store and does not independently survive process restart.

2. `src/runtime/governed-runtime.js` — governed execution and resume:
   - `resume(request, recovery)` requires an accepted recovery receipt.
   - Revalidates recovered-state SHA-256 integrity and provenance.
   - Requires a trusted `verifyRecoveryEvidence` composition-root hook; missing, thrown or mismatched verification fails closed.
   - Binds recovery ID and context digest into signed Governor admission.
   - Reuses the governed grant path and lifecycle journal.
   - Persists `EXECUTION_INTENT` before invoking the executor.
   - Reports `UNCERTAIN_OUTCOME` when outcome persistence fails after executor invocation.

3. `src/runtime/durable-grants-v3.js` — durable authorization/replay state:
   - Persists recovery-ID replay reservation, authority and decision markers, and grant-consumption reservation.
   - Uses temporary files, file sync, exclusive hard-link publication and directory sync.
   - Fails closed on corruption/storage failures and does not automatically retry an already-reserved identity.

4. `src/runtime/lifecycle-journal.js` — local execution evidence:
   - Persists ordered ADMISSION, GRANT_ISSUED, GRANT_CONSUMED, EXECUTION_INTENT and OUTCOME_RECORDED events.
   - Verifies sequence, previous-hash linkage and SHA-256 record integrity.
   - Uses temporary-file publication, file sync, hard-link publication and directory sync.
   - This is an experimental local hash-linked journal, not canonical TCS or an externally anchored ledger.

## Verification evidence

- CI run [43587933723](https://github.com/ARG-Experimental/ArgCore-X/actions/runs/43587933723) is recorded against recovery code checkpoint `42e75c05...`.
- GitHub Actions job inspection returned one job, `test`, completed with conclusion `success`; the deterministic-contracts-and-recovery-tests step completed successfully.
- The recovery integration contract covers accepted recovery evidence, restart replay, 12 concurrent resume attempts with one winner, tampered state, missing verifier, Governor denial, recovery-marker corruption, grant/journal/intent/outcome storage failures, and uncertain outcomes.
- The test fixture uses synthetic source-event identifiers and a fixture verifier. It does not authenticate real canonical TCS events.
- The CI result applies to `42e75c05...`, not automatically to later documentation-only PR head `4bb976f...`.

## Reconciliation against the newly identified continuity requirements

| Requirement | Existing evidence | Disposition |
|---|---|---|
| Written state vs durable commit | File grant store and journal explicitly sync files/directories before reporting publication success | IMPLEMENTED at local-filesystem boundary; backend durability beyond that boundary is not established |
| Verified state | SHA-256 state, marker and journal verification | IMPLEMENTED and exercised by tests |
| Recoverable state | Recovery receipt can be adopted and passed to governed resume; replay identity is durably reserved | IMPLEMENTED at the tested receipt-to-resume boundary |
| Resume after process restart | Grant/replay and journal state are reopened from the same filesystem; replay is rejected | TESTED at the local filesystem boundary |
| Reconcile execution history with actual durable state | Journal inspection verifies sequence and hash chain | PARTIAL; no general workspace/artifact inventory or content-manifest reconciliation is implemented here |
| Selective recovery | Invalid/replayed/corrupt recovery is rejected; intact grants/journal can be inspected | PARTIAL; no general selective artifact restoration protocol |
| Canonical TCS provenance | Trusted verifier hook is required, but tests use synthetic fixtures | NOT_PROVEN |
| Cross-host/distributed durability and arbitration | Local filesystem implementation only | NOT_PROVEN |
| Provider-side exactly-once external effects | Idempotency key is passed to executor; provider enforcement is outside this code | NOT_PROVEN |
| Termux/runtime portability | Existing hard-link publication path previously failed locally with EACCES; primitive probe passed, but full runtime suites were not rerun against a replacement publication protocol | UNVERIFIED |

## Material finding

The recovery candidate already integrates recovery into the durable Governor runtime and reuses existing grant/replay and lifecycle-journal mechanisms. It is not merely a design proposal.

However, the in-memory `createStateContinuity()` Maps are not durable storage. Durable recovery depends on a valid externally supplied recovery receipt, the trusted verifier, and the existing local-filesystem grant/journal stores. The candidate does not, by itself, prove canonical TCS authentication, autonomous reconstruction of arbitrary lost state, cross-host persistence, or provider-side exactly-once effects.

The distinction between a successful local write and a durable commit is partly addressed in the ArgCore-X file stores. It does not establish that every ARG state store or TCS adapter has the same guarantee. That broader claim requires source-level reconciliation at those actual boundaries; it must not be inferred from this candidate.

## Open evidence gaps and bounded next checks

1. Reconcile the trusted `verifyRecoveryEvidence` composition-root implementation with the canonical TCS adapter and authenticated event source. Do not substitute the synthetic test verifier.
2. Verify which production/ArgOS persistence adapter, if any, supplies the accepted recovery receipt to this candidate. This experimental branch is not production-authoritative.
3. Resolve the recorded Termux hard-link `EACCES` portability failure or retain it as an explicit platform limitation; do not claim full local runtime reproducibility until the normal and recovery suites pass on the exact replacement protocol.
4. Inspect actual TCS/storage implementations for write acknowledgement, fsync/transaction commit, read-back verification, restart behavior and fail-closed handling. Reuse existing contracts; do not create a new layer.
5. Keep PR #1 draft and unmerged pending its existing gates and local reproducibility evidence.

## Final status

- Architecture: FROZEN.
- ArgCore 008: FROZEN and unchanged.
- Existing recovery/resume candidate: IMPLEMENTED; CI-VERIFIED at `42e75c05...`; NOT DEPLOYED; NOT MERGED.
- Local Termux full runtime reproducibility: UNVERIFIED.
- Canonical TCS authentication through the candidate: NOT_PROVEN.
- Cross-host persistence and provider-side exactly-once effects: NOT_PROVEN.
- New recovery layer or infrastructure: NOT AUTHORIZED / NOT REQUIRED BY THIS FINDING.

SHIP / FREEZE / EXPAND: FREEZE architecture; EXPAND only the existing continuity verification through the bounded checks above.
