# User Manual

## What this system does

The system reads human-authored task sources and publishes a source-specific `spec.md` Question → Answer knowledge base. It is designed to prepare reliable knowledge for a later implementation or content-production step.

It does not execute the downstream goal. For example, it can specify a recipe but does not cook it; specify an event but does not book it; specify interview questions but does not conduct the interview; specify software behavior but does not implement the code.

## Requirements

- Visual Studio Code
- GitHub Copilot Chat with custom-agent support
- this workspace opened at its root
- one or more Markdown or plain-text files describing a coherent task

The framework uses VS Code custom agents under `.github/agents`. It is not a Claude Code or Google Antigravity project, and there is no terminal program to launch.

## Starting a run

1. Open the repository folder in VS Code.
2. Open Copilot Chat.
3. Use the agent picker to select **Text Spec Orchestrator**.
4. Enter `run`.

Equivalent start controls are `start`, `go`, `generate`, or `build`, including polite or punctuated forms.

Expected first action: source discovery and baseline analysis begin in the same turn.

Unexpected behavior: Copilot scans for package files, says “no runnable project detected,” suggests a script, asks for a programming language, or asks “what should I run?” That indicates the wrong agent/surface or a custom-agent loading problem.

## Why selecting the agent matters

The word `run` has no universal meaning in ordinary Agent mode. The custom Orchestrator defines it as a workflow control. If the Orchestrator is not selected, Copilot may interpret it as a request to execute software.

The Orchestrator is user-invocable. The Investigator, Interrogator, and Critic are hidden worker agents intended to be called by the Orchestrator.

## Preparing source files

Sources should make the goal and acceptance conditions discoverable. Useful content includes:

- requested artifact or outcome
- audience and intended use
- required sections or units
- quantities and fixed counts
- constraints and strict operators
- risks, safety, compliance, accessibility, or privacy rules
- ordering and dependencies
- acceptance criteria
- preferences clearly distinguished from requirements

You do not need fixed filenames or extensions. The framework inventories every readable non-reserved textual candidate and derives authority and relevance from meaning, not path, authorship, extension, or modification time. Fixed reserved/control trees receive pattern exclusions rather than becoming candidates.

Agent definitions, linked contracts, generated workflow state, legacy generated output, version-control data, dependency data, and prior `spec.md` are always excluded from current task evidence. Repository documentation is normally framework material in this teaching project. If the user explicitly designates a document or mixed-purpose input as source material, only its goal-relevant sections are admitted; embedded instructions for a different deliverable are excluded with reasons.

## Fresh run versus resume

Use a start word for a normal fresh build. `run`, `start`, `go`, `generate`, and `build` always mean **NEW**. The Orchestrator deletes every file in the agent-owned `.spec-workflow/**` and `outgoing/**` namespaces and verifies that they are empty. It inventories candidates and selects one goal, creates and rereads `.spec-workflow/research.md` first with one run identity and pending scope/control records, creates and verifies `.spec-workflow/source-snapshot.md` second, then persists and rereads `GUARD_CALL_BASELINE`. It never appends the new task to old research or calls a worker halfway through initialization. A candidate draft is created only after production acceptance.

If `spec.md` already exists, the NEW transaction preserves it through a verified `.spec-workflow/prior-spec.md` rollback checkpoint and leaves publication unchanged until the write guard passes. After final readiness, the candidate temporarily replaces `spec.md` for verification; success removes the checkpoint, while failure restores it or removes the failed new file when no prior spec existed. Human inputs, framework files, and unknown files are preserved throughout cleanup.

Use `proceed`, `continue`, or `resume` only when you want to continue one valid active Phase Gate Ledger from its earliest incomplete gate. Resume requires the same goal, exact candidate-path and logical-line snapshot, same scope decisions, one run identity, and matching physical artifacts. Any mismatch converts the operation to NEW with a recorded reset reason.

Do not manually combine records from different runs. Source changes after baseline acceptance normally justify a fresh run because obligations, line locations, contracts, and coverage may have changed.

## Advanced Autopilot's outer limit

The maximum of three automatic continuations applies only when the current session permission level is Autopilot and `chat.autopilot.advanced.enabled` is `true`. It does not apply to Default Approvals, Bypass Approvals, or Autopilot with the advanced setting disabled. A custom `.agent.md` file cannot increase the optional host limit.

