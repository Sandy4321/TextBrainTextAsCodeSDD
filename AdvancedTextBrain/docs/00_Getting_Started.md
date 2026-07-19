# Getting Started with the Text Specification Agents

This repository contains a Markdown-only specification-driven development workflow for VS Code GitHub Copilot custom agents. Its job is to turn human-authored workspace text into a source-grounded `spec.md` Question-and-Answer knowledge base. It does not perform the downstream work described by that specification.

## Learning goals

By the end of this chapter, you should be able to:

- identify the four custom agents and their responsibilities
- distinguish task sources, workflow memory, a candidate draft, and the published specification
- start a new run from a short control message such as `run`
- recognize the minimum shape of a valid published knowledge entry
- tell a normal fresh workspace from a blocked or incomplete run
- diagnose the most common first-run failures

## What the framework produces

The immediate output is always `spec.md`. The file is a source-specific knowledge base containing concrete `Question:` and `Answer:` entries. The questions are never generic questions about writing a specification.

The downstream result varies by task. It might be a recipe, an interview guide, a policy, an educational activity, an illustration brief, or another text-described result. The framework discovers that result from the active source files.

The distinction matters:

- `spec.md` records the knowledge needed for downstream work.
- A separately named downstream artifact is not created by this workflow.
- A real-world action, such as preparing food or conducting an interview, is not performed by this workflow.

## Repository map

| Path | Purpose |
| --- | --- |
| `.github/agents/text-spec-orchestrator.agent.md` | User-selectable custom agent that owns the full workflow and writes approved Markdown artifacts. |
| `.github/agents/text-spec-investigator.agent.md` | Read-only evidence and production specialist invoked by the Orchestrator. |
| `.github/agents/text-spec-interrogator.agent.md` | Read-only specialist that proposes internal questions only for genuine gaps. |
| `.github/agents/text-spec-critic.agent.md` | Read-only admission and readiness gate. |
| `.github/agent-contracts/text-spec-execution-firewall.md` | Reusable fail-closed invariant and action-guard instructions linked by the Orchestrator; not a fifth agent. |
| `jira.md` and `input-spec/**` | Protected candidate task text. Inclusion and section-level relevance are semantic rather than extension-based. |
| `.spec-workflow/source-snapshot.md` | Exact current-run candidate-source identity; never task evidence. |
| `.spec-workflow/research.md` | Current-run shared memory, ledgers, evidence, and internal resolutions. |
| `.spec-workflow/draft.md` | Candidate specification assembled only from accepted content. |
| `.spec-workflow/prior-spec.md` | Verified temporary rollback copy of a publication that existed at run initialization. |
| `outgoing/**` | Reserved legacy generated scratch; removed on NEW and never admitted as evidence. |
| `spec.md` | Published specification, written only after final acceptance. |
| `docs/` | Educational material. The Orchestrator excludes it from task evidence unless the user explicitly makes it an active source. |

## The current mixed-source test

The current workspace deliberately tests goal selection:

- [jira.md](../jira.md) selects a Python cartoon-film implementation specification and requests what/why/how knowledge.
- [description1.txt](../input-spec/description1.txt) contains school detective-game material plus an embedded game-specification directive.
- [description2.txt](../input-spec/description2.txt) contains clinic robot-toy material plus an embedded toy-specification directive.

The Jira goal controls the downstream artifact. The two text files are not silently merged as equal deliverables. If designated as cartoon source material, relevant descriptive sections may be admitted while incompatible embedded output directives are excluded. Otherwise they are recorded as unrelated. This classification is derived again after every NEW reset.

## How to start a run in VS Code

1. Open Copilot Chat in VS Code.
2. Select **Text Spec Orchestrator** from the custom-agent selector.
3. Send `run`, `start`, `go`, `generate`, or `build`.
4. Let the Orchestrator complete one uninterrupted initialization transaction in the same interaction: clean managed state, inventory and select one goal, create and reread `.spec-workflow/research.md` first, create and verify `.spec-workflow/source-snapshot.md` second, persist and reread `GUARD_CALL_BASELINE`, and only then begin baseline analysis.

A bare control word means “run this text-to-spec workflow.” It never means “find an executable.” The Orchestrator must not ask which program, task, server, or command to run when the workspace sources already identify one coherent specification goal.

`proceed`, `continue`, or `resume` continues only when the ledger, active goal, exact source snapshot, scope membership, and physical files still match. Any mismatch initializes NEW.

## What happens after `run`

The Orchestrator performs these activities in order:

1. Deletes and verifies managed `.spec-workflow/**` and `outgoing/**` state.
2. Inventories every readable non-reserved textual candidate without deleting human inputs.
3. Selects one authoritative goal and classifies files or sections by semantic relevance.
4. Creates and rereads the pending `.spec-workflow/research.md` ledger first.
5. Writes the exact-path source snapshot in bounded chunks through EOF and verifies every line/count equation.
6. Commits the initialization checks and baseline-call guard in research.
7. Infers the three-layer Artifact Contract and source obligations.
8. Sends the extraction-only baseline to the Critic and revises it automatically until accepted or blocked.
9. Uses the Interrogator only if a genuine material gap remains.
10. Plans and produces complete task-domain `KQ-*` and applicable `UNIT-*` pairs.
11. Assembles the candidate draft, performs final review, writes `spec.md`, and postpublication-checks it before reporting success.

