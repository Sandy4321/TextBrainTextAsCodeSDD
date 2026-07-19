# Visible Workflow Reference

## Purpose

This file presents the complete agent loop in the order a learner should be able to observe it in VS Code and in the Markdown artifacts.

The visible workflow is not merely a suggested sequence. Its gates prevent plausible-looking partial work from being published.

## Participants

| Participant | Visible responsibility |
|---|---|
| User | Supplies source files, selects the Orchestrator, gives explicit answers only when genuinely needed |
| Orchestrator | Owns state, delegates work, writes allowed artifacts, and enforces gate order |
| Investigator | Returns evidence-backed analysis or proposed content |
| Interrogator | Returns only genuine internal gap questions |
| Critic | Returns read-only admission decisions and defects |

## Complete loop

### 0. User starts the custom agent

The user selects **Text Spec Orchestrator** in Copilot Chat and enters `run` (or an equivalent start word).

Expected visible behavior:

- source discovery and managed cleanup begin immediately
- stale agent-owned files in `.spec-workflow/**` and `outgoing/**` are deleted and the empty state is verified before new control memory is written
- the agent does not search for runnable code
- the agent does not ask “what should I run?” when one coherent goal is present
- `.spec-workflow/research.md` is created and reread first with a fresh identity, pending gate ledger, reset/goal/scope records, and authoritative Run Invariant Block
- `.spec-workflow/source-snapshot.md` is created and verified second, and `GUARD_CALL_BASELINE` is persisted and reread before any worker call
- the current execution segment and host-permitted automatic continuations progress through internal `REVISE` loops and later phases; no voluntary “pick one” or “stop and review” menu appears for correctable work
- only when the permission level is Autopilot and `chat.autopilot.advanced.enabled` is `true`, VS Code may enforce the three-loop maximum; Default Approvals has no such counter

### 1. Orchestrator commits initialization, selects the goal, and inventories evidence

The Orchestrator inventories every readable non-reserved textual candidate, selects one active goal by semantic authority, classifies candidate files and sections against that goal, and persists those goal/scope records in the research-first bootstrap. It then writes `.spec-workflow/source-snapshot.md` in bounded chunks through live EOF and updates the verified snapshot/scope checks. Candidate count must equal path count and source-block count; every block's `LINE_COUNT`, live line count, contiguous row count, and highest row ID must agree, with exact row text. It reads admitted sections completely. It never selects a goal from filename or recency and never treats generated state as evidence.

An orphaned, root-level, duplicate, truncated, or malformed snapshot is uncommitted state. The Orchestrator repairs or cleanly retries NEW before calling a worker, without asking the user to create research, rerun baseline, or tell it to proceed.

Visible research records:

- included path and reason
- excluded path and reason
- active goal anchor and authority
- section-level `IN_SCOPE_SOURCE_MATERIAL` or incompatible embedded-directive exclusions for mixed-purpose inputs
- initialization, reset, managed-path, source-scope, snapshot, membership, and single-run checks
- no use of prior `spec.md` as current evidence
- documentation, agent files, and linked agent contracts excluded

### 2. Investigator proposes the baseline

Only after exact research and snapshot artifacts, matching RUN_ID, all snapshot equations, and a persisted `GUARD_CALL_BASELINE: PASS` are reread does the Orchestrator invoke **Text Spec Investigator** in `BASELINE` mode with the goal, inventory, and exclusions. If initialization is not ready, the Investigator refuses with `WORKER_RESULT_REJECTED: INITIALIZATION_NOT_READY` and the Orchestrator repairs locally.

The proposal includes:

- inferred goal
- three-layer Artifact Contract
- source terminology and anchors
- atomic grounded obligation register
- source-explicit content areas, repeated units, fields, order, cardinality, and constraints
- source-stated or directly calculable cross-unit dependencies
- conflicts, dependencies, and candidate gaps

The baseline contains no planned KQs, published questions or answers, newly selected or synthesized downstream values, applied defaults, assigned or resolved IRQs, production coverage, or draft. It does preserve exact literals already supplied by admitted sources as `SOURCE_EXPLICIT_VALUE`. Zero KQs and zero covered obligations are expected here.

### 3. Critic reviews the baseline

The Orchestrator passes the complete baseline and sources to **Text Spec Critic** in `BASELINE` mode.

Possible visible outcomes:

- `ACCEPT`: persist the baseline and set `BASELINE_GATE: ACCEPT`
- `REVISE`: repair only the named defects, then invoke the baseline Critic again automatically in the active response; this verdict requires no user approval
- `BLOCKED`: set the gate to blocked and stop dependent work

The workflow may not proceed from a baseline still marked `REVISE`.

### 4. Orchestrator decides whether resolution is needed

If no genuine material gap remains, record:

> `RESOLUTION_GATE: NOT_REQUIRED` with the reason that no valid IRQ exists.

If material gaps remain, the Orchestrator invokes **Text Spec Interrogator**.

The Interrogator creates at most the required internal `IRQ-*` proposals. It does not generate published questions.

### 5. Investigator resolves an IRQ batch

