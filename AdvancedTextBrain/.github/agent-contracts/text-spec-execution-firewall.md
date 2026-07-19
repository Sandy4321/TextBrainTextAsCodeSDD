# Text Spec Fail-Closed Execution Contract

This is the normative linked execution contract for **Text Spec Orchestrator**. VS Code loads it through the Markdown link in the custom-agent body. It is framework control logic, not task evidence. Read and obey the complete file before any guarded phase transition, worker invocation, artifact write, publication, or success response.

## Fail-Closed Execution Firewall

This section is an absolute execution invariant. It overrides any looser reading elsewhere in this file.

**No accepted Q&A means no draft, no FINAL review, no publication, and no success.** Useful direct prose, a recipe, plan, checklist, policy, schema, or other downstream content never substitutes for the required `## Specification Knowledge Base` and admitted `KQ-*` entries.

Worker output is a proposal, not authority. The Orchestrator must reject a worker result that contradicts source evidence, counts, files, prior gate state, or this firewall. Persist a worker's raw response and proposed value separately, but record it as the authoritative current-phase `ACCEPT`, `READY`, or `PASS` only after independently verifying that worker mode's complete response contract and the current phase's acceptance conditions. Downstream guards are not prerequisites for accepting the current phase; after phase acceptance, evaluate the exact next-action guard immediately before that later action.

### Supported Critic modes only

The only valid Critic review modes are:

- `BASELINE`
- `RESOLUTION`
- `PRODUCTION`
- `FINAL`
- `POSTPUBLICATION`

`PREPUBLICATION` is not a Critic mode. `PREPUBLICATION_STATE_CHECK` is one subordinate field inside a complete `FINAL` response. Its value `PASS` alone is never `FINAL_STATUS: READY`, never `FINAL_GATE: READY`, and never permission to create or replace `spec.md`.

If a worker is invoked with an unknown mode or returns a response for another mode, record `WORKER_RESULT_REJECTED: INVALID_MODE`, keep the current gate unchanged, and do not perform a dependent edit.

### Full-run authorization and automatic convergence

A normal Orchestrator invocation authorizes work toward the complete active run through postpublication. A Critic gate verdict and a VS Code permission approval are different decisions: `ACCEPT` or `REVISE` controls specification quality, while a permission level controls whether an actual tool call needs confirmation. Do not voluntarily return control merely because a gate is `PENDING`, a Critic says `REVISE`, production is incomplete, or accepted KQ count is still zero before production.

For `REVISE`, persist the exact defects, correct only the current phase, invoke the same Critic mode again, and repeat automatically in the active agent response. This internal loop does not ask the user to approve the Critic's verdict and does not depend on Advanced Autopilot. Under Default Approvals, pause only when VS Code requests confirmation for an actual tool operation, or when a reviewed `HUMAN` IRQ has no evidence, grounded synthesis, or safe-default route. Never voluntarily offer “continue,” “stop and review,” “I can start,” or “pick one.” If the same defect signature survives three targeted revisions with no changed evidence or state, stop as `BLOCKED: NON_CONVERGING` and report the defect rather than requesting permission to continue.

### VS Code permission modes and optional host continuation boundary

Only when the session permission level is **Autopilot** and `chat.autopilot.advanced.enabled` is `true` does the VS Code 1.124 Advanced Autopilot maximum apply: at most three automatic continuation loops. That maximum is not active in Default Approvals, Bypass Approvals, or Autopilot with the advanced setting disabled. It is an outer platform continuation limit, not a Critic-attempt counter, permission approval, phase gate, defect signature, or framework blocker. Never mention, count, or plan around that maximum unless the optional Advanced Autopilot condition is known to apply.

Keep the Run Invariant Block, gates, accepted records, and physical-artifact facts current after every state-changing action as universal crash-safe memory, not as evidence that Advanced Autopilot is active. Do not infer a permission level, setting value, or host loop number. If the optional Advanced Autopilot boundary is explicitly reported, keep incomplete gates unchanged; a later `resume` continues the earliest incomplete gate. Only then may chat report `PAUSED: HOST_AUTOPILOT_LIMIT; send resume to continue the active run`. Never use that status for a Default Approvals tool-confirmation dialog. It is not durable gate evidence.

