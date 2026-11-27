# ARG TCS Runtime Trace — 2026-11-16

**Disposition:** IN PROGRESS — read-only source and environment reconciliation  
**Architecture:** FROZEN  
**Repository scope:** Arg, ArgCore 008, ArgLearn, ArgAtlas only  
**Production/Preview/DB changes:** None performed

## Executive result

The TCS implementation is not selected consistently across ARG's two execution routes.

- `api/execute.js` always constructs the JSONL file adapter (`createTraceChronicle`) using `ARGOS_EXECUTION_TCS_PATH`. It does not import or select `createPostgresTcs`, so adding `ARGOS_TCS_DATABASE_URL` alone will not move this route to PostgreSQL.
- `api/conversation.js` selects PostgreSQL when `ARGOS_TCS_DATABASE_URL` is set. If it is not set, it falls back to the JSONL adapter using `ARGOS_EXECUTION_TCS_PATH`; if neither is configured, it fails closed.
- The conversation route writes proposals, approvals and execution claims/events to TCS. It does not instantiate the Knowledge Vault or ArgLearn memory cycle in the inspected handler.
- The general execute route instantiates both TCS and Knowledge Vault as separate JSONL files: `ARGOS_EXECUTION_TCS_PATH` and `${ARGOS_EXECUTION_TCS_PATH}.knowledge-vault.jsonl`.

**Production configuration finding:** Vercel project `argos-affiliate-ledger` has a READY production-target deployment from Arg `main` at `9158fdec5f7cad33727b443dcb110ed958b8ab6f`. Its production environment metadata contains no `ARGOS_TCS_DATABASE_URL`, no `ARGOS_EXECUTION_TCS_PATH`, and no route-specific ARGOS action/conversation credentials. `hiddenProductionEnvCount` was zero. Therefore the inspected production configuration does not provide either TCS backend to these routes. The source returns 503 for missing configuration (conversation authentication fails before TCS initialization; execute checks action key/target before TCS path).

**Preview finding:** Vercel has Preview-only, branch-scoped `ARGOS_TCS_DATABASE_URL` entries for `companion/arg-bootstrap`, `work/provider-independent-companion-001`, and `integration/communication-bus-001`. The comment on the PR #140 entry describes an isolated Neon Companion Preview branch. This is not a production database configuration. The secret value was not decrypted, so the exact connection-string-to-Neon-branch mapping remains UNVERIFIED.

A matching Neon project, `argos-companion-preview` (`sparkling-credit-97649722`), contains branch `pr140-integration-2026-11-09` (`br-broad-resonance-b8d567lu`) and database `argos_companion`. The three expected tables exist there: `tcs_execution_claims`, `tcs_journal_events`, and `tcs_journal_head`. This proves schema presence on that Preview/integration branch, not that production uses it.

## Reconciled baseline

- Arg `main` latest commit returned by repository commit search: `9158fdec5f7cad33727b443dcb110ed958b8ab6f` (2026-11-01).
- Production-target Vercel deployment: `dpl_4WFA1SYTrnn6B4WuU2iP2Sm4a642`, READY, branch `main`, same commit SHA.
- PR #140 remains separate: head `1c77db8d4f94fd4c8b4ebafb378b3dc7f5a2fb69`, branch `companion/arg-bootstrap`; no merge or deployment action was performed in this investigation.
- ArgAtlas continuity record `PR-140-CONTINUITY-RECONCILIATION-2026-11-09.md` retains architecture, ArgCore 008, PR #140, Preview and Production freeze boundaries.

## Runtime trace

### 1. Route dispatch

`server.mjs` imports `api/execute.js`, `api/conversation.js`, and the affiliate redirect handler. Its standalone Node server sends `/api/conversation` to the conversation handler and sends other non-affiliate paths to the execute handler.

The deployed Vercel project also has file-based API functions. `vercel.json` configures `api/execute.js` with a 30-second maximum duration; the API files remain the relevant serverless entry points.

### 2. General execution route — `api/execute.js`

Source path: `api/execute.js` at the reconciled Arg main SHA.

1. Requires `ARGOS_ACTION_API_KEY`.
2. Requires `ARGOS_EXTERNAL_ACTION_URL`.
3. Requires `ARGOS_EXECUTION_TCS_PATH`; missing path returns HTTP 503.
4. Creates TCS with `createTraceChronicle({ storagePath })`.
5. Creates Knowledge Vault with `${storagePath}.knowledge-vault.jsonl`.
6. Creates ArgLearn engine and the ArgLearn memory cycle, with TCS as evidence source and Knowledge Vault as durable observation/proposal store.
7. Creates ArgCore runtime adapter and webhook provider; orchestrator uses the file-backed TCS object.

**Critical:** This route does not read `ARGOS_TCS_DATABASE_URL` and does not import the PostgreSQL TCS adapter. It cannot be switched to PostgreSQL by environment configuration alone.

### 3. Companion conversation route — `api/conversation.js`

Source path: `api/conversation.js` at the reconciled Arg main SHA; PR #140 head was also inspected.

