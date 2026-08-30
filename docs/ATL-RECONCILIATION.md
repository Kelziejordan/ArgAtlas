# ATL Reconciliation Record

Status: OPEN — forensic review required
Date: 2026-08-30

## Finding

Two historical uses of the term ATL have been recovered.

### Interpretation A — semantic Translation Layer

ACL-001 places ATL between Intent and Planning. It converts external language and intent into canonical internal meaning and is responsible for semantic consistency, terminology normalization, ontology mapping, reversible meaning preservation, and deterministic interpretation.

### Interpretation B — foreign-language interface crate

A historical Rust workspace note defines `argos-atl` as an Autonomous Translation Layer / C-ABI crate. Its purpose is to expose C-ABI functions for Python, Node.js, C, and C++ clients to attach to the ArgOS shared-memory manifold and submit payloads.

## Why this matters

These responsibilities are materially different. Treating the C-ABI adapter and semantic translation protocol as the same architectural object could create boundary confusion. Treating them as separate without evidence could create unnecessary architecture.

## Current disposition

PRESERVE BOTH AS EVIDENCE.

Do not rename, delete, merge, or promote either interpretation until the implementation and historical artifacts have been reviewed.

## Questions for forensic review

1. Are the two ATL definitions historical stages of one design?
2. Are they two distinct layers that were incorrectly given the same name?
3. Does the current ArgOS implementation contain either responsibility, both responsibilities, or neither in complete form?
4. If the C-ABI interface remains necessary, what should its canonical architectural name be?
5. What executable contract should define semantic ATL behavior?
6. What evidence demonstrates semantic consistency rather than merely claiming it?
7. Does ATL belong inside ArgOS, beside ArgOS, or at an ecosystem boundary?

## Decision rule

Resolution must be evidence-driven. The smallest correction that restores a single coherent responsibility boundary is preferred. No new subsystem should be created until existing implementation is shown insufficient.
