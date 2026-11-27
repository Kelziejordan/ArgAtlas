# Arg Ecosystem — Master Project State

Status: CURRENT
Last Updated: 2026-11-26

## Identity

ArgAtlas is the engineering continuity and provenance system for the Arg ecosystem. It records authoritative current state, verified evidence, decisions, uncertainty, and the next justified engineering gate.

## Current project position

The Arg ecosystem is being advanced as a governed, economical, observable, recoverable, and provable AI execution system rather than as a collection of disconnected AI applications.

ArgCore remains the frozen constitutional/runtime foundation.
ArgOS is the governed execution layer.
ArgAtlas is the continuity/provenance layer.
ArgLearn is the intended adaptive intelligence/economic learning layer.
Knowledge Vault is the intended canonical knowledge substrate.
The Multi-LLM Orchestrator is now infrastructure within the ecosystem, not the destination.
VTM/CSRE, 7S/ARRE, Experience, DigitalHands, and application branches remain governed expansion boundaries and must be advanced only when evidence justifies them.

ARG Functional v1 persistence integration is now CI-verified in draft PR #143. This is a bounded implementation slice within G2; it does not close G2 or the full canonical lifecycle.

## Current benchmark state

### ArgOS Governance Benchmark v1

Status: FROZEN SPECIFICATION / HARNESS IMPLEMENTATION NEXT

The Governance Benchmark Specification v1 is now canonicalized in the active ArgOS repository at:
`docs/benchmark/ARGOS_GOVERNANCE_BENCHMARK_SPEC_V1.md`

Source artifact SHA-256:
`d657fc24c8a4d653c3bac17c3c7628036b631f1e6f8c338b2e9e86ff8a667d34`

The benchmark freezes the research question, hypotheses H0-H6, systems S1-S4, governed integrations G1-G3, workload suite WL-01 through WL-07, outcome taxonomy, metric definitions and denominators, evidence schema, failure budget, reproducibility requirements, and acceptance criteria AC-01 through AC-12.

The benchmark is explicitly independent of ArgOS. The harness must not depend on ArgOS internals to determine success, and it must remain external to both governed and ungoverned execution paths.

The required scientific sequence is:
`IMPLEMENT → VALIDATE HARNESS → BASELINE → GOVERN → MEASURE → REPRODUCE → ANALYZE → PUBLISH`

The immediate engineering gate is therefore implementation and validation of the independent benchmark harness and standardized adapter contract. The frozen ArgOS governance boundary is not to be modified to facilitate benchmark execution.

The benchmark's primary paired comparisons are:
- Temporal baseline vs ArgOS + Temporal
- Akka baseline vs ArgOS + Akka
- Conventional microservice baseline vs ArgOS + conventional orchestration

ArgOS native execution is an additional reference point.

The benchmark is scientifically successful even if H1 is rejected, provided the result is measured, reproducible, and reported without post-hoc modification.

## Current execution state

### Multi-LLM Orchestrator v2.3

Status: SHIPPED / FROZEN

The orchestrator transitioned through the following explicit states:

1. ADVANCED — implementation substantially established.
2. FINISH + SHIP — remaining product integration, economic controls, executable facade, deterministic verification, and CI were completed.
3. SHIPPED / FROZEN — v2.3 was merged to the canonical Arg main branch and its current scope is now frozen.

Verified ship commit:
`b350ae939aaa68c117a2da129d6689050ac7f20c`

Commit message:
`Ship multi-LLM orchestration v2.3`

The shipped boundary includes provider abstraction, multi-LLM execution, concurrency/timeout/cancellation/retry handling, partial-failure isolation, deterministic request IDs, provider adapters, economic authorization/preflight, executable orchestration, deterministic test coverage, and CI verification.

Policy: do not reopen orchestrator scope merely because adjacent architecture could be improved. Future changes require a separately justified boundary and explicit decision.

## Preserved secondary forensic state

### H-094 / Knowledge Vault investigation

