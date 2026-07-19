# Workflow Engine

The workflow engine is not a separate program or agent. It is the state machine defined by the Text Spec Orchestrator and enforced by the Text Spec Critic. Its durable state is a Markdown Run Invariant Block, Phase Gate Ledger, and action-authorization records in `.spec-workflow/research.md`.

## Learning goals

By the end of this chapter, you should be able to:

- explain every phase and legal gate transition
- distinguish a new run from continuation of an active run
- understand why `REVISE` is not acceptance
- explain how invariant counts and action guards prevent skipped phases
- trace production from baseline through postpublication verification
- explain how rollback protects a prior specification
- identify illegal shortcuts and circular state dependencies

## Why a workflow engine is necessary

Without gates, a model can jump from a quick source scan directly to `spec.md`, leave placeholders, call future review optional, or overwrite a valid prior file. The workflow engine makes readiness a state transition supported by evidence.

The engine answers three questions at every phase:

1. What evidence or artifact must exist before this phase begins?
2. Who evaluates the phase result?
3. What exact state allows dependent work to continue?

## Invocation and run selection

When selected, a bare `run`, `start`, `go`, `generate`, or `build` always starts NEW. It cleans managed state before source discovery and never continues an old ledger.

That one instruction authorizes work toward the complete active run. Internal `PENDING` and `REVISE` states do not cause a voluntary handoff: the Orchestrator corrects the current phase and invokes the same Critic mode again in the active response. A Critic verdict is a quality-gate decision, not a VS Code permission approval. The user is involved only when Default Approvals requires confirmation for an actual tool operation or a reviewed `HUMAN` domain decision cannot be resolved from evidence and safe synthesis.

`proceed`, `continue`, or `resume` continues only when one active ledger, goal, exact source snapshot, scope membership, and physical artifact state all still match. Otherwise it initializes NEW and records the mismatch reason.

A new run:

- deletes and verifies every managed `.spec-workflow/**` and `outgoing/**` file without deleting human, framework, or unknown files
- creates and rereads `.spec-workflow/research.md` first with one RUN_ID, the ordered pending invariant, reset record, goal/scope table, and baseline placeholder rather than appending
- then writes `.spec-workflow/source-snapshot.md` in bounded chunks through EOF and verifies candidate/path/block counts plus every live-count/row-count/highest-ID/text equation
- updates and rereads initialization checks and `GUARD_CALL_BASELINE`; orphaned, misplaced, malformed, or partial state is repaired internally with no worker call or chat handoff in the middle of the transaction
- creates the candidate draft only after production acceptance
- treats an existing `spec.md` as protected `PRIOR_PUBLISHED`, copies it verbatim to `.spec-workflow/prior-spec.md`, verifies exact equality, and leaves both unchanged until the final write guard passes
- records `PRIOR_SPEC_CHECKPOINT_STATE: NOT_REQUIRED` when no prior `spec.md` exists
- excludes the prior file from evidence and prepublication equality checks

## Outer Autopilot loop and inner Critic loop

These are separate control systems:

| Current VS Code session configuration | Tool-confirmation behavior | Advanced three-loop maximum? |
|---|---|---|
| Default Approvals | Uses configured tool, URL, and terminal confirmations | No. Critic correction and re-review remain internal to the active response. |
| Bypass Approvals | Auto-approves tool calls | No. Bypass Approvals is not Autopilot. |
| Autopilot, advanced setting disabled | Auto-approves tools and continuously iterates using ordinary Autopilot behavior | No documented Advanced Autopilot maximum. |
| Autopilot, `chat.autopilot.advanced.enabled: true` | A utility model decides whether another host turn is needed | Yes, at most three automatic continuation loops. |

| Loop | Owner | What is counted | What happens at its limit |
|---|---|---|---|
| Autopilot continuation with the advanced setting `true` | VS Code host | Utility-model decisions to keep the chat working | VS Code 1.124 stops after at most three; the active Markdown ledger remains resumable. |
| Critic correction | This framework | Completed targeted corrections after which the identical normalized defect signature still survives | At three unchanged corrections, independently verify `BLOCKED: NON_CONVERGING`. |

