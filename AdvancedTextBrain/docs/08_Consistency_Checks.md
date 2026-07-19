# 08 — Consistency Checks

Consistency checks ensure that a specification is not merely well written but structurally complete, source-faithful, internally compatible, and usable by the next implementation or content-production step.

The workflow checks individual entries during production, the complete candidate during final review, and the written file after publication.

## What is checked

### Source consistency

Before any source citation is trusted, `.spec-workflow/research.md` and `.spec-workflow/source-snapshot.md` must exist at their exact paths with matching RUN_ID and no duplicate identity artifact. The snapshot passes only when candidate count equals sorted path count and source-block count, and each block's `LINE_COUNT` equals the live line count, contiguous row count, and highest row ID through EOF, with exact text equality. Missing, orphaned, misplaced, partial, or mismatched state fails the baseline guard and is repaired locally before semantic review; it is never downgraded to a warning or a request for the user to rerun baseline.

Every obligation must come from exact active-source wording at a verified workspace-relative location. Recommendations and grounded synthesis remain separately classified; they cannot be disguised as source obligations.

Published locations use plain `path:line` or `path:start-end` form and include a human-readable obligation label. Guessed lines, absolute paths, Markdown source hyperlinks, and unverified locations fail review.

Exact operators and quantities are preserved. `<` is not interchangeable with `<=`; `each` is not optional coverage; a fixed count is not a suggested range.

### Relationship-specific completeness

Completeness depends on the Artifact Contract relationship:

| Relationship or treatment | Required completeness |
|---|---|
| `CONTENT_INSTANCE`, `IS_FINAL_ARTIFACT`, `EMBEDS_EXACT_UNITS` | Concrete instance content for every applicable required field and member |
| `DEFINES_SCHEMA` | Complete field, rule, dependency, applicability, and validation meaning; instances only when sources require them |
| `REFERENCES_ONLY` | Verified references plus all source-required selection, access, interpretation, and use knowledge |

A schema is not inherently invalid. It becomes invalid when it replaces exact content that the active source or relationship requires.

### Field instantiation

During baseline, record only source-explicit shape:

- `CONTENT_AREA`: requiredness, meaning, constraints, source basis, and completion test;
- `REPEATED_UNIT`: only source-required fields, conditions, order, dependencies, and source-supported cardinality; and
- `ENUMERATION_OR_SCALE`: only source-defined members, range, and progression.

Detailed production fields and applied defaults are post-baseline decisions. The baseline does not invent a schema merely because the requested content is plural.

A required field passes only when its applicable content is concrete, nonempty, type-correct, source-specific, and cardinality-correct.

### Conditional content

Every conditional member contains both halves:

- a concrete condition or trigger; and
- the resulting content or action.

For example, a follow-up collection containing only “probe deeper if needed” is incomplete because neither the trigger nor the exact follow-up is sufficiently instantiated.

When a required collection has neither a source-defined count nor a named set, durable research records `CARDINALITY: SOURCE_UNSPECIFIED`. The current Investigator response contract uses the compatibility spelling `SOURCE_CARDINALITY: UNSPECIFIED`. Any concrete production count must be a labeled, source-compatible post-resolution default, never a source fact.

### Lists

List-valued fields contain discrete members. A comma-separated paragraph does not satisfy a contract that requires independently checkable evidence items or warning signs.

### Finite enumerations and ordered scales

Every finite domain is expanded into an expected-member ledger. Set equality is required: no member may be omitted, duplicated, compressed, or added outside the domain.

For an ordered scale, each level must be:

- present exactly once;
- observable;
- specific to the unit;
- distinct from adjacent levels; and
- directionally coherent.

If a source requires a 1–5 rubric, the following do not pass:

- `1..5`;
- descriptions for only levels 1 and 5;
- generic labels such as “poor,” “average,” and “excellent” without question-specific evidence; or
- five descriptions whose quality progression is indistinguishable.

### Placeholder scan

Required produced values cannot contain or function as:

- ellipses;
- `TBD`, `TODO`, or `TBA`;
- blank or null values;
- “same as above”;
- “see template” when the referenced content is insufficient or unverified;
- bare ranges such as `1..N`;
- selected endpoints standing for a full scale; or
- instructions to complete the work later.

Quoted source evidence is the only exception for literal placeholder strings.

### Question and answer consistency

Every published `KQ-*` includes literal `Question:` and `Answer:` fields. Source-native aliases may supplement them but cannot replace them.

For `DOWNSTREAM_TARGET_QUESTION`:

- the question is the exact respondent-facing unit;
- the operational actor can use it verbatim;
- the perspective matches the operational layer; and
- the answer semantics provide expected evidence, evaluation guidance, a source-required canonical response, or a source-compatible response contract.