Status: PRESERVED SECONDARY FORENSIC TRACK / NOT CURRENT BENCHMARK GATE

H-094 remains unverified. The previously discussed savings claim must not be treated as established fact until the original experiment, denominator, methodology, and evidence are recovered and independently reproduced.

Primary investigation targets include the historical Knowledge Vault / Knowledge Object / Context Pack / data-saving material and any evidence connecting those mechanisms to the reported savings.

Historical evidence may include archived conversations, PDFs, repository artifacts, Gemma/TXT material, data-saving experiments, and other preserved project artifacts. Historical material is evidence, not automatically current architecture.

The investigation must determine what H-094 actually measured, what mechanism produced the result, whether the result is reproducible, and whether the mechanism belongs in the current Knowledge Vault/runtime architecture.

H-094 is not the current engineering gate unless evidence shows that it blocks the benchmark target, executable measured path, correctness/fixtures/assertions/interpretation, required constitutional dependency identity/version, or security/integrity conditions.

## Architectural preservation rules

1. Frozen constitutional/core boundaries are not changed during forensic work or benchmark implementation.
2. Historical artifacts are preserved as evidence and are not silently rewritten to match current architecture.
3. Unknown or conflicting state is recorded as unknown/conflicting until evidence resolves it.
4. No new subsystem is added solely to satisfy an architecture diagram.
5. Production capabilities must be real and verifiable; no simulated or placeholder behavior is accepted as implementation evidence.
6. The orchestrator remains infrastructure unless a new, independently justified requirement reopens its scope.
7. Knowledge Vault must have a clear canonical authority model before replicas, caches, or localized views are promoted.
8. Claims such as the H-094 savings percentage require reproducible evidence before becoming architectural invariants.
9. Benchmark workload semantics, metric denominators, evidence requirements, failure budget, and acceptance criteria are immutable for v1 once execution begins.
10. Benchmark infrastructure must remain independently versioned and must not use ArgOS-generated scores as authoritative benchmark results.

## Active technical execution — G2 / ARG Functional v1

The G1-G10 completion register and the 2026-11-24 operating/PR reconciliation addendum remain the controlling records for G2. ARG Functional v1 persistence integration is a bounded G2 deliverable that is now CI-verified.

Current evidence checkpoint:
`docs/PROJECT_STATE_2026-11-26.md`

Canonical Arg main baseline:
`9158fdec5f7cad33727b443dcb110ed958b8ab6f`

Working branch:
`work/arg-functional-v1-persistence-001`

Exact CI-verified head:
`41a8989ca87ad1bcce770ac90d473e9ccd4847ab`

Draft PR:
https://github.com/Kelziejordan/Arg/pull/143

All four exact-head CI workflows passed, including PostgreSQL TCS/Vault integration, fresh-process recovery, exactly-once observation identity, Companion UI E2E, replay rejection, and file-backed regressions. ArgCore 008 and both accepted architectures remain frozen. No production migration, deployment, or protected merge occurred.

PR #143 remains draft and unmerged pending review and explicit operator authorization. CI verification is not production proof. G2 remains active until its full register is satisfied.

## Current next gate

1. Review PR #143 and its exact-head evidence.
2. Merge only after explicit operator authorization.
3. Complete post-merge verification. Production migration/deployment remains a separate, explicitly authorized gate.
4. Continue remaining G2 items from the existing G1-G10 register; do not declare G2 complete from this bounded slice.
5. After ARG Functional v1 is shipped, the next user-selected milestone is LinkedIn profile and professional positioning.
6. Preserve the independent Governance Benchmark as a secondary research track; it does not displace the current completion objective.

## Evidence hierarchy

Current frozen canonical implementation > verified repository state > documented current decision > historical architectural decision > historical conversation/proposal.

## Housekeeping rule

After every meaningful engineering transition, update this file's `Last Updated` field and the affected state sections. A stale master state is itself a continuity defect.
