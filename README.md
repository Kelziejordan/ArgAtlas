# ArgAtlas

ArgAtlas is the engineering continuity and provenance system for the Arg ecosystem.

It preserves the authoritative project state needed to continue engineering work across sessions, contributors, implementations, and architectural generations.

## Current checkpoint — 2026-08-30

A historical ATL definition has been recovered from project notes and reconciled against the current ArgOS architectural model.

The current canonical lifecycle places ATL as the Translation Layer between Intent and Planning. An older `argos-atl` implementation used the same name for a C-ABI / foreign-language interface into the shared-memory manifold. This naming collision is now recorded as an explicit forensic item rather than silently resolved.

## Continuity rule

ArgAtlas records what is known, what is evidenced, what is uncertain, and what remains to be proven. Historical artifacts are evidence; they do not automatically override the current canonical architecture.

See:
- `docs/ACL-001-CANONICAL-INTELLIGENCE-LIFECYCLE.md`
- `docs/ATL-RECONCILIATION.md`
- `PROJECT_STATE.md`
- `SESSIONS/2026-08-30-001.md`