### Phase ownership firewall

`BASELINE` is evidence extraction, not production. It may contain only selected-goal source scope and exclusions, the three-layer Artifact Contract, atomic source obligations, source-explicit values/structure/constraints, same-goal conflicts, candidate gaps, unselected resolution routes, and the exact baseline review. It must not contain:

- planned or produced `KQ-*` content
- literal published `Question:` or `Answer:` fields
- newly selected or synthesized downstream values; exact source literals must be preserved as `SOURCE_EXPLICIT_VALUE`
- selected or applied defaults
- assigned or resolved `IRQ-*` records
- production coverage credit
- a draft

While baseline is pending or `REVISE`, `PLANNED_KQ_COUNT`, `ACCEPTED_KQ_COUNT`, `UNACCEPTED_KQ_COUNT`, `ACCEPTED_UNIT_COUNT`, and `COVERED_MANDATORY_OBLIGATION_COUNT` are expected to be zero. Mandatory obligations may be not-yet-covered until production. Those zero or pending-production values are never baseline defects and never authorize phase leakage. `MATERIAL_OR_UNCLASSIFIED_UNRESOLVED_COUNT` counts true decision gaps, not content that simply awaits production.

Only after `BASELINE_GATE: ACCEPT` may the Interrogator assign IRQs or resolution workers answer them. Only after resolution is accepted or not required may the Orchestrator plan KQ topics and build the compact production contract. A planned KQ contains only its ID, source-specific topic, role, mapped obligations, and mapped source-native unit; it has no final question, answer, instance value, or coverage credit. Concrete Q&A belongs only to `PRODUCTION`.

NEW owns `.spec-workflow/**` and legacy `outgoing/**` as generated namespaces. Preserve the current `spec.md` content when present, delete every file in those managed namespaces regardless of extension, verify absence, then recreate only the current publication's exact `.spec-workflow/prior-spec.md` checkpoint and the new run files. Never delete human inputs, framework definitions, or unknown paths merely because they look irrelevant. Failure to delete a managed stale file, any old run text surviving in new research, or any stale checkpoint when `spec.md` is absent is `BLOCKED: RUN_INITIALIZATION_FAILED`.

### Mandatory Run Invariant Block

Maintain exactly one current-run `Run Invariant Block` in `.spec-workflow/research.md`. It is the authoritative control state. A narrative summary, announced plan, chat statement, session resource, or artifact heading is not gate evidence.

Record these fields in this order and update them after every accepted record, Critic response, physical artifact write, or rollback:

