# Observability Reference

## Purpose

Observability answers four practical questions during a run:

1. What evidence did the workflow read?
2. What does it currently believe and why?
3. Which phase and units have passed review?
4. What exact defect prevents the next transition?

This framework uses Markdown records rather than a telemetry service. Observable state must be written into `.spec-workflow/research.md` and `.spec-workflow/draft.md`; chat narration alone is not authoritative.

## The observable surfaces

| Surface | What it reveals | Authority |
|---|---|---|
| Copilot Chat | Current delegation, tool use, worker summaries, and user interaction | Useful live view, not durable workflow state |
| `.spec-workflow/research.md` | First authoritative NEW artifact: current-run identity, pending and accepted state, evidence, contracts, ledgers, decisions, checks, coverage, and phase gates | Durable internal state and source of RUN_ID authority |
| `.spec-workflow/source-snapshot.md` | Second NEW artifact: matching RUN_ID, sorted candidate paths, one exact line-addressed source block per candidate, and freshness equations | Control authority only when matching research exists; never task evidence |
| `.spec-workflow/draft.md` | Exact candidate reviewed for publication | Authoritative candidate |
| `.spec-workflow/prior-spec.md` | Verified temporary checkpoint of an existing publication | Rollback authority until postpublication pass or verified restoration |
| `spec.md` | Published Q&A knowledge base | Authoritative final output only after postpublication pass |
| `.github/agents/*.agent.md` | The declarative implementation and validation contracts | Framework definition |
| `.github/agent-contracts/*.md` | Reusable instructions linked by an agent, never task evidence | Framework definition dependency |
| `outgoing/` | Legacy agent-owned generated output | Must be empty after NEW initialization; never current evidence |

VS Code displays subagent activity as expandable tool calls. That helps you see which worker ran and what it returned, but the Orchestrator must still persist accepted records to the Markdown ledger.

## Phase Gate Ledger

Every run exposes these states in order:

| Gate | Initial | Accepted outcome | Other terminal outcome |
|---|---|---|---|
| `BASELINE_GATE` | `PENDING` | `ACCEPT` | `BLOCKED` |
| `RESOLUTION_GATE` | `PENDING` | `ACCEPT` or justified `NOT_REQUIRED` | `BLOCKED` |
| `PRODUCTION_GATE` | `PENDING` | `ACCEPT` | `BLOCKED` |
| `DRAFT_GATE` | `PENDING` | `ACCEPT` | `BLOCKED` |
| `FINAL_GATE` | `PENDING` | `READY` | `CONDITIONAL` or `BLOCKED` |
| `POSTPUBLICATION_GATE` | `PENDING` | `PASS` | `FAIL` |

The ledger is monotonic within a valid run. A gate cannot be inferred from a friendly chat message, and `REVISE` is never recorded as acceptance.

## Run Invariant Block and action authorization

Exactly one ordered `Run Invariant Block` is the current run's authoritative control snapshot. It exposes:

- run identity and active phase
- run mode, reset reason, initialization state, active-goal ID, and active-goal anchor
- generated-artifact reset, managed-artifact path, source-scope, exact-snapshot, evidence-membership, and single-run-state checks
- source candidate count and positive included-source count
- current normalized Critic revision-defect signature and `SAME_DEFECT_REVISION_COUNT`
- baseline, resolution, production, draft, final, and postpublication state
- genuine and open material IRQ counts plus `RESOLUTION_NOT_REQUIRED_REASON`
- mandatory, accounted, covered, unresolved, material-or-unclassified unresolved, and not-yet-mapped obligation counts, including `MATERIAL_OR_UNCLASSIFIED_UNRESOLVED_COUNT` and `UNMAPPED_MANDATORY_OBLIGATION_COUNT`
- planned, accepted, and unaccepted KQ counts
- required and accepted unit counts
- physical draft KQ count, Q&A-envelope failures, and unmapped manifest blocks
- proposed versus authoritative final status
- required, present, missing, and failed final-check counts
- prior-gate and prepublication checks plus `PRIOR_SPEC_STATE`, `PRIOR_SPEC_CHECKPOINT_STATE`, and `PRIOR_SPEC_CHECKPOINT_EQUALITY_CHECK`
- write and success-report authorization

