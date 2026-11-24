# ArgAtlas

ArgAtlas is the engineering continuity, provenance, canonical lifecycle, and architectural-memory authority for the Arg ecosystem.

It preserves what is known, what is evidenced, what remains uncertain, and what must be proven before an implementation or claim is promoted.

## Current project state — 2026-11-24

The current operating and PR reconciliation record is [ARG Operating and PR Reconciliation Addendum — 2026-11-24](./docs/operations/ARG_Operating_and_PR_Reconciliation_Addendum_2026-11-24.md).

The canonical operational records are:
- [G1-G10 minimum operational completion gates](./docs/operations/ARG_Minimum_Operational_Completion_Gates_2026-11-16.csv)
- [Detailed TCS runtime findings ledger](./docs/operations/ARG_TCS_Runtime_Findings_2026-11-16.csv)
- [TCS runtime trace](./docs/operations/ARG_TCS_Runtime_Trace_2026-11-16.md)

These November 16 artifacts are preserved as dated evidence; the November 24 addendum records later findings and dispositions without rewriting historical records.

Current executable ArgOS dependency: `@kelziejordan/argcore@1.0.0-argcore-008`. ArgCore 008 and the ARG architecture remain FROZEN.

**Current active technical gate:** G2 — production TCS target, route/backend selection, runtime role permissions, and durable append/reopen/restart proof. G2 is the active critical-path gate, not a reason to stop independent, safe work. Companion governance, PR reconciliation, memory/recovery, and MCP interoperability may progress in separate lanes without changing the frozen architecture or claiming G2 complete.

The September Governance Benchmark Specification v1 remains a valid future empirical research plan. The November 16 G1-G10/G2 operating decision supersedes the stale September benchmark-next-gate checkpoint as the immediate engineering priority; it does not cancel the benchmark specification.

The prior [2026-10-09 project-state checkpoint](./docs/PROJECT_STATE_2026-10-09.md) and [PR #140 continuity reconciliation](./docs/PR-140-CONTINUITY-RECONCILIATION-2026-11-09.md) are retained as historical evidence and must not be treated as the current PR or operating state.

## Canonical lifecycle

[ACL-001](./docs/ACL-001-CANONICAL-INTELLIGENCE-LIFECYCLE.md) defines:

Intent -> Translation (ATL) -> Planning -> Execution -> Validation -> Observation -> Memory -> Next Intent

Recovery is a controlled exception path that rejoins through Observation. Learning is a governed feedback loop. Semantic ATL is the translation layer between Intent and Planning. The older `argos-atl` C-ABI/interface use remains a distinct forensic item.

## Continuity rule

ArgAtlas records evidence without promoting unsupported claims. Historical artifacts do not automatically override canonical architecture or current executable evidence. DOCUMENTED, IMPLEMENTED, INTEGRATED, EXERCISED, and PROVEN are distinct statuses; each test proves only its exercised boundary.

The [repository role declaration](./REPOSITORY_ROLE_DECLARATION.md) defines ArgAtlas's documentary authority and its boundary from executable runtime authority.

**Branch note:** the November 24 operations records and README update are currently on the draft ArgAtlas PR #4 branch. They are committed and verified on that branch, not yet merged to ArgAtlas main.