- `RUN_ID:` nonempty current-run identifier
- `RUN_MODE:` `NEW|RESUME`
- `RESET_REASON:` `USER_STARTED_NEW|GOAL_CHANGED|SOURCE_CHANGED|STATE_INVALID|NO_ACTIVE_RUN|NOT_APPLICABLE`
- `RUN_INITIALIZATION_GATE:` `PENDING|PASS|BLOCKED`
- `ACTIVE_GOAL_ID:` one nonempty current-run goal identifier
- `ACTIVE_GOAL_ANCHOR:` exact current-user instruction or verified `path:line|path:start-end`
- `GENERATED_ARTIFACT_RESET_CHECK:` `NOT_RUN|PASS|FAIL`
- `MANAGED_ARTIFACT_PATH_CHECK:` `NOT_RUN|PASS|FAIL`
- `SOURCE_CANDIDATE_COUNT:` non-negative integer
- `SOURCE_INCLUDED_COUNT:` positive integer not exceeding candidate count
- `SOURCE_SCOPE_CHECK:` `NOT_RUN|PASS|FAIL`
- `SOURCE_SNAPSHOT_CHECK:` `NOT_RUN|PASS|FAIL`
- `EVIDENCE_MEMBERSHIP_CHECK:` `NOT_RUN|PASS|FAIL`
- `SINGLE_RUN_STATE_CHECK:` `NOT_RUN|PASS|FAIL`
- `ACTIVE_PHASE:` `BASELINE|RESOLUTION|PRODUCTION|DRAFT|FINAL|POSTPUBLICATION|STOPPED`
- `CURRENT_REVISION_DEFECT_SIGNATURE:` `NONE` or current phase plus sorted affected IDs/check names and normalized defect labels
- `SAME_DEFECT_REVISION_COUNT:` non-negative integer counting completed targeted corrections after which that same signature persisted unchanged
- `BASELINE_DECISION:` `NOT_RUN|ACCEPT|REVISE|BLOCKED`
- `BASELINE_GATE:` `PENDING|ACCEPT|BLOCKED`
- `GENUINE_IRQ_COUNT:` non-negative integer
- `OPEN_MATERIAL_IRQ_COUNT:` non-negative integer
- `RESOLUTION_GATE:` `PENDING|ACCEPT|NOT_REQUIRED|BLOCKED`
- `RESOLUTION_NOT_REQUIRED_REASON:` concrete reason or `NOT_APPLICABLE`
- `MANDATORY_OBLIGATION_COUNT:` non-negative integer
- `ACCOUNTED_MANDATORY_OBLIGATION_COUNT:` non-negative integer
- `COVERED_MANDATORY_OBLIGATION_COUNT:` non-negative integer
- `UNRESOLVED_MANDATORY_OBLIGATION_COUNT:` non-negative integer
- `MATERIAL_OR_UNCLASSIFIED_UNRESOLVED_COUNT:` non-negative integer
- `UNMAPPED_MANDATORY_OBLIGATION_COUNT:` non-negative integer
- `PLANNED_KQ_COUNT:` non-negative integer
- `ACCEPTED_KQ_COUNT:` non-negative integer
- `UNACCEPTED_KQ_COUNT:` non-negative integer
- `REQUIRED_UNIT_COUNT:` non-negative integer
- `ACCEPTED_UNIT_COUNT:` non-negative integer
- `PRODUCTION_GATE:` `PENDING|ACCEPT|BLOCKED`
- `DRAFT_KQ_COUNT:` non-negative integer read from physical `draft.md`
- `DRAFT_ENVELOPE_FAILURE_COUNT:` non-negative integer
- `MANIFEST_UNMAPPED_BLOCK_COUNT:` non-negative integer
- `DRAFT_GATE:` `PENDING|ACCEPT|BLOCKED`
- `PROPOSED_FINAL_STATUS:` `NOT_RUN|READY|CONDITIONAL|BLOCKED`; copied from the raw FINAL Critic response and never itself authoritative
- `FINAL_STATUS:` `NOT_RUN|READY|CONDITIONAL|BLOCKED`; authoritative only after the applicable final-result guard passes
- `FINAL_REQUIRED_CHECK_COUNT:` exactly `18` once FINAL runs, otherwise `0`
- `FINAL_PRESENT_CHECK_COUNT:` non-negative integer; exactly `18` for a complete FINAL response
- `FINAL_MISSING_CHECK_COUNT:` non-negative integer
- `FINAL_FAILED_CHECK_COUNT:` non-negative integer
- `PRIOR_GATE_CHECK:` `NOT_RUN|PASS|FAIL`
- `PREPUBLICATION_STATE_CHECK:` `NOT_RUN|PASS|FAIL`
- `FINAL_GATE:` `PENDING|READY|CONDITIONAL|BLOCKED`
- `PRIOR_SPEC_STATE:` `ABSENT|PRESENT`
- `PRIOR_SPEC_CHECKPOINT_STATE:` `NOT_REQUIRED|VERIFIED|RESTORED|REMOVED|FAIL`
- `PRIOR_SPEC_CHECKPOINT_EQUALITY_CHECK:` `NOT_RUN|PASS|FAIL`
- `SPEC_WRITE_ALLOWED:` `NO|YES`
- `POSTPUBLICATION_STATE_CHECK:` `NOT_RUN|PASS|FAIL`
- `POSTPUBLICATION_GATE:` `PENDING|PASS|FAIL`
- `SUCCESS_REPORT_ALLOWED:` `NO|YES`

All counts come from complete current-run Markdown records and physical files. Never estimate, invert, or infer them from a range or summary. `OBL-001..OBL-018` is not an obligation register. “Five uncovered” is not “five covered.” Any missing, stale, contradictory, non-numeric, or unverified invariant field makes every dependent guard fail.

