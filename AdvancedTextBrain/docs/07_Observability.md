# 07 — Observability

Observability in this workflow means being able to determine what the current run knows, what it inferred, what remains unresolved, which material was accepted, and why publication is or is not allowed. The workflow provides that visibility through Markdown records rather than runtime logs or executable telemetry.

The primary state surface is `.spec-workflow/research.md`, the source-identity surface is `.spec-workflow/source-snapshot.md`, the candidate is `.spec-workflow/draft.md`, the conditional rollback surface is `.spec-workflow/prior-spec.md`, and the published surface is `spec.md`.

## What is observable

### Current-run identity

A NEW build first verifies deletion of managed workflow/outgoing state, then creates and rereads `.spec-workflow/research.md` before any snapshot. It next creates and verifies `.spec-workflow/source-snapshot.md`, updates the initialization checks, and persists and rereads `GUARD_CALL_BASELINE`. More than one RUN_ID, baseline, or old-domain record makes `SINGLE_RUN_STATE_CHECK` fail.

An existing `spec.md` is classified as `PRIOR_PUBLISHED` while a replacement is being prepared. At initialization it is copied verbatim to `.spec-workflow/prior-spec.md`, equality is verified, and both files remain unchanged until the write guard passes. The prior publication is excluded from current-run evidence and prepublication equality checks.

### Phase Gate Ledger

The Phase Gate Ledger records six gates:

| Gate | What it observes | Accepted terminal value for continued success |
|---|---|---|
| `BASELINE_GATE` | Source and contract readiness | `ACCEPT` |
| `RESOLUTION_GATE` | Completion of genuine internal gaps | `ACCEPT` or reasoned `NOT_REQUIRED` |
| `PRODUCTION_GATE` | Acceptance of every required `KQ-*` and `UNIT-*` pair | `ACCEPT` |
| `DRAFT_GATE` | Complete candidate draft and manifest | `ACCEPT` |
| `FINAL_GATE` | Full candidate review | `READY` |
| `POSTPUBLICATION_GATE` | Written-file verification | `PASS` |

The ledger exposes illegal jumps. For example, `PRODUCTION_GATE: ACCEPT` with `BASELINE_GATE: PENDING` is not a healthy state, even if the produced text looks plausible.

### Run Invariant Block and action authorizations

The ledger is accompanied by one authoritative Run Invariant Block. Before its normal gate/count fields, it exposes run mode/reset reason, active goal/anchor, generated cleanup, managed-artifact path validity, candidate/included counts, source scope, exact snapshot, evidence membership, and single-run checks. Those values are recalculated from current files; chat narration and worker summaries are not authority.

Before resolution, production, draft creation or acceptance, final review, publication, postpublication review, or success reporting, the Orchestrator writes and rereads an `ACTION_AUTHORIZATION`. A literal `FAIL` or any missing, stale, contradictory, or non-numeric tested value prevents the action.

### Host execution boundary

Only when the session permission level is Autopilot and `chat.autopilot.advanced.enabled` is `true` may VS Code 1.124 stop after at most three automatic host loops. This is visible chat behavior, not a seventh phase gate. `PAUSED: HOST_AUTOPILOT_LIMIT` is chat-only when that optional boundary is explicitly exposed; it is never used for a Default Approvals tool confirmation. The durable evidence is the unchanged active run ID, incomplete gate, accepted records, defect signature, and revision count. A later `resume` continues the earliest incomplete gate.

### Source snapshot, scope, and inventory

Every readable non-reserved textual candidate has exactly one sorted path and one `### SOURCE` block in the snapshot and is classified by semantic role. Fixed reserved/control exclusions are recorded in research; they are not snapshot candidates and do not contribute to `SOURCE_CANDIDATE_COUNT` or evidence. A file or section can be active goal evidence, in-scope support/source material, an unrelated goal, a framework document, a prior reference, or excluded. Only admitted snapshot lines can support obligations.

The snapshot is structurally observable: candidate count equals path count and source-block count. For every block, `LINE_COUNT` equals the live editor-reported line count, contiguous `L*` row count, and highest row ID, and every row matches the live text through EOF. A root-level, basename-only, or duplicate identity copy fails `MANAGED_ARTIFACT_PATH_CHECK`.