Counts come from unique current-run records and physical files. A summary, opaque ID range, announced plan, or worker statement is not count evidence. The key reconciliation rules include:

- planned KQs = accepted KQs + unaccepted KQs
- covered mandatory IDs are a subset of accounted mandatory IDs
- accounted mandatory IDs are a subset of all mandatory IDs
- covered and unresolved mandatory-ID sets are disjoint
- accounted mandatory obligations = covered mandatory obligations + unresolved mandatory obligations
- covered, unresolved, and accounted counts are each bounded by the mandatory-obligation count
- unmapped mandatory obligations may be nonzero before coverage planning but must be zero before production
- final missing checks = 18 required checks - present checks

Before each guarded action, the Orchestrator writes an `ACTION_AUTHORIZATION` record containing the action name, every tested field and current value, `GUARD_RESULT: PASS|FAIL`, and the reason. It rereads the record and acts only on literal `PASS`. Missing, stale, contradictory, estimated, or non-numeric evidence fails closed.

Only the Autopilot permission level with `chat.autopilot.advanced.enabled: true` has the documented three-loop host boundary. When VS Code explicitly exposes that optional boundary, `PAUSED: HOST_AUTOPILOT_LIMIT` is chat-only, not a durable phase-gate value. Never use it for a Default Approvals tool confirmation. The active run remains observable through its unchanged run ID, gates, accepted records, `CURRENT_REVISION_DEFECT_SIGNATURE`, and `SAME_DEFECT_REVISION_COUNT`; `resume` continues the earliest incomplete gate.

## Source snapshot and inventory

For every readable non-reserved textual candidate, the snapshot records one sorted workspace-relative path and one `### SOURCE path` block containing exact logical lines through editor-reported EOF. Fixed reserved/control exclusions and the included count remain in research rather than becoming snapshot candidates. Snapshot identity is valid only at `.spec-workflow/source-snapshot.md`; a root-level, basename-only, or duplicate current-run copy fails `MANAGED_ARTIFACT_PATH_CHECK`.

The structural equations are observable and exact:

- `SOURCE_CANDIDATE_COUNT` equals sorted candidate-path count and source-block count;
- for each block, rows are contiguous from `L000001` through `L{LINE_COUNT}`, and `LINE_COUNT` equals live line count, row count, and highest row ID; and
- every row text equals the live source text through EOF.

The inventory then records:

- workspace-relative path
- included or excluded
- reason

Mixed-purpose input can have admitted goal-relevant sections and excluded incompatible embedded directives. Verified line boundaries belong to each `OBL-*` record or other evidence claim. A source-inventory row does not need to repeat all of those ranges.

Normal exclusions include agent definitions, documentation, workflow records, prior `spec.md`, version-control data, generated output, dependency folders, and unrelated implementation files.

An inventory that lists `input-spec/jira.md` when the file is actually `jira.md` is unhealthy. Evidence locations must be verified against the current workspace, not guessed from a remembered layout.

## Obligation Register

Each `OBL-*` record should expose:

- immutable ID and human-readable label
- exact source wording
- heading
- verified `path:line` or `path:start-end`
- mandatory status
- treatment type
- coverage status

Treatment types are `STRUCTURE`, `CONTENT_INSTANCE`, `CONTENT_CONSTRAINT`, `COVERAGE`, `PROCESS_OR_ACCEPTANCE`, and `UNRESOLVED_DECISION`.

The register makes omission visible. It also prevents recommendations from being misrepresented as source obligations.

## Artifact Contract view

The research ledger should visibly separate:

- specification knowledge layer
- downstream deliverable layer
- operational or experience layer

For each field, expose value, provenance, source location where applicable, and knowledge status. A reasoned `NOT_APPLICABLE` is observable; silently omitting a role is not.

## Field Instantiation Contract view

For every source-native unit field, expose:

- applicability and required/optional/conditional status
- content kind and meaning
- nested children and dependencies
- exact or minimum cardinality
- finite members or scale levels
- progression direction for an ordered scale
- N/A policy
- source basis