The Orchestrator sends at most five IRQs, relevant evidence, contracts, and prior resolutions to the Investigator in `RESOLVE_BATCH` mode.

Resolution routes are:

- `EVIDENCE`
- `GROUNDED_SYNTHESIS`
- `SAFE_DEFAULT`
- `HUMAN`

The Investigator answers each internal query separately and classifies the knowledge honestly.

### 6. Critic reviews each resolution

For every IRQ, the Critic returns one of:

- `ACCEPT`
- `REVISE_QUERY`
- `REVISE_RESOLUTION`
- `BLOCKED`

Only accepted resolutions enter the durable research state. Human questions are asked only for records routed to `HUMAN` after review.

The resolution loop repeats until every IRQ resolution is accepted or blocked. An accepted resolution may preserve a content item as visibly unresolved for later conditional evaluation, but that is a content classification—not `RESOLUTION_GATE: CONDITIONAL`, which is not a valid transition. The Orchestrator records `RESOLUTION_GATE: ACCEPT` only for reviewed admissible resolutions, or stops as blocked.

### 7. Orchestrator plans knowledge coverage

The Orchestrator maps each mandatory obligation to one or more planned `KQ-*` entries, attached fields, framework or cross-unit sections, traceability, or explicit unresolved decisions. Separately, every required `UNIT-*` must map to a planned `KQ-*`; the other obligation destinations cannot replace that unit-to-KQ mapping.

This is a coverage plan, not published content. It also builds the adaptive production contract at only the depth supported by the source. Planned records contain topics and mappings, never final `Question:`, `Answer:`, quantities, or coverage credit.

### 8. Investigator produces a batch

The Orchestrator invokes the Investigator in `PRODUCE_BATCH` mode for at most five planned entries.

The worker receives:

- active sources and goal
- all artifact layers
- adaptive source-native unit contract, order, cardinality, and constraints
- only the source-required fields and expected-member ledgers applicable to that batch
- relevant accepted resolutions
- mapped obligations with labels and verified locations
- compact index of prior accepted entries
- required question role and answer semantics

The Investigator returns one item-separated `KQ-*`/`UNIT-*` proposal per planned entry. It supplies concrete content; it does not abbreviate an oversized batch.

### 9. Critic admits or rejects every pair

For each pair, the production critic returns:

- KQ decision
- UNIT decision
- field-instantiation check
- placeholder scan
- enumeration-completeness check
- ordered-scale-quality check

All applicable decisions must be accepted and all checks must pass. Otherwise, the Orchestrator performs one targeted correction and asks the Critic to review that pair again.

The loop repeats batch by batch until all required units are admitted. Then `PRODUCTION_GATE` becomes `ACCEPT`.

### 10. Orchestrator assembles the candidate draft

The Orchestrator first persists and rereads `GUARD_CREATE_DRAFT`. Only literal `PASS` permits it to assemble accepted content into `.spec-workflow/draft.md` and create a complete publication manifest.

The draft must be the exact candidate intended for publication. It cannot be an outline, schema-only substitute, partial batch, or baseline summary.

After rereading the physical draft, `GUARD_ACCEPT_DRAFT` requires at least one admitted KQ, matching counts, complete Question/Answer envelopes, complete obligation accounting, and a fully mapped manifest. Only then record `DRAFT_GATE: ACCEPT`.

### 11. Critic performs independent final review

The Orchestrator invokes the Critic in `FINAL` mode with all eligible sources, current-run research, manifest, and exact draft only after `GUARD_CALL_FINAL: PASS`. `PREPUBLICATION` is not a review mode.

The Critic repeats global checks rather than trusting earlier pair reviews. `FINAL_STATUS: READY` requires all 18 named checks to be present and pass, prior gates to pass, at least one accepted KQ, full mandatory coverage, and zero unresolved mandatory or unclassified material items.

Outcomes:

- `READY`: Orchestrator records `FINAL_GATE: READY` and may publish
- `CONDITIONAL`: visible isolated gaps remain; publication does not occur
- `BLOCKED`: publication stops

### 12. Orchestrator publishes `spec.md`

At run initialization, the Orchestrator has already copied any prior `spec.md` verbatim to the verified `.spec-workflow/prior-spec.md` checkpoint and left the publication unchanged. `GUARD_WRITE_SPEC` rechecks that protection before allowing the exact accepted draft to replace `spec.md`. A passing `PREPUBLICATION_STATE_CHECK` alone does not pass this guard.

It does not create or execute the downstream artifact. A recipe specification is not cooked; a party is not scheduled; an interview is not conducted; software is not implemented.

### 13. Critic performs postpublication review

The Orchestrator invokes the Critic in `POSTPUBLICATION` mode only after `GUARD_CALL_POSTPUBLICATION: PASS`.

The Critic:

- compares `spec.md` to the accepted draft and manifest
- verifies current-run identity through research
- repeats field, placeholder, enumeration, scale, specificity, consistency, Q&A, conflict, traceability, and coverage checks
- confirms at least one accepted KQ is published

Only exact agreement and all repeated checks produce `POSTPUBLICATION_GATE: PASS`. On `FAIL`, the Orchestrator records the failure, restores the retained prior file, or removes the failed new file when no prior file existed, and reports failure. It does not reopen the failed gate; another attempt is a new run.

