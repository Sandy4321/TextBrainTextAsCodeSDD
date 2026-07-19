# Model Configuration Reference

## Scope

This reference explains how model selection affects this repository's VS Code GitHub Copilot custom-agent workflow. It is intentionally model-name agnostic: model availability, entitlement, pricing, and UI labels can change.

The current implementation does not pin a `model` property in any `.agent.md` file. Therefore, the selected parent chat model is the normal fallback for the Orchestrator and its workers.

## Configuration locations

VS Code custom agents can select a model in two places:

1. **Chat model picker** — controls the active chat model when the agent does not specify one.
2. **Custom-agent frontmatter** — the optional `model` property can name one model or a prioritized list.

For a subagent, current VS Code behavior applies model choice in this order:

1. an explicit model supplied by the parent during subagent invocation
2. the worker custom agent's `model` frontmatter
3. the parent conversation's model

This priority is a VS Code capability, not a rule invented by this framework. See the official [Subagents in Visual Studio Code](https://code.visualstudio.com/docs/agents/subagents) and [AI language models in VS Code](https://code.visualstudio.com/docs/agent-customization/language-models) documentation.

## Permission modes and the optional Advanced Autopilot boundary

Permission behavior is host configuration, not model selection. Default Approvals uses configured security confirmations; Bypass Approvals auto-approves tools; Autopilot additionally provides continuous host iteration. VS Code 1.124's maximum of three automatic continuation loops applies only when the current permission level is Autopilot and `chat.autopilot.advanced.enabled` is `true`. Neither a stronger model nor an `.agent.md` instruction can extend that optional maximum.

The Critic's verdict is not a permission approval. Under Default Approvals, `REVISE` triggers internal correction and re-review unless an actual tool needs confirmation. The framework still persists current phase, accepted records, and same-defect state after every change. If the optional Advanced boundary stops the host, `resume` continues without incrementing `SAME_DEFECT_REVISION_COUNT`. Record the permission level and advanced-setting value when comparing runs. See [VS Code permission levels](https://code.visualstudio.com/docs/agents/approvals#_permission-levels) and [VS Code 1.124: Advanced Autopilot](https://code.visualstudio.com/updates/v1_124#_advanced-autopilot).

## Recommended learning configuration

Begin with no pinned model, as this repository currently does.

Why:

- the model picker makes experiments visible
- every worker uses a comparable baseline
- the tutorial remains portable across accounts and organizations
- unavailable model names cannot prevent agent loading
- you can learn whether failures come from instruction design or model variation

Record the selected model and relevant effort setting in your run notes when comparing outcomes. Do not write a model name into the source obligation register; model choice is operational metadata, not task evidence.

## Capability profile by role

Choose models by required behavior, not brand loyalty.

| Role | Most important capabilities | Main failure risk |
|---|---|---|
| Orchestrator | long-context coordination, instruction following, stable state tracking, precise tool routing | skipping gates or losing current-run state |
| Investigator | evidence extraction, structured synthesis, source-specific generation, quantitative consistency | generic output or invented requirements |
| Interrogator | restraint, ambiguity discrimination, concise question design | asking unnecessary or speculative questions |
| Critic | adversarial review, exact comparison, enumeration and traceability checking | approving plausible but incomplete content |

A fast model may be sufficient for a small, explicit source set. A stronger reasoning model is usually more reliable for many obligations, nested fields, repeated units, finite scales, cross-unit totals, or conflicting evidence.

## Why one model may fill every role

The roles are separated primarily by instructions, tools, and context boundaries. A single capable model can act as all four roles because each invocation receives a different contract:

- Investigator proposes.
- Interrogator identifies genuine gaps.
- Critic judges without editing.
- Orchestrator owns state and writes.

This is valuable for education because you can observe how role framing changes behavior while holding the underlying model constant.

## Why different models may help

After establishing a baseline, you may experiment with different worker models:

- a larger-context model for the Orchestrator or Investigator
- a highly analytical model for the Critic
- a lower-latency model for a narrowly bounded Interrogator batch

The trade-off is reproducibility. Different models can interpret the same contract differently. If you change several roles at once, you cannot tell which change affected quality.

Use one controlled change per experiment.

## Frontmatter behavior in this project

The Orchestrator frontmatter intentionally includes:

- `target: vscode`
- `tools` containing `read`, `search`, `edit`, and `agent`
- an `agents` allow-list naming the three workers
- `user-invocable: true`
- `disable-model-invocation: true`

The workers are `user-invocable: false`, making them internal subagents rather than choices intended for ordinary manual runs. Their `disable-model-invocation: false` setting allows delegated use.

The `agent` tool and `agents` allow-list control delegation. They do not select a model by themselves.

For the authoritative frontmatter fields, see [Custom agents in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-agents).

[GitHub documents a 30,000-character prompt limit](https://docs.github.com/en/copilot/reference/custom-agents-configuration) for a custom agent. The Orchestrator stays under that limit by linking [text-spec-execution-firewall.md](../.github/agent-contracts/text-spec-execution-firewall.md), which VS Code supports as a reusable instruction-file reference. The linked Markdown has no frontmatter and is not a fifth agent or a source file.

## Configuration experiment protocol

When comparing models, use the same:

- source files
- agent files and the linked execution contract
- invocation word
- fresh-run policy
- expected obligation set
- pass/fail rubric

Record:

- date and VS Code version
- selected parent model
- any worker `model` override
- thinking/effort setting if exposed
- source count and approximate size
- number of baseline revisions
- number of IRQs and how many required human input
- number of production revisions
- final and postpublication status
- observed defects, especially generic KQs, missing fields, placeholder leakage, or bad source locations

Compare complete runs, not isolated fluent answers.

## Model-change checklist

Before changing configuration:

- [ ] Save the current agent files and linked execution contract.
- [ ] Preserve the prior run artifacts outside active evidence if needed for comparison.
- [ ] Choose one variable to change.
- [ ] Confirm the candidate model is available in the VS Code picker or account policy.
- [ ] Keep the same test sources and rubric.

After changing configuration:

- [ ] Confirm all four custom agents still load.
- [ ] Confirm every `.agent.md` prompt remains under the documented character limit and the Orchestrator's contract link resolves.
- [ ] Confirm the Orchestrator can invoke the named workers.
- [ ] Start a fresh run rather than resuming incompatible state.
- [ ] Compare ledgers and critic decisions, not only prose style.
- [ ] Revert the change if it reduces gate discipline or source fidelity.

## Troubleshooting

### A requested model is ignored

Possible causes include unavailable entitlement, organization policy, a higher-priority explicit invocation setting, or a fallback to the parent. Check the model picker and VS Code diagnostics. Do not assume a requested model was used merely because its name appears in an instruction.

### Only `Auto` appears

VS Code may be in Restricted Mode, or account/organization policy may constrain choices. Trust only a workspace you understand, then inspect account policy and the model-management UI.

### A worker behaves differently from the parent

Inspect whether the worker has model frontmatter or whether the invocation selected a model explicitly. Also compare the worker's role instructions and tools; behavior differences are not proof of a different model.

### Better prose but worse specifications

Fluency is not the quality target. Measure obligation coverage, field instantiation, finite-member completeness, question-role correctness, consistency, and final gate outcomes.

## Related reading

- [Model Configuration chapter](11_Model_Configuration.md)
- [Critic and Convergence](06_Critic_and_Convergence.md)
- [Observability Reference](OBSERVABILITY.md)
- [Debugging](10_Debugging.md)

Official references reviewed for this guide:

- [Custom agents in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-agents)
- [Subagents in Visual Studio Code](https://code.visualstudio.com/docs/agents/subagents)
- [AI language models in VS Code](https://code.visualstudio.com/docs/agent-customization/language-models)
