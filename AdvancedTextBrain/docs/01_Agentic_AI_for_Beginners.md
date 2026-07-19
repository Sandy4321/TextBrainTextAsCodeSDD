# Agentic AI for Beginners

An AI chat response is usually a single model turn. An agentic workflow adds roles, tools, persisted state, decision gates, and rules about when work may advance. This framework uses those features to produce specifications that are traceable and usable rather than merely fluent.

## Learning goals

By the end of this chapter, you should be able to:

- explain what makes this workflow agentic
- describe the four real custom-agent roles without inventing extra components
- distinguish evidence, derivation, grounded synthesis, assumptions, and unresolved knowledge
- understand bounded autonomy and why workers have different tools
- explain the difference between internal questions and published knowledge questions
- recognize why independent criticism improves specification quality

## What an agent is in this framework

An agent is a VS Code GitHub Copilot custom-agent definition stored in a Markdown `.agent.md` file. Its front matter declares properties such as its name, target, tools, and whether users or other agents may invoke it. Its Markdown body defines behavior and output contracts.

The framework has exactly four agents. The Orchestrator also links one reusable Markdown execution contract; that contract contains rules but is not another agent:

| Agent | User-facing | Writes files | Main responsibility |
| --- | --- | --- | --- |
| Text Spec Orchestrator | Yes | Yes, within the allowed Markdown paths | Own the run, delegate work, maintain gates, assemble the draft, and publish after acceptance. |
| Text Spec Investigator | No | No | Read evidence, construct a baseline, resolve supplied gaps, and produce complete candidate knowledge entries. |
| Text Spec Interrogator | No | No | Propose internal `IRQ-*` records only for genuine gaps. |
| Text Spec Critic | No | No | Admit, revise, or block baseline, resolution, production, final, and postpublication states. |

There is no separate memory agent, planner agent, or execution agent. Planning and persisted memory are responsibilities of the Orchestrator using Markdown artifacts.

## How agentic collaboration works

The Orchestrator passes explicit context to the workers. That context includes the active goal, relevant sources, Artifact Contract fields, obligations, field rules, prior accepted indexes, and the exact requested response mode.

The workers do not reconstruct missing context from guesswork:

- The Investigator operates in `BASELINE`, `RESOLVE_BATCH`, or `PRODUCE_BATCH` mode.
- The Interrogator proposes questions but never answers or publishes them.
- The Critic receives the evidence needed to verify a proposal independently.
- The Orchestrator persists accepted results before advancing.

This explicit handoff is important because agent turns are bounded. Durable facts belong in shared Markdown memory, not in an assumption that every worker remembers every prior message. The same principle protects the main run when the VS Code host ends an Autopilot execution segment.

## The initialization loop before worker collaboration

A NEW run begins with a small fail-closed bootstrap loop. The Orchestrator creates and rereads `.spec-workflow/research.md` first, with one run identity and pending control state. It then writes `.spec-workflow/source-snapshot.md` in bounded chunks through the editor-reported end of every non-reserved textual candidate.

The snapshot passes only when its candidate count equals its sorted path count and source-block count. In every block, `LINE_COUNT` must equal the live file's line count, the number of contiguous `L*` rows, and the highest row ID, and every row must match the live text. `MANAGED_ARTIFACT_PATH_CHECK` also rejects a root-level, basename-only, or duplicate snapshot. Missing, orphaned, misplaced, truncated, or mismatched state is repaired automatically before either the Investigator or Critic is called.

## Why the roles use different tools

The Orchestrator has reading, searching, editing, and agent-delegation capabilities because it owns workflow state and publication.

The other three agents have read and search capabilities only. That limit is intentional:

- An Investigator cannot publish its own proposal.
- An Interrogator cannot turn an internal question into deliverable content.
- A Critic cannot silently repair and approve the same content.

This is separation of duties expressed through tool permissions.

## Bounded autonomy

The Orchestrator is autonomous inside a narrow scope. When selected, a bare `run` authorizes it to infer one coherent goal from eligible workspace text and execute the Markdown specification workflow. It does not authorize implementation of the downstream result.

Autonomy is bounded by:

- allowed input and exclusion rules
- exact workspace-relative managed Markdown paths, enforced by `MANAGED_ARTIFACT_PATH_CHECK`
- phase-gate prerequisites
- evidence and source-location requirements
- explicit treatment of unresolved knowledge
- independent Critic decisions
- rollback after failed postpublication verification
- the VS Code host's automatic-continuation boundary

The result is initiative without unbounded action.

Only a session using the Autopilot permission level with `chat.autopilot.advanced.enabled: true` has the VS Code 1.124 maximum of three automatic host loops. Default Approvals and Bypass Approvals have no documented Advanced Autopilot three-loop cap. A Critic verdict is not a permission request: `REVISE` starts another internal correction and review in the active response. If the optional outer boundary does apply, the Orchestrator preserves its ledger and `resume` continues the earliest incomplete gate. The separate framework count reaches `BLOCKED: NON_CONVERGING` only when the same Critic defect survives three completed targeted corrections.