This makes discovery auditable and prevents tutorials, outgoing evidence, an earlier specification, or an independent embedded goal from silently becoming a current requirement. Exact snapshot comparison also prevents RESUME after source text changes.

### Three-layer Artifact Contract

The contract separates:

1. the specification knowledge layer;
2. the downstream deliverable layer; and
3. the operational or experience layer.

Every field records a value, provenance, status, and a verified source location when source-backed. The statuses are `FACT`, `DERIVED`, `UNRESOLVED`, and reasoned `NOT_APPLICABLE`.

This separation makes audience mistakes visible. A downstream content generator is not automatically the respondent who will answer an interview question. A plan reader is not automatically the participant who experiences the planned activity.

### Obligation register

Each `OBL-*` records:

- a readable label;
- exact active-source wording;
- source heading and verified location;
- mandatory status;
- treatment type; and
- coverage status.

The obligation register answers “What must be satisfied?” without requiring the reader to infer requirements from generated prose.

### Field and member ledgers

The adaptive Markdown Field Instantiation Contract records only source-supported topics, fields, children, dependencies, cardinality, enumerations, scales, applicability, and source basis during baseline. Durable unsupported counts use `CARDINALITY: SOURCE_UNSPECIFIED`; the current Investigator proposal alias is `SOURCE_CARDINALITY: UNSPECIFIED`. Classified production decisions are added only after baseline and resolution acceptance.

Related research-only matrices expose completeness at two levels:

- unit by field; and
- unit by finite enumeration or ordered scale.

These matrices distinguish a declared schema from completed content. A field named `conditional_followups` is not complete unless its required trigger and action members are instantiated.

### Internal resolution records

`IRQ-*` records show genuine ambiguities, conflicts, missing decisions, or dependencies. They include the resolution route, evidence, downstream effect, resolution condition, and `WORKFLOW_ONLY` publication status.

The workflow does not expose chain-of-thought. It records decisions and evidence needed for audit, not hidden reasoning transcripts.

### Coverage plan

The coverage plan maps mandatory obligations to one or more legitimate destinations:

- a planned `KQ-*`;
- an attached source-required field;
- a framework or cross-unit section;
- navigation or traceability; or
- an explicit unresolved decision.

Every required `UNIT-*` maps to a planned `KQ-*`. This makes omissions and artificial meta-questions detectable before production begins.

### Publication manifest

The publication manifest is stored only in `.spec-workflow/research.md`. It maps every draft block and line range to `FRAMEWORK_CORE`, a source obligation, a `KQ-*`, or a `UNIT-*`.

The manifest must never be published. Its purpose is to prove that every block in the candidate belongs there and that no worker transcript, internal ledger, or untracked section leaked into `spec.md`.

## The artifact lifecycle

| Artifact | Meaning | May contain workflow-only material? | Can downstream users treat it as accepted? |
|---|---|---|---|
| `.spec-workflow/source-snapshot.md` | Exact line-addressed candidate-source identity | Yes; freshness control only | No; it is not evidence |
| `.spec-workflow/research.md` | Evidence, contracts, ledgers, decisions, coverage, manifest, and gates | Yes | No |
| `.spec-workflow/draft.md` | Exact candidate submitted to final review | No internal transcripts | Only after final review, and it is still not the published file |
| `.spec-workflow/prior-spec.md` | Verified temporary copy of the pre-run publication | Rollback-only content | No; it is never current-run evidence or the candidate |
| `spec.md` | Published Q&A knowledge base | No | Only after postpublication pass |
| `outgoing/**` | Reserved legacy generated scratch | Deleted on NEW | Never |

An absent `spec.md` at startup is normal. An empty `.spec-workflow` directory is also a clean state. A normal run creates current-run records as it advances.

## Example: reading the ledger

Suppose the ledger shows:

| Gate | State | Interpretation |
|---|---|---|
| Baseline | `ACCEPT` | Scope and contracts are usable. |
| Resolution | `NOT_REQUIRED` | No genuine ambiguity required an IRQ; a reason must be present. |
| Production | `PENDING` | No complete set of accepted entries exists yet. |
| Draft | `PENDING` | A candidate draft must not exist yet. |
| Final | `PENDING` | Final review is not available. |
| Postpublication | `PENDING` | No success response is allowed. |

