# Q&A Specification Manual

## Purpose

`spec.md` is always a Question → Answer knowledge base. This invariant gives downstream readers a predictable retrieval structure while allowing the subject matter to change completely.

The invariant is the **envelope**, not generic question wording. A good knowledge base for non-question-native work captures concrete decisions, constraints, sequences, and acceptance outcomes. A good interview guide contains exact candidate-facing questions and their evaluation packages. Neither should ask the workflow how to write a specification.

## Minimum published structure

A completed candidate draft artifact contains:

- task identity, purpose, and included source inventory
- concise three-layer audience and usage summary
- visible assumptions, conflicts, and unresolved decisions
- `## Specification Knowledge Base`
- accepted `KQ-*` entries in source-native order
- coverage and traceability
- final readiness status

Every published KQ uses this minimum envelope:

> ### KQ-NNN — source-specific title  
> **Question:** source-specific question  
> **Answer:** concrete source-specific answer  
> **Source basis:** OBL-ID — human-readable label — path:line or path:start-end  
> **Acceptance check:** observable completion or verification condition

The actual file uses the literal labels `Question:` and `Answer:`. Bold styling is optional; aliases cannot replace the labels.

Add source-required unit fields inside the entry. Do not publish internal worker metadata merely because research stores it.

## Why Question → Answer

The format makes every entry answer a downstream need:

- What exact content should be used?
- Which constraint controls this decision?
- What evidence distinguishes a strong response?
- What happens under a stated condition?
- How can completion be observed?

It also gives the Critic a stable object to validate: question, answer, evidence basis, and acceptance condition.

Without the Q&A invariant, an agent can hide missing decisions inside broad prose. With it, an unanswered or generic question is easier to detect.

## The two KQ roles

Every proposed KQ has exactly one internal role. The role is retained in research unless the source explicitly requires it in the deliverable.

### `DOWNSTREAM_TARGET_QUESTION`

Use only when the downstream unit is itself an exact respondent-facing question and the relationship is `IS_FINAL_ARTIFACT` or `EMBEDS_EXACT_UNITS`.

The published `Question`:

- is addressed to the detected respondent
- seeks a response or evidence
- matches the operational direction
- can be used verbatim by the asker
- represents the exact `UNIT-*`

The published `Answer` contains the source-required answer semantics:

- expected evidence
- evaluation guidance
- a canonical answer only when the source requires one
- otherwise a response contract compatible with the source

Hypothetical interview example (non-normative): assume a separate source requires one candidate-facing question about time-series validation and expected strong-answer evidence, but does not require follow-ups or a scoring scale.

> **Question:** Describe a forecasting project in which you separated genuine predictive signal from time leakage. What validation design did you use, and how did the result affect a decision?  
> **Answer:** A strong response identifies temporal ordering, explains a time-aware validation method, quantifies out-of-sample performance, compares against a baseline, and connects the result to an actual decision.

This is candidate-facing and specific to the stated hypothetical fixture. “What question should assess forecasting?” is not. If a different source also requires follow-ups, warning signs, or score anchors, every one must be concretely instantiated; this role-only example does not claim those fields apply.

### `SPEC_KNOWLEDGE_QUERY`

Use for downstream content that is not intrinsically a respondent-facing question. Also use it for concrete schema, selection, and use knowledge when a question-native deliverable has the relationship `DEFINES_SCHEMA` or `REFERENCES_ONLY`, because exact respondent-facing units are not being embedded in those cases.

The published `Question` asks for a concrete domain decision, instruction, content unit, constraint application, or acceptance outcome that the spec reader needs.

The published `Answer` supplies the actual knowledge now.

Hypothetical event-planning role example (non-normative): assume a separate source fixes attendance at 24 participants, requires a 45-minute activity, and asks for a concrete operating plan.

> **Question:** What participation and timing plan follows from the fixed attendance and duration?  
> **Answer:** Plan capacity for exactly 24 participants and fit the complete introduction, activity, and close inside 45 minutes. Any team arrangement not supplied by the hypothetical source remains an explicit decision rather than a fabricated source fact.

The real-world participants are not being asked this question. The downstream organizer retrieves implementation knowledge from it.

## Answer semantics

`ANSWER_SEMANTICS` is internal classification used by the Critic. It explains what kind of answer is valid for the KQ role.