1. Authenticates the request principal.
2. Requires approval signing secret and HTTPS allowed-origin configuration.
3. Calls `makeTcs()`.
4. If `ARGOS_TCS_DATABASE_URL` exists, constructs one PostgreSQL adapter singleton and wraps its `append`, `getByCorrelation`, `getAll`, and `claimExecutionIntent` operations.
5. If no database URL exists, requires `ARGOS_EXECUTION_TCS_PATH` and uses JSONL. The file-path check warns if the path is relative or appears ephemeral; it does not convert that path to durable storage.
6. `propose` appends `CONVERSATION_INTENT_PROPOSED`.
7. `approve` reads the proposal for the same principal and exact intent hash, then appends `CONVERSATION_INTENT_APPROVED`.
8. `execute` verifies the signed approval and calls `claimExecutionIntent` before browser execution. A duplicate claim returns HTTP 409 and automatic replay is prohibited.
9. The handler returns TCS evidence from the execution.

**Boundary:** The inspected conversation handler does not create a Knowledge Vault or invoke the ArgLearn memory cycle. Conversation TCS events are not automatically carried into the file-backed memory cycle in `api/execute.js`.

### 4. TCS adapters

| Adapter | Source | Durability model | Integrity behavior |
|---|---|---|---|
| JSONL | `src/tcs/trace-chronicle.js` | Local path supplied by `ARGOS_EXECUTION_TCS_PATH` | Sequence validation, file lock, synchronous append/flush, fresh-process reopen test; no event hash chain in this adapter |
| PostgreSQL | `src/tcs/postgres-adapter.js` | PostgreSQL connection via `ARGOS_TCS_DATABASE_URL` | Transactional append, idempotency, claim state transitions, ordered sequence, event hash chain, fail-closed unavailable DB and ambiguous commit handling |

The PostgreSQL migration creates `public.tcs_execution_claims`, `public.tcs_journal_events`, and `public.tcs_journal_head`, with transition, append-only, and hash-head guards. It enables RLS and revokes access from `public`, `anon`, and `authenticated`. The migration explicitly leaves runtime-role grants for separate review; those grants must be verified before production activation.

### 5. Knowledge Vault and ArgLearn

The general execute route stores the Knowledge Vault in a second JSONL journal adjacent to the TCS file. The memory cycle rehydrates saved observations on process start, derives observations from TCS correlations, and stores provisional lessons in the vault. Its source comments explicitly prohibit automatic promotion.

The conversation route currently persists TCS evidence only. A unified lifecycle needs an explicit, governed handoff from conversation TCS records to Knowledge Vault/ArgLearn; do not assume that this is already connected.

## Environment and infrastructure evidence

### Vercel project `argos-affiliate-ledger`

- Project ID: `prj_pYDpkvhNqqsdfCplEbF96ZP670zo`.
- Project metadata reported `live=false`.
- Project domains include `argos-affiliate-ledger.vercel.app`.
- Latest deployment metadata was a READY Preview deployment on `verify/companion-candidate-2026-11-10`, commit `cea6d57ffb7186cf4f840a5655dc1739fa10c155`, with `target=null`.
- The latest candidate Preview branch has no `ARGOS_TCS_DATABASE_URL` and no `ARGOS_EXECUTION_TCS_PATH` entry. The candidate commit's `api/conversation.js` uses the same PostgreSQL-or-file selection rule and its GET readiness check requires a TCS backend; the storage readiness condition is therefore false for this branch's current environment metadata.
- A separate READY production-target deployment exists on `main` at Arg SHA `9158fdec5f7cad33727b443dcb110ed958b8ab6f`.

Production environment variable metadata included only:
`ARGOS_AFFILIATE_DB_TIMEOUT_MS`, `ARGOS_AFFILIATE_OFFERS_JSON`, `GEMINI_API_KEY`, `NODE_AUTH_TOKEN`, `SUPABASE_SERVICE_ROLE_KEY`, `SUPABASE_URL`.

No production entries were returned for:
- `ARGOS_TCS_DATABASE_URL`
- `ARGOS_EXECUTION_TCS_PATH`
- `ARGOS_ACTION_API_KEY`
- `ARGOS_EXTERNAL_ACTION_URL`
- `ARGOS_CONVERSATION_API_KEY`
- `ARGOS_CONVERSATION_PRINCIPAL_ID`
- `ARGOS_CONVERSATION_APPROVAL_SECRET`
- `ARGOS_CONVERSATION_ALLOWED_ORIGINS`

`hiddenProductionEnvCount=0`. Environment values were not decrypted.

Preview-only TCS URL metadata:
- `companion/arg-bootstrap` — comment: least-privilege runtime role on isolated Neon Companion Preview branch.
- `work/provider-independent-companion-001` — comment: least-privilege role scoped to isolated Neon Preview database/public schema.
- `integration/communication-bus-001` — comment: isolated Neon `argos-companion-preview` TCS connection for Opportunity Branch Preview only.

These branch-scoped entries do not apply to the latest `verify/companion-candidate-2026-11-10` Preview deployment. No `ARGOS_EXECUTION_TCS_PATH` entry was found in the project's environment metadata.

### Neon project

