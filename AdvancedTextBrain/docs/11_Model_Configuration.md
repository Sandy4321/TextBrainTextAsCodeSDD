# 11. Model Configuration for VS Code GitHub Copilot

## Purpose

This project uses VS Code GitHub Copilot custom agents to turn workspace text into a source-grounded `spec.md`. Model configuration determines which language model performs that reasoning. It does not change the workflow contract, grant new tools, or authorize downstream implementation.

The current agent suite deliberately does not pin a model in YAML frontmatter. The model selected in the VS Code chat model picker therefore drives the Orchestrator, and subagents use the applicable VS Code Copilot subagent model behavior unless an agent is later given an explicit model setting.

## Current configuration at a glance

| Agent | User entry point | Tools | Model setting | Why |
|---|---:|---|---|---|
| [Text Spec Orchestrator](../.github/agents/text-spec-orchestrator.agent.md) | Yes | Read, search, edit, subagent delegation | Not pinned | Lets the user select an available Copilot model while keeping the workflow stable. |
| [Text Spec Investigator](../.github/agents/text-spec-investigator.agent.md) | No | Read and search | Not pinned | Evidence work inherits the active orchestration context without gaining write authority. |
| [Text Spec Interrogator](../.github/agents/text-spec-interrogator.agent.md) | No | Read and search | Not pinned | Gap detection remains read-only and source-bound. |
| [Text Spec Critic](../.github/agents/text-spec-critic.agent.md) | No | Read and search | Not pinned | Admission decisions are independent of file editing. |

The Orchestrator is explicitly user-invocable and cannot be selected automatically as another model's subagent. The three workers are hidden from the normal agent picker but remain available to the Orchestrator.

## Permission levels and Autopilot are not model settings

Permission levels control tool confirmations and host autonomy, not reasoning-model selection. Default Approvals uses configured tool confirmations. Bypass Approvals auto-approves tools. Autopilot adds continuous host iteration. The three-loop maximum exists only when the current permission level is Autopilot and `chat.autopilot.advanced.enabled` is `true`; the setting does not impose that maximum on Default Approvals or Bypass Approvals.

