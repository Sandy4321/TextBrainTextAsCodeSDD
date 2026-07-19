# 12. End-to-End Tutorial

## Purpose

This chapter follows the current workspace from three mixed text inputs to a validated `spec.md` Question -> Answer knowledge base.

The active goal is a specification for implementing a cartoon film in Python. Two longer descriptions contain potentially useful story and design material, but each also asks for a different final deliverable. The tutorial shows how the workflow can admit relevant propositions without turning a school detective game, a clinic toy, and a Python cartoon into one confused project.

The example classifications and Q&A below explain the method. The real run must persist and validate its own evidence, decisions, and exact line references.

## Current workspace fixture

The current task-facing files are:

- [jira.md](../jira.md), where line 5 requests a complete specification for implementing a cartoon film in Python and line 9 requires `spec.md` to explain what, why, and how to code;
- [description1.txt](../input-spec/description1.txt), which contains a missing-compass school mystery, tone and originality guidance, live-event logistics, and an embedded direction to produce a detective-game specification;
- [description2.txt](../input-spec/description2.txt), which contains a friendly imaginary robot-pet concept, expressive-behavior ideas, physical-device constraints, and an embedded direction to produce a clinic tabletop-toy specification.

For the clean-fixture walkthrough, assume no prior published `spec.md`; an actual workspace may already contain one, in which case the rollback-checkpoint branch applies. In either case, `spec.md` is an output, not an input prerequisite.

The framework files under `.github/`, this documentation directory, and generated `.spec-workflow/` state are control material rather than task evidence.

## What the workflow is producing

The workflow produces a Markdown specification knowledge base. It does not implement the cartoon, create Python files, render frames, or manufacture either physical activity described in the supporting text.

For this task:

| Layer | Current interpretation |
|---|---|
| Specification knowledge | `spec.md`, read by the person or agent that will implement the cartoon |
| Downstream deliverable | A Python cartoon-film implementation whose narrative, assets, behavior, architecture, and verification are defined by the specification |
| Operational experience | A developer builds and runs the implementation; a viewer experiences the resulting film |

The downstream work is not naturally a respondent-facing questionnaire. Its published entries therefore use `SPEC_KNOWLEDGE_QUERY`: each `Question:` asks for implementation knowledge, and its `Answer:` supplies the concrete decision or instruction.

## Step 1: start a clean NEW run

In VS Code Chat:

1. Select **Text Spec Orchestrator**.
2. Enter `run`, `start`, `go`, `generate`, or `build`.

Those words always mean **NEW**. They do not mean “look for a runnable Python project,” and they do not mean “continue whatever old research happens to exist.”

NEW initialization performs these operations in order:

1. Inventory path ownership without treating old generated content as evidence.
2. Preserve human inputs, framework definitions, unknown files, and any current published `spec.md`.
3. Delete every managed file under `.spec-workflow/**` and legacy `outgoing/**`, regardless of extension.
4. Verify that stale generated files, drafts, run IDs, baselines, and sidecar references are gone.
5. If a prior `spec.md` exists, copy it verbatim to `.spec-workflow/prior-spec.md` and verify equality; otherwise record the checkpoint as `NOT_REQUIRED`. The tutorial covers both branches rather than assuming generated publication state remains fixed.
6. Inventory every non-reserved textual candidate, select one active goal and its verified anchor, and classify candidate files or sections against that goal.
7. First create and reread `.spec-workflow/research.md` with exactly one new run ID, nonempty `ACTIVE_GOAL_ID` and `ACTIVE_GOAL_ANCHOR`, one pending invariant block, one reset record, the goal/scope table, and one baseline placeholder.

At this point the ledger is deliberately pending; snapshot-dependent checks cannot yet be `PASS`. The first reread must prove:

- `RUN_MODE: NEW`;
- `RUN_INITIALIZATION_GATE: PENDING`;
- `GENERATED_ARTIFACT_RESET_CHECK: PASS`;
- `MANAGED_ARTIFACT_PATH_CHECK: NOT_RUN`;
- exactly one current `RUN_ID`;
- one nonempty active-goal ID and verified anchor;
- no stale draft or old-domain record.

