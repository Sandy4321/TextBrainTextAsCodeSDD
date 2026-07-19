# Shared Memory

The framework has no separate memory service or memory agent. Its shared memory is explicit Markdown maintained by the Orchestrator and passed deliberately to read-only workers. This makes state inspectable, recoverable, and reviewable.

## Learning goals

By the end of this chapter, you should be able to:

- explain what is stored in the source snapshot, research, draft, conditional rollback checkpoint, and published specification
- understand how agents share accepted context without hidden assumptions
- describe the purpose of every research ledger
- distinguish current-run state from a prior published file
- explain evidence provenance and verified source locations
- recognize memory corruption and workflow leakage

## What “shared memory” means here

Shared memory has two forms:

1. **Persisted Markdown:** pending and accepted workflow state in `.spec-workflow/research.md`, exact source identity in `.spec-workflow/source-snapshot.md`, the candidate in `.spec-workflow/draft.md`, and a conditional `.spec-workflow/prior-spec.md` rollback checkpoint.
2. **Explicit invocation context:** the Orchestrator passes the relevant goal, contract fields, obligations, sources, prior indexes, and requested mode to each worker.

Workers do not rely on an invisible global conversation history. If a decision matters later, it must be persisted and passed.

This also applies across VS Code execution segments. Only a session using the Autopilot permission level with `chat.autopilot.advanced.enabled: true` has the documented three-continuation boundary. If that optional boundary stops the chat before a terminal gate, the current run ID, accepted records, incomplete gates, `CURRENT_REVISION_DEFECT_SIGNATURE`, and `SAME_DEFECT_REVISION_COUNT` remain in research. A later `resume` continues that same state; the chat-only host pause is not written as a phase gate. Under Default Approvals, Critic correction remains internal and only an actual tool may require confirmation.

## Four lifecycle roles plus one rollback checkpoint

The listing order below is not the creation order. NEW creates and rereads `.spec-workflow/research.md` first; only then may it create `.spec-workflow/source-snapshot.md`. A snapshot without matching research is orphaned, uncommitted state rather than resumable memory, so the Orchestrator repairs it or cleanly retries NEW.

### `.spec-workflow/source-snapshot.md`

The snapshot stores one RUN_ID, sorted non-reserved candidate paths, and one line-addressed `### SOURCE` block per candidate, written and reread in bounded chunks through EOF. Candidate count equals path count and block count. In each block, `LINE_COUNT` equals the live line count, contiguous `L*` row count, and highest row ID, and every row text matches disk. Fixed exclusions and the included count live in research, not in the identity snapshot. It is control memory, not source evidence. NEW replaces it; RESUME compares current disk state with it rather than rewriting it to manufacture a match.

### `.spec-workflow/research.md`

Research is the durable current-run control plane. It may contain:

- current-run identity
- run mode, reset reason, active goal, and verified goal anchor
- generated-artifact reset, exact managed-artifact paths, source-scope, source-snapshot, evidence-membership, and single-run checks
- included and excluded source inventory with reasons
- Phase Gate Ledger
- current normalized Critic defect signature and unchanged-defect revision count
- accepted three-layer Artifact Contract
- obligation register
- Field Instantiation Contract
- finite enumeration and ordered-scale member ledgers
- cross-unit dependencies and aggregate calculations
- genuine gaps and accepted `IRQ-*` resolutions
- KQ and UNIT coverage plan
- compact accepted-entry indexes
- unit-by-field and unit-by-enumeration completion matrices
- production decisions and checks
- research-only publication manifest
- conflict and override records
- prior-publication state and rollback-checkpoint verification

Research is not published wholesale. It contains workflow metadata that would confuse a downstream reader.

### `.spec-workflow/draft.md`

The draft is the exact candidate specification reviewed before publication. It begins blank after production acceptance and includes only:

- framework-required reader context
- accepted KQs and task-required fields
- visible assumptions, conflicts, and unresolved decisions
- coverage, traceability, and readiness

It must not contain worker transcripts, `IRQ-*` records, completion ledgers, or the publication manifest.

### `.spec-workflow/prior-spec.md`

This file exists only when `spec.md` was present at run initialization. It is a verbatim, equality-verified, temporary rollback checkpoint. It is never current task evidence or candidate content. Both it and the prior publication remain unchanged until the write guard passes. Postpublication success removes the checkpoint; on failure, the checkpoint is used to restore and verify the prior publication, and the current contract does not require deleting it at that failure point.