A Critic `ACCEPT` or `REVISE` result is a framework quality decision, never a tool approval. Under Default Approvals, the internal correction loop proceeds unless VS Code asks confirmation for an actual tool. If the optional Advanced boundary is reached, the durable ledger continues after `resume`. See [VS Code permission levels](https://code.visualstudio.com/docs/agents/approvals#_permission-levels) and [VS Code 1.124: Advanced Autopilot](https://code.visualstudio.com/updates/v1_124#_advanced-autopilot).

## Prompt size and the linked execution contract

GitHub's current custom-agent configuration reference limits an agent prompt to 30,000 characters. VS Code also supports reusing instructions by linking another file from an `.agent.md` body. See the official [custom-agent configuration](https://docs.github.com/en/copilot/reference/custom-agents-configuration) and [Custom agents in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-agents) references.

The Orchestrator therefore keeps its entry, source, workflow, Q&A, and publication instructions in `text-spec-orchestrator.agent.md`, while its detailed fail-closed invariant and action-guard rules live in [text-spec-execution-firewall.md](../.github/agent-contracts/text-spec-execution-firewall.md). The Orchestrator links that contract and requires it to be read completely before guarded work.

This split is a VS Code prompt-composition technique, not another agent framework. The contract:

- has no YAML frontmatter and is not a separately invocable agent;
- lives outside `.github/agents/`, so VS Code does not list it as a custom agent;
- is excluded from task-source discovery because it is framework control logic; and
- keeps every actual `.agent.md` file below the documented prompt-size limit.

## What model configuration controls

Model choice affects:

- how reliably the agent follows long, layered instructions;
- how much source and prior-batch context it can reason over;
- how consistently it preserves exact quantities and operators;
- how well it distinguishes source facts from synthesis and assumptions;
- how accurately it produces complete repeated packages;
- latency, availability, and Copilot usage cost.

Model choice does not override:

- the tools listed in each `.agent.md` frontmatter;
- the Markdown-only write boundary;
- the source exclusions;
- the phase gates;
- the Field Instantiation Contract;
- the Critic's acceptance rules;
- the prohibition on executing the downstream goal.

This separation matters because a stronger model can improve reasoning, but it must not become a substitute for explicit validation.

## How to choose a model

Choose a VS Code GitHub Copilot model that performs well on these characteristics:

1. **Instruction fidelity.** It must keep internal `IRQ-*` records separate from published `KQ-*` entries.
2. **Long-context reasoning.** It must compare sources, the Artifact Contract, obligation mappings, completion ledgers, and prior accepted topics.
3. **Structured text quality.** It must produce precise Markdown without silently switching to code or machine-oriented schemas.
4. **Tool use.** It must reliably call the named custom subagents and use read, search, and edit operations as instructed.
5. **Self-correction.** It must respond to `REVISE` and failed checks by correcting the targeted defect instead of claiming success.

Prefer a stable generally available model for repeatable project work. A preview model can be evaluated, but it should pass the same acceptance fixture before becoming the team's default.

## How to run with the selected model

1. Open the VS Code Chat view.
2. Select **Text Spec Orchestrator** from the custom-agent picker.
3. Select the desired GitHub Copilot model from the model picker.
4. Enter `run`, `start`, `go`, `generate`, or `build` for NEW. Use `proceed`, `continue`, or `resume` only to attempt continuation of a valid active ledger; a missing or mismatched ledger automatically becomes NEW.
5. For NEW, confirm that managed stale state is removed, `.spec-workflow/research.md` is created and reread first, and `.spec-workflow/source-snapshot.md` is then created and structurally verified at its exact path before `GUARD_CALL_BASELINE` passes.
6. Confirm that the first domain work is textual source discovery and one-goal scope selection rather than project execution.
7. Inspect the final status and counts reported after postpublication validation.

The shorthand control word is meaningful only while this custom agent is active. In an ordinary Copilot agent session, `run` may retain its normal general-purpose meaning.

## Why the repository does not pin a model

Leaving the `model` property absent provides three benefits:

- teams can use models available under their own Copilot entitlement;
- the workflow does not become coupled to a temporary model identifier;
- model upgrades do not require editing four agent files.

The trade-off is reproducibility: two users might select different models. Record the selected model in external run notes when model-level comparison matters, but do not treat it as source evidence or publish it as domain knowledge.

## When explicit model settings are justified

Consider adding a model setting only when testing shows a repeatable need, such as:

- the Investigator needs a larger context window than the normal chat model;
- the Critic needs stronger structured-verification behavior;
- an organization requires a specific approved Copilot model;
- a reproducibility study must compare fixed model configurations.

If a model is pinned, test the complete main-agent and subagent chain. A model name that is unavailable in a user's Copilot environment can prevent the intended routing or cause fallback behavior.

## Validation fixture

Use two substantially different non-normative fixtures when evaluating a model:

- a non-question-native fixture whose sources require concrete planning or implementation knowledge, strict constraints, quantities, and cross-unit checks;
- a question-native fixture in which exact respondent-facing questions and complete ordered evaluation levels are required.

These are test designs, not current workspace requirements. Run each in a clean fixture workspace or replace the declared task sources for a controlled NEW run. Do not place two equal, independent goals together and expect source discovery to guess which one is authoritative. Verify that research is bootstrapped before the snapshot, the snapshot changes with the fixture, all path/block/count/row/EOF/text equations pass, and no prior fixture record survives initialization. Also test a deliberately orphaned or misplaced snapshot: the model must repair initialization locally and must not call a worker or ask the user to rerun baseline.

The model passes only if it changes domain vocabulary and published question role while retaining the same evidence, completeness, and phase-gate discipline.

## Common failure modes

| Symptom | Likely cause | Corrective action |
|---|---|---|
| `run` triggers code-oriented advice | The custom Orchestrator was not selected, or entry semantics were ignored | Select **Text Spec Orchestrator** and retry in a new chat. |
| Internal questions appear in `spec.md` | Weak separation of `IRQ-*` and `KQ-*` | Reject through the Critic's IRQ leak and question-role checks. |
| Output contains `...`, bare ranges, or incomplete lists | Model abbreviated a batch | Reduce batch size and require every completion check to pass. |
| Source constraints are softened | The model treated synthesis as authority | Reapply source/default integrity and strict-operator checks. |
| Subagent loses the active domain | Too little context was passed | Include the Artifact Contract, mapped obligations, relevant sources, and accepted-topic index in the delegation. |
| A new fixture inherits an old domain or source path | Managed state was appended or RESUME ignored source changes | The Orchestrator must automatically initialize NEW, create research first, verify the exact-path snapshot and its equations, and locally repair partial state before baseline. |
| Good draft but failed publication | Postpublication content or state differs | Restore the prior file and diagnose the failing repeat check. |
| Advanced Autopilot stops after three automatic loops | VS Code reached its host continuation boundary | Keep the active ledger and send `resume`; do not change models, restart, or count it as Critic non-convergence. |

## Configuration checklist

- [ ] **Text Spec Orchestrator** is selected in VS Code Chat.
- [ ] The chosen model supports agent tools and custom-agent delegation.
- [ ] No terminal or execution tool was added.
- [ ] Worker agents remain read-only.
- [ ] The VS Code version and Advanced Autopilot setting are recorded for controlled comparisons.
- [ ] Every fixture starts from verified managed cleanup, research-first bootstrap, and one exact-path snapshot whose path/block/count/row/EOF/text equations pass.
- [ ] Orphaned, misplaced, duplicate, or truncated initialization is repaired internally before any worker call.
- [ ] The complete test fixture passes, not merely the first response.
- [ ] Placeholder, enumeration, cross-unit, traceability, and postpublication checks all pass.
- [ ] A model change does not alter persistent file formats.

## Related reading

- [12. End-to-End Tutorial](12_End_to_End_Tutorial.md)
- [13. File Formats](13_File_Formats.md)
- [14. Best Practices](14_Best_Practices.md)
- [15. Agent-to-Documentation Mapping](15_Code_Documentation_Mapping.md)
