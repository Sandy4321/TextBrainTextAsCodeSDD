# Text Brain  Agent QA Tutorial

This repository is an educational, Markdown-only implementation of a spec-driven development (SSD) agent loop for **VS Code GitHub Copilot custom agents**. It converts source documents into a source-specific `spec.md` Question → Answer knowledge base, reviews that knowledge through a critic loop, and publishes only after explicit quality gates pass.

There is no Python application to run. The implementation is the set of `.agent.md` custom agents in [`.github/agents`](.github/agents/) plus their linked reusable [execution contract](.github/agent-contracts/text-spec-execution-firewall.md). GitHub Copilot reads those Markdown instructions, invokes the worker agents, and writes Markdown run artifacts.

## What this project teaches

- how an orchestrator coordinates specialized worker agents
- how source text becomes obligations, internal questions, downstream units, and published Q&A
- why internal investigation questions must never leak into `spec.md`
- how a read-only critic controls admission, revision, convergence, and publication
- how Markdown ledgers act as shared memory and observable workflow state
- how fail-closed invariant counts and action guards prevent a worker's contradictory `READY` result from publishing
- how the same framework remains generic while each result stays task-specific
- how to diagnose a stalled, skipped, incomplete, or misleading run

## The implementation at a glance

| Agent | Responsibility | May write files? |
|---|---|---:|
| **Text Spec Orchestrator** | Owns the run, phase gates, delegation, assembly, and publication | Yes, only the allowed Markdown artifacts |
| **Text Spec Investigator** | Reads evidence and proposes baselines, resolutions, Q&A entries, and downstream units | No |
| **Text Spec Interrogator** | Identifies only genuine material gaps as internal `IRQ-*` records | No |
| **Text Spec Critic** | Accepts, revises, or blocks baseline, internal resolution, production, final, and postpublication records | No |

The Orchestrator links the separate fail-closed execution contract before guarded work. This keeps the VS Code `.agent.md` entry prompt below GitHub's documented 30,000-character limit while preserving exact invariant and authorization rules. The linked file has no frontmatter and is not a fifth agent.

The main information flow is:

> source text → `OBL-*` obligations → artifact and field contracts → optional `IRQ-*` resolution → `UNIT-*`/`KQ-*` production pairs → critic checks → `draft.md` → final review → `spec.md` → postpublication verification

## Quick start in VS Code

1. Open this folder in VS Code with GitHub Copilot enabled.
2. Put human/task inputs in `jira.md`, `input-spec/**`, or other clearly owned text files. Never store input under reserved `.spec-workflow/**` or `outgoing/**`.
3. In Copilot Chat, select **Text Spec Orchestrator** as the custom agent.
4. Enter `run`. This always performs a NEW-run cleanup and source-scope selection; it never resumes old evidence.
5. Let the current execution segment progress. If Advanced Autopilot reaches its host limit before completion, send `resume`; do not switch to a general coding agent or restart the run.
6. Inspect `.spec-workflow/source-snapshot.md`, `research.md`, `draft.md`, the temporary prior-spec checkpoint when present, and published `spec.md`.

`run`, `start`, `go`, `generate`, and `build` always mean NEW: delete verified managed state, create `research.md` first, snapshot current text through EOF, select one active goal, and commit one ledger. `proceed`, `continue`, and `resume` continue only when the stored goal, exact snapshot, scope, one-run state, and artifacts still match; otherwise they initialize NEW. An orphan snapshot is not an active run, and `REVISE` is an internal correction decision—not a “pick one” menu.

When the current permission level is **Autopilot** and Advanced Autopilot is enabled (`chat.autopilot.advanced.enabled: true`), VS Code 1.124 documents at most three automatic continuation loops before the host stops. That outer platform limit is different from this framework's three-unchanged-defect `BLOCKED: NON_CONVERGING` rule. The agent files cannot override the host. If that optional boundary stops a nonterminal run, the current Markdown ledger remains active; send `resume` to continue the same run rather than restarting it. See the official [VS Code 1.124 Advanced Autopilot notes](https://code.visualstudio.com/updates/v1_124#_advanced-autopilot).

