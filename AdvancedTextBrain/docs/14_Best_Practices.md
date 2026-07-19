# 14. Best Practices

## Purpose

These practices keep the VS Code GitHub Copilot custom-agent workflow generic, source-grounded, complete, and recoverable. They apply across domains; examples are illustrative rather than normative.

## 1. Enter through the Orchestrator

**What:** Select **Text Spec Orchestrator** before sending `run`.

**How:** Use the VS Code custom-agent picker and confirm that source discovery begins immediately.

**Why:** Agent instructions apply only while the custom agent is active. A general Copilot agent can interpret `run` as code or task execution.

## 2. Treat control utterances as control only

`run`, `continue`, and `use defaults` are workflow controls. They are not domain facts, constraint overrides, or permission to execute the downstream goal.

Record domain decisions only when a source or an explicit, specifically scoped user response supports them.

## 3. Infer scope from semantics, not filenames

Read every eligible text source and classify it by content. A file named `notes.md` can contain a mandatory constraint; a familiar filename can contain unrelated material.

For NEW, delete and verify only the reserved managed `.spec-workflow/**` and legacy `outgoing/**` namespaces; preserve human, framework, and unknown files. Inventory every non-reserved textual candidate, select one active goal by semantic authority, and create and reread `.spec-workflow/research.md` first with pending invariant, reset, goal/scope, and baseline-placeholder records. Then create `.spec-workflow/source-snapshot.md` at that exact path in bounded chunks through EOF. Verify candidate/path/block equality and every per-block live-count/row-count/highest-ID/text equation before persisting `GUARD_CALL_BASELINE`. Do not use prior generated artifacts or excluded snapshot lines as evidence. RESUME is valid only while the live candidate path set and every snapshotted line still match.

Treat orphaned, misplaced, duplicate, truncated, or malformed initialization as locally correctable managed state. Repair it or cleanly retry NEW before a worker call; never ask the user to create research, rerun baseline, or approve proceeding.

## 4. Keep the three actor layers separate

Distinguish:

- the specification producer and reader;
- the downstream deliverable and its producer;
- the operational presenter, participant, recipient, or respondent.

This prevents a specification-reader question from being published where a real-world respondent question is required.

## 5. Ground every obligation

Every `OBL-*` needs:

- exact source wording;
- human-readable label;
- verified workspace-relative location;
- heading;
- mandatory status;
- treatment type.

Do not convert helpful model ideas into fake source requirements. Keep synthesis and assumptions separately classified.

## 6. Preserve exact logical force

Operators and quantities matter:

- `each` is not “some”;
- `all` is not optional coverage;
- `<` is not `<=`;
- a fixed count is not a suggestion;
- an inclusive discrete scale is not its endpoints.

Normalize these rules into expected-member and cardinality ledgers before generation.

## 7. Prefer grounded synthesis over unnecessary questions

If goals, constraints, and acceptance conditions safely bound original content, use `GROUNDED_SYNTHESIS` and cite those bounds.

Create an `IRQ-*` only when a material choice cannot be resolved by evidence, derivation, or a conservative source-compatible default. Asking about every explicit field wastes time and can turn requirements into negotiable preferences.

## 8. Keep internal and published questions separate

`IRQ-*` asks the Investigator to resolve a knowledge gap. It is never publishable.

`KQ-*` is written in `PRODUCE_BATCH` for the specification reader or downstream respondent. It must be independently source-specific.

Never wrap valid domain content inside an internal question-and-answer record and publish the wrapper.

## 9. Select the correct published question role

Use `DOWNSTREAM_TARGET_QUESTION` only when:

- the downstream unit is genuinely a respondent-facing question;
- it seeks a response or evidence;
- exact units are final or embedded.

Use `SPEC_KNOWLEDGE_QUERY` for non-question-native work, schema/use knowledge, and references. Put non-interrogative prompts or instructions in the answer, not in a mislabeled target question.

## 10. Define completeness before generating content

The adaptive Field Instantiation Contract must precede production. It defines only the topics, fields, nested members, cardinalities, enumerations, dependencies, and applicability supported by sources or accepted post-baseline decisions. Persist `CARDINALITY: SOURCE_UNSPECIFIED` instead of inventing a numeric minimum; recognize `SOURCE_CARDINALITY: UNSPECIFIED` as the current Investigator proposal spelling, not a different semantic state.

This prevents the model from deciding after generation that a field label or placeholder is “close enough.”

## 11. Instantiate every required field

Reject:

- blank or null values;
- ellipses;
- `TBD`, `TODO`, or `TBA`;
- `same as above`;
- bare ranges;
- selected scale endpoints;
- field-name-only values;
- future instructions;
- cross-references used instead of required content.

For a conditional member, require both the trigger and resulting action/content. For a nested package, validate every child independently.

## 12. Expand discrete scales completely

When sources define a finite scale or enumeration, write every expected member exactly once. Each ordered member must be:

- observable;
- distinct from adjacent members;
- specific to the represented unit;
- directionally coherent with the source-defined progression.

Do not allow examples or endpoints to stand for the complete domain.

## 13. Batch for independence, not imitation

Use batches of no more than five planned entries. Supply a compact accepted-topic index to prevent duplication, but include full prior content only when there is a dependency or overlap.

If a batch is too large to complete without abbreviation, reduce it. Never trade completeness for fewer tool calls.

## 14. Pass complete subagent context

Each delegation should include the active goal, relevant sources, Artifact Contract, mapped obligations, field contract, applicable resolutions, and expected output.

Subagents have isolated working context. Do not make them infer a missing actor, field schema, or prior decision from a vague instruction.

## 15. Make the Critic decide, not advise

The Critic returns explicit decisions and named checks. `REVISE` is not acceptance.

For every production pair require:

- KQ decision;
- UNIT decision;
- field-instantiation result;
- placeholder result;
- enumeration-completeness result;
- ordered-scale result when applicable.

Correct the reported defect and rerun the gate.

## 16. Validate across units

Individual units can all look correct while their totals, ordering, coverage, or shared constraints conflict.

Compute source-required aggregates before final review. Examples include total time, total quantity, coverage of every named item, and dependency order.

## 17. Apply relationship-specific completeness

Do not demand instance values from a schema-only task, and do not accept a schema where exact instances are required.

- Final or embedded work requires exact content.
- Schema work requires complete definitions and validation meaning.
- Reference work requires verified references and use knowledge.

## 18. Handle defaults and overrides conservatively

A default can fill only a genuinely missing optional choice. It cannot erase a material constraint.

An override of conflicting source evidence needs a complete record: affected obligation, original text and location, conflict shown, exact user response, explicit supersession, provenance, and downstream effect. Material safety constraints require explicit acknowledgment.

## 19. Enforce phase gates

Maintain one reconciled Run Invariant Block alongside the Phase Gate Ledger. A no-gap resolution phase is `NOT_REQUIRED` with a reason, not absent.

Before baseline and every later guarded action, recompute run initialization, generated reset, managed-artifact path, source scope, snapshot freshness, evidence membership, and single-run state. Before every phase transition, draft or specification write, postpublication call, and success response, persist and reread an `ACTION_AUTHORIZATION`. Final review begins with `FINAL_GATE: PENDING`; the result is recorded afterward. Postpublication begins with `POSTPUBLICATION_GATE: PENDING` and success is reported only after `PASS`.

Keep permission approvals, the optional VS Code host boundary, and Critic phase gates separate. `REVISE` is an internal quality decision and needs no user approval. The three-loop maximum exists only for the Autopilot permission level with `chat.autopilot.advanced.enabled: true`; it does not apply in Default Approvals or Bypass Approvals. Persist state after every change, keep `CURRENT_REVISION_DEFECT_SIGNATURE` and `SAME_DEFECT_REVISION_COUNT` unchanged by host continuations, and use `resume` only when the optional boundary actually stops an incomplete run.

## 20. Publish transactionally

At NEW initialization, preserve any prior `spec.md` content while clearing managed state, then recreate `.spec-workflow/prior-spec.md` as its verified verbatim rollback checkpoint and leave the publication unchanged. Only after the write guard passes may the ready candidate temporarily replace it for postpublication checks. If validation fails:

- restore the previous content when it existed;
- remove the failed new file when the prior state was absence;
- verify the restored state.

This prevents a failed run from destroying the last accepted specification.

## 21. Keep runtime artifacts Markdown-native

Use Markdown tables and lists for obligations, field contracts, ledgers, manifests, and Q&A.

Do not persist internal JSON tool transport. Do not introduce JSON Schema as a default completeness mechanism; it validates shape more readily than meaning.

## 22. Test domain neutrality

Use substantially different fixtures.

For example, use one hypothetical non-question-native planning fixture with quantities, strict constraints, and cross-unit calculations, and one hypothetical question-native interview fixture with target-facing questions, conditional children, and a fully expanded evaluation scale. Both examples are non-normative. Run them separately through NEW initialization so one fixture cannot contaminate the other's source snapshot or ledger.