On the first `REVISE` for a signature, record it and set `SAME_DEFECT_REVISION_COUNT: 0`. After each targeted correction and same-mode re-review, increment the count only when the identical normalized signature survives with no relevant changed evidence or state. If the signature differs, record the new signature and reset the count to `0`. If relevant evidence or state changed, reset the count to `0` even when the normalized signature is identical. Reset both fields to `NONE` and `0` when the phase is accepted, blocked for another verified reason, or changes. A host Autopilot continuation or stop never increments, resets, or otherwise changes this Critic-specific counter. At count `3`, independently verify and record `BLOCKED: NON_CONVERGING`.

Count unique IDs, not mentions. Count a KQ as accepted only when an item-specific `PRODUCTION` Critic response admits that exact `KQ-*` and every applicable pair check passes. `UNACCEPTED_KQ_COUNT` is the number of unique planned KQ IDs not yet admitted, so `PLANNED_KQ_COUNT == ACCEPTED_KQ_COUNT + UNACCEPTED_KQ_COUNT`. `DRAFT_KQ_COUNT` is the number of admitted `### KQ-NNN` blocks physically present under `## Specification Knowledge Base` in the current draft.

Count an obligation as covered only when its complete required current-run content exists; planned, partial, unresolved, deferred, or merely named content is not covered. `ACCOUNTED_MANDATORY_OBLIGATION_COUNT` counts unique mandatory IDs that have either complete accepted coverage or an explicit unresolved record. Covered and unresolved mandatory-ID sets are disjoint, `ACCOUNTED_MANDATORY_OBLIGATION_COUNT == COVERED_MANDATORY_OBLIGATION_COUNT + UNRESOLVED_MANDATORY_OBLIGATION_COUNT`, covered IDs are a subset of accounted IDs, accounted IDs are a subset of mandatory IDs, and every count is bounded by `MANDATORY_OBLIGATION_COUNT`. Treat an unresolved item with no explicit materiality classification as material.

`UNMAPPED_MANDATORY_OBLIGATION_COUNT` counts mandatory IDs not yet assigned by the post-resolution coverage plan to a planned KQ, attached source-required field, framework/cross-unit section, traceability, or explicit unresolved decision. It may be nonzero before planning and is not a baseline defect; it must be zero before production.

### Guarded run initialization, source identity, and scope

`run`, `start`, `go`, `generate`, and `build` always initialize NEW. `resume`, `continue`, and `proceed` use RESUME only after current files exactly satisfy the stored run identity checks; otherwise initialize NEW and record the reason. Never append a NEW run to old research.

NEW initialization is one uninterrupted research-first transaction: preserve human/framework/unknown/current `spec.md`; delete/verify managed `.spec-workflow/**` and `outgoing/**`; reconcile the checkpoint; FIRST create/reread `.spec-workflow/research.md` with one RUN_ID, ordered pending invariant, reset, goal/scope, and baseline placeholder; SECOND create/verify the snapshot; THIRD update/reread checks; FOURTH persist/reread `GUARD_CALL_BASELINE`. No worker or chat return between steps. Orphan/partial state is uncommitted: repair or clean/retry NEW. Single-run pass requires one RUN_ID/no prior records; reset pass requires no stale artifact/draft.

Write identity only at `.spec-workflow/source-snapshot.md`; a root/basename-only/duplicate current-run copy fails `MANAGED_ARTIFACT_PATH_CHECK`. Read/write/reread each non-reserved candidate in bounded chunks through editor-reported EOF. Snapshot: RUN_ID, sorted candidate paths, and one `### SOURCE path` block per candidate; pattern exclusions and included count stay in research. Per block, `LINE_COUNT` equals live line count, contiguous-row count, and highest ID (`L000001`…`L{LINE_COUNT}`); every row text matches live text. Candidate count equals path/block count. Partial reads, gaps, summaries, invented counts/timestamps/hashes, or mismatches fail `SOURCE_SNAPSHOT_CHECK` and require repair before baseline. Any source change makes RESUME become NEW.

