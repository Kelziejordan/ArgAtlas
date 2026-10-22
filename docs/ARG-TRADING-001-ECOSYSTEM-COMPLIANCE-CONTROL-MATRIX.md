# ARG-TRADING-001 — Ecosystem Compliance and Control Matrix

Status: PROPOSED IMPLEMENTATION CONTROL MAP
Date: 2026-10-22
Authority: Existing ARG/ArgOS contracts remain authoritative; this document adds no constitutional authority.
Application registry: docs/ARG-TRADING-001-APPLICATION-REGISTRY.md
Canonical trading repository: https://github.com/Kelziejordan/ArgusTradingBotv11
Runtime repository: https://github.com/Kelziejordan/Arg
Constitutional dependency: @kelziejordan/argcore@1.0.0-argcore-008 (frozen)

## Purpose and non-duplication rule

This matrix maps trading-specific obligations to existing authoritative components. It does not replace ACL-001, GOV-001/GOV-002, ArgCore 008, TCS, Knowledge Vault, VTM, or ArgLearn contracts. Where an existing contract applies, the trading workload must consume it through a reviewed interface rather than reimplement its authority. Unknown or unverified integration is recorded NOT PROVEN, never inferred from shared code or a green unrelated workflow.

## Authority and integration matrix

| Capability | Existing authority / source of truth | Trading-specific obligation | Evidence required | Boundary / prohibited duplication |
|---|---|---|---|---|
| Application identity and repository placement | ArgAtlas application registry and domain application boundary | Keep trading implementation in ArgusTradingBotv11; record repository lineage and proof status in ArgAtlas | Registry links, source commit IDs, tested target commit | Do not move trading implementation into Arg runtime; do not use AegisKraken |
| Canonical lifecycle | ACL-001; ArgOS runtime in Kelziejordan/Arg | Submit bounded trading workload through Intent → ATL → Planning → Governance → Execution → Observation → Validation → Memory/TCS | Correlated lifecycle evidence for one paper workload | No parallel trading orchestrator; do not claim full lifecycle based on standalone session |
| Constitutional governance | Frozen ArgCore 008 contract, consumed by ArgOS | Consequential actions must pass through the existing governed authorization boundary | Authorization decision linked to workload, action, principal and correlation identity | No changes to ArgCore; no copied or competing governance/recovery authority |
| Market-data acquisition | Trading application domain implementation | Validate timestamps, freshness, sequencing, book integrity and resynchronization before strategy use | Feed-health events, snapshot recovery evidence, invalid/stale counters | ArgOS does not become a second market feed; public data only under current scope |
| Strategy evaluation | Trading application domain implementation; ArgLearn is feedback/evaluation layer | Version each strategy and configuration; log decision, inputs and reason, including no-trade reasons | Reproducible decision record tied to strategy/config/data version | ArgLearn does not silently select or mutate live strategy or risk policy |
| Paper execution and risk | Trading paper broker plus existing governance boundary | Enforce paper-only mode, exposure/loss limits, fresh quote/book, fees/slippage and duplicate/unresolved-order protections before every order | Paper order/fill/rejection records, risk checks, account ledger reconciliation | No real-money orders, exchange trading credentials, or production deployment authorized |
| Trading market journal | Trading-specific append-only market journal | Preserve detailed market events and verify hash chain; drain pending writes before closing | Integrity verification, event count/sequence, clean shutdown test | Do not make this a competing canonical execution history; do not duplicate every market tick into TCS without need |
| Canonical execution evidence | TCS under existing ArgOS contract | Publish linked lifecycle events, decisions, actions, outcomes, validation and evidence references | TCS records with stable correlation/execution identity and durable-write confirmation | No new TCS schema or adapter until existing contracts and actual persistence status are inspected |
| Knowledge retention | Knowledge Vault / memory lifecycle contract | Retain only validated, provenance-bearing lessons and useful session summaries | Source event IDs, validation disposition, reopen/retrieval tests | Raw market feed is not automatically promoted to authoritative memory; no duplicate memory authority |
| Learning and strategy comparison | ArgLearn governed learning feedback layer | Analyze comparable evidence, compare four strategies using common conditions, propose improvements with uncertainty | Reproducible experiment definitions, data split, costs, proposal and approval trail | No autonomous policy changes; no leakage from future data into historical-learning evaluations |
| Temporal reconstruction and oversight | VTM existing contract | Use VTM for analytical reconstruction and invariant monitoring where integrated | Reconstructed timeline linked to canonical events; divergence results | VTM remains non-authoritative; do not duplicate Governor, TCS, validation, or recovery authority |
| External-interface actions | Digital Hands within ArgOS governed execution | Use only where a workload requires UI/browser actions; retain observation and evidence | Authorized action trace, actual effect, observation and validation | Do not force Digital Hands into every market-feed tick or bypass ArgOS authorization |
| Architecture and engineering continuity | ArgAtlas | Record decisions, dependencies, evidence links, verified status and unresolved obligations | Updated registry/checkpoint tied to commits and test runs | ArgAtlas is not the trading runtime, execution ledger, or duplicate policy engine |
| Automatic recovery | ArgCore 008 authority as consumed by ArgOS; trading-specific feed recovery subordinate to it | Permit bounded reconnect/resnapshot/retry; restore durable state; reconcile ambiguous actions before resuming | Fault-injection tests, no duplicate action, verified snapshot/state, recovery trace | No competing recovery authority; fail closed when state/authority is unavailable |
| Self-repair | ArgOS governance + ArgLearn proposals; existing review/deployment controls | Diagnose and propose isolated minimal patches; test and report; deploy only through authorized workflow | Reproducible failure, patch diff, regression evidence, explicit approval/deployment record | A learned finding is not authorization; no self-modification of risk limits, governance, or frozen contracts |
| Operator dashboard | Trading application UI, constrained by server-side runtime policy | Provide concise status, drill-down evidence and validated configuration changes with audit history | UI/API contract tests, authorization tests, config version history, clear state labels | UI is not an authority boundary; client-side controls must not bypass server-side checks |
| Development and continuous operation | Trading workflow plus a suitable persistent runtime when approved | Development diagnostics should return quickly; continuous mode checkpoints every 15 minutes without resetting state | Runtime-specific health, checkpoint continuity, restart/recovery and persistence tests | Do not equate a short diagnostic with strategy proof; GitHub Actions is for bounded CI/session jobs, not presumed infinite hosting |
| Risk disclosure and results | Trading application reporting | Describe all results as observational/paper evidence, with uncertainty and costs; never imply guaranteed returns | Reports state mode, dates, assumptions, limitations and whether any trades occurred | No profitability claim; one session or Monte Carlo path is not proof of expected returns |