That maximum applies only when the current permission level is **Autopilot** and the advanced setting is `true`. It does not apply in **Default Approvals**, **Bypass Approvals**, or Autopilot with the advanced setting disabled. In Default Approvals, VS Code might request security confirmation for an actual tool call; the Critic's `ACCEPT` or `REVISE` verdict is an internal workflow decision and requires no user approval. See [VS Code approvals and permission levels](https://code.visualstudio.com/docs/agents/approvals#_permission-levels).

## Run artifacts

| Path | Purpose | Publication status |
|---|---|---|
| `.spec-workflow/research.md` | Run invariants, action authorizations, evidence, contracts, obligations, decisions, matrices, coverage, gates, and manifest | Internal working memory |
| `.spec-workflow/source-snapshot.md` | Exact line-addressed identity of current candidate source text | Internal freshness control; never task evidence |
| `.spec-workflow/draft.md` | Exact candidate knowledge base reviewed by the final critic | Internal candidate |
| `.spec-workflow/prior-spec.md` | Verified temporary copy of a publication that existed when the run started | Internal rollback checkpoint |
| `outgoing/**` | Reserved legacy generated scratch; emptied at every NEW run and not recreated by this workflow | Never source evidence |
| `spec.md` | Published Question → Answer knowledge base | Final artifact after all gates pass |

An absent `spec.md` at startup is normal. It is an output, not a prerequisite. At run initialization, an existing file is classified as `PRIOR_PUBLISHED`, copied verbatim to the verified Markdown rollback checkpoint `.spec-workflow/prior-spec.md`, and then left physically unchanged until the final write guard passes. Postpublication `PASS` keeps the candidate and removes the checkpoint; `FAIL` restores it (or removes the failed new file when no prior file existed).

NEW removes failed workflow records rather than appending another run below them. The new research file records only the current reset result and current run. See [Fresh Runs, Source Scope, and Cleanup](docs/17_Fresh_Run_Source_Scope_and_Cleanup.md) and [Debugging](docs/10_Debugging.md).

## Learning path

Read the numbered chapters in order when learning the framework:

| Chapter | Focus |
|---|---|
| [00 — Getting Started](docs/00_Getting_Started.md) | Environment, first run, and expected files |
| [01 — Agentic AI for Beginners](docs/01_Agentic_AI_for_Beginners.md) | Agents, roles, turns, tools, memory, and feedback loops |
| [02 — Framework Architecture](docs/02_Framework_Architecture.md) | Components, boundaries, layers, and namespaces |
| [03 — Workflow Engine](docs/03_Workflow_Engine.md) | Phase gates and state transitions |
| [04 — Shared Memory](docs/04_Shared_Memory.md) | Research ledger, draft, manifest, and current-run identity |
| [05 — Interrogation Algorithm](docs/05_Interrogation_Algorithm.md) | Evidence gaps, internal IRQs, and safe defaults |
| [06 — Critic and Convergence](docs/06_Critic_and_Convergence.md) | Review modes, targeted revision, and stopping rules |
| [07 — Observability](docs/07_Observability.md) | Seeing decisions and progress without hidden state |
| [08 — Consistency Checks](docs/08_Consistency_Checks.md) | Fields, enumerations, scales, totals, roles, and traceability |
| [09 — Run Health](docs/09_Run_Health.md) | Healthy, conditional, blocked, and invalid runs |
| [10 — Debugging](docs/10_Debugging.md) | Diagnosing VS Code and workflow failures |
| [11 — Model Configuration](docs/11_Model_Configuration.md) | Model roles, quality trade-offs, and configuration reasoning |
| [12 — End-to-End Tutorial](docs/12_End_to_End_Tutorial.md) | A complete source-to-publication walkthrough |
| [13 — File Formats](docs/13_File_Formats.md) | Normative Markdown structures and internal transport |
| [14 — Best Practices](docs/14_Best_Practices.md) | Design and maintenance guidance |
| [15 — Code/Documentation Mapping](docs/15_Code_Documentation_Mapping.md) | Where each behavior is implemented |
| [16 — Agent Loops Explained Like You Are 10](docs/16_Agent_Loops_Explained_for_Age_10.md) | Feedback loops and implementation references in very simple language |
| [17 — Fresh Runs, Source Scope, and Cleanup](docs/17_Fresh_Run_Source_Scope_and_Cleanup.md) | Preventing stale-task lock-in and unrelated-source merging |