If managed state cannot be removed or an old run survives, the Orchestrator first repairs or cleanly retries NEW. Only repeated concrete edit/path failure can end as `BLOCKED: RUN_INITIALIZATION_FAILED`.

## Step 2: take an exact source snapshot

Before baseline analysis, the Orchestrator writes `.spec-workflow/source-snapshot.md`. This exact path is mandatory: a root-level, basename-only, or duplicate current-run copy fails `MANAGED_ARTIFACT_PATH_CHECK`. The file is recoverable source identity, not a model-invented hash.

For each candidate text source, the snapshot records:

- its sorted workspace-relative path;
- its logical line count, including blank lines;
- every logical line in a form such as `L000005|exact text`.

The Orchestrator reads and writes bounded chunks until editor-reported EOF, then rereads source and snapshot. Candidate count must equal sorted path count and source-block count. For every block, the declared count, number of contiguous `L*` rows, highest row ID, and live line count must agree, and every row text must match. A partial read or invented count cannot pass.

Only after these equations and the goal/scope, membership, and single-run checks pass does the Orchestrator update `RUN_INITIALIZATION_GATE: PASS`, persist and reread `GUARD_CALL_BASELINE: PASS`, and call the Investigator. No worker call or chat handoff is permitted halfway through this transaction.

The three current task candidates include `jira.md`, `input-spec/description1.txt`, and `input-spec/description2.txt`. Reserved framework files need exclusion records rather than being copied as task evidence.

The snapshot supports two guarantees:

1. A claim can be traced to the exact text that existed when the run started.
2. `resume` is safe only when the path set and every logical line still match.

If a candidate is added, removed, renamed, or edited, `SOURCE_SNAPSHOT_CHECK` fails. The Orchestrator must initialize NEW with `RESET_REASON: SOURCE_CHANGED`; it cannot repair the old ledger against new evidence.

An orphan snapshot is also a failed initialization. Without `.spec-workflow/research.md`, no authoritative RUN_ID or active ledger exists; the Orchestrator cleans the partial state and automatically repeats NEW rather than asking the user to update research or rerun baseline.

## Step 3: inspect the active goal selected during initialization

The selection used before the first research write follows this semantic authority order:

1. an explicit current user instruction;
2. a current task or ticket that states a goal and requested output;
3. the only coherent goal-bearing source.

Here, [jira.md](../jira.md) is the active goal source. Its relevant anchors are:

- `jira.md:5`: a complete specification for implementing a Python cartoon film;
- `jira.md:9`: `spec.md` covering what, why, and how to code.

The typo in the ticket does not erase its clear meaning. The word `Python` is source evidence for this run, not a permanent competency or hard-coded framework domain.

The final sentence at `jira.md:13` mentions unspecified confirmations and attachments. Because no actual requested package is identified, the scope ledger should not silently invent one. It records that proposition as irrelevant boilerplate or a gap only if it materially affects completion.

## Step 4: inspect the mixed-source classifications persisted during initialization

Before writing research and the snapshot, the Orchestrator determines that the two description files are not equal competing goal sources. In this tutorial they are designated mixed source material for the cartoon. That permits relevant descriptive propositions to be admitted while incompatible embedded deliverable directives remain excluded. This step explains and verifies the scope table already persisted in Step 1; it is not a later goal-selection action.

A useful scope table has this shape:

| Source or proposition | Classification | Reason |
|---|---|---|
| `jira.md:5`; `jira.md:9` | `ACTIVE_GOAL_SOURCE` | Declares the current Python-cartoon goal and requested specification |
| `description1.txt:3`; `description1.txt:11`; `description1.txt:13` descriptive propositions | Candidate `IN_SCOPE_SOURCE_MATERIAL` | Supplies a missing-compass mystery, child-appropriate mysterious tone, old-fashioned investigation style, and original-name constraint |
| `description1.txt:5-9`; `description1.txt:15-21` live-event logistics | `EXCLUDED_EMBEDDED_SCOPE` unless explicitly adapted | Participant teams, supervisors, physical rooms, prizes, live timing, and material budget belong to the school activity, not automatically to a film implementation |
| `description1.txt:23` detective-game document directive | `EXCLUDED_INCOMPATIBLE_DIRECTIVE` | Requests a different final document and says not to write the full story or clue set |
| `description2.txt:1`; `description2.txt:3`; `description2.txt:9` descriptive propositions | Candidate `IN_SCOPE_SOURCE_MATERIAL` | Supplies a small robot-pet/friendly imaginary-animal concept and possible expressive, non-startling behavior |
| `description2.txt:5-29` physical-toy operations | `EXCLUDED_EMBEDDED_SCOPE` unless explicitly adapted | Clinic use, batteries, cleaning, controls, networking, dimensions, and prototype budget constrain a tabletop device, not automatically an animated film |
| `description2.txt:31` tabletop-toy specification directive | `EXCLUDED_INCOMPATIBLE_DIRECTIVE` | Requests another final artifact and prohibits selecting details for that hardware project |
| `description2.txt:33` reception-desk memorandum | `EXCLUDED_NOISE` | The source itself says it may be ignored |

