# Role and Naming Reference

## Why naming matters

This framework operates across several audiences and two different kinds of questions. Ambiguous words such as “user,” “author,” “question,” or “answer” can make an agent address the wrong person or publish workflow chatter. The role and ID vocabulary keeps those meanings separate.

## Agent roles

### Text Spec Orchestrator

The run owner and only writer.

It discovers sources, creates current-run state, invokes workers, records gates, assembles the candidate, publishes `spec.md`, and requests postpublication verification. It is the custom agent the user selects in VS Code.

### Text Spec Investigator

The evidence analyst and content synthesizer.

In `BASELINE` mode it constructs the extraction-only evidence model. In `RESOLVE_BATCH` mode it proposes resolutions to accepted internal gaps. In `PRODUCE_BATCH` mode it creates publication-ready `KQ-*`/`UNIT-*` proposals for topics already planned by the Orchestrator. It returns proposals; it does not edit files.

“Investigator” is deliberately broader than “software analyst.” It can analyze a recipe brief, illustration request, policy, event plan, interview guide, or any other text-described goal.

### Text Spec Interrogator

The gap detector.

It asks only internal questions necessary to resolve material ambiguity, conflict, dependency, missing decisions, or unbounded synthesis. It does not interview the downstream respondent and does not author published KQ wording.

### Text Spec Critic

The read-only admission gate.

It evaluates baseline, resolution, production, final, and postpublication records. It returns structured decisions and exact defects. It does not repair files itself; the Orchestrator coordinates the correction.

## The three actor layers

### Specification knowledge layer

Names the immediate `spec.md` artifact and its readers.

Typical roles:

- specification producer: the workflow that builds `spec.md`
- specification reader: the person or agent using its knowledge
- downstream implementer: the actor turning the knowledge into a result

### Downstream deliverable layer

Names what is being specified and its source-native units.

Examples include a software behavior, recipe, illustration brief, policy control, lesson activity, or candidate question. These are examples only; sources define the actual unit.

### Operational or experience layer

Names the real-world interaction when one exists.

Typical roles:

- operator, presenter, performer, or asker
- recipient, participant, consumer, viewer, addressee, or respondent
- interaction direction and intended response or outcome

These layers must not be collapsed. In an interview guide, the spec reader may be a recruiter, the operator an interviewer, and the respondent a candidate. In a recipe, the reader and operator may both be the cook, while the recipients are diners. Some tasks have no direct respondent; record a reasoned `NOT_APPLICABLE` instead of inventing one.

## Namespace reference

| Namespace | Full meaning | Lives where | May appear in published `spec.md`? |
|---|---|---|---:|
| `OBL-*` | Grounded source obligation | Research and traceability | Yes, as traceability references when useful |
| `IRQ-*` | Internal resolution query | Research only | No |
| `UNIT-*` | Exact source-native downstream content unit | Research, draft mapping, and associated KQ fields | Yes, when the format exposes unit identity |
| `KQ-*` | Published specification knowledge Q&A entry | Research plan/accepted records, draft, and `spec.md` | Yes; internal planning and decision fields remain unpublished |

IDs are stable references inside one run. Their prefixes describe record type, not importance.

### `OBL-*`: obligation

An obligation is anchored in exact active-source wording and a verified workspace-relative location. It includes a human-readable label, mandatory status, treatment type, and coverage state.

Do not invent a recommendation and label it as an obligation. Recommendations, derived assumptions, and grounded synthesis use different provenance.

### `IRQ-*`: internal resolution query

An IRQ describes one cohesive unresolved decision and its downstream effect. It is always `WORKFLOW_ONLY`.

An IRQ is not a draft published question, and accepted IRQ wording must not be promoted into `spec.md`.

### `UNIT-*`: downstream content unit

A UNIT represents the exact repeated or individually validated item required by the downstream deliverable. If sources require one complete candidate question per competency, the competency is the coverage key and each `UNIT-*` is the complete candidate-facing primary-question package mapped one-to-one to that key. If the deliverable is not repeated, one unit may capture the complete source-native artifact or decision group.

