# Fresh Runs, Source Scope, and Cleanup

This chapter explains how the framework prevents a new task from inheriting an old task's vocabulary, evidence, questions, or generated files.

## The rule in one sentence

`run` means: delete verified framework-generated state, select one current goal, snapshot the current text sources, and build a new Question -> Answer knowledge base without reusing prior-run evidence.

`resume` means: continue only when the existing run, active goal, exact source text, scope decisions, and physical artifacts still match.

The implementation entry points are [Text Spec Orchestrator](../.github/agents/text-spec-orchestrator.agent.md), line 25, and [Text Spec Fail-Closed Execution Contract](../.github/agent-contracts/text-spec-execution-firewall.md), lines 129-165.

## Why the old behavior failed

Replacing input text is not enough when generated workflow memory remains readable. A stale `.spec-workflow/research.md` can still contain old run IDs, old source paths, old obligations, accepted decisions, and old domain nouns. If a new baseline is appended instead of replacing that state, the model sees both tasks and can continue the old one.

The corrected framework treats this as a state-integrity problem, not merely a prompting problem:

- a NEW run may contain exactly one run ID;
- old research is deleted, not summarized or appended;
- legacy `outgoing/**` files are deleted before discovery;
- every candidate source is classified against one selected goal;
- excluded text cannot corroborate an obligation;
- every later guard rechecks that the source files still match the snapshot.

## File ownership comes before deletion

The agent must never delete a file merely because its content looks unrelated.

| Ownership class | Examples in this project | NEW-run behavior |
|---|---|---|
| Human/task input | `jira.md`, `input-spec/**` | Preserve. Inventory and classify semantically. |
| Framework definition | `.github/agents/**`, `.github/agent-contracts/**`, `README.md`, `docs/**` | Preserve. Exclude from task evidence unless specific domain content is explicitly designated. |
| Managed generated state | `.spec-workflow/**` | Delete and verify before creating the new run. |
| Legacy generated output | `outgoing/**` | Delete and verify. The specification-only workflow does not recreate it. |
| Prior publication | `spec.md` | Never use as current evidence. If present, preserve only through the verified transactional checkpoint. |
| Unknown | Any other path without proven ownership | Preserve and report; never guess that it is safe to delete. |

This ownership boundary is normative at [the execution contract](../.github/agent-contracts/text-spec-execution-firewall.md), line 55. Human input can be irrelevant to the selected goal and still must not be deleted.

## NEW-run sequence

The order matters:

1. Classify the invocation as NEW.
2. Inventory paths by ownership without treating old generated content as evidence.
3. Preserve human, framework, unknown, and current published files.
4. Delete every file in `.spec-workflow/**` and `outgoing/**`.
5. Verify that no stale managed file remains.
6. Recreate `.spec-workflow/prior-spec.md` only when a current `spec.md` needs rollback protection.
7. Select one active goal.
8. Create and reread `.spec-workflow/research.md` first, with one RUN_ID, one pending invariant block, one reset record, one scope table, and one baseline placeholder.
9. Write `.spec-workflow/source-snapshot.md` in bounded chunks after reading every candidate through editor-reported EOF.
10. Verify that `SOURCE_CANDIDATE_COUNT` equals sorted candidate-path count and `### SOURCE path` block count; then verify that each block's declared line count equals its number of contiguous `L*` rows, highest row ID, and live source line count, and that every row text matches.
11. Update and reread the research checks, then persist and reread `GUARD_CALL_BASELINE` before invoking the Investigator.

The Orchestrator's compact rule is at [text-spec-orchestrator.agent.md](../.github/agents/text-spec-orchestrator.agent.md), line 215. The independently testable guard is at [text-spec-execution-firewall.md](../.github/agent-contracts/text-spec-execution-firewall.md), lines 157-165.

## RESUME sequence

RESUME does not mean “trust whatever `.spec-workflow/research.md` says.” Before continuing, the Orchestrator rereads current files and proves:

- exactly one current RUN_ID exists;
- the active goal and its authority still match;
- candidate paths and every logical source line equal the stored snapshot;
- included and excluded source decisions still make sense;
- every obligation and fact points only to admitted source lines;
- draft, publication, checkpoint, and gate state agree physically.

Any mismatch changes the operation to NEW with a recorded reason such as `SOURCE_CHANGED`, `GOAL_CHANGED`, or `STATE_INVALID`. The old obligations and KQs are not migrated.

## Why the snapshot is Markdown instead of a hash

The custom Orchestrator has `read`, `search`, `edit`, and `agent` tools. It does not have a trusted checksum tool. Asking a language model to invent a SHA-256 value would create false confidence.

The full snapshot also carries the matching RUN_ID and sorted candidate-path identity. One source-block fragment looks like this:

```text
### SOURCE input-spec/example.txt
LINE_COUNT: 2
L000001|first exact line
L000002|second exact line
```

The path list is sorted, and `SOURCE_CANDIDATE_COUNT` must equal both its path count and its `### SOURCE path` block count. Each source block must cover `L000001` through its real editor-reported EOF without gaps or summaries. Added, removed, renamed, truncated, or changed text fails `SOURCE_SNAPSHOT_CHECK`. The snapshot is control memory and is never task evidence. Counts must come from the physical source, not display wrapping or model estimation.

## Orphan snapshot means initialization never committed