Classification can be proposition-level even when one paragraph contains both useful description and an incompatible directive. The obligation record quotes only the admitted wording and cites its real line. Excluded wording remains in the snapshot for freshness but cannot corroborate a requirement.

Admission also preserves modality:

- “must have original names” can become a mandatory constraint if that proposition is admitted;
- fog, coded notes, footprints, train timetables, and curious clocks at `description1.txt:13` remain optional ideas because the source explicitly says so;
- the possible expressive methods at `description2.txt:9` are not all mandatory;
- a physical-toy requirement does not become a film requirement merely because both projects involve children or animation-like behavior.

`SOURCE_SCOPE_CHECK: PASS` requires one selected goal, at least one included source or section, and a semantic reason for every admission, exclusion, and partial exclusion. `EVIDENCE_MEMBERSHIP_CHECK: PASS` requires every obligation and synthesis bound to use only admitted snapshot propositions.

## Step 5: build the extraction-only baseline

Only after `GUARD_CALL_BASELINE: PASS` may the Investigator run in `BASELINE` mode.

The baseline contains:

- the active goal and its anchor;
- the complete candidate scope table;
- included source ranges and exact exclusions;
- snapshot and evidence-membership status;
- the three-layer Artifact Contract;
- atomic source obligations;
- source-explicit values, structure, and constraints;
- same-goal conflicts and genuine gaps;
- possible but unselected evidence, synthesis, safe-default, or human resolution routes.

It does not contain planned KQs, published questions or answers, selected implementation architecture, chosen libraries, produced scenes, coverage credit, resolved IRQs, or a draft.

For the current Jira, the smallest source-explicit content contract begins with three required areas:

| Content area | Source basis | Baseline completion meaning |
|---|---|---|
| What | `jira.md:9` | Identify what Python cartoon film is being specified and the admitted narrative, visual, audio, and behavioral scope |
| Why | `jira.md:9` | Explain the source-bounded rationale for major content and implementation decisions |
| How to code | `jira.md:5`; `jira.md:9` | Define implementable Python structure, data flow, components, behavior, and verification without writing the code itself |

The sources provide no exact number of scenes, assets, modules, tests, or seconds of runtime. Durable baseline state therefore uses `CARDINALITY: SOURCE_UNSPECIFIED` rather than inventing counts; the Investigator's current proposal spelling is documented in [File Formats](13_File_Formats.md#current-cardinality-spelling-compatibility).

Source-explicit literals are preserved. `Python`, `cartoon film`, `spec.md`, the admitted missing-compass premise, and any admitted tone or character constraints are evidence. A library such as Pygame, Arcade, MoviePy, or Manim is not source evidence and must not appear as an already-decided baseline fact.

## Step 6: create the obligation register

Each `OBL-*` contains an immutable ID, human-readable label, exact admitted wording, mandatory status, treatment type, and verified location.

An illustrative register begins with:

- create a complete implementation specification for a cartoon film in Python, from `jira.md:5`;
- create `spec.md`, from `jira.md:9`;
- cover what, why, and how to code, from `jira.md:9`;
- preserve any admitted narrative, tone, originality, or character proposition from its exact description line;
- keep optional ideas optional and incompatible game/toy directives excluded.

The active source may bound original content without supplying the final scene order, architecture, or asset list. Those later choices are `GROUNDED_SYNTHESIS` or accepted decisions, not newly invented source obligations.