In Default Approvals, a Critic `REVISE` is not an approval prompt: the Orchestrator corrects and re-reviews automatically in the active response. Only an actual tool operation may need configured confirmation. If the optional Advanced boundary is explicitly reached, inspect `.spec-workflow/research.md` and send `resume`; do not confuse it with `SAME_DEFECT_REVISION_COUNT`. See [VS Code permission levels](https://code.visualstudio.com/docs/agents/approvals#_permission-levels) and [VS Code 1.124: Advanced Autopilot](https://code.visualstudio.com/updates/v1_124#_advanced-autopilot).

## Files you will see

### `.spec-workflow/research.md`

Internal durable memory. It should include source inventory, artifact layers, obligations, field contracts, resolution records, completion matrices, coverage, critic decisions, phase gates, and publication manifest.

### `.spec-workflow/source-snapshot.md`

Exact current non-reserved candidate paths and one line-addressed source block per candidate used to prove freshness. Candidate/path/block counts must agree; in every block, `LINE_COUNT`, live line count through EOF, contiguous row count, and highest row ID must agree, and every row text must match. It is control memory, never task evidence. It is valid only at `.spec-workflow/source-snapshot.md` with a RUN_ID matching research and no duplicate identity copy. NEW replaces it; RESUME compares live sources with the stored snapshot and converts to NEW on any mismatch rather than rewriting the snapshot to manufacture a match.

### `.spec-workflow/draft.md`

The exact candidate knowledge base assembled from accepted production entries. It is not a scratch outline.

### `.spec-workflow/prior-spec.md`

A temporary, byte-for-byte rollback checkpoint created only when `spec.md` already exists at run initialization. It is excluded from task evidence. Postpublication success removes it; postpublication failure uses it to restore and verify the prior file, without a contract requirement to delete the checkpoint at that failure point.

### `spec.md`

The published knowledge base. It appears or is replaced only after final readiness and is verified again after writing.

### `outgoing/`

A legacy generated-output namespace. A NEW run empties it and this specification-only workflow does not recreate its old contents. Do not store human input there.

## Understanding progress

Look for the Phase Gate Ledger:

1. `BASELINE_GATE`
2. `RESOLUTION_GATE`
3. `PRODUCTION_GATE`
4. `DRAFT_GATE`
5. `FINAL_GATE`
6. `POSTPUBLICATION_GATE`

The only fully successful ending is `POSTPUBLICATION_GATE: PASS` after `FINAL_GATE: READY`.

`REVISE` means the current proposal needs a targeted correction and another review. It is not a pass. `BLOCKED` prevents dependent phases. Formal `FINAL_GATE: CONDITIONAL` signals visible isolated gaps, prevents publication, and must not be represented as readiness.

## When the system asks you a question

Most source facts should not be turned into questions. The user is asked only when a reviewed internal `IRQ-*` is routed to `HUMAN` because the decision is material and cannot be resolved by evidence, grounded synthesis, or a safe default.

The question should identify:

- the exact missing decision
- affected obligations
- relevant source evidence
- downstream effect
- observable resolution condition

If a question is generic, speculative, or asks whether an explicit constraint should be removed, do not answer it blindly; inspect the research ledger and source mapping.

## Meaning of “use defaults”

`use defaults` means:

> Choose conservative, source-compatible assumptions only for genuinely missing optional decisions.

It does not mean:

- none
- no
- unrestricted
- ignore the source
- remove a constraint
- approve an unsafe interpretation
- accept all proposed content

A material conflict or safety constraint requires exact treatment. A broad phrase cannot override it.

## Reviewing the result

Do not judge only by fluency. Check that:

- questions are useful to the downstream reader or correct respondent
- answers contain actual decisions/content, not future instructions
- all named/repeated source items receive complete coverage
- every required field is instantiated
- conditional items include trigger and result
- all finite scale levels are individually described
- quantities, totals, batches, order, and dependencies reconcile
- source locations exist and support the claim
- assumptions and conflicts are visible
- no internal IRQ or workflow meta-question appears

Use [Q&A Spec Manual](QA-SPEC-MANUAL.md) for the full rubric.

## Common problems

### “No runnable project detected”

Cause: Copilot interpreted `run` as code execution.

Actions:

1. Confirm **Text Spec Orchestrator** is selected.
2. Confirm the agent appears in the workspace custom-agent list.
3. Open VS Code customizations diagnostics and look for frontmatter/tool errors.
4. Start a new chat with the Orchestrator selected.
5. Enter `run` again.

### The agent creates code

Stop the run. The selected surface is not following this Markdown-only contract. The Orchestrator's tool set intentionally omits terminal execution, and its body forbids scripts as substitutes.

### It asks generic interviewer questions

Distinguish operational audience from the spec author. When the required native unit is a candidate-facing interview question, the published question must address the candidate and assess the detected source competency. Workflow questions belong only in research.

### It produces a generic recipe/event/software specification

Apply the evidence-bounded specificity test: each question's purpose, mapped obligations, answer, and acceptance condition must be supported by admitted current-source evidence. Reusable wording is allowed when the sources intentionally request it; stale vocabulary or claims unsupported by the selected goal are not. Check the scope table, snapshot membership, and production evidence.

### It continues the previous task after inputs changed

Enter `run`, not `resume`, and inspect the new initialization records. `.spec-workflow/**` and `outgoing/**` must have been emptied before new control memory was written; `.spec-workflow/research.md` must be created and reread first and contain one RUN_ID and one canonical baseline placeholder/current baseline; `.spec-workflow/source-snapshot.md` must then contain every current candidate line through EOF and satisfy all path/block/count/row/text equations. If research is absent, no active ledger exists: the Orchestrator must initialize NEW automatically and must not ask you to update research, rerun baseline, or approve proceeding. An old run ID, removed path, excluded-path evidence, misplaced or truncated snapshot, or prior-domain decision requires automatic local repair or a clean NEW retry. Publication stops during repair; terminal `BLOCKED: RUN_INITIALIZATION_FAILED` is reserved for repeated concrete edit/path failure.

### Required fields contain placeholders

Any `...`, `1..5`, “same as above,” blank field, endpoint-only scale, or future instruction fails production. The pair must be regenerated at a smaller batch size if necessary.

### A baseline says `REVISE`, but a draft exists

The run has violated gate order. A draft created before `BASELINE_GATE: ACCEPT` is not eligible for publication. Restart or repair from the baseline phase; do not treat the draft as accepted.

### Source line numbers are wrong

The claimed evidence is unverified. Re-inventory current files and regenerate verified workspace-relative locations. Do not fix this by broadening references to fictitious ranges.

### `spec.md` exists but postpublication is missing

The write is not proven complete. The Critic must compare it to the accepted draft and repeat the required checks. Until `PASS`, the final file is not an accepted current-run publication.

## Verifying custom-agent loading

Current VS Code provides an Agent Customizations editor and a chat customization diagnostics view. Use them to verify:

- all four `.agent.md` files load without errors
- the Orchestrator appears in the agent picker
- the three workers are available as allowed subagents
- the Orchestrator has `read`, `search`, `edit`, and `agent`
- workers have read/search-only tools as defined

See [Custom agents in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-agents) and [Subagents in Visual Studio Code](https://code.visualstudio.com/docs/agents/subagents).

## Safe editing guidance

When modifying the framework:

- change the smallest responsible agent contract
- keep the Orchestrator as the only writer
- preserve domain neutrality
- use stable headings and IDs rather than fragile line-number references in documentation
- test at least two unrelated domains
- use fresh runs for before/after comparisons
- never “fix” a quality failure by weakening the Critic

## Completion checklist

- [ ] Orchestrator was selected before invocation.
- [ ] Eligible sources describe one coherent goal.
- [ ] NEW initialization removed stale managed workflow and outgoing files.
- [ ] `.spec-workflow/research.md` was created and reread before the matching snapshot.
- [ ] `RUN_INITIALIZATION_GATE`, reset, managed-path, active-goal, scope, snapshot, evidence-membership, and single-run checks pass.
- [ ] Snapshot candidate/path/block counts and every per-block live-count/row-count/highest-ID/EOF/text equation pass.
- [ ] Source inventory and locations are verified.
- [ ] All phase gates advanced in order.
- [ ] Every required unit and field passed production checks.
- [ ] `.spec-workflow/draft.md` and the research-only manifest existed before final review.
- [ ] `FINAL_GATE` is `READY`.
- [ ] `spec.md` contains at least one accepted KQ.
- [ ] `POSTPUBLICATION_GATE` is `PASS`.
- [ ] The final chat reports specification status, not downstream execution.

## Related reading

- [Getting Started](00_Getting_Started.md)
- [Generic Usage](GENERIC-USAGE.md)
- [Visible Workflow](VISIBLE-WORKFLOW.md)
- [Debugging](10_Debugging.md)
- [Run Health](09_Run_Health.md)