- Project: `argos-companion-preview` (`sparkling-credit-97649722`), PostgreSQL 17, `aws-us-east-1`.
- Branch: `pr140-integration-2026-11-09` (`br-broad-resonance-b8d567lu`), non-default, non-primary, state `ready`.
- Database: `argos_companion`, owner `argos_companion_owner`.
- TCS tables present: `tcs_execution_claims`, `tcs_journal_events`, `tcs_journal_head`.
- This branch is a disposable/integration Preview resource, not a production database.
- Branch metadata reports `written_data_bytes: 0`; this is a provider usage metric, not a SQL row count. No claim about the current event row count is made in this report.

## Test and evidence status

Inherited ArgAtlas continuity evidence records successful PostgreSQL 16 integration and Companion end-to-end CI for exact PR #140 head `1c77db8...`. The recorded test scope included migrations, claims, transitions, concurrency, fresh-process recovery, hash-chain verification, unavailable-database fail-closed behavior, explicit approval, replay rejection, and nine ordered TCS events independently read in a fresh process. This is inherited evidence; it was not rerun in this read-only trace.

Source test `tests/tcs-postgres-integration.mjs` exits with `SKIP` if `ARGOS_TCS_TEST_DATABASE_URL` is absent. It must be run against a disposable test database, not production. `tests/tcs-durable-reopen.mjs` covers JSONL fresh-process reopen and malformed-history fail-closed behavior.

No new tests were run in this investigation.

## Findings register

| ID | Finding | Status | Consequence |
|---|---|---|---|
| TCS-01 | `api/execute.js` always selects JSONL and ignores `ARGOS_TCS_DATABASE_URL` | VERIFIED SOURCE | DB configuration alone will not unify execution evidence |
| TCS-02 | `api/conversation.js` selects PostgreSQL only when `ARGOS_TCS_DATABASE_URL` exists | VERIFIED SOURCE | Companion route has a separate backend-selection contract |
| TCS-03 | Production Vercel env has neither TCS URL nor file path | VERIFIED CONFIG METADATA | No production TCS backend is configured |
| TCS-04 | Production route-specific action/conversation credentials are absent | VERIFIED CONFIG METADATA | Requests fail before governed execution/TCS write |
| TCS-05 | Preview branch-scoped TCS URLs exist for selected branches | VERIFIED CONFIG METADATA | Preview configuration is not production configuration |
| TCS-06 | Neon integration branch contains TCS schema | VERIFIED DATABASE METADATA | Schema deployed in Preview/integration only |
| TCS-07 | Exact secret URL-to-Neon-branch mapping not checked | UNVERIFIED | Do not claim a live DB connection target from comments alone |
| TCS-08 | PostgreSQL runtime-role grants not verified | UNVERIFIED | Migration alone does not prove deployed role can read/write |
| TCS-09 | Conversation TCS is not wired to Knowledge Vault/ArgLearn in handler | VERIFIED SOURCE | Companion evidence does not automatically enter the memory cycle |
| TCS-10 | Production TCS append/reopen/fresh-process proof absent | NOT PROVEN | Production persistence cannot be certified |

## Recommended smallest next steps

1. Keep all architecture, deployment, environment, and database state frozen.
2. Treat TCS-01 as the primary source-level defect: define one canonical TCS factory/interface used by both execution routes. Do not merely add `ARGOS_TCS_DATABASE_URL` to production; `api/execute.js` will still ignore it.
3. Before any implementation, inspect the exact production runtime target and confirm which deployment is actually serving the production alias. The Vercel project reports `live=false`, while a READY production-target deployment exists; reconcile this metadata without changing configuration.
4. Decide the canonical production TCS backend (likely PostgreSQL, subject to role/grant and operational review). Verify the connection target without exposing/decrypting credentials.
5. Verify the deployed PostgreSQL migration and runtime-role grants on a disposable/integration branch first. Do not write test events to production.
6. Establish a separate durable Knowledge Vault persistence strategy. Do not assume a TCS PostgreSQL connection automatically persists the JSONL Knowledge Vault.
7. Only after the above design is accepted, implement on a protected working branch and run focused tests. No PR merge, Vercel Preview/Production deployment, production DB migration, secret change, or infrastructure mutation is authorized by this report.

## Failure budget / limitations

- This report is a read-only source and metadata trace, not a live execution proof.
- Environment variable values and secrets were not decrypted or printed.
- No production endpoint was invoked.
- No SQL write, migration, branch creation, test event append, code edit, CI run, merge, or deployment occurred.
- The exact Vercel secret username-to-Neon-role mapping, production role grants, production-serving alias, and production restart durability remain UNVERIFIED. The Preview integration branch row counts and stored hash-link/head consistency were verified read-only.
- The latest candidate Preview's storage-readiness condition is false from the current branch-scoped environment metadata; no live HTTP readiness request was sent.

**Disposition:** IN PROGRESS — source routing and environment configuration reconciled; production TCS persistence NOT PROVEN.  
**SHIP / FREEZE / EXPAND:** FREEZE architecture and infrastructure. EXPAND only into a canonical shared TCS adapter design and read-only role/target verification.