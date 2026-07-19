# 06 — Critic and Convergence

The Text Spec Critic is the admission boundary for the Markdown-only specification workflow. It does not author `spec.md`, repair files, or invent missing requirements. Its job is to decide whether evidence, internal resolutions, produced knowledge entries, the candidate draft, and the published file are sufficiently complete and consistent to advance.

This separation matters because a plausible-looking document is not the same thing as an accepted specification. The workflow converges by repeatedly exposing a precise defect, revising only the affected material, and asking the Critic to evaluate it again.

## What the Critic reviews

The Critic participates in five review contexts.

| Review context | Input under review | Successful result | Why it exists |
|---|---|---|---|
| Baseline | Exact research and snapshot artifacts, persisted `GUARD_CALL_BASELINE: PASS`, source inventory, Artifact Contract, atomic obligations, source-explicit structure and constraints, conflicts, and candidate gaps | `BASELINE_DECISION: ACCEPT` | Prevents later planning from starting with invalid initialization or a false scope while keeping production content out of baseline. |
| Internal resolution | One or more accepted-candidate `IRQ-*` resolutions | `ACCEPT` for each resolution | Ensures a real ambiguity was resolved without overwriting source facts. |
| Production | A proposed `KQ-*` and its associated `UNIT-*`, when applicable | Both applicable decisions are `ACCEPT`, and all completeness checks pass | Prevents placeholders, partial scales, missing children, or generic questions from entering the draft. |
| Final | Complete `.spec-workflow/draft.md`, all active sources, current-run `.spec-workflow/research.md`, and the publication manifest | `FINAL_STATUS: READY` with every named check `PASS` | Validates the whole candidate, including cross-unit and state consistency. |
| Postpublication | The newly written `spec.md`, accepted draft, and current-run research | `POSTPUBLICATION_STATE_CHECK: PASS` | Proves that the file on disk is exactly the accepted result and still passes the substantive checks. |

Before semantic `BASELINE` review, the Critic requires exact `.spec-workflow/research.md` and `.spec-workflow/source-snapshot.md`, matching RUN_ID, a physically persisted `GUARD_CALL_BASELINE: PASS`, complete snapshot structure/equality, one goal, evidence membership, and single-run state. If anything is absent, misplaced, truncated, or malformed, it returns `WORKER_RESULT_REJECTED: INITIALIZATION_NOT_READY` with `ALLOWED_NEXT_ACTION: REPAIR_RUN_INITIALIZATION`. The Critic remains read-only; the Orchestrator repairs locally and reevaluates the guard without asking the user to rerun baseline.

The modes are deliberately cumulative. A production pair can be valid in isolation but still fail final review because its duration conflicts with other units, its source mapping is incomplete, or the assembled draft contains workflow-only material.

The only valid mode names are `BASELINE`, `RESOLUTION`, `PRODUCTION`, `FINAL`, and `POSTPUBLICATION`. `PREPUBLICATION` is not a sixth mode. `PREPUBLICATION_STATE_CHECK` is one named field inside the complete `FINAL` response.

In `FINAL`, the Critic returns all 18 named checks plus audit counts. `READY` is an exact conjunction: all checks are present and pass, earlier gates pass, at least one accepted KQ exists in the physical draft, mandatory coverage is complete, and unresolved mandatory or unclassified material counts are zero. A friendly overall label cannot override contradictory audit facts.

## How decisions work

### `ACCEPT`

`ACCEPT` means the reviewed item satisfies the applicable contract at that review boundary. It is evidence for advancing the current phase; it is not a blanket approval of later work.

Examples:

- A baseline is accepted after the complete source inventory, three-layer Artifact Contract, atomic obligation register, exact locations, and source-explicit structure and constraints are coherent. It is expected to have zero planned or accepted KQs and zero production coverage.
- An `IRQ-*` resolution is accepted only when it resolves a genuine gap, preserves source constraints, cites verified evidence, and remains workflow-only.
- A production pair is admitted only when `KQ_DECISION` and every applicable `UNIT_DECISION` are `ACCEPT`, and the field, placeholder, enumeration, and scale checks pass.

### `REVISE`

`REVISE` is a precise request for correction. It is never equivalent to acceptance and never permits the next dependent phase to begin.

The Orchestrator keeps the phase open, applies a targeted correction, and invokes the same Critic mode again automatically while VS Code permits execution. A baseline marked `REVISE` cannot support resolution, planning, or production, but it also cannot produce a voluntary “pick one” user handoff while the defect is internally correctable. A host-enforced Autopilot stop preserves the pending phase for `resume`; a question package marked `REVISE` cannot be copied into `.spec-workflow/draft.md`.

A good revision report identifies:

- the affected `OBL-*`, `IRQ-*`, `KQ-*`, or `UNIT-*`;
- the exact defect;
- the required correction;
- the allowed next action; and
- confidence in the decision.

For example, “scoring anchors incomplete” is less useful than “KQ-004 is missing levels 2 and 4; all five levels must have distinct, observable, question-specific descriptions.”

### `BLOCKED`

`BLOCKED` means the workflow cannot safely or usefully advance from the current evidence. Every dependent phase stops.

Typical causes include:

- no eligible source identifies a goal;
- conflicting goals would materially change the requested artifact and cannot be isolated;
- a material decision cannot be resolved from evidence, grounded synthesis, a safe default, or an explicit human answer;
- required gate evidence is missing;
- a draft or manifest required for final review does not exist; or
- required content remains unsafe or unusable.

`BLOCKED` is not a synonym for “difficult.” The Interrogator is specifically instructed not to manufacture blockers from optional polish or unrelated possibilities.

## The convergence loop

Internal convergence is controlled by gates and defect reduction, not by a fixed total number of retries. It still runs inside VS Code's separately bounded outer Autopilot loop.