Use the reference manuals while operating or modifying the framework:

| Reference | Use it when… |
|---|---|
| [Generic Usage](docs/GENERIC-USAGE.md) | applying the framework to a new domain |
| [Model Configuration](docs/MODEL-CONFIG.md) | choosing or changing models in VS Code |
| [Observability Reference](docs/OBSERVABILITY.md) | reading ledgers, checks, and status records |
| [Q&A Spec Manual](docs/QA-SPEC-MANUAL.md) | designing or reviewing published `KQ-*` entries |
| [Role Naming](docs/ROLE-NAMING.md) | interpreting agents, actors, layers, and ID namespaces |
| [User Manual](docs/USER-MANUAL.md) | running the system day to day |
| [Visible Workflow](docs/VISIBLE-WORKFLOW.md) | following the exact agent/critic loop |

## Two kinds of questions

This distinction prevents most bad specifications:

- An `IRQ-*` is an **internal resolution query**. It asks what the workflow still needs to know. It belongs only in research.
- A `KQ-*` is a **published knowledge question**. It gives the downstream reader concrete, source-specific knowledge.

When the source-native unit is a respondent-facing question and `spec.md` is the final artifact or embeds the exact units, the published question is the exact question addressed to the respondent—for example, the candidate in an interview guide. For non-question-native work, and for question-native work whose relationship is `DEFINES_SCHEMA` or `REFERENCES_ONLY`, a `SPEC_KNOWLEDGE_QUERY` instead retrieves concrete implementation, schema, selection, or use knowledge.

Questions such as “What must the specification contain?” are workflow questions, not useful published knowledge. The framework rejects them.

## Domain neutrality

The framework can specify software, visual work, narratives, events, policies, processes, education, documents, media, and other text-described goals. Domain neutrality does **not** mean vague output. The algorithm is generic; each accepted Q&A must be traceably bounded by the selected goal, admitted evidence, complete implementation/content knowledge, and an observable acceptance condition. Explicit task vocabulary is required only when the sources require it.

Agent definitions, workflow records, outgoing output, and prior publication are never normal task evidence. This repository's tutorials and README are framework documents unless the user explicitly designates particular domain text. Every other readable text candidate receives an included/excluded semantic reason rather than being accepted merely because of its path.

## Important limits

- This project creates a specification knowledge base; it does not execute the downstream recipe, event, interview, artwork, or software implementation.
- Persistent and normative artifacts are Markdown. Internal agent-to-agent transport can be structured, including JSON-like records, but that is not a generated JSON Schema.
- A critic saying `REVISE` is not approval. Publication requires every non-skippable gate and the postpublication comparison to pass.
- `PREPUBLICATION` is not a Critic mode. Its similarly named state check is only one part of the complete 18-check `FINAL` review.
- Zero accepted KQs, incomplete mandatory coverage, or unresolved material always forbids `READY` and publication.
- Placeholders, bare ranges, endpoint-only scoring scales, and “complete later” instructions cannot satisfy required fields.

Start with [00 — Getting Started](docs/00_Getting_Started.md), then keep [Visible Workflow](docs/VISIBLE-WORKFLOW.md) open beside VS Code during your first run.
