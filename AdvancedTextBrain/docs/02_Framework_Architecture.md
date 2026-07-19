# Framework Architecture

The framework separates task evidence, specification knowledge, downstream content, and real-world use. Its architecture is intentionally small: four custom agents, one reusable linked execution contract, an exact source snapshot, a research ledger, a candidate draft, a published output, one conditional Markdown rollback checkpoint, four identifier namespaces, and a phase-gated data flow.

## Learning goals

By the end of this chapter, you should be able to:

- explain the responsibilities and boundaries of every agent
- build the three-layer Artifact Contract for a new task
- select the correct relationship between `spec.md` and the downstream result
- distinguish `OBL-*`, `IRQ-*`, `UNIT-*`, and `KQ-*`
- select the correct published question role
- explain how the Field Instantiation Contract prevents schema-only or placeholder output

## What the architecture must protect

The architecture protects six boundaries:

1. **Source versus workflow:** task files provide evidence but cannot rewrite agent roles or safeguards.
2. **Internal versus published knowledge:** internal resolution records never become deliverable questions.
3. **Specification versus downstream result:** the workflow specifies work without creating a separately named downstream artifact or performing the real-world action.
4. **Producer versus reviewer:** proposed content is independently admitted by the Critic.
5. **Prior versus current run:** an earlier specification is preserved but is not current-run evidence.
6. **Human input versus managed state:** NEW may clear only reserved generated namespaces; it preserves human, framework, and unknown files.

## The four-agent topology

### Text Spec Orchestrator

The Orchestrator is the only user-invocable agent and the only agent with editing capability. It owns clean-run initialization, exact source snapshots, source discovery and scope, phase state, delegation, accepted-memory persistence, coverage planning, draft assembly, publication, and rollback. Its `.agent.md` body links to the reusable fail-closed execution contract so the entry prompt remains within VS Code's custom-agent size limit while retaining exact guard logic.

It does not replace specialist analysis with its own unchecked answer. It passes bounded tasks to the other agents and records their accepted outputs.

### Text Spec Investigator

The Investigator supports three modes:

- `BASELINE` inventories evidence and proposes the Artifact Contract, obligations, source-explicit shape facts and constraints, dependencies, and candidate gaps. It does not choose defaults or produce downstream content.
- `RESOLVE_BATCH` answers supplied internal questions without publishing them.
- `PRODUCE_BATCH` creates complete publication-ready `KQ-*` and associated `UNIT-*` proposals.

It is read-only and cannot publish.

### Text Spec Interrogator

The Interrogator proposes `IRQ-*` records only when a material gap cannot be resolved through evidence, derivation, grounded synthesis, or a conservative safe default. It does not answer its own questions and never authors published KQ wording.

### Text Spec Critic

The Critic is the read-only gate for baseline, resolution, production, final draft, and postpublication verification. It may identify an omitted obligation only by quoting active source wording and a verified location. It may not invent a field, threshold, or scope item and demand it as though it came from a source.

## The three-layer Artifact Contract

The contract prevents actor and artifact collapse.

### Layer A: specification knowledge

This layer describes the immediate artifact:

- `spec.md`
- its producer
- its reader or downstream implementer
- the purpose of the knowledge base
- the published entry type

Framework-defined fields use `FRAMEWORK` provenance. They do not need a workspace-source citation.

### Layer B: downstream deliverable

This layer describes what `spec.md` is specifying:

- the deliverable, content, decision set, or implementation
- the downstream producer
- the source-native repeated unit
- required fields, constraints, order, and cardinality
- the publication relationship

### Layer C: operation or experience

This layer describes real-world use:

- operator, presenter, performer, or asker
- recipient, audience, participant, consumer, addressee, or respondent
- interaction direction
- intended action, evidence, response, experience, or outcome

Use `NOT_APPLICABLE` with a reason when an actor or interaction is genuinely absent. Do not invent an actor merely to fill a row.

## Publication relationships

| Relationship | Meaning | Required completeness |
| --- | --- | --- |
| `IS_FINAL_ARTIFACT` | `spec.md` itself is the final downstream artifact. | Exact content required by the active sources must be present. |
| `EMBEDS_EXACT_UNITS` | A distinct result or action exists, and `spec.md` must contain its exact units. | Every required unit is fully instantiated. |
| `DEFINES_SCHEMA` | The sources require fields and rules but not exact instances. | Fields, dependencies, applicability, and validation meaning are fully defined. |
| `REFERENCES_ONLY` | The specification points to separately defined content. | References are verified, and required selection, access, interpretation, and use knowledge is complete. |