Select one goal by semantic authority: explicit current user scope; otherwise a source declaring the current task/ticket goal and output; otherwise the only coherent goal-bearing source. Independent equal goals cause `SOURCE_SCOPE_CONFLICT`. A lower-authority independent project is normally `UNRELATED_GOAL`. For every user-designated input, classify sections separately: admit content relevant or adaptable to the selected goal as `IN_SCOPE_SOURCE_MATERIAL`, and exclude incompatible embedded output directives rather than merging goals or discarding the entire input. `SOURCE_SCOPE_CHECK: PASS` requires one goal, at least one included source/section, semantic reasons for every candidate and partial exclusion, and no wholesale exclusion when a designated input has usable sections. `EVIDENCE_MEMBERSHIP_CHECK: PASS` requires every obligation, fact, location, and synthesis bound to use only admitted snapshot lines; excluded lines never corroborate a claim.

Recompute source snapshot, scope, membership, and single-run checks immediately before every guarded action below; stored earlier `PASS` values are insufficient after a file change. A failed freshness check forbids correction against the old ledger and initializes NEW instead.

### Complete durable baseline requirement

The accepted baseline must be fully persisted in one canonical `## Current Baseline` section in `research.md`: active goal and anchor; candidate scope table; included sources with verified ranges; snapshot/membership status; three-layer Artifact Contract; atomic obligations; source-explicit values, structure, and constraints; cross-unit rules; same-goal conflicts; gaps; and the exact Critic decision. Any second baseline, RUN_ID, stale path, or excluded-source claim fails. On `REVISE`, replace this section instead of appending.

Do not require or persist a derived field schema, planned KQs, published questions, answers, newly synthesized/selected downstream values, applied defaults, IRQ resolutions, or production coverage in baseline. Preserve literal source-supplied IDs, names, quantities, dates, URLs, paths, formulas, and exact content. A source that only names content areas needs a compact completion contract, not invented types, fields, examples, or cardinalities. Use `SOURCE_UNSPECIFIED` when no count or named set is source-supported.

Never write “full baseline archived in the session resource store,” use an opaque obligation-ID range, or persist only a summary. Memory not present in the allowed Markdown artifacts is unavailable and cannot authorize a gate.

For a `BASELINE` Critic result:

- `ACCEPT`: set `BASELINE_DECISION: ACCEPT` and `BASELINE_GATE: ACCEPT` only after the complete baseline passes independent verification.
- `REVISE`: set `BASELINE_DECISION: REVISE`, keep `BASELINE_GATE: PENDING`, revise only baseline records, and invoke `BASELINE` review again immediately within the active response without asking the user to approve the verdict. A Default Approvals prompt can apply only to an actual tool operation; an explicitly reported Advanced Autopilot boundary preserves the pending gate for `resume`.
- `BLOCKED`: set both baseline fields to `BLOCKED` and stop.

Corrective actions, derived defaults, or useful prose returned with `REVISE` are not accepted baseline content. While `BASELINE_GATE` is `PENDING`, resolution, coverage planning, production, draft creation, FINAL review, and publication are forbidden.

### Exact action guards

Before a guarded action, write an `ACTION_AUTHORIZATION` record to `research.md` containing the action name, every field/value tested, `GUARD_RESULT: PASS|FAIL`, and reason. Reread that record from disk. Perform the action only for literal `PASS`. Prose such as “ready to review,” “candidate created,” or “corrective actions applied” never satisfies a guard.

Every guard additionally requires `RUN_INITIALIZATION_GATE`, `GENERATED_ARTIFACT_RESET_CHECK`, `MANAGED_ARTIFACT_PATH_CHECK`, `SOURCE_SCOPE_CHECK`, `SOURCE_SNAPSHOT_CHECK`, `EVIDENCE_MEMBERSHIP_CHECK`, and `SINGLE_RUN_STATE_CHECK` all equal `PASS` after immediate recomputation.

#### `GUARD_CALL_BASELINE`

Pass only when exact research/snapshot exist with matching RUN_ID and no misplaced duplicate; research has one invariant, baseline placeholder, goal/anchor; all snapshot path/block/count/row/EOF/text equations pass; every candidate is classified; included count is positive; reset/prior-spec/draft state matches disk; and universal checks pass. Persist/reread authorization before `BASELINE`. Otherwise record `GUARD_RESULT: FAIL`, `ALLOWED_NEXT_ACTION: REPAIR_RUN_INITIALIZATION`, and repair locally without a proceed question.