This contract turns “complete” into inspectable conditions.

## Completion matrices

### Unit-by-field matrix

Rows represent `UNIT-*` records; columns represent required field paths. Each cell should show a concrete state such as populated, not applicable with reason, unresolved, or failed.

### Unit-by-enumeration matrix

Rows represent units and columns represent each expected finite member. This reveals missing score levels, duplicated stages, compressed ranges, or selected endpoints.

### Coverage map

The post-resolution plan accounts for every mandatory obligation by mapping it to one or more destinations:

- planned `KQ-*` or associated planned `UNIT-*`;
- an attached source-required field;
- a framework or cross-unit section;
- a navigation or traceability section; or
- an explicit unresolved decision.

Planning and an unresolved record provide accounting, not coverage credit. An obligation counts as covered only when its complete required current-run content exists in an admitted KQ/UNIT, attached field, or accepted framework/cross-unit/traceability section. Covered and unresolved mandatory sets remain disjoint. Every required unit must map to a KQ, and the reverse mapping should show why each KQ exists without manufacturing one meta-question per obligation.

## Production decision record

For each proposed KQ/UNIT pair, the Critic returns:

- `KQ_DECISION`
- `UNIT_DECISION`
- `FIELD_INSTANTIATION_CHECK`
- `PLACEHOLDER_SCAN`
- `ENUMERATION_COMPLETENESS_CHECK`
- `ORDERED_SCALE_QUALITY_CHECK`

The pair is admitted only when every applicable decision is accepted and every applicable check passes. A summary such as “looks good overall” is not an observable admission record.

## Final review dashboard

The final critic record should expose all named checks, including:

- source inventory
- artifact contract
- Q&A knowledge-base structure
- field instantiation
- placeholder scan
- enumeration completeness
- ordered-scale quality
- source specificity
- cross-unit consistency
- question roles
- IRQ leak
- unit coverage
- defaults and conflicts
- strict operators
- variant completeness
- prepublication state
- source locations
- manifest

The final audit also exposes `18` required checks, the present/missing/failed totals, accepted and physical draft KQ counts, obligation totals, unresolved totals, and `PRIOR_GATE_CHECK`.

`FINAL_STATUS: READY` is valid only when all 18 checks are present and `PASS`, missing and failed counts are zero, prior gates pass, at least one accepted KQ exists and matches the physical draft count, mandatory coverage is complete, and unresolved mandatory or unclassified material counts are zero.

The raw Critic value is observable first as `PROPOSED_FINAL_STATUS`. The authoritative `FINAL_STATUS` and `FINAL_GATE` change only after the matching ready or non-ready result guard passes. This separates “the reviewer proposed it” from “the state machine admitted it.”

`PREPUBLICATION_STATE_CHECK` is one named check inside `FINAL`; `PREPUBLICATION` is not a review mode.

## Publication manifest

The manifest connects the accepted production records to exact draft blocks. It should make these properties visible:

- current-run identity
- expected KQ and UNIT IDs
- block order and counts
- coverage mappings
- accepted source basis
- candidate draft identity

The final critic checks the draft against this manifest. The postpublication critic checks `spec.md` against the same accepted state.

## Postpublication evidence

Publication is not complete when the file write finishes. The final observable transition is:

> `spec.md` written → exact comparison and repeated checks → `POSTPUBLICATION_GATE: PASS`

A mismatch, missing entry, changed count, placeholder, or stale content produces `FAIL`. The Orchestrator records the failure and restores the retained prior `spec.md`; when no prior file existed, it removes the failed new file and verifies absence. It reports that failed run rather than reopening the monotonic `FAIL` gate. A corrected candidate requires a new run; validation cannot be replaced by a promise to check later.

## Run-health signals

Healthy signals:

- `.spec-workflow/research.md` was created and reread before `.spec-workflow/source-snapshot.md`
- exact managed paths, matching RUN_ID, candidate/path/block counts, and every row/EOF/text equation pass
- source paths exist and line references match
- NEW initialization left no stale file in `.spec-workflow/**` or `outgoing/**`
- active goal, scope, exact snapshot, and evidence membership all pass
- one coherent current-run identity
- gates advance in order
- critic defects name affected IDs and allowed next actions
- corrections are targeted
- a host Autopilot stop leaves the active ledger intact and resumable
- all required finite members are visible
- draft and final counts agree
- postpublication check passes