### `KQ-*`: knowledge Q&A entry

A KQ is a published `Question` plus `Answer` and its supporting source basis, semantics, checks, and unit mapping. It is useful to the downstream reader, not to the workflow itself.

## Question roles

Each KQ has exactly one internal `QUESTION_ROLE`.

### `DOWNSTREAM_TARGET_QUESTION`

Use this only when all are true:

- the native unit is itself a respondent-facing question
- it seeks a response or evidence
- `spec.md` is the final artifact or embeds the exact question
- the wording is usable verbatim by the operational asker

The answer then contains the source-required expected evidence, evaluation guidance, canonical response when required, or a response contract.

### `SPEC_KNOWLEDGE_QUERY`

Use this for every other kind of downstream content. The question asks what concrete decision, instruction, content, constraint application, or acceptance outcome the downstream reader needs. The answer supplies that knowledge now.

Do not disguise an instruction as a respondent-facing question. A non-interrogative prompt belongs inside an answer when appropriate.

## Provenance names

| Provenance | Meaning |
|---|---|
| `FRAMEWORK` | A stable invariant of this agent workflow |
| `SOURCE` | Directly supported by exact source wording |
| `DERIVED_FROM_SOURCE` | Logically derived or synthesized within source bounds |

Recommended content classifications used in the workflow include `FACT`, `DERIVED`, `UNRESOLVED`, and `NOT_APPLICABLE`. Grounded original content should be identified as `GROUNDED_SYNTHESIS`; conservative missing optional choices can be `SAFE_DEFAULT`.

Provenance answers “where did this authority come from?” Status answers “what kind of knowledge state is this?” They are related but not interchangeable.

## Decision words

| Word | Meaning |
|---|---|
| `ACCEPT` | The reviewed phase or item meets its contract |
| `REVISE` | A specific repair is required; this is not acceptance |
| `BLOCKED` | Dependent work must stop because a required condition cannot currently be satisfied |
| `READY` | Final critic checks all passed and publication may occur |
| `CONDITIONAL` | Formal `FINAL_GATE` result for isolated visible gaps; it is not a valid resolution-gate value and it prevents publication |
| `PASS` | A named validation check succeeded |
| `FAIL` | A named validation check did not succeed |
| `NOT_REQUIRED` | A phase or check is genuinely unnecessary for a stated reason |
| `NOT_APPLICABLE` | The concept does not apply to this item for a concrete reason |

Never use `NOT_APPLICABLE` as a synonym for missing, unknown, inconvenient, or unfinished.

## Common naming mistakes

### Calling every human “the user”

This hides who reads the spec, who performs the downstream work, and who receives it. Name the role at the correct layer.

### Calling an IRQ a KQ

This publishes questions such as “What should the agent create?” instead of useful task knowledge.

### Calling a schema a unit instance

A list of field names is not an instantiated recipe step, candidate question, policy control, or storyboard panel when exact content is required.

### Treating `REVISE` as a successful review

`REVISE` keeps the gate open. The corrected artifact must be reviewed again.

### Using domain names in framework rules

A framework sentence such as “The word Python is a competency label” hardcodes one task. The general rule should refer to the detected source-native unit and let active evidence supply its name.

## Naming checklist

- [ ] Every actor belongs to one explicit layer.
- [ ] Interaction direction names the real operator and recipient when applicable.
- [ ] `OBL-*` records quote actual source wording.
- [ ] `IRQ-*` records remain internal.
- [ ] Each required repeated item has a `UNIT-*`.
- [ ] Each published entry has a `KQ-*` and one valid question role.
- [ ] Provenance and knowledge status are both explicit.
- [ ] Decision words use their exact gate semantics.
- [ ] Framework rules use generic nouns; task output uses source-specific nouns.

## Related reading

- [Framework Architecture](02_Framework_Architecture.md)
- [Shared Memory](04_Shared_Memory.md)
- [Q&A Spec Manual](QA-SPEC-MANUAL.md)
- [Visible Workflow](VISIBLE-WORKFLOW.md)
