# 10 — Debugging

This chapter diagnoses VS Code GitHub Copilot invocation and workflow-state problems for the Markdown-only Text Spec agents. The workflow does not require a runnable software project, package manager, programming language, or test command.

## Select the correct custom agent

The user-facing entry point is `Text Spec Orchestrator` in `.github/agents/text-spec-orchestrator.agent.md`.

Its frontmatter declares:

- `target: vscode`;
- `user-invocable: true`;
- `disable-model-invocation: true`; and
- the three worker agents it may call.

This combination makes explicit selection important: choose **Text Spec Orchestrator** in the Copilot Chat custom-agent picker before sending the request. Do not select the Investigator, Interrogator, or Critic directly; their files declare `user-invocable: false` because they are worker roles used by the Orchestrator.

If the Orchestrator is not listed:

1. Confirm the workspace opened in VS Code is the repository root containing `.github/agents/`.
2. Confirm the file name ends in `.agent.md`.
3. Confirm the frontmatter name is `Text Spec Orchestrator` and `target` is `vscode`.
4. Confirm the three names under `agents:` exactly match the worker frontmatter names.
5. Confirm `.github/agent-contracts/text-spec-execution-firewall.md` exists and the Orchestrator's Markdown link resolves.
6. Confirm each `.agent.md` prompt is below GitHub's documented 30,000-character limit.
7. Refresh VS Code’s custom-agent discovery after agent or contract files change, then open a new Chat session and select the agent again.

If discovery or linked instructions still look wrong, right-click the VS Code Chat view and open **Diagnostics** to inspect loaded custom agents and instruction-file errors.

Refreshing discovery is editor troubleshooting. It does not modify workflow records or authorize a run.

## Invoke the workflow

With the Orchestrator selected, a short control utterance is enough:

- `run`
- `start`
- `go`
- `generate`
- `build`

Polite or punctuated equivalents have the same meaning.

`proceed`, `continue`, and `resume` continue the earliest incomplete gate only when one valid active Phase Gate Ledger exists. Without one, they begin a normal new run.

A bare control utterance is not task evidence. It does not mean “use defaults,” approve a source override, authorize code execution, or weaken any constraint.

## Verify source eligibility

The Orchestrator inventories every readable non-reserved textual candidate regardless of extension, authorship, directory name, or apparent domain. Fixed reserved/control trees are excluded by pattern:

- `.github/agents/**`;
- `.github/agent-contracts/**`;
- `.spec-workflow/**`;
- `spec.md`;
- version-control data;
- dependency data; and
- generated `outgoing/**` state.

All other readable text is semantically classified. In this teaching repository, `docs/**` and the root `README.md` are framework material unless the user explicitly designates particular domain content. Other generated output, prior references, unrelated goals, and unrelated implementation text are excluded with recorded reasons rather than hidden by a filename rule.

If no source is found, verify that task inputs are inside the opened workspace and are not stored only under an excluded path.

The agent proceeds without clarification when exactly one coherent source-backed goal and output exist. It asks only when no eligible source identifies a goal or unresolved conflicting goals would materially change the artifact.

## Symptom-based debugging

### Copilot says “no runnable project detected”

Expected behavior: the Orchestrator begins Markdown source discovery.

Likely causes:

- the Orchestrator was not selected;
- a general coding agent handled the message;
- the wrong workspace root is open; or
- eligible source files are missing or located under exclusions.

The Orchestrator does not require an entry point, runtime, package manager, build task, test command, or CI task before specifying a text-described goal. A technology explicitly named by active sources remains domain evidence; it is not a prerequisite for a runnable project.

### Copilot asks “What should I run?”

If active sources already identify a goal and output, this is not Orchestrator behavior. Recheck agent selection and source visibility.

### Copilot only announces a plan

Expected behavior: baseline work begins in the same turn. A plan-only response indicates the invocation did not follow the Orchestrator's selected-agent semantics.

### Advanced Autopilot stops after three automatic loops

First verify both prerequisites: the current session permission level is Autopilot and `chat.autopilot.advanced.enabled` is `true`. Only that combination has the documented VS Code 1.124 maximum of three automatic loops. The host limit cannot be raised by `.agent.md` instructions.

