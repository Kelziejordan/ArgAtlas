# ACL-001 — ArgOS Canonical Intelligence Lifecycle

Version: 1.0
Status: CANONICAL — pending forensic verification of implementation alignment
Classification: Constitutional Architecture Artifact
Recovered: 2026-08-30

## Purpose

The Canonical Intelligence Lifecycle defines the authoritative path by which information, decisions, and state propagate through the ArgOS ecosystem.

## Lifecycle

Intent
-> Translation (ATL)
-> Planning
-> Execution
-> Validation
-> Observation
-> Memory
-> Next Intent

Recovery is the controlled exception path from failed validation back into the lifecycle through observation. Learning operates as a governed feedback loop that produces improvement proposals without modifying constitutional law or the frozen runtime directly.

## Core invariants

- Every action begins with Intent.
- Intent is translated into canonical meaning before planning.
- Execution is planned before it occurs.
- Execution is validated.
- Observable results are recorded.
- Persistent memory is intentional.
- Recovery is deterministic.
- Learning may improve future decisions but may not alter constitutional law by itself.
- No subsystem may bypass these guarantees.

## ATL role

ACL-001 defines ATL as the Translation Layer. Its responsibilities include terminology normalization, ontology mapping, audience abstraction, reversible meaning preservation, deterministic interpretation, and production of canonical intent and translation metadata.

## Architectural position

ArgCore / ACR provides constitutional authority.
ArgOS provides the governed runtime and lifecycle execution.
ATL establishes canonical meaning before planning.
Planning establishes executable strategy.
Execution performs approved work.
Validation determines whether intent was fulfilled.
Observation records what occurred.
Memory establishes intentional persistence.
Recovery restores valid state after failure.
ArgLearn analyzes evidence and proposes governed improvements.
ArgAtlas preserves engineering continuity, decisions, provenance, and project state.

## Important implementation discrepancy

An older Rust workspace note also defines a crate named `argos-atl` as an Autonomous Translation Layer / C-ABI interface for Python, Node.js, and C/C++ access to the ArgOS shared-memory manifold.

That older use of the name is not assumed to be equivalent to ACL-001's semantic Translation Layer. The relationship is an explicit forensic question tracked in `docs/ATL-RECONCILIATION.md`.

## Preservation rule

Do not silently rewrite either historical interpretation. Determine the relationship from artifacts and implementation evidence before changing canonical terminology or interfaces.
