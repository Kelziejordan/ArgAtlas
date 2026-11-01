# ADR-0001 — ARG Ecosystem Naming and Hierarchy

- **Status:** ACCEPTED — SHIPPED — FROZEN
- **Decision date:** 2026-10-31
- **Approved by:** Kelsea (explicit approval recorded in the project conversation)
- **Repository:** Kelziejordan/ArgAtlas
- **Decision scope:** Active system naming and architectural hierarchy only

## Context

The ecosystem's active naming had become inconsistent across current discussion and historical engineering records. A stable naming decision is required so that implementation, evidence, documentation, and future governance work refer to the same system boundaries.

## Decision

The accepted active names are:

1. **ARG** — the overarching governed system and authority framework.
2. **ArgCore** — the constitutional authority and foundational governance contracts.
3. **ArgOS** — the governed operational runtime and orchestration layer.
4. **ArgLearn** — the learning, analysis, discovery, and improvement-proposal system. It does not independently grant authority.
5. **ArgAtlas** — the engineering continuity, provenance, decision, checkpoint, and architectural-memory repository. It is not runtime execution authority.

The hierarchy and these responsibility boundaries are frozen by this decision. This record documents the accepted naming and existing role boundaries; it does not create new constitutional authority, alter runtime contracts, or certify that ARG governance is enforced through ArgOS.

## Retired naming

**Argus** is retired as the active name for this system and its operating role. It must not be used as the current system name in new documentation, interfaces, or engineering instructions.

Historical files, commits, package names, and records containing the former name must remain intact as historical evidence. They are not to be silently renamed or rewritten by this decision. Where necessary, new records should identify the former term as historical.

## Explicit non-decisions

This ADR does not:

- change ArgCore contracts or the frozen ArgCore 008 dependency;
- authorize architecture expansion or new repositories;
- prove or imply mandatory ARG-to-ArgOS enforcement of engineering actions;
- authorize code changes, deployments, releases, merges, or workflow executions;
- change the existing canonical lifecycle or evidence authorities.

## Approval and disposition

Kelsea explicitly approved the naming decision on 2026-10-31.

Disposition:

- **SHIP:** accepted naming decision.
- **FREEZE:** names and hierarchy listed above.
- **EXPAND:** not authorized.
- **Next engineering gate:** prove that ARG governance is actually enforced through ArgOS before treating engineering operations as formally governed.

## Required engineering sequence

FREEZE → LOAD STATE → RECONCILE → DEFINE LANDING ZONE → IMPLEMENT → HARDEN → VERIFY → STOP AT FIRST DISCONTINUITY → REPAIR → PROVE.

This sequence is an operating requirement. Its existence as written procedure is not evidence that an executable enforcement path already exists.