A snapshot without its matching research ledger is uncommitted initialization state, not an active run. A root-level, basename-only, duplicate, truncated, or structurally inconsistent snapshot also fails initialization. The Orchestrator repairs the managed state locally before `BASELINE`; it does not ask the user to create research, rerun baseline, or tell it to proceed.

The process is deliberately stricter than “write a plausible document.” It provides evidence that every required field, item, constraint, and finite scale has been handled.

## VS Code's outer Autopilot boundary

The workflow's Critic retries are internal workflow actions, not permission approvals. In Default Approvals, `REVISE` triggers correction and re-review immediately in the active response; only an actual tool operation might show a configured security-confirmation dialog. The three-loop maximum applies only when the current permission level is Autopilot and `chat.autopilot.advanced.enabled` is `true`. It does not apply to Default Approvals, Bypass Approvals, or Autopilot with the advanced setting disabled.

If the host stops before postpublication, the run is not automatically `BLOCKED` or invalid. Its accepted records and incomplete gates remain in `.spec-workflow/research.md`. Send `resume`; the Orchestrator validates the existing ledger and continues the earliest incomplete gate. The framework's separate three-count rule applies only when the same normalized Critic defect survives three completed targeted corrections. See the official [VS Code 1.124 release notes](https://code.visualstudio.com/updates/v1_124#_advanced-autopilot).

## What a published entry looks like

A valid knowledge entry has a universal envelope and task-specific content. A compact current-goal example is:

> ### KQ-001 — Python cartoon-film implementation target
> **Question:** What implementation target and decision coverage must the cartoon-film plan define?
> **Answer:** Define a complete plan for implementing a cartoon film in Python. The knowledge base must state the intended film and behavior, explain why the selected decisions support that target, and give enough concrete implementation direction for a developer to code and verify it.
> **Source basis:** OBL-001 — Implement a Python cartoon film — jira.md:5; OBL-002 — Cover what, why, and how to code — jira.md:9.
> **Acceptance check:** A downstream developer can identify the implementation target, rationale, implementation approach, and verification expectations without consulting workflow internals.

This is a task-domain question. “What must `spec.md` contain?” would be a meta-question and must be rejected.

## Fresh runs, active runs, and prior specifications

The absence of `spec.md` is normal before the first successful run.

For NEW, the Orchestrator deletes all managed workflow and outgoing files, verifies cleanup, creates and rereads exactly one `.spec-workflow/research.md` ledger first, and then creates and verifies exactly one `.spec-workflow/source-snapshot.md`. It never appends below old research or accepts a misplaced managed artifact. Human input remains untouched. A new `.spec-workflow/draft.md` appears only after production acceptance. An existing `spec.md` is transactionally checkpointed but excluded from current evidence.

If postpublication verification fails:

- the earlier file is restored when one existed
- the failed new file is removed when no earlier file existed

This protects users from a partially written or falsely approved specification.

## Why the workflow uses custom agents

The roles create useful separation of duties:

- The Orchestrator controls state and writes files.
- The Investigator gathers evidence and creates proposed content.
- The Interrogator isolates only real unknowns.
- The Critic independently rejects incomplete or unsupported work.

No worker may silently approve its own output, and the read-only workers cannot directly publish a file.

## Failure modes

### The agent asks what executable to run

The selected agent has not followed its invocation semantics. A bare `run` should start source discovery, not executable discovery.

### `spec.md` is treated as an input

The published specification is an output. A missing file is not a blocker. A prior file is not source evidence.

### The output contains generic questions

Questions such as “What are the requirements?” reveal that internal workflow reasoning leaked into the deliverable. Published questions must be source-specific domain questions or exact downstream respondent-facing questions.

### Required content is abbreviated

Ellipses, bare ranges, endpoint-only scales, empty lists, and “see template” references are not complete content. The batch must be reduced and regenerated.

### The agent announces future validation

Required validation is a phase gate, not an optional next step. “The strict review can run later” means the run has failed.

### A source constraint disappears

Missing information and generic user control phrases never negate an explicit fact. Material safety and compliance constraints require an explicit, fully recorded override before they can change.

## Practical checklist

- [ ] VS Code Copilot Chat has **Text Spec Orchestrator** selected.
- [ ] Eligible task sources state one coherent goal or requested result.
- [ ] `spec.md` is treated as output, not prerequisite evidence.
- [ ] A bare `run` begins source discovery without asking for an executable.
- [ ] `.spec-workflow/research.md` contains the current Phase Gate Ledger.
- [ ] `MANAGED_ARTIFACT_PATH_CHECK` passes for the exact managed paths, with no root-level or duplicate snapshot.
- [ ] Snapshot candidate, path, and source-block counts agree; every block reaches live EOF with matching `LINE_COUNT`, contiguous row count, highest row ID, and exact text.
- [ ] No dependent phase starts before its prerequisite gate is accepted.
- [ ] Every published KQ has `Question`, `Answer`, source basis, and acceptance check.
- [ ] Questions are specific to the active task.
- [ ] No placeholder substitutes for required content.
- [ ] Success is reported only after postpublication verification passes.

## Related reading

- [Agent Loops Explained Like You Are 10](16_Agent_Loops_Explained_for_Age_10.md)
- [Agentic AI for Beginners](01_Agentic_AI_for_Beginners.md)
- [Framework Architecture](02_Framework_Architecture.md)
- [Workflow Engine](03_Workflow_Engine.md)
- [Shared Memory](04_Shared_Memory.md)
- [Interrogation Algorithm](05_Interrogation_Algorithm.md)
- [Text Spec Orchestrator definition](../.github/agents/text-spec-orchestrator.agent.md)