### `spec.md`

The published file is an exact accepted copy of the candidate draft. It is the durable Q&A knowledge base for downstream work.

An earlier `spec.md` is classified as `PRIOR_PUBLISHED` during a new run. It remains protected but is excluded from current-run evidence.

## Why research and draft are separate

Research answers “How do we know and validate this?” The draft answers “What must the downstream reader know?”

Combining them produces familiar failures:

- internal questions appear as deliverable questions
- Critic decisions appear as user-facing sections
- placeholders or rejected proposals survive into publication
- opaque IDs replace understandable knowledge
- old and current states become indistinguishable

The blank-draft rule is a deliberate memory boundary.

## The Phase Gate Ledger

The ledger records the state of:

- baseline
- resolution
- production
- draft
- final review
- postpublication review

It is not a checklist of intentions. Each accepted state requires the evidence defined by the Orchestrator. `REVISE` remains pending, and missing gate evidence blocks publication.

The run identity stays in research and its manifest. It is verified after publication but is not forced into `spec.md`.

## The obligation register

Each `OBL-*` record contains:

- immutable ID
- readable label
- exact active-source wording
- heading
- mandatory status
- treatment type
- coverage status
- verified workspace-relative line location

An obligation is evidence-backed. A recommendation or synthesized decision cannot be relabeled as an obligation simply because it would be useful.

Published traceability must be understandable without opening research. Bare ID lists are insufficient; labels and verified source locations accompany IDs.

## The Field Instantiation Contract as memory

The field contract gives every worker the same source-grounded definition of “complete.” During baseline it records the smallest applicable shape:

- a topic and completion meaning for a `CONTENT_AREA`;
- only source-required children, order, dependencies, and supported cardinality for a `REPEATED_UNIT`;
- exact source-defined members and progression for an `ENUMERATION_OR_SCALE`.