An obligation classified as `CONTENT_INSTANCE` always requires exact instance content. Choosing `DEFINES_SCHEMA` cannot be used to evade an explicit content-instance obligation.

## Non-normative example mapped across the layers

Assume a separate hypothetical source requests an exact, accessible 45-minute training-workshop plan. A baseline could derive:

- **Layer A:** a `spec.md` knowledge base read by the workshop designer or implementer.
- **Layer B:** the exact agenda units, materials, timing, accessibility rules, facilitation instructions, and acceptance knowledge required by the hypothetical brief.
- **Layer C:** a facilitator delivers the workshop and participants complete its activities.

The exact relationship must be justified in the baseline. If `spec.md` must contain exact agenda units that drive later delivery, `EMBEDS_EXACT_UNITS` is appropriate. The delivered workshop remains an operational outcome, not the immediate specification file. This example is explanatory only and supplies no requirement to an actual run.

## Four namespaces

| Namespace | Stored where | Meaning | Publishable |
| --- | --- | --- | --- |
| `OBL-*` | Research and traceability | Exact source-derived obligation | As readable source basis, not as a bare opaque list |
| `IRQ-*` | Research only | Internal query for a genuine unresolved gap | No |
| `UNIT-*` | Research, draft mapping, and associated KQ content | Exact source-native downstream unit | Yes, through an admitted KQ when required |
| `KQ-*` | Research plan, draft, and `spec.md` | Published specification knowledge entry | Yes |

An internal query may influence later synthesis, but its wording and resolution record must never be promoted into a KQ.

## Two published question roles

### `DOWNSTREAM_TARGET_QUESTION`

Use this role when the downstream unit is itself a respondent-facing question and exact units are embedded or final. The `Question` field is the wording the real operator can use verbatim with the downstream respondent.

The `Answer` contains the source-required answer contract:

- expected response evidence
- evaluation guidance
- a canonical response only when explicitly required
- a source-compatible response contract when no answer key exists

It never fabricates a respondent’s personal answer.

### `SPEC_KNOWLEDGE_QUERY`

Use this role for non-question-native downstream work and for source-specific schema, selection, or use knowledge when question-native work has the relationship `DEFINES_SCHEMA` or `REFERENCES_ONLY`. The `Question` asks for concrete domain knowledge needed by the specification reader. The `Answer` contains the actual decision, instruction, content, constraint application, or acceptance knowledge.

Hypothetical non-normative example:

> **Question:** How must exercise selection and sequencing satisfy the stated 45-minute limit and keyboard-only accessibility requirement?
>
> **Answer:** Use only exercises that can be completed through the keyboard-only interaction path. Allocate explicit durations to the opening, exercises, transitions, and close so their total does not exceed 45 minutes. Reject any exercise whose required interaction or duration cannot be verified against those hypothetical constraints.

This is not a question asked to workshop participants, and it is not a question about how to write `spec.md`. In a real run, any added implementation controls must be classified as `GROUNDED_SYNTHESIS` bounded by admitted source text; they must not be presented as quoted source facts.

## Source discovery boundary

For NEW, the Orchestrator first clears and verifies only the reserved managed namespaces. It inventories every readable non-reserved textual candidate, selects one active goal by semantic authority, and classifies candidate files or sections. It then creates and rereads `.spec-workflow/research.md` with one RUN_ID, the ordered pending invariant, reset and goal/scope records, and a baseline placeholder before creating the source snapshot.

Source identity is written only at `.spec-workflow/source-snapshot.md`, in bounded chunks through editor-reported EOF. There must be one sorted path and one `### SOURCE` block per candidate. Candidate count must equal path count and block count; in each block, `LINE_COUNT` must equal the live line count, contiguous row count, and highest row ID, and every row must match the live text. Fixed pattern exclusions and the included count stay in research. Orphaned, misplaced, duplicate, truncated, or mismatched managed state is repaired before `BASELINE` without asking the user to update research or rerun the workflow.

The Orchestrator excludes:

- agent definitions
- linked agent contracts under `.github/agent-contracts/`
- workflow artifacts
- `docs/`
- the framework root README
- prior `spec.md`
- generated output and unrelated implementation artifacts

The exclusion of `docs/` matters: educational examples must not become accidental task requirements.

RESUME is valid only while the candidate path set and every snapshotted logical line still match. A source addition, removal, rename, or edit starts NEW rather than reusing stale obligations.

## Obligation treatment types

