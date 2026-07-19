# Interrogation Algorithm

Interrogation is a narrow internal algorithm for resolving genuine evidence gaps. It is not a general brainstorming stage, a required question count, or the source of published KQ wording.

## Learning goals

By the end of this chapter, you should be able to:

- decide when an `IRQ-*` is allowed
- distinguish evidence, grounded synthesis, safe defaults, and human resolution routes
- prevent explicit source facts from becoming unnecessary questions
- handle user control messages and conflicting overrides safely
- keep internal resolution records out of `spec.md`
- understand how unresolved fields return to production without placeholders

## What interrogation is

The Interrogator examines the accepted knowledge state and proposes an internal query only for a material:

- ambiguity
- source conflict
- missing decision
- dependency
- synthesis choice that evidence and a conservative source-compatible default cannot safely resolve

The purpose is to isolate a decision whose absence changes safe or correct downstream work.

## What interrogation is not

The Interrogator must not create an IRQ for:

- an explicit source fact or constraint
- a restatement of a required section or field
- every named item when treatment is already clear
- speculative restrictions outside source scope
- conversational exploration
- information already resolved by an accepted record
- a missing field that production can concretely fill through grounded synthesis

This negative definition is essential. Over-interrogation slows the workflow and turns clear requirements into fake uncertainty.

## Preconditions

Interrogation begins only after:

- the current run has `RUN_INITIALIZATION_GATE`, `GENERATED_ARTIFACT_RESET_CHECK`, `MANAGED_ARTIFACT_PATH_CHECK`, `SOURCE_SCOPE_CHECK`, `SOURCE_SNAPSHOT_CHECK`, `EVIDENCE_MEMBERSHIP_CHECK`, and `SINGLE_RUN_STATE_CHECK` all at `PASS` after recomputation; RESUME additionally requires unchanged source identity
- one active goal and its admitted source sections have passed scope and evidence-membership checks
- source discovery is complete
- the three-layer Artifact Contract is accepted
- the obligation register is accepted
- the Field Instantiation Contract is accepted
- genuine candidate gaps have been identified

No resolution or production work may begin before baseline acceptance.

## The four resolution routes

### `EVIDENCE`

Use when the answer exists in active sources but needs focused retrieval or reconciliation.

### `GROUNDED_SYNTHESIS`

Use when sources require original content and provide enough goals, constraints, and acceptance conditions to create it safely. The result is labeled synthesis, not fact.

### `SAFE_DEFAULT`

Use for a genuinely missing optional choice when a conservative source-compatible assumption is safe and useful. A default cannot weaken an explicit constraint.

### `HUMAN`

Use only when the choice is material and evidence, derivation, synthesis, and safe defaults cannot decide it. Optional polish must not become a blocker.

## Decision procedure

For each candidate gap, apply this sequence:

1. **Is the value explicit in a source?** If yes, record it as evidence. Do not create an IRQ.
2. **Can it be derived without adding a new choice?** If yes, record the derived requirement.
3. **Do the sources bound a safe, useful synthesis?** If yes, route to grounded synthesis.
4. **Is the missing choice optional and safely defaultable?** If yes, record a visible safe default.
5. **Would remaining uncertainty materially affect safe or correct completion?** If no, keep it out of scope.
6. **If yes, can a focused human decision resolve it?** Create one cohesive `IRQ-*` with route `HUMAN`.
7. **If no safe answer is possible,** record the downstream effect and block or isolate the result. Never insert a placeholder.

## Required IRQ shape

Every proposed internal query contains:

- `IRQ-*` ID
- exact internal query
- mapped obligation IDs and readable labels
- verified source locations and headings
- source-specific anchors
- downstream effect
- resolution route
- observable resolution condition
- duplicate check
- publication status `WORKFLOW_ONLY`
- affected planned KQ or UNIT topic when known

The query resolves one cohesive gap. It does not bundle unrelated decisions.

## Generic textual examples

The examples below are non-normative. They illustrate the decision procedure; they are not current workspace requirements and must never be added to a run unless its admitted source snapshot supports them.

| Candidate issue | Correct handling | Why |
| --- | --- | --- |
| What participant count applies? | Record the exact count as evidence when the active source resolves it. | An explicit quantity is not an ambiguity. |
| Is the prohibited device feature allowed? | Record the prohibition as evidence. | A material constraint must not be reopened as a preference. |
| Which visual treatment should be selected? | Use grounded synthesis only when the admitted goals, constraints, and acceptance conditions safely bound a useful choice. | Original content can be required even when its final wording is not supplied. |
| Which of two source-stated alternatives controls? | Create one focused conflict IRQ when neither source has higher authority. | A genuine unresolved conflict can materially change the deliverable. |
| Are there unrelated accessibility, safety, or policy restrictions? | Do not create an IRQ unless admitted source text places them in scope. | Missing unrelated information is not a material gap. |
| Which executable should be run? | Never ask. | The selected agent runs a Markdown specification workflow, not an executable project. |

