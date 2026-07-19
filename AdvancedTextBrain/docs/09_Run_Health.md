# 09 — Run Health

Run health describes whether the current Markdown specification run is progressing through valid states with sufficient evidence. It is not measured by response length, elapsed time, or how polished an intermediate document appears.

A healthy run has one current identity, one coherent Run Invariant Block and Phase Gate Ledger, complete evidence for its current gate, passing action authorization before every dependent action, and no illegal jump to a later phase.

## Health states by gate

| Gate | Healthy pending state | Healthy accepted state | Unhealthy evidence |
|---|---|---|---|
| Baseline | Sources are being inventoried and contracts built | `ACCEPT` | `REVISE` treated as acceptance, missing source paths, invented obligations |
| Resolution | Genuine IRQs are being resolved | `ACCEPT` or reasoned `NOT_REQUIRED` | Explicit facts converted into questions, generic defaults erase constraints |
| Production | Bounded batches are being completed and reviewed | `ACCEPT` after every pair and check passes | Partial entries, placeholder preservation, unreviewed pairs |
| Draft | Accepted material is being assembled into a blank candidate | `ACCEPT` with complete manifest | Draft copied from research, missing blocks, manifest published in draft |
| Final | Complete candidate is under whole-document review | `READY` | Missing named checks, unresolved material defect, cross-unit conflict |
| Postpublication | Written file is being compared with the accepted candidate | `PASS` | File differs from draft, repeat checks fail, prior file mistaken for current |

A successful completion response is healthy only after postpublication passes. Terminal blocked, final-conditional, and postpublication-failed runs still require an accurate non-success status report.

## Signals of healthy progress

### Source discovery starts immediately

When the selected Orchestrator receives `run`, `start`, `go`, `generate`, or `build`, it begins the uninterrupted NEW initialization transaction immediately when eligible sources identify one coherent goal and output. It does not search for a runtime or stop after announcing a plan. The Investigator begins semantic baseline work only after the exact research and snapshot artifacts exist and a persisted `GUARD_CALL_BASELINE: PASS` authorizes the call.

### The baseline is accepted before production

The baseline contains the complete source inventory, Artifact Contract, obligation register, source-explicit shape facts, source-defined finite members, and source-stated or directly calculable cross-unit constraints. It contains no planned KQs, answers, chosen defaults, or production coverage. A Critic `REVISE` result keeps the baseline open and triggers an automatic baseline-only correction and re-review in the active response. That verdict needs no user approval; Default Approvals may pause only an actual tool operation. If an explicitly active Advanced Autopilot boundary stops the host, the baseline remains pending for `resume`.

### Counts, files, and guards agree

The invariant block is recalculated from unique current-run records and physical files. Its current revision-defect signature and same-defect count persist across execution segments; planned KQs equal accepted plus unaccepted KQs; covered and unresolved mandatory-ID sets are disjoint; accounted mandatory obligations equal covered plus unresolved mandatory obligations; covered, unresolved, and accounted counts are each bounded by the mandatory total; unmapped mandatory obligations may remain before planning but must be zero before production; the physical draft count equals the admitted count; prior-spec checkpoint state and equality match the physical files; and final present, missing, and failed counts reconcile. Before a major transition, a durable `ACTION_AUTHORIZATION` names the tested values and records literal `PASS`.

### Interrogation is selective

An empty IRQ set can be healthy. `RESOLUTION_GATE: NOT_REQUIRED` is valid when accompanied by the reason that no genuine ambiguity exists.

Conversely, asking questions merely because a field needs content is unhealthy. Grounded synthesis and production are responsible for content that active sources sufficiently bound.

### Production is complete, not merely broad

Production batches contain at most five planned entries. Every pair receives decisions and completeness checks. If full completion would require abbreviation, the batch becomes smaller; the workflow does not trade completeness for throughput.

### Revisions are targeted

A healthy `REVISE` cycle closes a named defect. Examples include adding missing scale members, replacing a semantic placeholder, correcting a source location, or resolving a cross-unit total.

The workflow does not define one fixed total retry count. Health is shown by defect reduction and valid state transitions, but the identical normalized defect may survive at most three completed targeted corrections before verified `BLOCKED: NON_CONVERGING`. That inner counter is separate from VS Code's outer Advanced Autopilot maximum. When evidence cannot close a material gap, the item remains visible and the run ultimately reaches `FINAL_GATE: CONDITIONAL` or `BLOCKED` rather than cycling indefinitely.

### Candidate and publication remain distinct

`.spec-workflow/draft.md` is the exact candidate reviewed at the final gate. `spec.md` is written only after readiness. Postpublication review then proves the written result still matches the accepted candidate.

## Healthy meanings of final status

### `READY`

All named checks pass, all mandatory material is covered, and no material unresolved or blocked obligation remains. `READY` permits publication but is not the end of the run; postpublication must still pass.

### `CONDITIONAL`

Useful, traced content exists, but isolated unresolved decisions remain. The unresolved content cannot make the knowledge base unsafe or unusable. Under the implemented gate rule, `spec.md` is not written unless the final gate is `READY`.

### `BLOCKED`

Missing evidence or input prevents a safe, usable specification. Dependent phases stop. The blocker should identify the missing decision and its downstream effect.

### `FAIL` after publication

The written file did not pass comparison or repeated checks. The Orchestrator restores the retained prior file. If no prior `spec.md` existed, it removes the failed new file and verifies absence.

## Resume and fresh-run health

`proceed`, `continue`, or `resume` continues only when one ledger, one goal, the exact source snapshot, semantic scope membership, and physical artifacts all match. Otherwise the Orchestrator begins NEW and records why.

A normal new run:

- preserves human/framework/unknown files but deletes and verifies all `.spec-workflow/**` and `outgoing/**` files;
- creates and rereads `.spec-workflow/research.md` first with one identity, pending invariant, reset, goal/scope records, and baseline placeholder rather than appending to old state;
- creates `.spec-workflow/source-snapshot.md` second at that exact path, with one sorted path and source block per non-reserved candidate and exact rows through live EOF;
- verifies candidate/path/block equality and, in every block, equality among `LINE_COUNT`, live line count, contiguous row count, and highest row ID, with exact text equality;
- updates and rereads all initialization checks plus `GUARD_CALL_BASELINE` before calling a worker;
- creates a candidate draft only after production acceptance;
- treats an existing `spec.md` as `PRIOR_PUBLISHED`;
- excludes that prior file from evidence and prepublication equality; and
- creates and verifies `.spec-workflow/prior-spec.md` at initialization, leaves the prior file unchanged until the write guard passes, then removes the checkpoint on postpublication success or restores and verifies the prior publication from that checkpoint on failure.

An empty `.spec-workflow` directory and an absent `spec.md` are both healthy clean-start conditions.

An orphaned, misplaced, duplicate, truncated, or malformed snapshot is not a resumable run. It is locally repairable initialization state: the Orchestrator cleans or repairs it and automatically retries NEW without asking the user to update research, rerun baseline, or approve proceeding. Only repeated concrete edit/path failure can end as `BLOCKED: RUN_INITIALIZATION_FAILED`.

### Source replacement requires NEW

Healthy RESUME requires live candidate paths and logical lines to match the stored `.spec-workflow/source-snapshot.md` without rewriting it. A changed source, second RUN_ID, second baseline, stale path, misplaced identity copy, or excluded-source citation invalidates the old state. The Orchestrator automatically converts the attempt to NEW; it does not edit old domain nouns in place.

### A host-enforced pause can be healthy

When the current permission level is Autopilot and `chat.autopilot.advanced.enabled` is `true`, VS Code 1.124 Advanced Autopilot stops after at most three automatic continuation loops. Default Approvals, Bypass Approvals, and ordinary Autopilot do not use that documented maximum. If the optional outer limit is reached while gates remain incomplete, a healthy run keeps its current run ID, accepted records, pending gate, revision signature/count, and protected prior-publication state. It is neither complete nor `BLOCKED`; `resume` continues it. A host continuation must never increment the Critic-specific unchanged-defect count.

## Unhealthy patterns

### “No runnable project detected”

This workflow is not a code runner. When eligible text sources exist, that response indicates the wrong agent or invocation path, not a healthy workflow decision.

### Plan-only response

The Orchestrator must begin baseline work in the same turn. Announcing future analysis without reading sources is not progress.

### Optionalized validation

Statements such as “the Critic can be run later” or “automated validation is a next step” are unhealthy. Required review is part of the current run.

### Published partial content

A baseline outline, schema, direct prose deliverable, or partly accepted batch cannot become `spec.md`.

### Gate and artifact disagreement

Examples include a draft when production is pending, a final status when no manifest exists, or a success response while postpublication is pending.

### Mixed-goal or stale-source baseline

A task-ticket goal, an unrelated source's embedded deliverable, and a previous run are not three equal inputs. The source-scope table must select one goal, admit only semantically relevant lines, and exclude incompatible directives. Any old outgoing/workflow claim is a current-run integrity failure.

### Stale prior output presented as current success

The prior file may remain on disk during a failed replacement, but it is not evidence that the current run succeeded. The Phase Gate Ledger and final response must make the failed current run clear.

### Contradictory `READY`

Reject `READY` when any decisive fact disagrees with it: zero accepted KQs, a draft-count mismatch, incomplete mandatory coverage, unresolved material, a missing earlier gate, fewer than 18 present and passing final checks, or `PREPUBLICATION` used as a review mode. A passing `PREPUBLICATION_STATE_CHECK` is only one check and cannot override those failures.

## Run-health checklist

- [ ] Is the intended Orchestrator explicitly selected?
- [ ] Is there exactly one current-run identity?
- [ ] Did NEW delete and verify all managed workflow/outgoing state?
- [ ] Was `.spec-workflow/research.md` created and reread before `.spec-workflow/source-snapshot.md`?
- [ ] Is the snapshot at the exact managed path with no root-level or duplicate identity copy?
- [ ] Do candidate/path/block counts and every per-block live-count/row-count/highest-ID/EOF/text equation pass?
- [ ] Are initialization, reset, managed path, one goal, scope, snapshot, membership, and single-run checks `PASS`?
- [ ] Is there exactly one complete current-run Run Invariant Block?
- [ ] Are all six gates present and in legal order?
- [ ] Does every major action have a reread `ACTION_AUTHORIZATION` with `GUARD_RESULT: PASS`?
- [ ] Did source discovery begin in the invocation turn?
- [ ] Was the baseline accepted rather than merely revised?
- [ ] Are IRQs limited to genuine material gaps?
- [ ] Are production batches bounded and complete?
- [ ] Does every produced pair have decisions and completeness checks?
- [ ] Is the draft assembled only from accepted material?
- [ ] Is the publication manifest complete and research-only?
- [ ] Did final review evaluate the exact candidate draft?
- [ ] Did every named check pass before readiness?
- [ ] Did postpublication validation pass before the success response?
- [ ] On failure, was the prior published-file state restored while current-run diagnostics remained available?

## Related reading

- [06 — Critic and Convergence](06_Critic_and_Convergence.md)
- [07 — Observability](07_Observability.md)
- [08 — Consistency Checks](08_Consistency_Checks.md)
- [10 — Debugging](10_Debugging.md)
- [Text Spec Orchestrator](../.github/agents/text-spec-orchestrator.agent.md)
- [Text Spec Interrogator](../.github/agents/text-spec-interrogator.agent.md)