## Step 7: run the BASELINE Critic loop

The Critic checks initialization, exact snapshot equality, one coherent goal, evidence membership, single-run state, source classification, exact locations, contract separation, obligations, explicit values, conflicts, and genuine gaps.

A valid baseline normally has:

- `PLANNED_KQ_COUNT: 0`;
- `ACCEPTED_KQ_COUNT: 0`;
- `ACCEPTED_UNIT_COUNT: 0`;
- `COVERED_MANDATORY_OBLIGATION_COUNT: 0`;
- no `.spec-workflow/draft.md`.

Those zeros are expected before production and are not reasons for `REVISE`.

The possible Critic outcomes are:

- `ACCEPT`: independently verify and close the baseline gate;
- `REVISE`: replace only the defective baseline records and invoke the same Critic mode again immediately;
- `BLOCKED`: stop only after independently verifying that no legal in-scope correction exists.

`REVISE` is not a prompt asking the user whether work should continue. A Critic verdict is also not a VS Code tool approval. Under Default Approvals, only an actual configured tool operation may require confirmation.

The framework counter is precise: the first defect signature starts at zero; it increments only after a targeted correction and same-mode re-review leave the identical defect unchanged with no relevant evidence or state change. Changed evidence, changed state, a changed signature, phase acceptance, or phase change resets it. A verified count of three becomes `BLOCKED: NON_CONVERGING`.

## Step 8: resolve only genuine decisions

After baseline acceptance, the Interrogator may create internal `IRQ-*` records for material decisions that evidence and safe synthesis cannot resolve.

Possible current-task questions include:

- whether the cartoon should combine the missing-compass mystery with the friendly imaginary robot-pet character or use only one admitted strand;
- whether a specific Python animation/rendering library must be selected when no source names one;
- whether a runtime length or exact scene count is required for implementation acceptance.

These are source-specific only when the missing choice materially changes the specification. The workflow should prefer a bounded `GROUNDED_SYNTHESIS` when the goal and admitted constraints safely support one. It must not ask the user to reconfirm that the deliverable uses Python or that `spec.md` needs what/why/how; those are explicit facts.

Every resolution is reviewed. Only accepted resolutions enter current-run memory. If no genuine IRQ exists, the resolution gate becomes `NOT_REQUIRED` with a concrete reason.

## Step 9: plan knowledge coverage

After baseline and resolution, the Orchestrator maps every mandatory obligation to one or more appropriate destinations:

- a planned `KQ-*`;
- an attached source-required field;
- a framework or cross-unit section;
- traceability or navigation;
- an explicit unresolved decision.

For this non-question-native downstream task, useful planned topics may include:

- accepted narrative and adaptation boundary;
- target viewer experience and tone;
- scene, visual-asset, and audio-asset model;
- Python component architecture and responsibility boundaries;
- timeline, state, rendering, and audio flow;
- configuration and asset loading;
- failure behavior and diagnostics;
- testing, acceptance, packaging, and execution instructions.

These topics are not permanent framework labels. They are accepted only when required to answer the current Jira’s what/why/how request and bounded by admitted evidence or an accepted synthesis.

The plan may contain several source-native unit types, such as `SCENE`, `VISUAL_ASSET`, `AUDIO_CUE`, `PYTHON_COMPONENT`, and `ACCEPTANCE_SCENARIO`. These are post-baseline implementation units, not facts smuggled into source extraction. Different content areas may use different relationships, such as embedding exact scene decisions while defining component schemas and referencing external asset locations.

Before production, `UNMAPPED_MANDATORY_OBLIGATION_COUNT` must be zero.

## Step 10: produce the Question -> Answer knowledge base

Production runs in independent batches of at most five planned KQs. Each Investigator call receives only the relevant admitted sources, obligations, accepted resolutions, production contract, expected members, cross-unit constraints, and a compact accepted-topic index.

Every proposal has a literal Q&A envelope:

```text
### KQ-NNN - source-specific title
Question: concrete implementation question
Answer: concrete source-bounded decision or instruction
Source basis: OBL-ID - human-readable label - path:line or path:start-end
Acceptance check: observable completion or verification condition
```

An illustrative entry shape for this task is:

```text
### KQ-001 - Narrative and adaptation boundary
Question: Which admitted story and character material defines the Python cartoon film, and which embedded project directives are excluded?
Answer: The accepted adaptation uses only the narrative and design propositions admitted by the scope ledger. The school live-event logistics and its detective-game document directive are excluded; the clinic hardware, privacy, power, cleaning, networking, budget, and tabletop-toy document directive are excluded. Any combination of the missing-compass mystery and friendly imaginary robot-pet concept must match the accepted resolution record rather than being presented as a source fact.
Source basis: OBL-001 - Implement a Python cartoon film - jira.md:5; OBL-004 - Missing-compass mystery material - input-spec/description1.txt:3; OBL-005 - Friendly imaginary robot-pet material - input-spec/description2.txt:1-3
Acceptance check: Every story or character claim maps to an admitted proposition or accepted synthesis, and no excluded game or physical-toy directive appears as a film requirement.
```

The IDs are illustrative, but the entry demonstrates the required specificity. A real run uses the IDs actually assigned by its accepted obligation register.

Bad questions include:

- “What must `spec.md` contain?”
- “What are the requirements?”
- “Is the specification ready?”

They ask about workflow rather than the cartoon. A useful KQ asks something the Python implementer needs answered and supplies that answer now.

## Step 11: run the production Critic loop

For every proposed KQ and associated unit, the Critic returns:

- `KQ_DECISION`;
- `UNIT_DECISION`;
- `FIELD_INSTANTIATION_CHECK`;
- `PLACEHOLDER_SCAN`;
- `ENUMERATION_COMPLETENESS_CHECK`;
- `ORDERED_SCALE_QUALITY_CHECK` when applicable.

The Critic verifies that:

- every answer is concrete and usable for implementation;
- every source claim comes from an admitted snapshot proposition;
- synthesis is labeled rather than disguised as fact;
- excluded game and toy directives do not leak back into scope;
- Python-specific implementation decisions answer the Jira without inventing unsupported source facts;
- every required field, unit, list member, dependency, and acceptance condition is complete;
- no `...`, `TBD`, future-work instruction, field-name-only value, or schema-only substitute remains.

`REVISE` triggers a targeted correction and same-mode re-review. The workflow reduces batch size if necessary instead of truncating required content. Only item-specific `ACCEPT` decisions increase accepted counts.

## Step 12: assemble and validate the candidate draft

Only accepted production material enters `.spec-workflow/draft.md`. The draft contains:

- task identity, purpose, and admitted source inventory;
- the three-layer audience and usage summary;
- visible assumptions, conflicts, partial-source exclusions, and unresolved decisions;
- `## Specification Knowledge Base`;
- all accepted `KQ-*` entries in a useful implementation order;
- coverage and traceability;
- final readiness status.

The research-only publication manifest maps every draft block to framework content, an obligation, a KQ, or a unit. It is not copied into `spec.md`.

`DRAFT_GATE: ACCEPT` requires a physical complete draft, at least one accepted Q&A entry, exact accepted counts, zero envelope failures, and zero unmapped manifest blocks.

## Step 13: perform FINAL review

Before FINAL, the Orchestrator recomputes initialization, scope, snapshot, membership, and single-run checks. An earlier `PASS` is not trusted after a file change.

The FINAL Critic receives the complete accepted draft, current research, admitted sources, and publication manifest. It returns all 18 required checks and a separate `FINAL_STATUS`.

`READY` is allowed only when:

- all 18 checks are present and pass;
- every prior gate is accepted;
- accepted and draft KQ counts match and are nonzero;
- every mandatory obligation is covered;
- no material or unclassified unresolved item remains;
- the exact snapshot and scope still match;
- no blocked item exists.

A passing `PREPUBLICATION_STATE_CHECK` by itself is not readiness.

## Step 14: publish transactionally

Only `FINAL_GATE: READY`, a passing final-acceptance guard, and `GUARD_WRITE_SPEC: PASS` allow `spec.md` to be created or replaced.

When `PRIOR_SPEC_STATE: ABSENT`:

1. write the accepted candidate to `spec.md`;
2. set `POSTPUBLICATION_GATE: PENDING`;
3. invoke the Critic in `POSTPUBLICATION` mode;
4. prove exact draft equality and repeat the substantive checks;
5. keep the file only on `POSTPUBLICATION_GATE: PASS`.