If `PRIOR_SPEC_STATE: ABSENT` and `spec.md` appears in this state, observability has exposed a publication violation. If prior state is `PRESENT`, the old `spec.md` is expected to remain and must exactly equal the verified `.spec-workflow/prior-spec.md` checkpoint until the write guard passes.

## Failure modes

### A snapshot is orphaned, misplaced, or incomplete

The state is uncommitted and cannot authorize `BASELINE`. The managed-path or snapshot check fails, and the Orchestrator repairs or cleanly retries NEW before calling a worker; it never asks the user to update missing research or rerun baseline.

### A partial research record claims completion

A source inventory and outline are not equivalent to accepted baseline evidence. Look for the baseline decision and the complete contract ledgers.

### A manifest is embedded in the draft

The manifest is research-only. Publishing it leaks workflow state and fails manifest integrity.

### IDs exist without readable labels

Bare IDs force readers to chase internal records. Published source bases require readable obligation labels and verified locations.

### A prior specification is mistaken for current evidence

The prior file is rollback state only. The current run must independently rediscover sources and rebuild its ledgers.

### Final status is reported before postpublication verification

`FINAL_GATE: READY` approves the candidate. Only `POSTPUBLICATION_GATE: PASS` verifies the file that users will read.

### `READY` contradicts the invariant block

Zero accepted KQs, incomplete mandatory coverage, unresolved material, or fewer than 18 present and passing final checks forbids `READY`. `PREPUBLICATION_STATE_CHECK` is only one final check, and `PREPUBLICATION` is not a Critic mode.

## Observability checklist

- [ ] Does `.spec-workflow/research.md` identify one current run and precede the matching snapshot in NEW initialization?
- [ ] Did NEW delete and verify all managed workflow and outgoing state before creating that run?
- [ ] Is `.spec-workflow/source-snapshot.md` the only identity snapshot, with no root-level or duplicate copy?
- [ ] Do candidate/path/block counts and every per-block live-count/row-count/highest-ID/EOF/text equation pass?
- [ ] Are initialization, reset, managed-path, active-goal, scope, snapshot, evidence-membership, and single-run checks all `PASS`?
- [ ] Does exactly one complete Run Invariant Block reconcile with current records and files?
- [ ] Are `CURRENT_REVISION_DEFECT_SIGNATURE` and `SAME_DEFECT_REVISION_COUNT` durable and unaffected by host Autopilot continuations?
- [ ] Is `UNMAPPED_MANDATORY_OBLIGATION_COUNT` zero before production?
- [ ] Do `PRIOR_SPEC_STATE`, `PRIOR_SPEC_CHECKPOINT_STATE`, and `PRIOR_SPEC_CHECKPOINT_EQUALITY_CHECK` match the physical publication and checkpoint?
- [ ] Are all six phase gates present?
- [ ] Does every guarded action have a reread authorization record with literal `PASS`?
- [ ] Is every source included or excluded with a reason?
- [ ] Are the three Artifact Contract layers distinct?
- [ ] Does every source-backed contract field have provenance and a verified location?
- [ ] Are all obligations labeled and classified?
- [ ] Are field, member, and scale ledgers complete?
- [ ] Are IRQ records visibly `WORKFLOW_ONLY`?
- [ ] Does the coverage plan account for every mandatory obligation and unit, leaving zero mandatory obligations unmapped before production?
- [ ] Is the publication manifest present only in research?
- [ ] Does `.spec-workflow/draft.md` equal the exact candidate under review?
- [ ] Was the written `spec.md` verified after publication?

## Related reading

- [06 — Critic and Convergence](06_Critic_and_Convergence.md)
- [08 — Consistency Checks](08_Consistency_Checks.md)
- [09 — Run Health](09_Run_Health.md)
- [10 — Debugging](10_Debugging.md)
- [Text Spec Orchestrator](../.github/agents/text-spec-orchestrator.agent.md)
- [Text Spec Investigator](../.github/agents/text-spec-investigator.agent.md)