## Defaults are not domain answers

`use defaults` is workflow control. It authorizes conservative assumptions only for identified optional gaps.

It does not mean:

- no stated risk
- no constraints
- unrestricted implementation choices
- approval to change a strict quantity
- permission to implement the downstream result

A user response resolves only the specific question it unambiguously answers.

## Conflict and override algorithm

If a user response conflicts with an explicit source fact:

1. Identify the affected obligation.
2. Present the exact source wording and location.
3. State the material conflict and downstream effect.
4. Obtain an exact, specific response.
5. Require explicit acknowledgment for safety or comparable material constraints.
6. Record original fact, response, supersession, provenance, and effect.
7. Revalidate every affected KQ, UNIT, aggregate, and acceptance condition.

Without this record, the original source remains authoritative.

## Resolution batch loop

The Orchestrator works with batches of at most five IRQs:

1. Interrogator proposes focused queries.
2. Investigator resolves the exact supplied queries.
3. Critic reviews every resolution.
4. Accepted resolutions are persisted in research.
5. Revised items receive one targeted correction.
6. Blocked or unresolved material decisions stop or condition dependent work.

The loop ends when every genuine IRQ resolution is accepted or blocked. An accepted resolution may preserve a content item as visibly unresolved for later final evaluation, but `RESOLUTION_GATE: CONDITIONAL` is not a valid state. If no IRQ exists, `RESOLUTION_GATE: NOT_REQUIRED` records that fact and reason.

## Interrogation versus production

Interrogation decides unknowns. Production creates complete published knowledge.

A required field that can be synthesized is production work, not a reason to ask the user. A required field that cannot be safely determined may cause an IRQ. In neither case may the Investigator emit:

- ellipses
- blank values
- compressed scale ranges
- “same as above”
- future-work instructions

If a production batch is too large, the batch size is reduced. Completeness is not traded for speed.

## Preventing IRQ leakage

The safeguards are redundant by design:

- IRQs have their own namespace.
- Their publication status is always `WORKFLOW_ONLY`.
- The Interrogator never authors KQ wording.
- The Investigator never formats a resolution as a published Question-and-Answer entry.
- The Orchestrator starts a blank draft from admitted content.
- The Critic rejects published IRQ records and meta-questions.

A resolution may inform a KQ answer, but the KQ is independently produced from a planned domain topic.

## Failure modes

### Asking about an explicit fact

This creates fake uncertainty and may invite the user to weaken a valid source constraint.

### One IRQ per obligation

Obligations are coverage records, not a required question count. Many explicit obligations need no interrogation.

### Treating silence as absence

An unmentioned condition is not proof that it is false.

### Using a broad user response as an override

A response to another question or a generic “no” cannot supersede a specific material source fact.

### Publishing the internal resolution

Headings such as “Accepted Q&A entries” and fields such as “Exact accepted question” reveal workflow records and must not appear in `spec.md`.

### Filling unresolved work with placeholders

Placeholders conceal incomplete knowledge. The item must be revised, conditioned, or blocked.

## Practical checklist

- [ ] Baseline is accepted before interrogation.
- [ ] Exact managed paths and all snapshot path/block/count/row/EOF/text equations still pass, together with scope and evidence membership.
- [ ] Every IRQ represents a genuine material gap.
- [ ] Explicit facts and constraints are not questioned.
- [ ] Unrelated possibilities remain outside scope.
- [ ] The resolution route is evidence, synthesis, safe default, or human.
- [ ] HUMAN is used only when safer routes cannot decide.
- [ ] Every IRQ has verified source linkage and downstream effect.
- [ ] Generic workflow controls are not treated as domain facts.
- [ ] Material overrides contain the complete conflict record.
- [ ] Accepted resolutions remain in research only.
- [ ] Production independently creates KQ wording.
- [ ] No placeholder substitutes for unresolved required content.

## Related reading

- [Getting Started](00_Getting_Started.md)
- [Agentic AI for Beginners](01_Agentic_AI_for_Beginners.md)
- [Framework Architecture](02_Framework_Architecture.md)
- [Workflow Engine](03_Workflow_Engine.md)
- [Shared Memory](04_Shared_Memory.md)
- [Interrogator definition](../.github/agents/text-spec-interrogator.agent.md)