Warning signals:

- a snapshot exists without matching research, at a root/duplicate path, or with gaps, summaries, truncation, or invented counts
- Copilot searches for a package manager or asks what code to run
- source inventory uses nonexistent paths or broad fake line ranges
- a NEW run appends a second run or baseline to old research
- `outgoing/**`, prior workflow state, prior `spec.md`, or another excluded path supports a current obligation
- RESUME proceeds after the goal, candidate paths, logical lines, scope, or physical artifacts changed
- baseline remains `REVISE` while production or draft begins
- baseline contains planned KQs, answers, concrete values, defaults, IRQ resolutions, or production coverage
- `PREPUBLICATION` is invoked as though it were a Critic mode
- `READY` appears beside zero accepted KQs, partial coverage, unresolved material, or missing final checks
- worker output is described as “archived in session” rather than persisted
- a gate is accepted without its required evidence
- a host Autopilot loop increments `SAME_DEFECT_REVISION_COUNT` or is recorded as workflow `BLOCKED`
- `spec.md` appears before `.spec-workflow/draft.md` acceptance
- a prior `spec.md` changes or disappears before `GUARD_WRITE_SPEC: PASS`
- accepted fields contain ellipses, `1..5`, “same as above,” or future instructions
- internal IRQ wording appears in `spec.md`

## Reading a current run

Use this order:

1. Verify that exact `.spec-workflow/research.md` exists and supplies the one authoritative RUN_ID; without it, no active run exists.
2. Verify NEW/RESUME classification, `RUN_INITIALIZATION_GATE`, generated-artifact reset, and `MANAGED_ARTIFACT_PATH_CHECK`.
3. Compare current candidate paths and logical lines with the same-RUN_ID `.spec-workflow/source-snapshot.md`, including every path/block/count/row/EOF/text equation.
4. Verify the selected goal, scope table, evidence membership, and single-run state.
5. Identify prior-publication classification, checkpoint state, and equality check.
6. Read and reconcile the Run Invariant Block with the physical files.
7. Read the Phase Gate Ledger.
8. Inspect the first non-accepted gate.
9. Read the Critic decision attached to that phase.
10. Read the latest `ACTION_AUTHORIZATION` for the attempted next action.
11. Follow affected IDs into obligation, unit, field, and coverage records.
12. Confirm only the allowed next action is being taken.
13. If final output exists, check the publication manifest and postpublication state.

## Observability checklist

- [ ] Chat progress has corresponding durable ledger state.
- [ ] Research exists first and provides the authoritative RUN_ID; an orphan snapshot is treated as uncommitted state.
- [ ] Initialization, reset, managed-path, active-goal, scope, snapshot, evidence-membership, and single-run records match physical files.
- [ ] Snapshot candidate/path/block counts and every per-block live-count/row-count/highest-ID/EOF/text equation pass.
- [ ] Exactly one complete Run Invariant Block agrees with unique records and physical files.
- [ ] Every major transition or write has a reread `ACTION_AUTHORIZATION` with literal `PASS`.
- [ ] Every evidence claim has a real workspace-relative location.
- [ ] Every gate has the required decision record.
- [ ] Required fields and finite members are visible in matrices.
- [ ] Coverage is many-to-many where the task requires it, not artificially one obligation per KQ.
- [ ] Assumptions, derivations, synthesis, conflicts, and unresolved decisions are classified.
- [ ] Draft identity and manifest are present before final review.
- [ ] Published content exactly matches the accepted draft.
- [ ] Postpublication state is `PASS` before the run is reported complete.

## Related reading

- [Observability chapter](07_Observability.md)
- [Shared Memory](04_Shared_Memory.md)
- [Run Health](09_Run_Health.md)
- [Debugging](10_Debugging.md)
- [Visible Workflow](VISIBLE-WORKFLOW.md)