The agents pass when the framework mechanics remain stable while all domain vocabulary and fields change with the sources.

## Failure-mode table

| Failure | Root cause | Preventive practice |
|---|---|---|
| Generic “What are the requirements?” entries | Internal resolution leaked into publication | Separate IRQ and KQ roles. |
| Candidate question replaced by author question | Artifact layers collapsed | Validate question role and operational perspective. |
| `conditional_items: ...` | Presence mistaken for completeness | Use nested field/member checks. |
| `scale: 1..5` | Range compressed instead of expanded | Use expected-member equality and ordered-scale checks. |
| New threshold appears as an obligation | Synthesis disguised as evidence | Require exact source wording for every OBL. |
| Output becomes a recipe, plan, or policy without Q&A | Universal envelope skipped | Require at least one accepted KQ and literal Question/Answer fields. |
| Valid schema rejected for lacking instances | Relationship ignored | Apply relationship-specific completeness. |
| Prior output influences current facts | Generated file included as source | Exclude spec and workflow records. |
| A NEW run contains an old RUN_ID, domain record, or outgoing artifact | Managed state was appended or incompletely cleared | Delete and verify reserved generated namespaces before creating and rereading one research-first ledger, then the snapshot. |
| A snapshot exists without research, at the wrong path, or with incomplete rows | Initialization was interrupted or identity checks were skipped | Repair or cleanly retry NEW internally; verify exact paths and all path/block/count/row/EOF/text equations before baseline. |
| RESUME uses obligations after a source edit | Snapshot freshness was not recomputed | Convert to NEW and rebuild from the current candidate paths and logical lines. |
| `run` searches for a project entry point | Wrong agent or missing entry semantics | Select the Orchestrator and use its invocation contract. |
| `READY` appears with zero KQs or partial coverage | Worker label trusted over evidence | Reconcile invariant counts and require the exact write guard. |
| Critic is called in `PREPUBLICATION` mode | A check name was mistaken for a mode | Use `FINAL`; treat `PREPUBLICATION_STATE_CHECK` as only one of 18 checks. |

## Author checklist

- [ ] Agent prompts contain no current-task terminology as framework rules.
- [ ] Every rule says whether it is framework invariant or source-derived.
- [ ] New fields are source-supported or explicitly classified post-baseline decisions and are reflected in the Field Instantiation Contract.
- [ ] Worker response contracts match Orchestrator requests.
- [ ] Critic decisions match Orchestrator admission logic.
- [ ] Guard predicates are non-circular and use the same field names as Critic responses.
- [ ] Host Autopilot continuation and Critic revision counters are explicitly separate and durable.
- [ ] Research-first initialization, managed-path ownership, source-snapshot equations, automatic repair, and single-run guards map to the linked execution contract.
- [ ] Every `.agent.md` stays below the documented prompt-size limit; reusable linked contracts resolve and remain excluded from task evidence.
- [ ] Final and postpublication checks repeat material production checks.
- [ ] Documentation links stable headings rather than line numbers.

## Run checklist

- [ ] Correct VS Code custom agent selected.
- [ ] NEW removed and verified reserved managed state without deleting human, framework, or unknown files.
- [ ] Research was created and reread before the exact-path snapshot, and orphaned or malformed state was repaired without a user rerun prompt.
- [ ] One active goal, one current RUN_ID, and one exact source snapshot control the run; candidate/path/block and per-block EOF/count/text equations pass.
- [ ] Run Invariant Block and action authorization records are complete.
- [ ] Any host-enforced pause resumed the same run rather than becoming `BLOCKED` or a fresh run.
- [ ] Complete source inventory accepted.
- [ ] Every obligation grounded and classified.
- [ ] Field, enumeration, and dependency ledgers accepted.
- [ ] Genuine gaps resolved or visibly isolated.
- [ ] Every production pair and check accepted.
- [ ] Cross-unit totals and dependencies agree.
- [ ] Candidate draft and manifest accepted.
- [ ] Final gate is `READY`.
- [ ] Postpublication gate is `PASS`.

## Related reading

- [11. Model Configuration](11_Model_Configuration.md)
- [12. End-to-End Tutorial](12_End_to_End_Tutorial.md)
- [13. File Formats](13_File_Formats.md)
- [15. Agent-to-Documentation Mapping](15_Code_Documentation_Mapping.md)