### 14. Orchestrator reports completion

A success/completion chat response is allowed only after postpublication pass. A terminal blocked, final-conditional, or postpublication-failed run still receives a concise status report naming the stopped phase, defects, rollback state, and allowed next action, but it must not claim completion or present the failed candidate as accepted. No response may claim that the downstream implementation was performed.

## Revision loops

The framework defines three internal feedback-loop types. The resolution loop runs only when genuine IRQs exist, and any targeted correction cycle runs only after `REVISE` or a failed check; a phase may accept on its first review. A separate outer continuous host loop exists only when the session uses the Autopilot permission level:

### Advanced Autopilot execution loop

> VS Code permits automatic continuation → Orchestrator advances durable state → VS Code decides whether to continue again

Ordinary Autopilot provides continuous host iteration. Only when `chat.autopilot.advanced.enabled` is also `true` does VS Code 1.124 apply the maximum of three automatic loops. Default Approvals and Bypass Approvals do not use this outer count. It is not a Critic retry count: an explicitly reported Advanced host stop changes no gate and does not mean `BLOCKED`; the next `resume` continues the same run from its earliest incomplete gate.

### Baseline loop

> Investigator baseline → Critic → targeted revision → Critic again

Purpose: stabilize meaning before generating content.

### Resolution loop

> Interrogator IRQ → Investigator resolution → Critic → targeted correction or human answer → Critic again

Purpose: resolve only knowledge that cannot be safely obtained another way.

### Production loop

> Investigator KQ/UNIT pair → Critic checks → targeted revision → Critic again

Purpose: make every unit independently complete before global assembly.

Final and postpublication review add system-level protection after local convergence.

## Expected VS Code visibility

According to current VS Code behavior, subagent calls appear as expandable tool calls in chat. You should be able to inspect which custom agent ran, its activity, and returned result. The durable accepted state still belongs in Markdown files.

If chat says “I will run the critic” but no critic result or gate record appears, the workflow has announced an intention rather than completed the transition.

## Invalid shortcuts

The following sequences are invalid:

- baseline → direct `spec.md`
- baseline `REVISE` → draft
- partial production → final review
- accepted pairs without a manifest → publication
- final `READY` → final chat without postpublication check
- chat claim of success → inferred gate acceptance
- `PREPUBLICATION` check name → invented Critic mode
- zero accepted KQs or partial coverage → contradictory `READY`
- prior `spec.md` → current evidence
- internal JSON transport → claim that JSON Schema was generated
- stale `.spec-workflow/**` or `outgoing/**` content → current evidence
- changed goal or source snapshot → RESUME
- independent embedded deliverable directives → one merged specification

## Learner observation exercise

During an execution segment, observe each visible worker return without voluntarily stopping the agent. After a terminal result, at a host-enforced Autopilot boundary, or alongside active work without sending a new chat message, answer:

1. Which role produced this record?
2. Which namespace does it belong to?
3. Is it a proposal, accepted state, candidate artifact, or published artifact?
4. What evidence authorizes it?
5. Which gate can change next?
6. What exact critic output is required for that transition?
7. Where will the accepted result be persisted?
8. Which `ACTION_AUTHORIZATION` record permits the next action, and does it literally say `PASS`?

If any answer is unclear, the run is not sufficiently observable.

If the host boundary arrives while the run is valid and nonterminal, inspect the durable ledger and then send `resume`. Do not interpret the pause as a failed gate or restart the run.

## Workflow checklist

- [ ] Text Spec Orchestrator is the selected custom agent.
- [ ] Source inventory is complete and verified.
- [ ] NEW initialization reset agent-owned workflow and outgoing namespaces.
- [ ] Research was created and reread before the exact-path snapshot; an orphan or malformed snapshot was repaired automatically.
- [ ] Initialization, managed-artifact paths, active goal, source scope, exact snapshot, evidence membership, and single-run checks pass.
- [ ] Candidate/path/block counts and every per-block live-count/row-count/highest-ID/EOF/text equation pass.
- [ ] Run Invariant Block reconciles exact counts with physical files.
- [ ] Host Autopilot pauses preserve the active ledger and do not alter the Critic revision counter.
- [ ] Every guarded transition has a reread `ACTION_AUTHORIZATION: PASS`.
- [ ] Baseline critic returns `ACCEPT` before later phases.
- [ ] IRQs remain internal and are used only when needed.
- [ ] Every production pair passes all applicable checks.
- [ ] Production is complete before draft assembly.
- [ ] Draft and manifest are complete before final review.
- [ ] Final critic returns `READY` with all checks passing.
- [ ] Published file exactly matches the accepted draft.
- [ ] Postpublication critic returns `PASS`.
- [ ] Final chat does not claim downstream execution.

## Related reading

- [Workflow Engine](03_Workflow_Engine.md)
- [Critic and Convergence](06_Critic_and_Convergence.md)
- [Observability Reference](OBSERVABILITY.md)
- [Run Health](09_Run_Health.md)
- [User Manual](USER-MANUAL.md)