For target questions it might be expected evidence, evaluation guidance, canonical response, or response contract. For specification knowledge queries it might be an implementation decision, instruction, concrete content, or acceptance knowledge.

Do not publish “Answer: the agent should research this later.” That is neither an answer nor a valid unresolved-decision treatment.

## Source specificity test

Use the title-swap test:

> Would this question still fit an unrelated task after changing only the task title?

If yes, it is probably too generic.

Weak:

> What are the requirements?

Strong non-question-native form (hypothetical and non-normative):

> Which material-selection, substitution, and handling decisions keep every delivered unit within the source-stated safety and prohibited-content constraints?

Strong interview-oriented form:

> Tell me about a model you monitored after deployment when its input distribution changed; what evidence triggered action and how did you validate the response?

Specificity comes from source anchors: named items, actors, quantities, constraints, risks, tools only when source-relevant, and required outcomes.

## Grouping obligations into knowledge

Do not automatically create one KQ per OBL.

An obligation is a source requirement. A KQ is a coherent retrieval unit. One KQ may satisfy several obligations, and one obligation may require several KQs or unit fields.

Good grouping criteria:

- one downstream decision or use moment
- one source-native repeated unit
- fields that must be evaluated together
- one coherent conditional or safety control

Bad grouping criteria:

- matching ID numbers
- one heading per source sentence
- combining unrelated concerns only to reduce KQ count
- creating meta-questions to host global metadata

Global task identity, audience, shared constraints, aggregate calculations, and traceability may belong in framework sections or attached fields rather than separate author-facing KQs.

## Unit completeness

The complete `UNIT-*` may span the KQ's Question, Answer, and source-required fields. It cannot be hidden inside an IRQ.

Completeness depends on the artifact relationship:

### Exact-content relationships

For `CONTENT_INSTANCE`, `IS_FINAL_ARTIFACT`, and `EMBEDS_EXACT_UNITS`, every applicable required field, nested child, and finite member contains concrete instance content.

Field names or instructions do not count as values.

### `DEFINES_SCHEMA`

Every required field, rule, dependency, applicability condition, and validation meaning is concretely defined. Instances are required only if the sources request them.

### `REFERENCES_ONLY`

References are verified and include all required knowledge for selection, access, interpretation, and use.

## Required collection rules

### Plural collections

When a source requires a collection but gives no count or named member set, durable baseline state records `CARDINALITY: SOURCE_UNSPECIFIED`. The current Investigator proposal contract spells the same state `SOURCE_CARDINALITY: UNSPECIFIED`; this is an implementation compatibility alias, not a numeric value. The framework never invents a numeric minimum. If production needs a count, it must come from a labeled, source-compatible post-resolution decision and must not be presented as a source fact.

“Conditional follow-ups: …” fails both cardinality and concreteness.

### Conditional members

Each conditional member contains:

- a concrete trigger or condition
- the resulting action, question, content, or decision

“If needed, ask a follow-up” fails because neither “needed” nor the follow-up is defined.

### Finite enumerations

Every expected member appears exactly once. No member may be missing, duplicated, compressed, or out of domain.

If the source requires score levels 1–5, `1..5` is invalid. Five distinct entries are required.

### Ordered scales

Every level has a distinct, observable, unit-specific description and progresses consistently in the source-defined direction.

Endpoint-only descriptions fail because levels 2–4 remain undefined. Generic anchors repeated across every interview question also fail unit specificity when the source requires question-specific scoring.

## Placeholder policy

Produced required values must not contain or function as:

- `...` or an ellipsis character
- `TBD`, `TODO`, or `TBA`
- blank or null
- `same as above`
- `see template`
- a bare range such as `1..N`
- endpoint-only scale descriptions
- field-name-only text
- an unverified or insufficient cross-reference
- an instruction to add, list, research, or describe content later

These strings may appear only when quoted as source evidence or as bad examples in documentation—not as accepted values.

## Source basis

Every published source basis includes:

- `OBL-*` ID
- human-readable obligation label
- verified workspace-relative `path:line` or `path:start-end`

Do not use guessed lines, absolute file paths, or a Markdown link pretending to locate source evidence. The cited text must exist at the current location, must be admitted by the current source snapshot and scope table, and must support the entry.

Framework invariants use `FRAMEWORK` provenance and do not need a fake source citation.

## Acceptance checks

An acceptance check states an observable condition, not a restatement of intent.

Weak:

> The section is complete.

Stronger:

> The published entry lists each required score level exactly once, and every level describes observable evidence specific to this candidate question.

Stronger non-question-native example (hypothetical and non-normative):

> Component totals equal the declared output count, every substitution preserves the source-stated safety constraints, and all production batches fit the documented capacity within the strict time limit.

## Assumptions, conflicts, and unresolved items

Material assumptions must be visible. Do not present derived content as quoted fact.

An explicit source conflict cannot be silently resolved. A valid override record identifies the affected obligation, original wording and location, exact conflict shown to the user, exact response, explicit supersession, provenance, and downstream effect.

Generic controls such as `use defaults` do not constitute an override.

An unresolved content item may remain visibly classified as conditional only when isolated and not unsafe or unusable. The formal result `FINAL_GATE: CONDITIONAL` prevents publication; `RESOLUTION_GATE: CONDITIONAL` is not a valid gate state. A material unsafe or unusable gap blocks readiness.

## Published versus research-only fields

Normally publish:

- KQ heading and source-specific title
- `Question`
- `Answer`
- `Source basis`
- `Acceptance check`
- source-required unit fields
- task-level audience, assumptions, status, coverage, and traceability sections

Normally keep in research:

- `QUESTION_ROLE`
- `ANSWER_SEMANTICS`
- worker metadata
- IRQ records
- detailed completion matrices
- publication manifest
- downstream-use notes and forbidden-inference notes

Do not publish headings such as `Accepted Q&A entries` or fields such as `Exact accepted question` and `Accepted answer`; they expose obsolete workflow record language.

## Critic admission checklist per pair

- [ ] Literal `Question:` and `Answer:` are present.
- [ ] Question role matches the unit and relationship.
- [ ] Answer semantics are appropriate.
- [ ] The question's purpose, mapping, answer, and acceptance condition are bounded by active-source evidence; task vocabulary appears when context requires it, while intentionally reusable wording remains valid when the sources request it.
- [ ] Answer is concrete, self-contained, and usable.
- [ ] Obligation labels and verified locations support the content.
- [ ] Required fields, nested children, order, and cardinality are satisfied.
- [ ] Every conditional has trigger plus result.
- [ ] Finite enumerations have exact set equality.
- [ ] Ordered levels are distinct, observable, and unit-specific.
- [ ] No placeholder or deferred instruction appears.
- [ ] KQ and UNIT are not duplicates.
- [ ] Assumptions and unresolved decisions remain visible.
- [ ] Cross-unit totals and shared constraints remain consistent.

## Final knowledge-base checklist

- [ ] NEW removed managed stale state, created and reread exactly one `.spec-workflow/research.md` ledger first, and created and verified exactly one `.spec-workflow/source-snapshot.md` second.
- [ ] `RUN_INITIALIZATION_GATE`, generated reset, managed-path, source-scope, snapshot, evidence-membership, and single-run checks all pass; candidate and included counts are valid.
- [ ] Snapshot candidate/path/block counts and every per-block live-count/row-count/highest-ID/EOF/text equation pass.
- [ ] Every cited source line is admitted by the current scope and still matches the snapshot.
- [ ] At least one accepted source-grounded KQ is present.
- [ ] Every required unit maps to an accepted KQ.
- [ ] Every mandatory obligation has valid coverage.
- [ ] Exact respondent-facing units embedded in or finalized by `spec.md` are target-facing; non-question-native work and question-native schema/reference knowledge use specification knowledge queries.
- [ ] No IRQ or workflow meta-question is published.
- [ ] Global sections do not replace required unit content.
- [ ] Draft blocks map to the internal manifest.
- [ ] Final status accurately reflects unresolved items.
- [ ] All 18 `FINAL` checks are present and pass; `PREPUBLICATION_STATE_CHECK` is not treated as an overall decision or mode.
- [ ] Accepted KQ count is nonzero and equals the number of admitted KQ blocks physically present in the draft.
- [ ] Full mandatory coverage and zero unresolved material independently support `READY`.
- [ ] Published file matches the accepted draft exactly.

## Related reading

- [Role Naming](ROLE-NAMING.md)
- [Interrogation Algorithm](05_Interrogation_Algorithm.md)
- [Consistency Checks](08_Consistency_Checks.md)
- [File Formats](13_File_Formats.md)
- [Generic Usage](GENERIC-USAGE.md)