For `SPEC_KNOWLEDGE_QUERY`:

- the question asks for concrete domain knowledge needed by the specification reader; and
- the answer supplies that knowledge now.

Meta-questions about writing the specification fail both roles.

### One-main-mapping rules

When a source requires one main competency or another single native mapping, each unit must map to exactly one valid target. A cross-cutting meta category or multiple simultaneous main targets is not equivalent.

### Cross-unit consistency

Final review recomputes aggregate limits, totals, ordering, coverage, and shared dependencies.

Examples include:

- the sum of question durations must fit the recommended total, including any source-required overhead;
- every required competency must appear in a primary unit when follow-up-only coverage is forbidden;
- a single-oven schedule cannot place two simultaneous oven tasks in the same interval;
- a variant must provide every field, quantity, dependency, and step required to make that variant usable; and
- a global safety constraint must apply consistently to every relevant unit and substitution.

Individual validity never excuses an incompatible aggregate.

### Workflow and state consistency

All universal initialization, reset, managed-path, scope, snapshot, membership, and single-run checks must still pass. The candidate must agree with current-run research, accepted IDs, coverage, conflicts, readiness, and the research-only manifest. A preserved `PRIOR_PUBLISHED` file is excluded from prepublication comparison.

After publication, the newly written `spec.md` must exactly match the accepted draft and preserve the same status, counts, Q&A entries, conflicts, traceability, and coverage.

## Named production checks

Each produced pair reports:

- `FIELD_INSTANTIATION_CHECK`;
- `PLACEHOLDER_SCAN`;
- `ENUMERATION_COMPLETENESS_CHECK`; and
- `ORDERED_SCALE_QUALITY_CHECK`.

The ordered-scale result may be `NOT_APPLICABLE` only when the Field Instantiation Contract explicitly shows that no ordered scale applies.

## Named final checks

Final review reports independent results for:

- source inventory;
- Artifact Contract;
- Q&A knowledge base;
- field instantiation;
- placeholders;
- enumeration completeness;
- ordered-scale quality;
- source specificity;
- cross-unit consistency;
- question role;
- IRQ leakage;
- unit coverage;
- defaults and conflicts;
- strict operators;
- variant completeness;
- prepublication state;
- source locations; and
- manifest integrity.

`FINAL_STATUS: READY` is forbidden unless every named check passes and no material unresolved or blocked obligation remains.

## Failure examples

### Complete schema, incomplete instances

A guide declares that each question has follow-ups, evidence, warnings, and scoring anchors, but most entries contain only field names. This fails field instantiation.

### Locally valid, globally impossible

Fifteen interview questions each have reasonable time estimates, but their typical total exceeds the stated interview duration. This fails cross-unit consistency.

### Attractive but untraceable

The content is detailed, but its source references use guessed links or bare IDs. This fails source-location and traceability checks.

### Finished-looking placeholder

A scoring field says `1..5`. The document looks compact, but four required semantics remain unknown. This fails placeholder, enumeration, and ordered-scale checks.

## Consistency checklist

- [ ] Do exact research and snapshot paths exist with one matching RUN_ID and no duplicate identity artifact?
- [ ] Do snapshot candidate/path/block counts and every per-block live-count/row-count/highest-ID/EOF/text equation pass?
- [ ] Is every obligation grounded in active source wording?
- [ ] Are exact operators and quantities preserved?
- [ ] Does each unit satisfy the correct relationship-specific contract?
- [ ] Are all required children populated?
- [ ] Do collections satisfy source-explicit cardinality, or remain durably `CARDINALITY: SOURCE_UNSPECIFIED` until a labeled post-resolution default is accepted?
- [ ] Does every conditional member have both trigger and action?
- [ ] Are list members discrete?
- [ ] Are all finite members present exactly once?
- [ ] Are ordered levels observable and progressively distinct?
- [ ] Is the placeholder scan clean?
- [ ] Are question roles and answer semantics correct?
- [ ] Does every one-main mapping have exactly one target?
- [ ] Do totals, ordering, coverage, variants, and shared constraints agree?
- [ ] Does every draft block map to the manifest?
- [ ] Does the published file exactly match the accepted draft?

## Related reading

- [06 — Critic and Convergence](06_Critic_and_Convergence.md)
- [07 — Observability](07_Observability.md)
- [09 — Run Health](09_Run_Health.md)
- [10 — Debugging](10_Debugging.md)
- [Text Spec Critic](../.github/agents/text-spec-critic.agent.md)
- [Text Spec Investigator](../.github/agents/text-spec-investigator.agent.md)