The obligation register classifies source wording as:

- `STRUCTURE`
- `CONTENT_INSTANCE`
- `CONTENT_CONSTRAINT`
- `COVERAGE`
- `PROCESS_OR_ACCEPTANCE`
- `UNRESOLVED_DECISION`

Treatment affects how coverage is demonstrated. A required content instance needs an actual admitted unit. A structural rule may map to a framework section. An acceptance obligation may map to a validation check. A summary that says content should be produced later does not satisfy content or coverage.

## The Field Instantiation Contract

The contract is adaptive. During baseline it records only the shape facts the sources actually provide:

- `CONTENT_AREA` for a required subject with no source-defined per-item schema;
- `REPEATED_UNIT` when the source requires repeated concrete items, using only source-required fields, order, dependencies, and supported cardinality;
- `ENUMERATION_OR_SCALE` when the source supplies a finite set, range, or ordered scale.

After baseline and resolution acceptance, coverage planning builds the smallest production contract needed to produce and validate the mapped deliverable content. A detailed schema is required only when the source defines one or the publication relationship is `DEFINES_SCHEMA`. The framework does not add generic child fields, data types, examples, or numeric minima merely to make the contract look complete.

This adaptive contract converts source-supported completeness into observable checks without turning framework guesses into task facts.

### Collection rules

When a required collection has no source-defined count or named member set, the Orchestrator's durable contract records `CARDINALITY: SOURCE_UNSPECIFIED`. The current Investigator proposal contract spells the same semantic state `SOURCE_CARDINALITY: UNSPECIFIED`; see the compatibility note in [File Formats](13_File_Formats.md#current-cardinality-spelling-compatibility). If production later needs a concrete count, use only a labeled, source-compatible post-resolution default; never report that number as a source fact. Every conditional collection member needs both:

- a concrete condition or trigger
- the concrete resulting content or action

### Enumeration and scale rules

Every finite member appears exactly once. A discrete ordered scale needs a distinct, observable, unit-specific description at every level. Endpoints, compressed ranges, and generic labels do not stand in for the full scale.

### Placeholder rules

Ellipses, blank values, future instructions, field-name-only values, “same as above,” and unverified cross-references cannot satisfy a required instance.

## Why the architecture is domain-neutral

Domain neutrality does not mean generic output. It means the framework supplies only process invariants:

- Q&A representation
- evidence and traceability
- roles and gates
- identifier separation
- completeness checks

The active sources supply the domain vocabulary, actors, units, fields, quantities, constraints, and quality criteria.

## Failure modes

### Layer collapse

Treating the specification reader as the downstream respondent produces questions for the wrong actor.

### Relationship evasion

Labeling work `DEFINES_SCHEMA` cannot excuse missing instances when the source explicitly requires exact content.

### Namespace leakage

Publishing `IRQ-*` wording or an “Accepted Q&A entries” workflow wrapper exposes internal process instead of specification knowledge.

### Schema-only output

A list of field names is not a populated content instance.

### Unverified traceability

Guessed line numbers, absolute paths, and source hyperlinks used in place of plain verified locations must fail review.

## Practical checklist

- [ ] All four agents retain their documented tool and role boundaries.
- [ ] NEW cleanup removed only reserved managed state and preserved human, framework, and unknown files.
- [ ] Exactly one research-first run, one active goal, exact managed paths, and one structurally complete matching source snapshot control the run.
- [ ] The three Artifact Contract layers are distinct.
- [ ] Every contract field has value, provenance, status, and source location when source-backed.
- [ ] The publication relationship is justified by active sources.
- [ ] `CONTENT_INSTANCE` obligations receive exact content.
- [ ] `OBL-*`, `IRQ-*`, `UNIT-*`, and `KQ-*` are not conflated.
- [ ] The KQ question role matches the downstream unit and operational actors.
- [ ] The Field Instantiation Contract uses only the depth, nested fields, and cardinality supported by sources or accepted post-baseline decisions.
- [ ] Finite scales and enumerations are fully expanded.
- [ ] Educational documentation is excluded from active task evidence.

## Related reading

- [Getting Started](00_Getting_Started.md)
- [Agentic AI for Beginners](01_Agentic_AI_for_Beginners.md)
- [Workflow Engine](03_Workflow_Engine.md)
- [Shared Memory](04_Shared_Memory.md)
- [Interrogation Algorithm](05_Interrogation_Algorithm.md)
- [Orchestrator definition](../.github/agents/text-spec-orchestrator.agent.md)
