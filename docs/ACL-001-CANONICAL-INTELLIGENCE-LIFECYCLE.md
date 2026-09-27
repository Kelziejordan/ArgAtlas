# ACL-001 — ArgOS Canonical Intelligence Lifecycle

Version: 1.0
Status: CANONICAL — FROZEN ARCHITECTURAL SPECIFICATION
Classification: Constitutional Architecture Artifact
Canonical repository: Kelziejordan/ArgAtlas
Recovered: 2026-08-30
Frozen/reconciled: 2026-09-27

## Purpose

The Canonical Intelligence Lifecycle defines the single authoritative path by which information, decisions, and state propagate throughout the ArgOS ecosystem.

Every subsystem, organism, application, branch, service, and future implementation SHALL conform to this lifecycle unless explicitly exempted by constitutional governance.

This document describes how intelligence flows, not how any individual subsystem is implemented.

## Constitutional Principles

The lifecycle SHALL satisfy these invariants:

- Every action begins with an Intent.
- Every Intent is translated into canonical meaning.
- Every execution is planned before it occurs.
- Every execution is validated.
- Every observable result is recorded.
- Every persistent memory is intentional.
- Recovery is deterministic within the authoritative recovery contract.
- Learning improves future decisions without altering constitutional law.
- No subsystem may bypass these guarantees.

## Canonical lifecycle

Intent
  |
  v
Translation (ATL)
  |
  v
Planning
  |
  v
Execution
  |
  v
Validation ---- failure ----> Recovery
  |                              |
 success                         v
  |                          Observation
  v                              |
Observation <--------------------+
  |
  v
Memory
  |
  v
Next Intent

Learning operates as a governed feedback loop that produces improvement proposals.

## Phase 1 — Intent

Intent is the origin of intelligent behavior.

Intent may originate from:
- human interaction
- internal subsystem
- autonomous organism
- external API
- scheduler
- continuity restoration
- recovery event

Intent SHALL be parsed, authenticated, contextualized, bounded, and validated.

Output: Intent State.

## Phase 2 — Translation (ATL)

ATL is the canonical semantic Translation Layer.

Responsibilities:
- terminology normalization
- ontology mapping
- audience abstraction
- reversible meaning preservation
- deterministic interpretation

Outputs:
- Canonical Intent
- Normalized Vocabulary
- Translation Metadata

ATL establishes meaning. It does not replace planning, governance, execution, validation, or observation.

## Phase 3 — Planning

Planning transforms canonical intent into an executable strategy.

Planning SHALL determine, as applicable:
- participating subsystems
- execution order
- dependency graph
- deterministic windows
- constitutional constraints
- required permissions
- continuity anchors
- resource budgets

Outputs:
- Execution Plan
- Ordered Task Graph
- Execution Context

## Phase 4 — Execution

Execution performs the approved plan.

Execution SHALL be:
- observable
- constitutional
- continuity-safe
- reversible where possible
- bounded by the applicable governance and resource contracts

Execution represents action, not decision.

DigitalHands is the intended external digital-actuation mechanism within this phase.

## Phase 5 — Validation

Validation determines whether execution fulfilled the canonical intent.

Validation SHALL verify, as applicable:
- structural integrity
- behavioral correctness
- constitutional compliance
- safety constraints
- continuity consistency
- diagnostic health

The canonical outcomes are Success or Failure. Failure enters Recovery.

## Phase 6 — Recovery

Recovery exists only when normal execution cannot continue safely.

Recovery SHALL restore valid state, reconcile continuity, preserve audit history, and rejoin the lifecycle through Observation.

Critical boundary:
ArgOS must not invent a canonical recovery/resume authority when ArgCore does not expose one. Until an authoritative recovery contract exists, recovery/resume remains an explicit NOT_PROVEN boundary.

## Phase 7 — Observation

Observation captures what actually occurred.

It records:
- outputs
- environmental changes
- user reactions
- subsystem feedback
- performance metrics
- continuity markers

Observation records reality; it does not silently redefine canonical meaning.

VTM may provide historical reconstruction and analytical observation support, but VTM must not become a competing canonical state authority.

## Phase 8 — Memory

Memory determines what deserves persistence.

Memory may include:
- knowledge
- continuity anchors
- event history
- architectural decisions
- organizational context
- operational context

Persistence must remain structured, searchable, versioned, auditable, and governed.

## Phase 9 — Learning

Learning analyzes historical behavior to improve future execution.

Learning may optimize translation mappings, planning heuristics, execution strategies, scheduling, diagnostics, resource allocation, thresholds, and recommendations.

Learning SHALL NOT directly modify:
- Constitutional Law
- Frozen Runtime
- Governance Rules

Learning produces Improvement Proposals that require governed evaluation before adoption.

## State Continuum

Intent -> Intent State
Translation -> Canonical State
Planning -> Execution State
Execution -> Runtime State
Validation -> Verified State
Observation -> Observed State
Memory -> Persistent State
Recovery -> Restored State
Learning -> Improved State

State is transformed through governed transitions; it is not treated as an unbounded stream of unrelated outputs.

## Supporting constitutional services

Recovery operates conditionally after failed validation.
Learning operates asynchronously as a governed feedback loop.

Neither may bypass constitutional governance.

## Architectural position

- ArgCore -> constitutional authority and enforcement boundary.
- ArgOS -> governed runtime and lifecycle execution.
- ATL -> canonical semantic translation.
- Planning -> executable strategy.
- Execution/DigitalHands -> governed action.
- Validation -> outcome verification.
- Observation/VTM -> capture/reconstruct/analyze state without becoming a competing authority.
- TCS -> canonical chronological runtime evidence.
- Memory/ArgAtlas -> intentional persistence and engineering continuity.
- ArgLearn -> governed learning and improvement proposals.
- Governor -> cross-lifecycle oversight.

## ATL naming reconciliation

An older Rust workspace used `argos-atl` for a C-ABI / foreign-language interface into a shared-memory manifold.

That historical implementation is not automatically identical to the semantic ATL defined here.

The reconciliation record remains:
`docs/ATL-RECONCILIATION.md`

Until executable evidence establishes otherwise:
- semantic ATL remains the canonical ACL-001 meaning;
- the historical C-ABI usage remains preserved evidence;
- neither interpretation is silently deleted or promoted into the other.

## Conformance rule

A subsystem conforms to ACL-001 only when its implementation and executable evidence demonstrate the relevant lifecycle phase and its required invariants.

Architecture diagrams, documentation, connectivity, and simulated behavior are not sufficient evidence of conformance.