## The knowledge ladder

Not every statement has the same epistemic status.

### Fact

A fact is directly stated by active source evidence. In the current example, “12 children” and “one guest has a nut allergy” are facts.

### Derived requirement

A derived requirement follows logically from explicit evidence. For example, serving 12 children with individual portions implies that quantities must yield at least 12 appropriate portions.

### Grounded synthesis

Sources often ask for original content without supplying its exact wording or values. A bounded recipe plan, sequence, or presentation direction may be synthesized when source goals, constraints, and acceptance conditions provide enough guidance. Synthesized content must not be presented as a quoted fact.

### Assumption or safe default

An assumption fills a genuinely missing optional decision. It must remain visible. A safe default may not remove or weaken an explicit constraint.

### Unresolved knowledge

A material decision is unresolved when evidence, derivation, grounded synthesis, and safe defaults cannot decide it. The downstream effect must be visible, and invention is prohibited.

## Four identifiers with different jobs

The framework uses separate namespaces so internal reasoning cannot masquerade as deliverable content:

- `OBL-*` identifies a source-grounded obligation.
- `IRQ-*` identifies an internal ambiguity-resolution query.
- `UNIT-*` identifies an exact downstream content unit when the active relationship requires one.
- `KQ-*` identifies a published specification Question-and-Answer entry.

An `IRQ-*` never becomes a `KQ-*`. Its resolution may inform later production, but its wording and workflow record remain in research memory.

## Internal questions versus published questions

An internal question helps the workflow decide something it cannot yet determine:

> Does the missing optional decoration choice materially affect safe completion, or can a source-compatible default resolve it?

A published specification question retrieves useful task knowledge:

> What allergy controls must the dessert preparation follow?

The first belongs only in research. The second belongs in `spec.md` with a concrete answer, source basis, and acceptance check.

When a downstream deliverable is itself question-native, the published question can instead be the exact target-facing question. The Artifact Contract determines the operator and respondent, preventing the specification reader from being substituted for the real respondent.

## Why the Critic is independent

A generative proposal can sound complete while hiding omissions. The Critic checks observable properties:

- Does every cited location contain the claimed evidence?
- Is every required field instantiated?
- Are all finite scale members present exactly once?
- Are ordered levels distinct and observable?
- Did an internal query leak into the draft?
- Did a source constraint change without an authoritative override?
- Do current-run research, draft, manifest, and gate status agree?

The Critic returns structured decisions rather than a vague quality score. `REVISE` is not acceptance.

## Textual example: autonomy without invention

Suppose the sources say the dessert must serve 12 children, use chocolate and strawberry flavors, avoid nuts and alcohol, finish in under 90 minutes, and use one oven.

The framework may synthesize a concrete recipe direction that satisfies those bounds. It may not:

- claim the sources explicitly named that recipe when they did not
- ask whether the known nut allergy can be ignored
- interpret silence about other allergies as proof that none exist
- change “under 90 minutes” to “90 minutes or less”
- leave ingredient quantities as “to be determined”

This difference—creative completion inside verified bounds—is central to source-grounded agentic work.

## Failure modes

### One agent performs every role

If the producer silently approves and publishes its own output, the separation of duties has failed.

### Hidden memory is treated as authoritative

If a worker assumes a prior decision that was never persisted or passed, traceability is lost.

### Fluent text is mistaken for complete content

A well-written paragraph can still omit required fields, repeated items, scale members, or source constraints.

### Internal and external actors are collapsed

The reader of `spec.md`, a downstream producer, a real-world operator, and a respondent may be different people or systems. Treating them as one actor produces questions for the wrong audience.

### Generic controls become domain facts

Messages such as `run` or `use defaults` control workflow behavior. They do not mean “no constraints” and do not answer a domain question.

## Practical checklist

- [ ] I can name the four agents and their real responsibilities.
- [ ] Only the Orchestrator writes workflow and publication files.
- [ ] Worker invocations include explicit context and a response mode.
- [ ] Facts, derivations, synthesis, assumptions, and unresolved knowledge are distinguished.
- [ ] `IRQ-*` records remain internal.
- [ ] `KQ-*` entries are source-specific and publishable.
- [ ] The Critic independently checks observable admission criteria.
- [ ] Autonomy stops at the specification boundary.
- [ ] A bare control word is not treated as domain evidence.

## Related reading

- [Getting Started](00_Getting_Started.md)
- [Framework Architecture](02_Framework_Architecture.md)
- [Workflow Engine](03_Workflow_Engine.md)
- [Shared Memory](04_Shared_Memory.md)
- [Interrogation Algorithm](05_Interrogation_Algorithm.md)
- [Investigator definition](../.github/agents/text-spec-investigator.agent.md)
- [Critic definition](../.github/agents/text-spec-critic.agent.md)
