# Design Review Finding — Workload Normalization Specification

Date: 2026-09-28
Status: REVISE
Boundary under review: Workload Envelope → Workload Normalization → Canonical Task/Intent → ACL-001 ATL

## Finding

The first generated Workload Normalization specification was rejected at design review before implementation.

The review identified that the proposal introduced unsupported architectural assumptions despite being internally coherent.

## Evidence and Authority Rules

- Architecture is defined deliberately by the project maintainers.
- Repositories establish what currently exists.
- Tests and CI establish what has been verified within their tested boundaries.
- Historical artifacts establish provenance and prior design lineage.
- LLM-generated analysis and specifications are proposals, not architectural authority.

Generated architecture must not become authoritative merely because it is internally consistent.

## Specific Findings

1. The proposed specification introduced a new canonical Task/Intent schema instead of mapping into the existing frozen ArgOS Canonical Task/Intent Contract V1.
2. The proposal expanded ATL as "Agent Transparency Layer", conflicting with the established Arg Translation Layer terminology.
3. Some semantic/policy responsibilities were allowed to leak into normalization, contrary to the intended separation between structural normalization and ACL-001 semantic translation.
4. The proposed TR-001 fixture classifications were more specific than the available repository evidence supported. They must not be treated as facts without inspecting the actual fixture definitions.
5. The proposed source envelope was hypothetical and must not be presented as the existing TR-001 workload shape.
6. Benchmark/control status must be established from fixture evidence rather than inferred from workload IDs or benchmark membership.
7. Normalization must distinguish structural insufficiency from downstream semantic insufficiency; otherwise normalization begins performing ATL's job.
8. Ownership/provenance entries must distinguish documented authorship from legal ownership, repository control, contribution, and inference.

## Decision

REVISE the design artifact.

Do not modify:
- ArgOS repository code
- PR #53
- TR-001 harness
- ArgCore
- ACL-001 canonical contract

The revised design must reference the existing frozen canonical Task/Intent contract rather than create a competing canonical schema. TR-001 classifications must be evidence-derived.

## Process Lesson

The design-review gate successfully prevented an internally coherent but insufficiently evidenced LLM-generated architecture from being promoted into runtime implementation.

Required sequence:

EVIDENCE → CLASSIFICATION → DESIGN → REVIEW → ACCEPT/REVISE → IMPLEMENT

This finding is recorded as a governance/design-review event, not as a model-specific failure.

## Ownership / Provenance

This record documents the design-review decision and its rationale. It does not by itself establish legal IP ownership of any historical or generated artifact.