1. Complete and verify the uninterrupted research-first initialization transaction, then establish the extraction-only baseline contract.
2. Ask the Critic for `BASELINE_DECISION`.
3. If the result is `REVISE`, correct the cited baseline defect and review again.
4. Resolve only genuine `IRQ-*` gaps.
5. Build the adaptive production contract and map KQ topics without writing their final questions or answers.
6. Produce `KQ-*` and `UNIT-*` work in batches of at most five.
7. Review each pair and its completeness checks.
8. Reduce the batch size if full content cannot be produced without abbreviation. The Investigator may return `BATCH_TOO_LARGE`; placeholders are never an acceptable way to preserve batch size.
9. Assemble a new blank draft only from accepted material.
10. Run all final checks across the complete candidate.
11. Publish only after `FINAL_STATUS: READY`.
12. Validate the written file again before reporting success.

There is no “accept after N attempts” shortcut. Repeated revision is healthy only while each pass closes a named defect. Three consecutive targeted attempts with the same defect signature and no changed evidence are treated as non-convergence and stop as `BLOCKED`; they never become acceptance and never produce a continue/stop menu.

## Two different limits that both use the number three

They must not be confused:

- **VS Code 1.124 Advanced Autopilot:** only when the current permission level is Autopilot and `chat.autopilot.advanced.enabled` is `true`, at most three automatic host continuations occur before the host stops. Default Approvals, Bypass Approvals, and ordinary Autopilot do not use this maximum.
- **Framework non-convergence:** three completed targeted corrections after which the same normalized Critic defect survives without changed evidence or state. This is verified as `BLOCKED: NON_CONVERGING`.

A Critic result is not a VS Code permission decision. Under Default Approvals, `REVISE` starts the framework correction loop without asking the user to approve that verdict; only a tool operation may need security confirmation. The release notes do not equate a host continuation with a Critic call. The framework persists `CURRENT_REVISION_DEFECT_SIGNATURE` and `SAME_DEFECT_REVISION_COUNT`; host continuations never increment them. When the optional Advanced limit is reached, accepted state remains authoritative and `resume` continues the earliest incomplete gate. See [VS Code permission levels](https://code.visualstudio.com/docs/agents/approvals#_permission-levels) and [VS Code 1.124: Advanced Autopilot](https://code.visualstudio.com/updates/v1_124#_advanced-autopilot).

## Why per-item and final review are both required

Per-item review catches local defects:

- missing `Question:` or `Answer:` fields;
- incorrect question role;
- an ellipsis in conditional follow-ups;
- a list compressed into one prose sentence;
- a five-level scale containing only levels 1, 3, and 5; or
- a source location without a readable obligation label.

Final review catches system defects:

- all units are individually valid, but their time estimates exceed the total duration;
- every competency appears somewhere, but one appears only in an optional follow-up;
- a valid target-facing question is wrapped inside a workflow transcript;
- the manifest omits a draft block;
- the draft claims readiness while the Phase Gate Ledger still says `REVISE`; or
- a source-required constraint is applied inconsistently across variants.

## Failure modes

### Critic review is requested before initialization is ready

The Critic must reject the worker call as `INITIALIZATION_NOT_READY`; it must not review or accept the semantic baseline. The Orchestrator repairs the exact managed artifacts and reruns the guard locally before making another baseline call.

### Treating `REVISE` as “good enough”

This bypasses the admission boundary. The Phase Gate Ledger must remain pending until a later Critic result accepts the corrected material.

### Publishing before final review

An outline, schema, baseline, or partial production batch is not a candidate draft. The Orchestrator explicitly forbids writing `spec.md` from any of them.

### Deferring checks

Text such as “run stricter validation later” is a failure. Required reviews are non-optional and must complete before publication.

### Correcting one unit by weakening the contract

The contract comes from active source evidence. It cannot be relaxed merely because a produced entry is incomplete. A missing scale level requires completing the scale, not redefining the source-required scale.

### Confusing `CONDITIONAL` with publishable readiness

`CONDITIONAL` records useful, traced content with isolated unresolved decisions. The implemented publication rule still requires `FINAL_GATE: READY` before `spec.md` is written.

### Accepting a contradictory `READY`

A response such as `READY (PREPUBLICATION_STATE_CHECK: PASS)` with zero KQs, partial coverage, unresolved material, or missing earlier gates is malformed. The Orchestrator rejects the worker result and the write guard fails. The similarly named state check proves only one comparison; it is never the overall status.

## Review checklist

- [ ] Did the Critic receive the complete evidence required for this review context?
- [ ] Before baseline review, did exact research and snapshot artifacts plus persisted `GUARD_CALL_BASELINE: PASS` prove complete initialization?
- [ ] Is every decision recorded against a specific ID or check?
- [ ] Is `REVISE` keeping the current gate open?
- [ ] Did `BLOCKED` stop dependent work?
- [ ] Are production batches small enough to complete without abbreviation?
- [ ] Are all applicable pair decisions `ACCEPT`?
- [ ] Are all required production checks `PASS`?
- [ ] Was the candidate assembled only after production acceptance?
- [ ] Are all 18 final checks present and `PASS`, with zero missing and failed counts?
- [ ] Do accepted KQ, physical draft, coverage, and unresolved counts independently permit `READY`?
- [ ] Was the published file revalidated before the final chat response?

## Related reading

- [07 — Observability](07_Observability.md)
- [08 — Consistency Checks](08_Consistency_Checks.md)
- [09 — Run Health](09_Run_Health.md)
- [10 — Debugging](10_Debugging.md)
- [Text Spec Critic](../.github/agents/text-spec-critic.agent.md)
- [Text Spec Orchestrator](../.github/agents/text-spec-orchestrator.agent.md)
