# SPEC-WORKLOAD-NORMALIZATION-001 — Revised Design Specification

Status: PROPOSED — DESIGN GATE
Revision: 2
Date: 2026-09-28
Boundary: Workload Envelope → Workload Normalization → Canonical Task/Intent → ACL-001 ATL

## 1. Purpose and boundary

This specification defines only the structural normalization boundary between an existing workload envelope and the frozen ArgOS Canonical Task/Intent Contract V1.

It does not create a competing canonical task schema.

The normative target is the existing frozen contract:

```
{
  taskId,
  intent: {
    prompt,
    action,
    payload
  },
  requestedAt,
  principalId,
  sessionId,
  correlationId,
  authority,
  policy,
  budget
}
```

The frozen contract states that existing runtime task shapes may continue during migration and that the canonical envelope is the representation against which those shapes must be normalized and validated.

Normalization therefore answers:

"Can the source workload establish the required canonical structure without semantic invention?"

It does not answer:

"What does this request mean semantically?"

That remains the ACL-001 ATL boundary.

The normalization boundary MUST NOT manufacture authority, policy, budget, execution permission, governance decisions, semantic classifications, or missing identity.

## 2. Source workload envelope

The current TR-001 source evidence does not establish a universal workload-envelope schema matching the hypothetical envelope from Revision 1.

The actual TR-001 runner constructs workload objects with these observed fields:

```
{
  id,
  classId,
  className,
  instance,
  index,
  payload,
  expectedOutput,
  key,
  estimatedCostUsd,
  expectPreflightRejected
}
```

The runner contains five frozen workload classes:

- WL-01 — Multi-LLM independent execution
- WL-02 — Economic preflight + authorized execution
- WL-03 — Economic preflight rejection
- WL-04 — Repeated reusable workload
- WL-05 — Deterministic local workload

Each class contains five input strings, producing 25 workload instances.

These observations are repository evidence. They do NOT establish that every TR-001 workload is already an ACL-001 semantic Intent.

The source workload may therefore contain fields that are benchmark/execution metadata rather than canonical intent fields.

## 3. Canonical Task/Intent target

Normalization targets the existing frozen Canonical Task/Intent Contract V1 without renaming or redefining its fields.

Required canonical fields are:

```
taskId
intent.prompt
intent.action
intent.payload
requestedAt
principalId
sessionId
correlationId
authority
policy
budget
```

The frozen contract defines:

- taskId as the stable identity of one governed task;
- intent.prompt as a non-empty human/model-readable statement of requested intent;
- intent.action as a non-empty canonical requested operation;
- intent.payload as action-specific task data;
- requestedAt as the canonical governed-boundary timestamp;
- principalId, sessionId, and correlationId as distinct immutable identities;
- authority, policy, and budget as governance-context fields.

Normalization must establish these fields only from legitimate source evidence or from explicitly authorized non-semantic ingress generation rules already permitted by the canonical contract.

Normalization must not redefine their semantics.

## 4. Field-by-field mapping rules

The mapping is deliberately conservative.

| Canonical field | Permitted source evidence | Rule |
|---|---|---|
| taskId | Source workload identity such as TR-001 `id`, if the workload is actually entering the governed task lifecycle | Preserve a supplied stable identity. Do not substitute classId, key, instance, or index for taskId without an explicit contract rule. |
| intent.prompt | A source field explicitly representing the requested human/model-readable intent | Directly preserve the supplied value. TR-001 `payload` is candidate evidence only; it is not automatically declared equivalent to `intent.prompt` by this specification. |
| intent.action | A source field explicitly representing the requested operation | No inference from prose, class name, workload ID, or expected output. If required action semantics are absent, normalization cannot manufacture them. |
| intent.payload | Source data explicitly supplied as task/action input | Preserve the supplied task data. Benchmark metadata is not automatically intent payload. |
| requestedAt | Trusted canonical-ingress timestamp | Generate or normalize only according to the frozen canonical contract. TR-001's fixture object does not itself establish a canonical requestedAt field. |
| principalId | Explicit principal identity supplied by the ingress boundary | Never derive from class, provider, workload ID, or benchmark metadata. |
| sessionId | Existing governed-session identity | Must be supplied or generated only by the authorized governed ingress mechanism. It is not derivable from TR-001 fixture metadata. |
| correlationId | Existing cross-boundary correlation identity | Must be supplied or generated only at the authorized boundary. It is not derivable from workload class or index. |
| authority | Existing authority context | Never inferred from `estimatedCostUsd`, `paidExecutionAuthorized`, class membership, or any textual workload content. |
| policy | Existing policy context | Never synthesized from workload class or economic metadata. |
| budget | Existing declared/governed budget context | Never inferred as governance authority from `estimatedCostUsd`. Economic metadata may inform an external caller, but it is not by itself the canonical budget context. |