If no count or named set is supported, durable research records `CARDINALITY: SOURCE_UNSPECIFIED`. The current Investigator proposal label `SOURCE_CARDINALITY: UNSPECIFIED` has the same meaning and is documented as a compatibility spelling in [File Formats](13_File_Formats.md#current-cardinality-spelling-compatibility). After baseline and resolution acceptance, coverage planning adds only the classified decisions needed for production. The expected-member ledger expands finite source-defined sets before production; it does not manufacture a set for every plural noun.

Completion matrices make admission explicit. A `CONTENT_INSTANCE` remains uncovered until its UNIT passes every field and member check.

## The publication manifest

The manifest maps every candidate draft block and line range to one of:

- `FRAMEWORK_CORE`
- a source obligation
- a `KQ-*`
- a `UNIT-*`

It exists only in research. Its purposes are:

- prove that every draft block has an authorized origin
- prove that every required item appears in the candidate
- detect research-record leakage
- compare the accepted draft with the new published file

Publishing the manifest would expose workflow mechanics without helping the specification reader.

## Explicit context passing

The Orchestrator sends only the context needed for the active worker task, while preserving critical details:

- active goal and relevant sources
- all relevant Artifact Contract layers
- mapped obligations and verified locations
- the applicable adaptive field contract and any source-defined scale members
- accepted resolutions that affect the item
- a compact prior-entry index for duplicate and coverage checks

Full prior entries are passed only for a genuine dependency or overlap. This keeps batches bounded without forcing workers to reconstruct hidden context.

## Provenance and knowledge status

Artifact Contract fields use provenance:

- `FRAMEWORK` for invariant workflow facts
- `SOURCE` for directly stated source facts
- `DERIVED_FROM_SOURCE` for justified inference

They also use status:

- `FACT`
- `DERIVED`
- `UNRESOLVED`
- reasoned `NOT_APPLICABLE`

Resolution and production knowledge separately distinguishes fact, derived requirement, grounded synthesis, assumption, and unresolved knowledge.

Verified source locations use plain workspace-relative `path:line` or `path:start-end`. They are read from the current source, not guessed from memory.

## Non-normative memory sketch

Assume a separate hypothetical source describes a supervised activity. Research might need to remember source facts such as:

- exactly 24 participants
- a 45-minute total duration
- four supervisors
- a prohibited set of locations
- one explicit accessibility requirement
- two unresolved grouping alternatives

It should separately record any synthesized activity decisions and their bounding evidence. A synthesized format, sequence, or presentation choice is not rewritten as a source fact. This sketch is explanatory only; it is not current task evidence.

If no genuine material gap remains after safe synthesis, the resolution gate can be `NOT_REQUIRED`. The framework must not invent questions about unrelated dietary possibilities, shopping regions, or execution environments.

## Conflict and override memory

A user response can supersede an explicit source fact only through a complete override record containing:

- affected obligation
- original wording and verified location
- exact conflict presented to the user
- exact user response
- explicit supersession statement
- provenance
- downstream effect

Material safety constraints require acknowledgment of the exact fact. A broad “no” or `use defaults` response cannot erase a source-stated safety restriction.

## Prior-published isolation

During a new run:

- all prior `.spec-workflow/**` and `outgoing/**` files are deleted and verified before new state is created
- `.spec-workflow/research.md` is created and reread first; `.spec-workflow/source-snapshot.md` is created and verified second; old research is never appended and a misplaced or orphaned copy is never accepted
- the old `spec.md` is copied verbatim to `.spec-workflow/prior-spec.md` and exact equality is verified
- the checkpoint and old publication remain unchanged until the final write guard passes
- it is excluded from source discovery
- it is excluded from prepublication equality
- the new candidate is compared only with current-run research and manifest

After publication, the new file is compared with the accepted draft. If the comparison fails, the prior `spec.md` contents are restored, or the failed file is removed when prior state was absence. Current-run research and draft remain as diagnostic memory.

## Failure modes

### Research is copied into the draft

Internal queries, Critic labels, and completion matrices leak into the user-facing knowledge base.

### A worker relies on an unstated prior decision

The result becomes irreproducible and may contradict current evidence.

### IDs are reused across runs

Coverage and traceability can point to stale content. New runs must not reuse prior obligations, resolutions, or units as current state.

### A new baseline is appended below old research

Two RUN_IDs or two canonical baselines allow stale paths and domain decisions to masquerade as current evidence. NEW must delete the managed workflow state first and then create exactly one ledger.

### A snapshot exists without matching research

The snapshot's RUN_ID has no authority by itself. This is uncommitted initialization state, so the Orchestrator must repair or cleanly retry NEW before calling a worker; it must not ask the user to create research or rerun baseline.

### The source changed during RESUME

An old ledger can be internally consistent and still describe files that no longer exist. Exact path-and-line snapshot comparison turns the operation into NEW before any old obligation is reused.

### The prior specification becomes evidence

Old assumptions can silently override current sources.

### A guessed line location is persisted

Traceability becomes misleading. Unverified locations remain internal and cannot be published.

### The manifest is absent

The final review cannot prove that every draft block belongs or detect leaked workflow content.

## Practical checklist

- [ ] Research contains the current run identity and Phase Gate Ledger.
- [ ] Research was created and reread before the matching snapshot, and no orphaned or duplicate managed artifact exists.
- [ ] Snapshot candidate/path/block counts and every per-block live-count/row-count/highest-ID/EOF/text equation pass.
- [ ] Initialization, generated cleanup, managed path, scope, snapshot, membership, and single-run checks are `PASS`.
- [ ] Every source is included or excluded with a reason.
- [ ] Every OBL has exact wording and verified location.
- [ ] Artifact Contract provenance and status are recorded.
- [ ] Baseline shape facts and the post-resolution adaptive production contract are persisted before production.
- [ ] IRQ resolutions remain in research only.
- [ ] Completion matrices reflect actual admitted content.
- [ ] The draft starts blank and contains only publication candidates.
- [ ] The manifest remains research-only.
- [ ] When `PRIOR_SPEC_STATE: PRESENT`, `.spec-workflow/prior-spec.md` exists, exact equality was verified, and checkpoint state agrees with the physical files.
- [ ] Prior-published content is isolated from current-run evidence.
- [ ] Workers receive explicit, sufficient context.
- [ ] Postpublication failure restores the prior published-file state and retains current-run diagnostics.

## Related reading

- [Getting Started](00_Getting_Started.md)
- [Framework Architecture](02_Framework_Architecture.md)
- [Workflow Engine](03_Workflow_Engine.md)
- [Interrogation Algorithm](05_Interrogation_Algorithm.md)
- [Orchestrator definition](../.github/agents/text-spec-orchestrator.agent.md)