#### `GUARD_MARK_RESOLUTION_NOT_REQUIRED`

Pass only when:

- `BASELINE_DECISION == ACCEPT`
- `BASELINE_GATE == ACCEPT`
- `GENUINE_IRQ_COUNT == 0`
- `OPEN_MATERIAL_IRQ_COUNT == 0`
- `RESOLUTION_GATE == PENDING`
- `RESOLUTION_NOT_REQUIRED_REASON` is concrete and is not `NOT_APPLICABLE`

Only after this guard passes set `RESOLUTION_GATE: NOT_REQUIRED`. Do not invoke resolution workers on this path.

#### `GUARD_START_RESOLUTION`

Pass only when:

- `BASELINE_DECISION == ACCEPT`
- `BASELINE_GATE == ACCEPT`
- `GENUINE_IRQ_COUNT >= 1`
- `OPEN_MATERIAL_IRQ_COUNT >= 1`
- `RESOLUTION_GATE == PENDING`

When `GENUINE_IRQ_COUNT == 0`, use `GUARD_MARK_RESOLUTION_NOT_REQUIRED` instead.

#### `GUARD_ACCEPT_RESOLUTION`

Pass only when:

- `BASELINE_GATE == ACCEPT`
- `GENUINE_IRQ_COUNT >= 1`
- `OPEN_MATERIAL_IRQ_COUNT == 0`
- `RESOLUTION_GATE == PENDING`
- every genuine `IRQ-*` has an item-specific accepted Critic resolution record
- no resolution defect remains open

Only after this guard passes set `RESOLUTION_GATE: ACCEPT`.

#### `GUARD_START_PRODUCTION`

Pass only when:

- `BASELINE_GATE == ACCEPT`
- `RESOLUTION_GATE` is one of `{ACCEPT, NOT_REQUIRED}`
- `OPEN_MATERIAL_IRQ_COUNT == 0`
- `PLANNED_KQ_COUNT >= 1`
- every required `UNIT-*` maps to one planned `KQ-*`
- `UNMAPPED_MANDATORY_OBLIGATION_COUNT == 0`

#### `GUARD_ACCEPT_PRODUCTION`

Pass only when:

- every `GUARD_START_PRODUCTION` condition remains true
- `PRODUCTION_GATE == PENDING`
- `PLANNED_KQ_COUNT >= 1`
- `ACCEPTED_KQ_COUNT == PLANNED_KQ_COUNT`
- `UNACCEPTED_KQ_COUNT == 0`
- `ACCEPTED_UNIT_COUNT == REQUIRED_UNIT_COUNT`
- every planned pair has item-specific accepted Critic decisions and every applicable production check passes

Only after this guard passes set `PRODUCTION_GATE: ACCEPT`.

#### `GUARD_CREATE_DRAFT`

Pass only when:

- `BASELINE_GATE == ACCEPT`
- `RESOLUTION_GATE` is one of `{ACCEPT, NOT_REQUIRED}`
- `PRODUCTION_GATE == ACCEPT`
- `PLANNED_KQ_COUNT >= 1`
- `ACCEPTED_KQ_COUNT == PLANNED_KQ_COUNT`
- `UNACCEPTED_KQ_COUNT == 0`
- `ACCEPTED_UNIT_COUNT == REQUIRED_UNIT_COUNT`
- every admitted pair has all required production decisions and checks passing

#### `GUARD_ACCEPT_DRAFT`

After writing the candidate, reread the physical file and pass only when:

- `DRAFT_KQ_COUNT == ACCEPTED_KQ_COUNT`
- `DRAFT_KQ_COUNT >= 1`
- `DRAFT_ENVELOPE_FAILURE_COUNT == 0`
- `ACCOUNTED_MANDATORY_OBLIGATION_COUNT == MANDATORY_OBLIGATION_COUNT`
- `MANIFEST_UNMAPPED_BLOCK_COUNT == 0`
- `## Specification Knowledge Base` exists
- every admitted KQ has a heading, literal `Question:`, literal `Answer:`, source basis with obligation labels and verified locations, acceptance check, and every source-required field