A mapping is valid only when the source field's semantics are established, not merely because its data type happens to fit.

## 5. Required versus optional fields

The frozen canonical contract requires all of the following before consequential execution:

1. taskId
2. intent.prompt
3. intent.action
4. intent.payload
5. requestedAt
6. principalId
7. sessionId
8. correlationId
9. authority
10. policy
11. budget

The contract states that intent.action is required and non-empty. Therefore this normalization specification MUST NOT weaken that requirement.

The contract also states that taskId and requestedAt may be generated at the governed ingress boundary under defined conditions. Such generation is not permission for the normalization layer to invent semantic fields.

An absent required semantic field therefore remains absent unless an authoritative source explicitly supplies it.

## 6. Deterministic derivation rules

Only derivations explicitly supported by the canonical contract are permitted.

### 6.1 taskId

The frozen contract permits the governed ingress boundary to generate taskId exactly once when the caller does not supply one.

This specification does not prescribe a new UUID, hash, or namespace algorithm.

### 6.2 requestedAt

The frozen contract permits requestedAt to be generated once at canonical admission when not supplied by a trusted ingress record and requires ISO-8601 UTC normalization.

This specification does not redefine that mechanism.

### 6.3 Structural normalization

Whitespace/type normalization is permitted only where it does not change the semantic value and where the canonical contract or implementation contract explicitly requires it.

No semantic normalization is introduced here.

### 6.4 Provenance digest

A digest may be added as provenance metadata outside the canonical Task/Intent fields, but no RFC 8785/JCS requirement is introduced in this revision because that requirement was not established by the frozen ArgOS contract or the examined TR-001 evidence.

A future provenance specification may define it separately.

## 7. Rejection and unresolved rules

Normalization is fail-closed with respect to the canonical contract.

### REJECT_STRUCTURAL_DEFICIT

Use when required canonical structure cannot be established from authoritative source data.

Examples:

- missing task identity where no authorized ingress generation is available;
- missing required principal/session/correlation identity;
- malformed task envelope;
- required governance context absent.

### REJECT_REQUIRED_INTENT_FIELD

Use when the workload is intended to enter the ACL-001 lifecycle but the source does not establish required `intent.prompt` or `intent.action` semantics.

This is not permission to infer the missing value.

### ROUTE_NON_SEMANTIC_WORKLOAD

Use only when evidence establishes that the object is benchmark/control infrastructure rather than a semantic task.

Membership in TR-001 alone is insufficient evidence for this route.

### ROUTE_UNRESOLVED

Use when the source object could represent a semantic task but available evidence cannot establish the canonical mapping deterministically.

Unresolved cases remain unresolved. They must not be converted into canonical tasks through heuristic invention.

A rejection or unresolved result must retain the original workload identity and sufficient provenance to explain why normalization did not proceed.

## 8. Provenance of supplied versus derived fields

Every normalization result must distinguish:

SUPPLIED:
A value explicitly present in the authoritative source workload or trusted ingress context.

DERIVED:
A value produced by a deterministic rule explicitly authorized by the canonical contract.

UNRESOLVED:
A canonical field for which available source evidence is insufficient.

INFERRED:
A semantic value guessed from context, prose, class name, or convention.

INFERRED values are forbidden.

Provenance must also identify the source boundary at which taskId, requestedAt, principalId, sessionId, and correlationId were supplied or generated.

Normalization provenance must not be used to imply authorization.

## 9. Identity preservation

The following identities retain their distinct meanings:

- taskId = governed task identity
- sessionId = execution/session identity
- correlationId = cross-boundary evidence identity
- principalId = governing principal identity

No workload field may be silently substituted for another identity.

In particular:

- classId is not taskId;
- key is not taskId;
- instance is not taskId;
- index is not taskId;
- provider ID is not principalId;
- `estimatedCostUsd` is not budget authority;
- `paidExecutionAuthorized` is not canonical authority context.

The canonical contract's identity invariants remain authoritative.

## 10. TR-001 fixture mapping

The actual TR-001 evidence establishes five classes and five textual inputs per class.

### WL-01 — Multi-LLM independent execution

Observed description: three configured model executions against the same coding task.

Observed inputs include requests such as writing TypeScript functions for task-envelope validation, provider-result normalization, malformed-request rejection, aggregate metrics, and stable request identifiers.

Potential semantic evidence:
- `payload` contains a textual requested task.
- The class describes an execution pattern involving multiple providers.

Not established:
- that `payload` is formally `intent.prompt`;
- a canonical `intent.action`;
- principalId/sessionId/correlationId;
- authority/policy/budget in the canonical contract.

Normalization decision:
UNRESOLVED pending an explicit workload-to-intent contract. Do not invent action or governance context.

### WL-02 — Economic preflight + authorized execution

Observed description: economic preflight authorizes a bounded execution before provider invocation.

Observed inputs are bounded textual requests.

Potential semantic evidence:
- `payload` contains a requested task.
- `estimatedCostUsd` exists as fixture metadata.
- The runner supplies `paidExecutionAuthorized: true` to the current orchestrator.

Not established:
- equivalence between these fields and canonical budget/authority;
- canonical intent.action;
- canonical identity fields.

Normalization decision:
UNRESOLVED pending explicit mapping. Economic fixture metadata must not be promoted to canonical authority or budget semantics.

### WL-03 — Economic preflight rejection

Observed description: a spend-ineligible task is rejected before provider execution.

Observed inputs are explicit spend-ineligible execution requests.

The runner sets `expectPreflightRejected` only for the fifth instance and configures the economic gate accordingly.

Not established:
- canonical authority;
- canonical budget;
- intent.action;
- identity context.

Normalization decision:
UNRESOLVED as an ACL-001 task unless the workload-to-intent contract explicitly maps it. Its expected rejection behavior is an execution/economic fixture property, not proof of canonical governance semantics.

### WL-04 — Repeated reusable workload

Observed description: a previously validated result is available as reusable state, allowing external execution to be avoided.

The runner preloads the expected result into a cache under the workload's `key`.

Potential semantic evidence:
- `payload` contains a textual requested operation.
- The workload has a reusable execution disposition.

Not established:
- canonical intent.action;
- principal/session/correlation identity;
- authority/policy/budget.

Normalization decision:
UNRESOLVED pending explicit mapping.

### WL-05 — Deterministic local workload

Observed description: a locally satisfiable deterministic task is completed without provider execution.

The runner classifies WL-05 as local and returns `expectedOutput` from the local executor.

Potential semantic evidence:
- `payload` contains a textual deterministic transformation request.

Not established:
- canonical intent.action;
- canonical identities;
- authority/policy/budget.

Normalization decision:
UNRESOLVED pending explicit mapping.

### TR-001 conclusion

The actual fixture evidence does NOT support the earlier five-way partition into benchmark-control, well-formed-intent, partial-envelope, ambiguous, and syntactic-mismatch groups.

That classification is withdrawn.

The evidence currently supports a narrower conclusion:

TR-001 contains 25 controlled workload fixtures across five execution/economic classes. The fixtures contain textual workload payloads and benchmark/execution metadata. The existing evidence does not establish a complete canonical ACL-001 Task/Intent envelope for any class.

This does not mean the fixtures cannot become canonical tasks. It means that the missing workload-to-canonical-intent mapping must be explicitly designed and evidenced before implementation.

## 11. Negative and falsification cases

The revised specification is falsified if implementation does any of the following:

1. Converts a TR-001 `payload` into `intent.prompt` merely because both are strings, without an accepted mapping rule.
2. Generates an `intent.action` from words appearing in the payload.
3. Converts `classId`, `className`, `key`, or `index` into semantic action.
4. Converts `estimatedCostUsd` into canonical budget authority.
5. Converts `paidExecutionAuthorized` into canonical authority context.
6. Creates principalId, sessionId, or correlationId from workload metadata without an authorized identity-generation rule.
7. Treats expectedOutput as proof of intended semantic action.
8. Treats a benchmark workload as non-semantic control traffic solely because it belongs to TR-001.
9. Creates a second canonical Task/Intent schema.
10. Weakens the frozen requirement that intent.prompt and intent.action are non-empty.
11. Allows an unresolved workload to reach consequential execution as though canonical normalization succeeded.
12. Changes PR #53 or TR-001 solely to make normalization tests pass.

## 12. Explicit non-claims

This specification does not claim:

- that TR-001 currently implements ACL-001;
- that every TR-001 workload is a canonical Intent;
- that `payload` equals `intent.prompt`;
- that any existing workload field equals `intent.action`;
- that economic-gate metadata constitutes canonical authority or budget;
- that normalization authorizes execution;
- that normalization performs semantic interpretation;
- that ATL is an authorization layer;
- that ArgCore proves taskId-to-authority binding;
- that the frozen Phase 1.2 contract has been fully implemented at runtime;
- that any unresolved workload should be forced through the lifecycle.

## 13. Acceptance criteria

The design is accepted only when:

1. The canonical target remains exactly the existing frozen Task/Intent contract.
2. No competing canonical schema is introduced.
3. Every proposed field mapping identifies actual source evidence.
4. Unsupported mappings remain unresolved rather than being invented.
5. TR-001 classifications correspond to actual fixture definitions.
6. No semantic responsibility is assigned to normalization that belongs to ACL-001 ATL.
7. Authority, policy, budget, and execution permission remain outside normalization.
8. Identity meanings remain distinct and immutable.
9. Negative cases demonstrate that missing semantics are rejected or routed unresolved.
10. The specification can be implemented without modifying PR #53 or TR-001 merely to accommodate the design.
11. Ownership/provenance is recorded separately from architectural semantics.

Acceptance of this design does not constitute implementation proof.

## 14. Relationship to ACL-001 and ATL

The intended relationship is:

```
Existing Workload Envelope
        |
        v
Workload Normalization
  structural mapping only
        |
        v
Frozen Canonical Task/Intent
        |
        v
ACL-001 ATL
  semantic translation
        |
        v
Planning
        |
        v
ArgCore Governance
        |
        v
Execution
```

Normalization establishes whether the canonical structure can be established without invention.

ATL operates on the canonical Intent and performs the semantic translation defined by ACL-001.

Neither normalization nor ATL grants authority, policy approval, budget authorization, or execution permission.

The frozen canonical contract explicitly states that Intent is not authority and that the canonical `action` must not silently be treated as ArgCore's lower-level admission action.

The current ArgOS ATL implementation on PR #53 remains a separate implementation/proof gate and is not changed by this specification.

## 15. Ownership and provenance record

Artifact:
`SPEC-WORKLOAD-NORMALIZATION-001`

Revision:
2 — evidence-corrected design revision.

Design authorship:
Recorded as project design work; authorship does not by itself establish legal IP ownership.

Repositories:
- ArgOS: `Kelziejordan/Arg`
- Continuity/provenance record: `Kelziejordan/ArgAtlas`

Protected artifacts:
- PR #53 remains frozen.
- TR-001 remains frozen.
- ArgCore remains untouched by this design.
- Frozen Canonical Task/Intent Contract remains normative.

Evidence basis:
- `docs/governance/ARGOS_CANONICAL_TASK_INTENT_CONTRACT_V1.md`
- `src/data-saving/tr001-runner.js`
- existing ArgOS benchmark/workload documentation
- prior design-review finding recorded in ArgAtlas

Revision rationale:
Revision 1 introduced a competing canonical schema and unsupported TR-001 fixture classifications. Revision 2 removes those assumptions and constrains the design to evidence-supported normalization into the existing canonical contract.

Review state:
REVISE → READY FOR DESIGN REVIEW

Required next gate:
DESIGN REVIEW → ACCEPT / REVISE

No implementation is authorized by this document.
