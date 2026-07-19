# Generic Usage Reference

## Purpose

This guide explains how to use one generic Markdown agent framework with many kinds of source material while still producing a highly specific `spec.md`.

The central rule is:

> Keep the workflow vocabulary stable; derive the deliverable vocabulary, units, fields, questions, and answers from the active sources.

The framework must never contain a permanent `Python`, `candidate`, `recipe`, `birthday`, or other domain label unless it is presented as a clearly non-normative example. Those terms belong to runtime evidence.

## What stays generic

The following concepts are framework invariants:

- the four agent roles
- the three artifact layers
- the `OBL-*`, `IRQ-*`, `UNIT-*`, and `KQ-*` namespaces
- Question → Answer publication
- evidence locations and provenance
- the Field Instantiation Contract
- critic decisions and phase gates
- current-run identity, manifest, and postpublication comparison
- placeholder, enumeration, scale, consistency, and traceability checks

These invariants describe **how knowledge is processed**, not what the task is about.

## What changes for every task

The active sources determine:

- the goal and requested artifact
- the downstream producer and reader
- the real-world operator and recipient, if any
- the source-native repeated unit
- required fields, order, cardinality, constraints, and aggregates
- whether exact content instances or only a schema/reference are required
- the wording and role of each published question
- answer semantics and acceptance evidence

For example, an interview competency can be a required coverage key, while one `UNIT-*` can be a complete candidate-facing primary-question package. A recipe may need ingredient groups plus steps; a media project may need scenes plus assets. The Artifact Contract therefore records zero or more source-native unit types and every applicable relationship rather than forcing one universal unit or one-to-one mapping. The framework should not know any of those nouns before it reads the sources.

## Source setup

Place the active goal and supporting constraints in human-authored Markdown or plain-text files. You may organize them by concern, such as:

- ticket or brief
- audience or participant context
- requirements and constraints
- preferences
- policy or compliance material
- acceptance criteria

Filenames, extensions, authorship, and modification times do not establish authority. Source discovery inventories every readable non-reserved textual candidate and classifies it semantically. Fixed framework/generated trees receive pattern exclusions rather than becoming candidates. `jira.md` is not mandatory, and a file named `notes.md` can be authoritative if its content defines the goal.

Do not put active task evidence under these reserved generated or framework locations:

- `.github/agents/**`
- `.github/agent-contracts/**`
- `.spec-workflow/**`
- `outgoing/**`
- prior `spec.md`

In this repository, `docs/**` and root `README.md` are classified as framework documentation by default. A user may explicitly designate particular domain text as source material, but designation does not make every instruction inside it authoritative: goal-relevant sections can be admitted while embedded directives for an incompatible deliverable are excluded and recorded.

## Domain adaptation table

| Goal type | Possible source-native unit | Likely operational direction | Suitable published KQ role |
|---|---|---|---|
| Software feature | behavior, acceptance scenario, interface decision | user → system or component → component | Usually `SPEC_KNOWLEDGE_QUERY` |
| Illustration | scene, asset, panel, visual element | creator → viewer | `SPEC_KNOWLEDGE_QUERY` |
| Storyboard | shot or panel | storyteller → audience | `SPEC_KNOWLEDGE_QUERY` |
| Recipe | ingredient group, step, safety decision, acceptance outcome | cook → diners | `SPEC_KNOWLEDGE_QUERY` |
| Birthday party | activity, timeline block, food item, safety control | organizer → guests | `SPEC_KNOWLEDGE_QUERY` |
| Interview guide | candidate-facing primary question | interviewer → candidate | `DOWNSTREAM_TARGET_QUESTION` |
| Policy | rule, exception, control, decision path | policy owner → affected actor | `SPEC_KNOWLEDGE_QUERY` |
| Educational activity | learner-facing question or activity unit | facilitator → learner | Depends on whether the exact question is the required unit |
| Media project | segment, scene, asset, delivery requirement | producer → audience | `SPEC_KNOWLEDGE_QUERY` |

This table is illustrative, not a built-in schema. The relationship and source wording decide the actual result.

## Question-role decision

Use this sequence for each planned published entry:

1. Identify the source-native downstream unit.
2. Ask whether that unit is intrinsically a question addressed to a respondent.
3. Check whether `spec.md` is the final artifact or embeds the exact unit.
4. If both conditions hold, use `DOWNSTREAM_TARGET_QUESTION`.
5. Otherwise use `SPEC_KNOWLEDGE_QUERY`.

### Interview example

If a source asks for one primary interview question per competency, the published `Question` is the exact candidate-facing question. The `Answer` contains expected evidence, warning signs, follow-ups, and question-specific scoring guidance required by the source.

Bad published question:

> What must the interview specification contain?

Why it fails: it addresses the spec author, does not assess a candidate, and could apply to any job.

### Birthday dessert example

If sources request a safe dessert recipe, the recipe is not naturally a respondent-facing question. A useful `SPEC_KNOWLEDGE_QUERY` can ask:

> What exact ingredient quantities produce the required number of chocolate-and-strawberry individual portions?

Its answer must provide the concrete quantities, yield, substitutions, and relevant allergy constraints. It must not answer “the cook should decide quantities later.”

## Relationship selection

The artifact relationship determines what “complete” means:

| Relationship | Meaning | Completeness expectation |
|---|---|---|
| `IS_FINAL_ARTIFACT` | `spec.md` is the requested final deliverable | All required final content is present |
| `EMBEDS_EXACT_UNITS` | Another deliverable/action exists, but the spec must contain exact native units | Every required unit is fully instantiated |
| `DEFINES_SCHEMA` | The spec defines how later instances must be structured | All fields, rules, dependencies, applicability, and validation meaning are defined |
| `REFERENCES_ONLY` | The spec relies on separately defined material | References are verified and include required selection/access/interpretation/use guidance |

Do not use `DEFINES_SCHEMA` to avoid writing exact content when the source requires exact instances.

## Handling missing information

Classify each missing-looking item before asking the user:

- Explicit in sources: use it; do not ask.
- Directly derivable: record a transparent derivation.
- Bounded original content: create `GROUNDED_SYNTHESIS` using the goal and constraints.
- Optional and safely defaultable: use a conservative `SAFE_DEFAULT`.
- Material and not safely resolvable: create an internal `IRQ-*`, then route it to evidence, synthesis, default, or human input.

“Use defaults” authorizes only conservative source-compatible assumptions for genuinely missing optional choices. It cannot erase an allergy, security, legal, accessibility, privacy, or other explicit material constraint.

## Plural, conditional, and finite structures

The generic completeness rules adapt to any domain:

- An unspecified required collection is persisted as `CARDINALITY: SOURCE_UNSPECIFIED`; the current Investigator proposal contract calls the same state `SOURCE_CARDINALITY: UNSPECIFIED`. The framework does not invent a numeric minimum. A concrete production count, if needed, must be a labeled source-compatible post-resolution decision rather than a source fact.
- A conditional item contains both its trigger and its resulting action/content.
- A finite enumeration contains each expected member exactly once.
- An ordered scale gives every level a distinct, observable, unit-specific description.
- A required field contains concrete content, not a label, range, ellipsis, or instruction to complete it later.

Examples:

- “Follow-ups: …” fails because it is a placeholder.
- “Scores: 1..5” fails because it compresses a finite scale.
- “If weather changes” fails without the resulting action.
- “Include activities” does not authorize the baseline to invent a count; production must still provide a concrete, useful collection and classify any chosen count that was not source-supplied.

## Starting and resuming

With **Text Spec Orchestrator** selected in VS Code:

- `run`, `start`, `go`, `generate`, or `build` always starts a NEW Markdown SSD run. It empties `.spec-workflow/**` and `outgoing/**`, verifies cleanup, creates and rereads `.spec-workflow/research.md` first, creates and verifies `.spec-workflow/source-snapshot.md` second, then persists and rereads `GUARD_CALL_BASELINE` before any worker call.
- `proceed`, `continue`, or `resume` continues the earliest incomplete gate only when the goal, exact source snapshot, scope decisions, run identity, and physical artifacts still match; otherwise it converts to NEW.

The snapshot is valid only at its exact managed path, with one sorted path and one source block per non-reserved candidate. Candidate/path/block counts must agree; in every block, `LINE_COUNT`, live EOF line count, contiguous row count, and highest row ID must agree, and every row text must match. Orphaned, misplaced, duplicate, truncated, or malformed state is repaired automatically before baseline without asking the user to update research or rerun the workflow.

The control word is not task evidence. The agent infers the goal from eligible files.

Only the Autopilot permission level with `chat.autopilot.advanced.enabled: true` uses the VS Code 1.124 maximum of three automatic host loops. Default Approvals and Bypass Approvals do not use that maximum; a Critic verdict is never a permission approval. If the optional outer limit stops a valid nonterminal run, send `resume` instead of starting over. See [VS Code permission levels](https://code.visualstudio.com/docs/agents/approvals#_permission-levels) and [VS Code 1.124: Advanced Autopilot](https://code.visualstudio.com/updates/v1_124#_advanced-autopilot).

## Generic run checklist

Before the run:

- [ ] One coherent goal is described in eligible text sources.
- [ ] Critical constraints and acceptance criteria are explicit.
- [ ] Framework documentation is not mixed with task evidence.
- [ ] Human inputs are outside agent-owned `.spec-workflow/**` and `outgoing/**` paths.
- [ ] Text Spec Orchestrator is selected.

During the run:

- [ ] Source inventory includes every eligible source and explains exclusions.
- [ ] Initialization, reset, managed-path, scope, snapshot, evidence-membership, and single-run checks pass.
- [ ] Research was created first; snapshot candidate/path/block and per-block line/row/EOF/text equations pass.
- [ ] Artifact layers and relationship are explicit.
- [ ] Obligations quote real source wording with verified locations.
- [ ] IRQs are limited to genuine material gaps.
- [ ] KQs use the correct role and source-native perspective.
- [ ] Required fields and finite members are concrete and complete.
- [ ] `REVISE` triggers correction and re-review, not publication.

After the run:

- [ ] `FINAL_GATE` is `READY`.
- [ ] `POSTPUBLICATION_GATE` is `PASS`.
- [ ] `spec.md` matches the accepted draft.
- [ ] Published questions and answers are usable for the actual downstream task.

## Related reading

- [Framework Architecture](02_Framework_Architecture.md)
- [Interrogation Algorithm](05_Interrogation_Algorithm.md)
- [Q&A Spec Manual](QA-SPEC-MANUAL.md)
- [User Manual](USER-MANUAL.md)
- [Visible Workflow](VISIBLE-WORKFLOW.md)