If the session uses Default Approvals or Bypass Approvals, do not diagnose a three-loop boundary. A Critic `REVISE` requires immediate internal correction and re-review; Default Approvals can pause only for a real tool-confirmation dialog or a genuine reviewed `HUMAN` IRQ. If both Advanced Autopilot prerequisites are true and VS Code explicitly reports its maximum, inspect `.spec-workflow/research.md`, keep accepted records and pending gates unchanged, and send `resume`. Only that explicit boundary permits `PAUSED: HOST_AUTOPILOT_LIMIT; send resume to continue the active run`.

This is not `BLOCKED: NON_CONVERGING`. That framework status requires the same normalized Critic defect to survive three completed targeted corrections without changed evidence or state. Host Autopilot continuations do not increment `SAME_DEFECT_REVISION_COUNT`. See [VS Code 1.124: Advanced Autopilot](https://code.visualstudio.com/updates/v1_124#_advanced-autopilot).

### Copilot stops after baseline and says “pick one”

This is invalid when the baseline is merely `REVISE`, its defects are internally correctable, and the host has not enforced its Autopilot boundary. A normal `run` authorizes work toward the whole active workflow across host-permitted execution segments. The Orchestrator must replace only the defective baseline records, invoke the BASELINE Critic again, and continue without asking whether it may produce the KQs.

Do not select either menu option. Inspect `.spec-workflow/research.md` for the earlier defect:

- planned KQs, `Question:`/`Answer:` content, quantities, or IRQ resolutions while `ACTIVE_PHASE: BASELINE`;
- accepted coverage while `ACCEPTED_KQ_COUNT: 0`;
- assigned IRQ records while `GENUINE_IRQ_COUNT: 0`; or
- a Critic treating zero pre-production KQs as a baseline failure.

Refresh custom-agent discovery and begin a new chat after correcting the definitions. The fixed baseline is extraction-only and a `REVISE` result is an automatic loop event, not a user handoff.

### A prior `spec.md` disappears during baseline

This is a state-integrity failure. A fresh run may remove stale `.spec-workflow/draft.md`, but it must leave an existing `spec.md` physically unchanged until the final write guard passes. Restore the prior publication before continuing and invalidate the run that deleted it.

### Copilot creates source code or offers a script

The selected workflow is Markdown-only and does not execute the downstream goal. Verify that the current chat is using Text Spec Orchestrator and that the user request did not switch to a different task outside this agent.

### No `spec.md` exists at startup

This is normal. `spec.md` is an output. A normal run creates and rereads research first, then creates and verifies the exact source snapshot and baseline-call guard. It creates `.spec-workflow/draft.md` only after production acceptance and writes `spec.md` only after final readiness.

### `.spec-workflow` is empty

This is also a valid clean start. The Orchestrator assigns a run identity and creates current-run records as the phases advance.

### An old `spec.md` remains after a failed run

This may be correct rollback behavior. The previous contents are retained verbatim in the verified `.spec-workflow/prior-spec.md` checkpoint. After final readiness the candidate temporarily replaces the file; postpublication failure restores the checkpoint, while success keeps the candidate.

Inspect `.spec-workflow/research.md` to distinguish:

- the prior published file;
- the current run identity;
- current gate states; and
- whether postpublication passed or failed.

Do not infer current success merely from the presence of `spec.md`.

### Research says `REVISE`, but `spec.md` appears

That state is invalid for a newly published result. `REVISE` is not baseline acceptance, production cannot begin, and a missing accepted draft blocks publication.

### The draft contains `...`, `1..5`, or “complete later”

These are placeholders, not compact valid content. The production and final checks must fail. Reduce the production batch and regenerate complete entries.

### Questions ask the author how to write the specification

These are meta-questions. When a question-native downstream artifact has the relationship `IS_FINAL_ARTIFACT` or `EMBEDS_EXACT_UNITS`, the published question must be the exact respondent-facing unit. With `DEFINES_SCHEMA` or `REFERENCES_ONLY`, a source-specific `SPEC_KNOWLEDGE_QUERY` may instead explain the schema, selection, or use knowledge. Global rules belong in framework sections or attached fields.

### A recipe or plan contains no Q&A knowledge base

For non-question-native work, the published `SPEC_KNOWLEDGE_QUERY` entries ask concrete domain implementation questions and answer them fully. A direct prose deliverable with no published knowledge base fails final review.

### Copilot reports `READY` with zero KQs or incomplete coverage

This result is internally contradictory and must be rejected. `PREPUBLICATION` is not a Critic mode; the valid whole-draft mode is `FINAL`. `PREPUBLICATION_STATE_CHECK: PASS` is only one of 18 final checks, not an overall decision.

The following non-normative pattern demonstrates the diagnosis. It intentionally does not cite a live `.spec-workflow/research.md` line because generated run records are replaceable and must never be documented with guessed or stale locations:

| Durable or reported fact | Required result |
|---|---|
| Baseline remained `REVISE` | Baseline was pending; resolution, production, and drafting were forbidden. |
| Planned Q&A and quantities appeared in baseline | Reject the baseline for phase leakage and replace only its defective baseline records. |
| Zero accepted KQs but one covered obligation was claimed | Recalculate counts; pre-production coverage is zero and final readiness is forbidden. |
| A user menu appeared during correctable `REVISE` | Continue the same-mode correction loop automatically. |
| The prior publication was deleted early | Restore it, invalidate the run, and require a verified initialization checkpoint next time. |

Recover from the earliest invalid gate, not by patching the final prose:

1. Mark the contaminated run invalid, restore the last verified Q&A publication, remove its unaccepted draft, and preserve a concise reset record for diagnosis.
2. Refresh VS Code custom-agent discovery after the `.agent.md` changes.
3. Open a new Copilot Chat, select **Text Spec Orchestrator**, and start a normal new run.
4. Confirm the complete baseline is persisted and receives `ACCEPT` before any Q&A production or draft file is created.
5. If the run stops, inspect the first failed `ACTION_AUTHORIZATION` record and correct only that phase.

### Final review says READY but there is no success response

Check `POSTPUBLICATION_GATE`. `READY` approves the candidate draft; success is reported only after the written file receives `POSTPUBLICATION_STATE_CHECK: PASS` and the gate is recorded as `PASS`.

## Debug the current phase

Read the Run Invariant Block and Phase Gate Ledger to find the first incomplete or invalid gate. Then inspect the latest `ACTION_AUTHORIZATION` for the action that Copilot attempted.

| First unhealthy gate | Inspect next |
|---|---|
| Baseline | Managed cleanup, one-run state, exact source snapshot, active-goal/scope table, evidence membership, Artifact Contract, obligations, Critic decision |
| Resolution | IRQ legitimacy, evidence, route, accepted resolution, unresolved material effect |
| Production | Per-pair decisions, placeholder scan, enumerations, scales, batch size |
| Draft | Accepted-entry list, Q&A envelope, manifest, leaked research material |
| Final | Named final checks, cross-unit totals, traceability, state agreement |
| Postpublication | Exact draft/file equality, repeated checks, rollback result |

Always debug the earliest failing gate. Repairing a later artifact cannot compensate for an unaccepted earlier phase.

### A NEW run still discusses the previous input

This is run-state contamination. Inspect the physical files, not the chat summary:

1. `.spec-workflow/research.md` must contain exactly one RUN_ID and one `## Current Baseline`.
2. `.spec-workflow/source-snapshot.md` must be the only identity snapshot and must contain one sorted path and source block per current non-reserved candidate, with exact logical lines through live EOF.
3. `outgoing/**` must have been emptied during NEW and must not appear in evidence.
4. Every obligation location must belong to a source or section admitted by the current scope table.
5. Removed paths, an old domain noun used as a current fact, a second baseline, or an appended invalid-run narrative makes `SINGLE_RUN_STATE_CHECK` fail.

Do not patch a few stale nouns. Enter `run` and require the guarded cleanup transaction. Human input files are preserved; only managed generated state is removed.

### A snapshot exists but `.spec-workflow/research.md` does not

This is an uncommitted initialization, not a snapshot blocker and not an active run. A RUN_ID written only in `.spec-workflow/source-snapshot.md` has no authority because the invariant must live in `.spec-workflow/research.md`.

The Orchestrator must not ask you to update research, rerun baseline verification, or tell it to proceed. It must remove the managed partial state and automatically execute NEW again, creating and rereading research first. Before baseline, verify every snapshot block reaches the live editor-reported EOF and that its declared count, contiguous row count, highest row ID, and exact text all agree.

Current implementation note: the execution contract's early phase-ownership summary describes any managed deletion failure as `BLOCKED: RUN_INITIALIZATION_FAILED`, while its later detailed fail-closed rule permits local repair/retry and reserves that terminal status for repeated concrete edit/path failure. This guide follows the later, more specific repair rule. Maintainers should standardize those two implementation sentences before treating the first failed deletion attempt as a machine-checkable terminal condition.

## Common state-recovery choices

### Start a normal new run

Use this when inputs or the goal changed, or the previous run is unrelated/corrupt. `run` deletes and verifies all managed workflow/outgoing files, creates and rereads the research bootstrap first, writes a complete exact source snapshot, retains prior `spec.md` only for rollback, and delays draft creation until production acceptance.

### Resume

Use this only when one valid ledger, active goal, exact source snapshot, scope membership, and physical artifacts still match. A changed source makes RESUME invalid and automatically becomes NEW.

### Stop as blocked

Use this when a material source decision cannot be safely derived or isolated. Report the specification blocker; do not translate it into a missing executable or fabricate an answer.

## Debugging checklist

- [ ] Is the repository root open in VS Code?
- [ ] Is **Text Spec Orchestrator** selected in Copilot Chat?
- [ ] Are all four `.agent.md` files discoverable?
- [ ] Do worker names exactly match the Orchestrator’s `agents:` list?
- [ ] Are active task sources outside excluded paths?
- [ ] Does one coherent source-backed goal exist?
- [ ] Did NEW remove and verify all `.spec-workflow/**` and `outgoing/**` state before creating the current ledger?
- [ ] Was `.spec-workflow/research.md` created and reread before the matching snapshot?
- [ ] Does `MANAGED_ARTIFACT_PATH_CHECK` prove that `.spec-workflow/source-snapshot.md` is the only identity snapshot?
- [ ] Do candidate/path/block counts agree, and does each block satisfy `LINE_COUNT == live EOF line count == contiguous row count == highest row ID` with exact row text?
- [ ] Are initialization, reset, scope, snapshot, evidence-membership, and single-run checks `PASS`?
- [ ] Did source discovery start in the invocation turn?
- [ ] Is there one valid current-run Phase Gate Ledger?
- [ ] Is the Run Invariant Block complete and reconciled with physical files?
- [ ] Are `CURRENT_REVISION_DEFECT_SIGNATURE` and `SAME_DEFECT_REVISION_COUNT` present and unchanged by host continuations?
- [ ] Did every attempted major action have `GUARD_RESULT: PASS`?
- [ ] Is the earliest incomplete gate being addressed first?
- [ ] Was a host Autopilot stop kept separate from `BLOCKED: NON_CONVERGING`?
- [ ] Are `REVISE` and failed checks preventing advancement?
- [ ] Is `spec.md` written only after an accepted draft and final readiness?
- [ ] Did postpublication validation pass before success was reported?

## Related reading

- [06 — Critic and Convergence](06_Critic_and_Convergence.md)
- [07 — Observability](07_Observability.md)
- [08 — Consistency Checks](08_Consistency_Checks.md)
- [09 — Run Health](09_Run_Health.md)
- [Text Spec Orchestrator](../.github/agents/text-spec-orchestrator.agent.md)
- [Text Spec Investigator](../.github/agents/text-spec-investigator.agent.md)
- [Text Spec Interrogator](../.github/agents/text-spec-interrogator.agent.md)
- [Text Spec Critic](../.github/agents/text-spec-critic.agent.md)