`.spec-workflow/source-snapshot.md` without `.spec-workflow/research.md` is not progress and its RUN_ID has no authority. The Orchestrator is the only writer, so it must not ask the user to “update research,” “rerun baseline,” or “tell me to proceed.” It must classify the partial state as no active ledger, clean the managed namespaces, and execute NEW again using the research-first transaction.

Likewise, a root-level or basename-only `source-snapshot.md` is not the managed artifact. Preserve an unknown human file, exclude it from evidence, and write the current-run identity only at `.spec-workflow/source-snapshot.md`. A duplicate that claims the current RUN_ID makes the path check fail.

## Selecting one goal without hard-coding a domain

The framework uses semantic authority:

1. an explicit current user instruction;
2. a source that explicitly declares the current task or ticket goal and requested output;
3. the only coherent goal-bearing source.

Filenames and modification times do not select the goal.

For the current workspace, [jira.md](../jira.md) line 5 asks for a Python cartoon-film specification, and line 9 requires a `spec.md` explaining what, why, and how to code. The word `Python` is therefore current source evidence—not a permanent framework competency or hard-coded domain.

The two files under `input-spec/` contain different embedded deliverable directions. [description1.txt](../input-spec/description1.txt) line 3 describes a school detective game, while [description2.txt](../input-spec/description2.txt) line 3 describes a clinic robot toy. The framework must not silently replace the Jira goal with either embedded goal or combine all three as equal deliverables.

There are two valid semantic treatments, depending on the user's designation:

- If those files are merely unrelated projects, classify them `UNRELATED_GOAL` and exclude them.
- If the user designates them as source material for the cartoon—as in the current request—classify each section, admit relevant story, character, setting, or design material as `IN_SCOPE_SOURCE_MATERIAL`, and exclude embedded instructions that demand a different final artifact.

This section-level distinction prevents both extremes: merging incompatible goals and throwing away useful adaptation material.

## What “generic” means for the resulting questions

Generic describes the agent algorithm, not the published wording.

The algorithm always performs the same operations:

- select goal;
- classify evidence;
- extract obligations;
- resolve only genuine gaps;
- plan task-domain knowledge;
- produce and criticize Question -> Answer entries;
- publish only after all guards pass.

The produced questions must change with the active goal. For the current Python-cartoon goal, useful specification-reader questions might cover narrative scope, scene and asset structure, selected animation approach, program architecture, timing and rendering behavior, audio handling, configuration, error behavior, testing, acceptance criteria, and packaging. These topics are not permanent prompt labels. They are produced as `GROUNDED_SYNTHESIS` only when needed to make the source-requested implementation specification complete.

With a policy input, the same algorithm would instead ask policy-domain implementation questions. With an illustration brief, it would ask composition, subject, style, asset, and acceptance questions. No prior domain vocabulary may survive the NEW reset.

## Run-invariant fields that expose the decision

The current invariant block includes:

- `RUN_MODE`
- `RESET_REASON`
- `RUN_INITIALIZATION_GATE`
- `ACTIVE_GOAL_ID`
- `ACTIVE_GOAL_ANCHOR`
- `GENERATED_ARTIFACT_RESET_CHECK`
- `MANAGED_ARTIFACT_PATH_CHECK`
- `SOURCE_CANDIDATE_COUNT`
- `SOURCE_INCLUDED_COUNT`
- `SOURCE_SCOPE_CHECK`
- `SOURCE_SNAPSHOT_CHECK`
- `EVIDENCE_MEMBERSHIP_CHECK`
- `SINGLE_RUN_STATE_CHECK`

They are defined at [text-spec-execution-firewall.md](../.github/agent-contracts/text-spec-execution-firewall.md), lines 63-79. A narrative statement such as “I checked the new files” cannot replace these fields.

## Critic responsibility

Before semantic `BASELINE` review, the Critic requires exact research and snapshot artifacts plus persisted `GUARD_CALL_BASELINE: PASS`. If any initialization artifact or equation is absent, misplaced, truncated, or malformed, it returns `WORKER_RESULT_REJECTED: INITIALIZATION_NOT_READY` with `ALLOWED_NEXT_ACTION: REPAIR_RUN_INITIALIZATION`. The Orchestrator repairs locally; the Critic never tells the user to rerun baseline.

The Critic does not merely check grammar. In BASELINE mode it rejects:

- two independent goals merged into one contract;
- a current claim supported by `outgoing/**`, old workflow state, prior `spec.md`, or another excluded path;
- source paths that are not snapshot members;
- a second RUN_ID or second canonical baseline;
- a newly invented value presented as a source fact;
- omission of a literal value that the selected source actually supplies.

The controlling review rule is at [text-spec-critic.agent.md](../.github/agents/text-spec-critic.agent.md), lines 14-20.

## Troubleshooting checklist

If the output still discusses the previous task:

1. Confirm that you entered `run`, not `resume`.
2. Verify `.spec-workflow/research.md` contains one RUN_ID and one `## Current Baseline`.
3. Verify `.spec-workflow/source-snapshot.md` contains only current candidate text.
4. Verify `outgoing/` is empty after initialization.
5. Read the goal/scope table and check every included and excluded reason.
6. Search research for a removed path or old domain term; either one makes `SINGLE_RUN_STATE_CHECK` fail.
7. Confirm every later `ACTION_AUTHORIZATION` recomputed all source-identity checks.

Do not repair contamination by editing a few old nouns. The Orchestrator invalidates the state, cleans or repairs the managed artifacts, and automatically retries NEW. It ends as `BLOCKED: RUN_INITIALIZATION_FAILED` only after repeated concrete edit/path failure.