When `PRIOR_SPEC_STATE: PRESENT`, NEW initialization protects the existing `spec.md` with the verified checkpoint and leaves publication unchanged until the write guard passes. A failed publication restores that checkpoint; a successful publication keeps the candidate and removes the checkpoint.

Success may be reported only after postpublication passes. The final chat response names the `spec.md` path, accepted KQ count, covered/total mandatory obligations, and unresolved count. It does not paste an unverified specification into chat.

## Gate sequence at a glance

| Gate | Required state before the next dependent action |
|---|---|
| Run initialization | `PASS` |
| Source scope, snapshot, membership, and single-run checks | `PASS`, freshly recomputed |
| Baseline | `ACCEPT` |
| Resolution | `ACCEPT` or justified `NOT_REQUIRED` |
| Production | `ACCEPT` for every required pair |
| Draft | `ACCEPT` |
| Final | `READY` |
| Postpublication | `PASS` |

## Common failures in this fixture

| Failure | Why it is wrong | Correct recovery |
|---|---|---|
| Treating all three files as equal goals | Produces an incoherent cartoon/game/toy hybrid | Keep Jira as the active goal and classify description propositions semantically |
| Excluding both descriptions wholesale | Loses explicitly designated source material | Admit relevant propositions and exclude only incompatible directives and unrelated logistics |
| Importing the school’s team count or clinic battery rules into the film | Confuses embedded project operations with cartoon requirements | Repair the baseline and invoke its Critic internally |
| Calling an optional motif mandatory | Changes source modality | Preserve `optional`, `possible`, `unresolved`, and `must` exactly |
| Choosing a Python library during baseline | Converts synthesis into a source fact and leaks production into extraction | Route the decision through resolution and production |
| Creating generic meta-questions | Produces no implementation knowledge | Ask source-bounded what/why/how questions and answer them concretely |
| Resuming after any candidate source changes | Uses a stale evidence ledger | Initialize NEW with `RESET_REASON: SOURCE_CHANGED` |
| Writing `spec.md` before FINAL readiness | Bypasses admission and rollback guards | Restore prior state and continue from the earliest legal gate |

## End-to-end checklist

- [ ] `run` initialized NEW rather than appending to stale research.
- [ ] Managed `.spec-workflow/**` and `outgoing/**` state was deleted and verified.
- [ ] Human inputs and framework definitions were preserved.
- [ ] `.spec-workflow/research.md` was created and reread before the snapshot.
- [ ] `.spec-workflow/source-snapshot.md` was the only identity snapshot; no root-level or duplicate copy existed.
- [ ] Candidate/path/block counts and every per-block live-count/row-count/highest-ID/EOF/text equation passed.
- [ ] Initialization defects were repaired locally before any worker call, without a user rerun prompt.
- [ ] `GUARD_CALL_BASELINE: PASS` was persisted and reread before baseline.
- [ ] `jira.md:5` and `jira.md:9` selected one Python-cartoon goal.
- [ ] Every proposition in both descriptions received a semantic admission or exclusion reason.
- [ ] Incompatible detective-game and tabletop-toy directives were excluded.
- [ ] Excluded propositions were never used as evidence.
- [ ] Baseline preserved source literals but contained no produced Q&A or selected implementation values.
- [ ] Only genuine material gaps became IRQs.
- [ ] Every mandatory obligation was mapped before production.
- [ ] Every accepted entry contained literal `Question:` and `Answer:` fields plus source basis and an acceptance check.
- [ ] Critic `REVISE` results caused targeted correction and same-mode re-review.
- [ ] Draft counts, manifest mappings, and physical content agreed.
- [ ] All 18 FINAL checks passed before publication.
- [ ] The published file exactly matched the accepted draft and passed postpublication review.

## Related reading

- [03. Workflow Engine](03_Workflow_Engine.md)
- [06. Critic and Convergence](06_Critic_and_Convergence.md)
- [10. Debugging](10_Debugging.md)
- [13. File Formats](13_File_Formats.md)
- [17. Fresh Runs, Source Scope, and Cleanup](17_Fresh_Run_Source_Scope_and_Cleanup.md)
