# ARG-TRADING-001 — Trading Domain Application Registry

Status: ASSIGNED — IMPLEMENTATION CONTINUES
Decision date: 2026-10-21
Authority: ArgAtlas application registry and provenance record

## Application identity

- Application ID: ARG-TRADING-001
- Application name: ARG Trading
- Purpose: Build and verify a paper-only trading application, beginning with one bounded live-market-data paper session and a retained evidence report.
- Canonical repository: https://github.com/Kelziejordan/ArgusTradingBotv11
- Default branch: main
- Implementation layer: ARG → Branches / Domain Applications → ARG-TRADING-001
- Runtime profile: paper-only; public market data initially; no real-money order path.

This record assigns the existing `ArgusTradingBotv11` repository as the canonical implementation home for ARG-TRADING-001. The repository's current code is an implementation baseline, not proof that the required trading lifecycle or a live-data paper session is complete.

## Architecture boundaries

- ArgCore remains frozen. No application code or application-specific policy is added to ArgCore.
- ArgOS / `Kelziejordan/Arg` remains the governed runtime. Trading strategy, market-data, broker, UI, and trading-ledger implementation must not be merged into ArgOS runtime source.
- ArgAtlas records decisions, provenance, acceptance evidence, and project state; it is not the trading implementation repository.
- The trading application may consume shared capabilities only through explicitly reviewed, versioned interfaces. No direct import of ArgOS internals or frozen ArgCore implementation internals.
- `Kelziejordan/AegisKraken` is explicitly excluded: it is not a source, destination, dependency, or implementation reference for this application.
- No real-money orders, exchange trading credentials, paid data, cloud spend, or production deployment are authorized by this record.

## Existing implementation baseline

Initial inspection of the canonical repository found:
- A React trading dashboard and TypeScript trading engine.
- A server using CCXT's Kraken adapter to poll public ticker data for BTC/USD, ETH/USD, and SOL/USD.
- In-memory strategy state and simulated market-data code.
- Strategy implementations labelled TREND, MEAN_REV, and STAT_ARB.

These findings make the repository a plausible and now assigned application home. They do not establish durable ledgering, validated exchange-market observations, a fully integrated paper broker, restart-safe order continuity, cost-realistic performance, or a completed live-data paper run. Those remain acceptance requirements.

## Preserved prior work and provenance

- `Kelziejordan/Arg` PR #130, branch `work/live-market-paper-trader-001`, is preserved as a source of candidate implementation and test evidence only: https://github.com/Kelziejordan/Arg/pull/130
- PR #130 must not be merged into `Kelziejordan/Arg`.
- Any transfer from PR #130 must be selective and reviewed in this application repository. Preserve source commit SHAs, identify every transferred file, adapt imports and interfaces, and rerun target-repository tests. Do not blindly copy the PR or treat its CI as proof of target integration.
- The PR specification reports passing component tests, but explicitly states that the capture CLI has not yet connected to the live Kraken endpoint and that a live-data paper session is not complete.

## Definition of done — first paper-session gate

1. Target-repository build and tests pass on the exact candidate commit.
2. A documented profile fixes the instrument, venue, timeframe, strategy version, and data inputs. Any crypto adaptation of the Chris Creamer futures/GEX framework is explicitly named; unavailable GEX must not be silently replaced with a proxy.
3. Public live market observations are received, timestamped, health-checked, and durably journaled with integrity verification.
4. Strategy evaluation consumes only valid, closed, time-causal evidence and emits a decision with a traceable reason.
5. Paper execution consumes a fresh validated quote/book; simulated fills are labelled PAPER and record quote, timestamp, quantity, fee, slippage, and reason.
6. Risk limits are checked before every simulated order. Stale data, missing required inputs, journal failure, unresolved order state, or a risk breach fail closed.
7. The run is bounded by explicit duration, starting paper balance, exposure and loss limits, and stop conditions.
8. The session leaves retained raw market evidence, decisions, order/fill records, risk events, and a summary report. The report distinguishes no-trade periods from execution failures and includes costs.
9. Evidence proves the session used live public market observations; a simulator or historical replay cannot substitute for this gate.

## Immediate next actions

1. Create an isolated working branch in `Kelziejordan/ArgusTradingBotv11`.
2. Run and inspect the existing build/tests; establish a baseline before changing code.
3. Compare the target repository's engine and server against PR #130's feed, journal, broker, footprint, and strategy components.
4. Transfer only the components required for the first paper-session gate, preserving provenance and adapting them to the target app.
5. Run target-repository CI and close the live-feed → strategy → paper execution → retained evidence path.
6. Start one bounded paper session only after all fail-closed and risk gates pass.

## Current status

- Canonical application repository: ASSIGNED.
- Existing repository baseline: PRESENT; integration and evidence maturity NOT YET PROVEN.
- PR #130: PRESERVED AS EVIDENCE; NOT TO BE MERGED INTO ARG.
- First live-data paper session: NOT YET PROVEN.
- Real-money trading: OUT OF SCOPE.