## Development feedback policy

The goal of development mode is faster diagnosis, not shortening every proof to an arbitrary wall-clock duration.

- Use fast deterministic tests and fault-injection tests as the first feedback loop.
- Use short live-feed smoke checks to verify connection, valid trades/books, journal writes, and orderly shutdown.
- The requested six-minute session is a useful bounded integration smoke test only if it can reach the relevant candle boundary. It is not guaranteed to produce five closed five-minute candles or enough context for a strategy decision.
- The current Chris Creamer-style runtime requires five closed five-minute bars before context evaluation. A fresh six-minute process with no verified prior context will likely test ingestion and initialization rather than strategy quality. Preserve this distinction in reports.
- Use longer sessions only for questions that require longer observation; do not use 30 minutes as the default test duration.
- Continuous paper mode is a separate operational mode: 15-minute checkpoints, persistent account/position/risk state, durable journal, bounded automatic recovery, and fail-closed behavior if integrity cannot be established.
- A checkpoint is a report boundary, not a process restart or account reset.

## Dashboard usability and control requirements

Design goal: sophisticated and information-dense without clutter. The primary view should answer within seconds: Is the system healthy? Which strategy is active? What is it doing and why? What is the paper account/risk state? What needs attention?

1. Top status strip: mode (development/continuous/halted), feed health, journal integrity, active strategy/config version, last checkpoint and current time.
2. Main workspace: market context and current strategy decision with plain-language reason; clear states for EVALUATING, WAITING FOR SETUP, PAPER ORDER OPEN/FILLED, RECONNECTING, RECOVERING, and HALTED.
3. Account/risk panel: simulated balance, realized/unrealized P&L when applicable, open exposure, fees/slippage, daily loss budget and remaining risk. Label all values PAPER.
4. Activity timeline: decisions, proposed/rejected orders, fills, recovery events, validation and checkpoint history; drill down to evidence without showing all raw market ticks by default.
5. Configuration drawer: strategy selection and approved parameters, risk-per-trade, max position, loss limits and reporting cadence. Display units, allowed range, effective time, validation result and audit history. Existing positions are not silently rewritten by a new setting.
6. Evidence/export view: download report and linked event/journal evidence; distinguish verified results from NOT PROVEN obligations.
7. No profit-guarantee language or success-only presentation. Show quiet/no-trade periods, failed checks, costs, uncertainty and market volatility explicitly.

## Current blockers and disposition

1. The latest live paper run processed 110,947 market events, 2,725 trades, 108,222 book events, six closed bars and six decisions; it produced no proposals/orders/fills. No-trade reasons were four insufficient closed bars and two unconfirmed internal swing structures. It reported zero feed errors, stale events and invalid events.
2. After reporting completion, the process emitted: `durable journal failure; market feed stopped fail-closed: market journal is not open`. The exact cause and shutdown race status remain NOT PROVEN until artifact/journal inspection and a regression test establish the fix.
3. Full ArgOS ACL-001 integration for the trading workload is NOT PROVEN.
4. ArgLearn, ArgAtlas, VTM, TCS and Knowledge Vault participation in this specific trading session is NOT PROVEN until evidence shows their actual role and interface.
5. Four-strategy comparison framework is not yet proven complete.
6. Persistent continuous paper runtime, 15-minute checkpoints, and safe cross-restart account continuity are NOT PROVEN.
7. Real-money trading, paid market data and production deployment remain OUT OF SCOPE.

## Recommended order

1. Review this matrix against existing executable contracts and mark any stale assumptions.
2. Inspect the retained paper-session report/journal and fix the post-completion journal write/shutdown defect with regression tests.
3. Add a fast diagnostic ladder: deterministic tests → feed/journal smoke test → six-minute bounded integration test where appropriate. Preserve the five-bar/context caveat.
4. Prove one trading workload through the existing ArgOS lifecycle with TCS/Knowledge Vault/ArgLearn/ArgAtlas roles evidenced and no duplicate authority.
5. Build the clean operator dashboard and validated server-side configuration contract.
6. Add continuous mode only after durable persistence, restart continuity, duplicate-order prevention, and recovery tests pass.
7. Expand to the four-strategy comparison under shared data, cost and risk assumptions.

## Acceptance rule

A component is not counted as integrated because it is installed, imported, running, or mentioned in documentation. Integration is counted only when the actual trading workload crosses its reviewed interface and produces evidence for the contract it is supposed to fulfil.