Only the last configuration row creates the bounded outer loop. The `.agent.md` instructions cannot extend that host limit. The release notes do not define one Autopilot loop as one Critic call, so the counters are never combined. `CURRENT_REVISION_DEFECT_SIGNATURE` and `SAME_DEFECT_REVISION_COUNT` persist only the inner Critic state. If the optional outer loop stops first, `resume` begins a new execution segment over the same run ID and earliest incomplete gate. See [VS Code permission levels](https://code.visualstudio.com/docs/agents/approvals#_permission-levels) and [VS Code 1.124: Advanced Autopilot](https://code.visualstudio.com/updates/v1_124#_advanced-autopilot).

## Gate transition table

| Gate | Initial state | Accepted result | Other result |
| --- | --- | --- | --- |
| `BASELINE_GATE` | `PENDING` | `ACCEPT` | `BLOCKED` |
| `RESOLUTION_GATE` | `PENDING` | `ACCEPT` or reasoned `NOT_REQUIRED` | `BLOCKED` |
| `PRODUCTION_GATE` | `PENDING` | `ACCEPT` | `BLOCKED` |
| `DRAFT_GATE` | `PENDING` | `ACCEPT` | `BLOCKED` |
| `FINAL_GATE` | `PENDING` | `READY` for publication | `CONDITIONAL` or `BLOCKED` |
| `POSTPUBLICATION_GATE` | `PENDING` | `PASS` | `FAIL` |

`REVISE` is a review decision, not an accepted gate state, permission request, or voluntary user handoff. The phase stays open, receives a targeted correction, and is reviewed again automatically in the active response. A Default Approvals dialog may confirm an actual tool operation; only an explicitly active Advanced Autopilot boundary leaves the phase pending for `resume`. A nonterminal recommendation menu is a workflow defect.

## Run invariants and action guards

The six gate names describe the workflow at a high level. The Orchestrator's linked [Fail-Closed Execution Contract](../.github/agent-contracts/text-spec-execution-firewall.md#fail-closed-execution-firewall) makes those gates executable as textual preconditions.

**What:** `.spec-workflow/research.md` contains one authoritative `Run Invariant Block`. In addition to phase, gate, coverage, draft, final, and publication values, it records run mode/reset reason, active goal and anchor, generated cleanup, exact managed-artifact paths, candidate/included counts, source scope, exact snapshot, evidence membership, and single-run-state checks.

**How:** after every accepted worker result or physical file change, the Orchestrator updates the block from complete current-run Markdown evidence. Before a dependent action, it writes an `ACTION_AUTHORIZATION` record containing the action, every tested field and value, and `GUARD_RESULT: PASS|FAIL`. It rereads that record from disk and acts only on literal `PASS`.

**Why:** worker output is a proposal. A polished paragraph, an announced plan, or a Critic label cannot prove that prerequisite artifacts and counts exist. The separate authorization record makes an illegal transition observable before it changes a file.

| Guarded action | Decisive conditions |
|---|---|
| Call baseline | Exact research and snapshot paths exist with matching RUN_ID and no duplicate; all initialization, reset, path, scope, snapshot, membership, and single-run checks plus every EOF/count/text equation pass |
| Start resolution | Baseline decision and gate are both `ACCEPT`, a genuine material IRQ exists, and resolution is pending |
| Start production | Baseline and resolution gates allow it, no material IRQ is open, at least one KQ is planned, every required unit is mapped, and `UNMAPPED_MANDATORY_OBLIGATION_COUNT == 0` |
| Create draft | Production passed, at least one KQ was planned, every planned KQ and required unit was accepted, and none remains unaccepted |
| Accept draft | The physical draft contains at least one admitted KQ, every entry has the complete Question/Answer envelope, obligation accounting is complete, and the manifest maps every block |
| Call final review | All earlier gates are accepted and accepted/draft counts agree |
| Write `spec.md` | All 18 final checks pass, mandatory coverage is complete, unresolved material is zero, at least one accepted KQ exists, and prior-state/checkpoint fields agree with the unchanged physical files |
| Call postpublication review | The write guard passed and the physical file exactly matches the accepted draft |
| Report success | Postpublication state and gate both pass |

`PREPUBLICATION` is not a Critic mode. `PREPUBLICATION_STATE_CHECK` is only one of the 18 checks returned in `FINAL` mode. It cannot independently authorize `READY`, a file write, or success.

## Phase 1: baseline

After `GUARD_CALL_BASELINE: PASS`, the Investigator receives the RUN_ID, selected goal, exact snapshot member list, included source sections, and exclusions. It returns:

- inferred goal and requested result
- complete three-layer Artifact Contract
- source-native terminology
- atomic obligation register with exact wording and locations
- source-explicit content areas, repeated units, fields, order, cardinality, and constraints
- cross-unit constraints stated by or directly calculable from the sources
- conflicts, genuine gaps, dependencies, and possible resolution routes

Baseline is extraction-only. It contains no planned KQs, published Q&A, newly synthesized or selected final values, selected defaults, resolved IRQs, production coverage, or draft. It must preserve exact values already supplied by admitted sources. Zero planned/accepted KQs and zero covered obligations are expected and are not reasons for `REVISE`.

The Critic reviews the complete baseline. Production cannot start until `BASELINE_GATE: ACCEPT`; a baseline `REVISE` can modify only the canonical current-baseline record before another baseline review.

This gate prevents a downstream batch from being generated against a guessed audience, incomplete source inventory, or missing field contract.

## Phase 2: resolution

Resolution is used only for genuine gaps. If no genuine `IRQ-*` exists, the gate becomes `NOT_REQUIRED` with that reason.

When resolution is needed:

1. The Interrogator proposes a bounded batch of internal queries.
2. The Investigator resolves the supplied queries in `RESOLVE_BATCH` mode.
3. The Critic accepts, revises, or blocks each resolution.
4. Accepted records are persisted only in research memory.

The phase may not produce published KQs or UNITs.

## Phase 3: knowledge coverage planning

The Orchestrator maps mandatory obligations according to treatment type. A mapping can target:

- a planned `KQ-*`
- a source-required field attached to a KQ
- a framework or cross-unit section
- navigation or traceability
- an explicit unresolved decision

Every required `UNIT-*` maps to a planned KQ. Structural metadata does not receive an artificial question merely to increase the KQ count.

This is where the Orchestrator builds the adaptive production contract. A source that only names required topics gets compact topic-and-completion rules. Detailed per-unit fields, member ledgers, or scales appear only when the source actually defines them. A planned KQ contains its ID, topic, role, mappings, and unit relationship—but no final `Question:`, `Answer:`, quantities, or coverage credit.

## Phase 4: independent production batches

The Investigator receives at most five planned KQs at a time, plus all context needed to complete them. For each topic it returns a publication-ready KQ and any associated UNIT.

Each pair receives these Critic results:

- KQ decision
- UNIT decision
- field-instantiation check
- placeholder scan
- enumeration-completeness check
- ordered-scale-quality check

A pair is admitted only when all applicable decisions accept and all applicable checks pass.

If a batch is too large to complete without abbreviation, the Investigator returns `BATCH_TOO_LARGE` with a smaller safe size. The workflow reduces the batch and regenerates. It never preserves batch size by inserting ellipses or compressed ranges.

`PRODUCTION_GATE: ACCEPT` means every required pair and completeness check passed.

## Phase 5: candidate draft

Only after production acceptance does the Orchestrator start a blank `.spec-workflow/draft.md`. The draft contains:

- task identity, purpose, and included sources
- a concise three-layer audience and usage summary
- visible assumptions, conflicts, and unresolved decisions
- the Specification Knowledge Base
- coverage and traceability
- final readiness status

The draft is assembled from admitted content, not copied from worker transcripts or research records.

A research-only publication manifest maps every draft block and line range to framework content, obligations, KQs, or UNITs. `DRAFT_GATE: ACCEPT` requires both the complete candidate and manifest.

## Phase 6: final review

Before the Critic runs in `FINAL` mode, the Orchestrator passes `GUARD_CALL_FINAL` and records `FINAL_GATE: PENDING`. This avoids circular logic: the Critic is not asked to verify a final result that must already exist.

The Critic checks the already completed baseline, resolution, production, and draft gates, then independently validates the complete draft. Required checks include:

- source inventory and Artifact Contract
- Q&A knowledge-base shape
- field and enumeration completeness
- scale quality
- source specificity
- cross-unit consistency
- question roles
- internal-query leakage
- unit coverage
- defaults and conflicts
- strict operators and variants
- source locations and manifest
- prepublication state consistency

The Critic must return all 18 named checks and the audit counts. Its `FINAL_STATUS` is first stored as `PROPOSED_FINAL_STATUS` while authoritative `FINAL_STATUS: NOT_RUN` and `FINAL_GATE: PENDING` remain unchanged. `GUARD_ACCEPT_FINAL_READY` atomically records authoritative `READY`; a separate guard records valid `CONDITIONAL` or `BLOCKED` results without evaluating the READY-only path.

Only a successful `GUARD_WRITE_SPEC` permits writing `spec.md`; that guard includes `FINAL_GATE: READY` but also proves nonzero accepted Q&A, full mandatory coverage, zero unresolved material, no missing or failed final checks, no substantive change to the reviewed draft or evidence after final acceptance, and correct `PRIOR_SPEC_STATE`, `PRIOR_SPEC_CHECKPOINT_STATE`, and `PRIOR_SPEC_CHECKPOINT_EQUALITY_CHECK` values against disk.

## Phase 7: postpublication verification

After final readiness:

1. `.spec-workflow/prior-spec.md` is verified again: when `PRIOR_SPEC_STATE: PRESENT`, the current `spec.md` must still match the verified checkpoint exactly; when prior state is `ABSENT`, checkpoint state must be `NOT_REQUIRED` and `spec.md` must still be absent.
2. The accepted draft replaces `spec.md` only after the write guard passes.
3. `POSTPUBLICATION_GATE: PENDING` is recorded.
4. The Critic compares the new file with the accepted draft and current-run research.
5. All substantive validation checks are repeated against the published file.

On success, the gate becomes `PASS`, the checkpoint is removed, and the Orchestrator may report completion.

On failure:

- a retained prior file is restored, or
- the failed new file is removed when no prior file existed

The previous published-file state is therefore restored. Current-run research and draft remain available for diagnosis.

## Current mixed-source walkthrough

For the current workspace, a healthy run looks like this:

1. NEW removes stale workflow and outgoing artifacts.
2. `jira.md:5,9` selects a Python cartoon-film implementation specification covering what, why, and how.
3. The two `input-spec/*.txt` files are classified by section: relevant adaptation material can be admitted only when designated, while embedded game/toy output directives cannot replace the selected film goal.
4. Baseline records only admitted facts, explicit literals, source roles, obligations, and genuine gaps.
5. Production emits concrete cartoon-film implementation Q&A rather than party, game-plan, or toy-product Q&A.
6. Final and postpublication checks prove that no excluded or stale domain evidence entered `spec.md`.

At no point should the workflow demand an existing runnable Python project. Python is covered because the current source names it, while implementation remains downstream work.

## Why the transition graph is non-circular

Each Critic evaluation sees the gate under review as pending and only requires earlier gates to be accepted:

- Baseline evaluates source analysis.
- Production relies on accepted baseline and resolution state.
- Final relies on accepted production and draft state while final itself is pending.
- Postpublication relies on final readiness while postpublication itself is pending.

The Orchestrator records each returned result after review. No reviewer requires its own future result as an input precondition.

## Failure modes

### `REVISE` is recorded as accepted

The phase is still open. Dependent phases must not begin.

### Production begins before baseline acceptance

The unit schema, actors, constraints, or relationship may be wrong. This is blocked.

### Drafting begins from partial batches

Completeness and cross-unit consistency cannot be established. This is blocked.

### `spec.md` is written directly from research

Research includes internal workflow records and unadmitted proposals. Direct publication is forbidden.

### Final review is described as optional

Required reviews cannot be deferred. A promise of future validation is not gate evidence.

### `PREPUBLICATION` is used as a Critic mode

That mode does not exist. The valid modes are `BASELINE`, `RESOLUTION`, `PRODUCTION`, `FINAL`, and `POSTPUBLICATION`. A request using `PREPUBLICATION` must be blocked without a dependent edit.

### `READY` contradicts its audit counts

`READY` is invalid when accepted KQs are zero, mandatory coverage is partial, unresolved material remains, an earlier gate did not pass, or any final check is missing or failed. `PREPUBLICATION_STATE_CHECK: PASS` cannot cancel any of those failures because it is only one final check.

### Prior output is compared as current state

`PRIOR_PUBLISHED` must be excluded from current-run prepublication equality. Only the candidate draft, manifest, and current research are compared.

## Practical checklist

- [ ] Exactly one complete current-run Run Invariant Block and Phase Gate Ledger exist in research memory.
- [ ] NEW created and reread research first, then created and verified the exact-path snapshot, with no worker call or chat handoff before `GUARD_CALL_BASELINE` was persisted and reread.
- [ ] Candidate/path/block counts and every per-block live-count/row-count/highest-ID/EOF/text equation pass.
- [ ] `UNMAPPED_MANDATORY_OBLIGATION_COUNT` is zero before production.
- [ ] `PRIOR_SPEC_STATE`, `PRIOR_SPEC_CHECKPOINT_STATE`, and `PRIOR_SPEC_CHECKPOINT_EQUALITY_CHECK` reconcile with `spec.md` and `.spec-workflow/prior-spec.md`.
- [ ] A host Autopilot stop leaves the run active and resumable; it does not increment `SAME_DEFECT_REVISION_COUNT` or become `BLOCKED`.
- [ ] Every guarded action has a durable, reread `ACTION_AUTHORIZATION` record with literal `PASS`.
- [ ] Every phase begins in `PENDING`.
- [ ] `REVISE` leaves the gate open.
- [ ] A blocked gate stops dependent phases.
- [ ] Resolution is `NOT_REQUIRED` only with a reason.
- [ ] Every production pair and completeness check passes before draft assembly.
- [ ] `.spec-workflow/draft.md` and its research-only manifest are complete before final review.
- [ ] `FINAL_GATE` is pending during final evaluation and recorded afterward.
- [ ] `spec.md` is written only after final readiness.
- [ ] Postpublication review repeats substantive checks.
- [ ] Failure restores the prior published-file state while retaining current-run diagnostics.
- [ ] Final chat success occurs only after postpublication pass.

## Related reading

- [Getting Started](00_Getting_Started.md)
- [Framework Architecture](02_Framework_Architecture.md)
- [Shared Memory](04_Shared_Memory.md)
- [Interrogation Algorithm](05_Interrogation_Algorithm.md)
- [Orchestrator definition](../.github/agents/text-spec-orchestrator.agent.md)
- [Critic definition](../.github/agents/text-spec-critic.agent.md)