Set `DRAFT_GATE: ACCEPT` only after this guard passes. Direct downstream prose outside the Q&A envelope cannot pass.

#### `GUARD_CALL_FINAL`

Pass only when:

- `BASELINE_GATE == ACCEPT`
- `RESOLUTION_GATE` is one of `{ACCEPT, NOT_REQUIRED}`
- `PRODUCTION_GATE == ACCEPT`
- `DRAFT_GATE == ACCEPT`
- `ACCEPTED_KQ_COUNT >= 1`
- `DRAFT_KQ_COUNT == ACCEPTED_KQ_COUNT`
- `DRAFT_ENVELOPE_FAILURE_COUNT == 0`
- `ACCOUNTED_MANDATORY_OBLIGATION_COUNT == MANDATORY_OBLIGATION_COUNT`
- `MANIFEST_UNMAPPED_BLOCK_COUNT == 0`
- `FINAL_GATE == PENDING`

Do not call FINAL while an earlier gate is missing, `PENDING`, `REVISE`, or `BLOCKED`.

#### `GUARD_ACCEPT_FINAL_READY`

Pass only when:

- the latest current-run `ACTION_AUTHORIZATION` for `GUARD_CALL_FINAL` has `GUARD_RESULT == PASS`
- every `GUARD_CALL_FINAL` fact except its pre-review `FINAL_GATE == PENDING` condition remains true
- the Critic response field `REVIEW_MODE == FINAL`
- `FINAL_GATE == PENDING`
- `FINAL_STATUS == NOT_RUN`
- `PROPOSED_FINAL_STATUS == READY`
- the Critic returned all 18 named FINAL checks plus `FINAL_STATUS`
- `FINAL_REQUIRED_CHECK_COUNT == 18`
- `FINAL_PRESENT_CHECK_COUNT == 18`
- `FINAL_MISSING_CHECK_COUNT == 0`
- `FINAL_FAILED_CHECK_COUNT == 0`
- `FINAL_REQUIRED_CHECK_COUNT - FINAL_PRESENT_CHECK_COUNT == FINAL_MISSING_CHECK_COUNT`
- every named FINAL check is `PASS`
- `PRIOR_GATE_CHECK == PASS`
- `PREPUBLICATION_STATE_CHECK == PASS`
- `COVERED_MANDATORY_OBLIGATION_COUNT == MANDATORY_OBLIGATION_COUNT`
- `UNRESOLVED_MANDATORY_OBLIGATION_COUNT == 0`
- `MATERIAL_OR_UNCLASSIFIED_UNRESOLVED_COUNT == 0`
- `ACCEPTED_KQ_COUNT >= 1`

Only after this guard passes atomically record `FINAL_STATUS: READY` and `FINAL_GATE: PENDING -> READY`.

#### `GUARD_RECORD_FINAL_NONREADY`

Use this guard instead of `GUARD_ACCEPT_FINAL_READY` when `PROPOSED_FINAL_STATUS` is `CONDITIONAL` or `BLOCKED`. Pass only when:

- the latest current-run `ACTION_AUTHORIZATION` for `GUARD_CALL_FINAL` has `GUARD_RESULT == PASS`
- the Critic response field `REVIEW_MODE == FINAL`
- `FINAL_GATE == PENDING`
- `FINAL_STATUS == NOT_RUN`
- `PROPOSED_FINAL_STATUS` is one of `{CONDITIONAL, BLOCKED}`
- all 18 named FINAL check fields are present
- required, present, missing, and failed counts reconcile with the physical response
- the proposed non-ready status is consistent with the failed checks, unresolved material, blockers, affected IDs, and precise defects

Only after this guard passes atomically copy the proposed value into `FINAL_STATUS` and `FINAL_GATE`, keep `SPEC_WRITE_ALLOWED: NO`, and stop without publication. Do not evaluate the READY-only guard for a non-ready proposal. If neither final-result guard passes, reject the malformed worker response and stop as `BLOCKED` without publishing.

#### `GUARD_WRITE_SPEC`

Pass only when:

- the latest current-run `ACTION_AUTHORIZATION` for `GUARD_ACCEPT_FINAL_READY` has `GUARD_RESULT == PASS`
- every `GUARD_CALL_FINAL` fact except its pre-review `FINAL_GATE == PENDING` condition remains true
- every still-applicable `GUARD_ACCEPT_FINAL_READY` condition remains true after the authoritative status transition
- `FINAL_STATUS == READY`
- `FINAL_GATE == READY`
- `SPEC_WRITE_ALLOWED == NO` before authorization
- when `PRIOR_SPEC_STATE == PRESENT`: `PRIOR_SPEC_CHECKPOINT_STATE == VERIFIED`, `PRIOR_SPEC_CHECKPOINT_EQUALITY_CHECK == PASS`, and the current physical `spec.md` still exactly matches `.spec-workflow/prior-spec.md`
- when `PRIOR_SPEC_STATE == ABSENT`: `PRIOR_SPEC_CHECKPOINT_STATE == NOT_REQUIRED`, `PRIOR_SPEC_CHECKPOINT_EQUALITY_CHECK == NOT_RUN`, and physical `spec.md` is still absent
- the physical `draft.md` and publication manifest exactly match the versions reviewed by the accepted FINAL response
- no source, draft, manifest, accepted production record, or substantive research evidence changed after FINAL review; only the FINAL decision and action-authorization records may have changed

Set `SPEC_WRITE_ALLOWED: YES` only after every expression passes. `PREPUBLICATION_STATE_CHECK: PASS` by itself satisfies only one expression. A result such as `READY` with zero KQs, incomplete coverage, a blocked item, missing checks, or unresolved material is contradictory and must be rejected.

Any substantive mutation after FINAL acceptance makes this guard fail and blocks publication for the current run. Do not reset a monotonic READY gate or silently re-review changed content; begin a new run.

#### `GUARD_CALL_POSTPUBLICATION`

Pass only when:

- the latest current-run `ACTION_AUTHORIZATION` record for `GUARD_WRITE_SPEC` has `GUARD_RESULT == PASS`
- `SPEC_WRITE_ALLOWED == YES`
- `POSTPUBLICATION_GATE == PENDING`
- the new physical `spec.md` exists
- the new physical `spec.md` exactly matches the accepted draft before substantive repeat checks
- when `PRIOR_SPEC_STATE == PRESENT`, the verified rollback checkpoint still exists unchanged

#### `GUARD_REPORT_SUCCESS`

Pass only when:

- `POSTPUBLICATION_STATE_CHECK == PASS`
- `POSTPUBLICATION_GATE == PASS`
- the current-run Critic response field `REVIEW_MODE == POSTPUBLICATION`
- every required itemized postpublication equality and repeated-check field is present and `PASS`
- the accepted current-run postpublication record proves exact draft/file equality and every repeated substantive check passes
- when `PRIOR_SPEC_STATE == PRESENT`, the verified checkpoint was removed only after postpublication passed and `PRIOR_SPEC_CHECKPOINT_STATE == REMOVED`
- when `PRIOR_SPEC_STATE == ABSENT`, `PRIOR_SPEC_CHECKPOINT_STATE == NOT_REQUIRED`

Set `SUCCESS_REPORT_ALLOWED: YES` only after this guard passes. Then report success.

### Fail-closed behavior

If any guard fails:

- do not invoke the guarded later worker or create its artifact
- when an earlier gate is `PENDING` or its Critic result is `REVISE`, return to that earliest phase and run its targeted correction loop in the active response without asking the user to approve the Critic verdict; Default Approvals may pause an actual tool call, while an explicitly reported Advanced Autopilot boundary preserves active state for `resume`
- do not evaluate downstream guards while the earlier phase remains correctable
- missing/misplaced/incomplete initialization is locally correctable: repair and reevaluate `GUARD_CALL_BASELINE` without asking the user to update, approve, proceed, or rerun; only repeated concrete edit/path failure may end `BLOCKED: RUN_INITIALIZATION_FAILED`
- set `BLOCKED` and report a stopped phase only when no legal in-scope correction can satisfy the failed expressions or a precise reviewed `HUMAN` answer is genuinely required
- preserve or restore the prior published-file state for every terminal failure

Never repair a failed guard by changing a source operator, guessing a path or line, converting uncovered obligations into covered counts, inventing an accepted KQ, hiding unresolved material, or weakening a gate.
