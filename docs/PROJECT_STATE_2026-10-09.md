# Arg Ecosystem Project State — 2026-10-09

Status: CURRENT CROSS-REPOSITORY CHECKPOINT
Canonical continuity repository: Kelziejordan/ArgAtlas
Runtime repository: Kelziejordan/Arg
Constitutional dependency: @kelziejordan/argcore@1.0.0-argcore-008

## 1. Authority map

- **ArgCore:** constitutional/foundational contracts, governance, identity/state primitives, and recovery authority within published contracts.
- **Arg:** current ArgOS runtime, orchestration, TCS integration, VTM runtime behavior, and DigitalHands integration.
- **ArgAtlas:** canonical lifecycle, engineering continuity, provenance, checkpoints, and architectural memory.
- **ArgLearn:** evidence analysis, governed experiments, findings, and improvement proposals.
- Historical ArgOS/Omega/Evolved repositories remain evidence/reference unless explicitly promoted under the repository promotion rule.

## 2. Frozen authority and recovery status

ArgOS consumes `@kelziejordan/argcore@1.0.0-argcore-008`. ArgOS PR #84 consumed the released package; post-merge CI passed the bounded R8 recovery/resume proof. R8/008 authority gate: CLOSED at the tested boundary. ArgCore-008 foundation: FROZEN.

ArgOS must not silently upgrade, copy, or recreate ArgCore internals. ArgOS must not create a competing recovery authority. Recovery scenarios outside the tested contract remain NOT_PROVEN until an authoritative contract and executable evidence establish them.

Historical ArgCore-007 and earlier statements remain valid as historical records, but they must not be described as the current consumed dependency.

## 3. DigitalHands evidence

The ArgOS mainline DigitalHands recovery path was exercised with ArgCore 008. The CI assertion passed for ArgCore 008 authorization -> ArgOS -> DigitalHands -> real multi-action Playwright/Chromium browser session -> observation -> TCS. Exercised actions included GOTO, FILL, SELECT, CLICK, READ, EXTRACT, WAIT, and SCREENSHOT; the actuator reported `synthetic: false`.

Classification: real browser actuation is proven at this exercised integration boundary. This does not by itself prove all production use cases, complete canonical lifecycle continuity, or autonomous operation without bounded permissions.

## 4. Canonical lifecycle

ACL-001 is the canonical lifecycle specification:

Intent -> Translation (ATL) -> Planning -> Execution -> Validation -> Observation -> Memory -> Next Intent

Recovery is a controlled exception path that rejoins through Observation. Learning is a governed feedback loop. Semantic ATL is the translation layer between Intent and Planning. The older `argos-atl` C-ABI/interface usage remains a separate forensic question.

## 5. Proven boundaries (bounded)

- Phase 0 governance certification.
- Domain B Universal Task Lifecycle and consequential ArgCore correlation at tested scopes.
- ArgCore execution/state evidence bridge into TCS.
- Durable TCS append/reopen and fail-closed malformed-history handling.
- B1 denial boundary.
- Governed external HTTP POST through the tested ArgCore/ArgOS lifecycle.
- VTM reconstruction/comparison at implemented test boundaries.
- Multi-provider orchestration infrastructure at its existing tested boundaries.
- ArgCore-008 R8 recovery/resume at the bounded ArgOS-tested boundary.
- DigitalHands real multi-action Playwright browser integration through observation and TCS, as described above.

These proofs must not be aggregated into a claim that the entire ecosystem is complete.

## 6. Remaining proof obligations

- Complete ACL-001 lifecycle from raw Intent through semantic ATL, planning, governed execution, observation, validation, and memory.
- Semantic ATL as a complete executable lifecycle phase.
- Complete canonical recovery across the entire lifecycle, beyond the tested R8/008 boundary.
- Production readiness of DigitalHands beyond exercised browser flows.
- Complete VTM temporal/governance integration.
- Complete ArgLearn adaptive loop and Knowledge Vault adaptive retention/degradation loop.
- Full Multi-LLM-001 integration into main unless current branch, CI, and explicit merge evidence establish it.
- Full Arg ecosystem operation as one continuously proven system.
- First paid external revenue, customer validation, and commercial readiness remain separate economic proof obligations; they are not established by engineering tests.

## 7. Current next gates

**Economic track:** `ARGOS-AFF-001.2 — Affiliate Offer Validation and Economic Unit Definition`. Validate the candidate offer pool, attribution route, and economic unit before implementation expansion. The working model is mixed ClickBank + direct affiliates. This is a decision/validation gate, not permission to begin speculative implementation.

**Separate engineering track:** publishing-workflow proof associated with PR #91. Verify the proposed contract against actual runtime behavior and existing tests, including prerequisite verification and human-gated actions. Repair only a demonstrated gap. Do not assume PR status or CI state without a fresh GitHub check.

No architecture expansion is authorized merely because these tracks exist. The economic track must not reopen the frozen R8/008 foundation.

## 8. Target operational proof

When authorized by the applicable gate, the target path is:

Intent -> ATL -> Planning -> ArgCore governance -> Execution/DigitalHands -> Observation/VTM boundary -> Validation -> TCS/Memory -> verified result

A deliberate validation failure must exercise the authoritative recovery boundary and rejoin through Observation and Validation. If authority is unavailable for the scenario, record NOT_PROVEN rather than inventing an alternative authority.

## 9. Operating and evidence rules

The operating sequence remains:

FREEZE -> RECONCILE -> VERIFY -> PROVE -> SHIP -> RECONCILE -> VERIFY -> DISPOSITION -> MERGE -> POST-MERGE VERIFY

Executable evidence outranks narrative claims. CONNECTED, IMPLEMENTED, and EXERCISED are not synonyms for PROVEN. A passing test proves only its exercised boundary. Historical checkpoints remain unchanged and are interpreted as historical where they conflict with current executable evidence.

Security invariant: no provider secrets in source control. External effects remain governed, observable, bounded, and evidence-producing.

## 10. Current disposition

- ArgCore 008 / R8: FROZEN; bounded proof passed.
- Arg runtime: current implementation authority; no parallel architecture.
- DigitalHands: exercised real browser integration proven at the recorded boundary.
- AFF-001.2: next economic validation gate; implementation expansion on hold pending evidence.
- Publishing workflow / PR #91: separate contract-to-runtime verification gate; current PR and CI state must be refreshed before disposition.
- Complete canonical lifecycle: NOT_PROVEN.
- Full Arg ecosystem completion: NOT_PROVEN.
