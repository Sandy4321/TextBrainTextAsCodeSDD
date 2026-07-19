# 15. Agent-to-Documentation Mapping

## Purpose

This repository contains no executable implementation for the SSD workflow. Its implementation is the set of VS Code GitHub Copilot custom-agent Markdown files plus the linked reusable execution contract. This chapter maps those contracts to their explanatory documentation.

The filename retains “Code Documentation Mapping” for tutorial sequence compatibility, but the mapped implementation is prompt/configuration Markdown rather than an executable programming-language implementation.

Mappings use agent filename plus stable heading or contract name. They intentionally avoid fragile line-number claims.

## Top-level ownership

| Agent or linked contract | Primary responsibility | Main documentation |
|---|---|---|
| [text-spec-orchestrator.agent.md](../.github/agents/text-spec-orchestrator.agent.md) | User entry, orchestration, writes, gates, assembly, and publication | [End-to-End Tutorial](12_End_to_End_Tutorial.md), [Best Practices](14_Best_Practices.md) |
| [text-spec-investigator.agent.md](../.github/agents/text-spec-investigator.agent.md) | Evidence baseline, gap resolution, and production payloads | [End-to-End Tutorial](12_End_to_End_Tutorial.md), [File Formats](13_File_Formats.md) |
| [text-spec-interrogator.agent.md](../.github/agents/text-spec-interrogator.agent.md) | Genuine-gap identification and `IRQ-*` shape | [End-to-End Tutorial](12_End_to_End_Tutorial.md), [Best Practices](14_Best_Practices.md) |
| [text-spec-critic.agent.md](../.github/agents/text-spec-critic.agent.md) | Admission, completeness, consistency, and readiness | [Best Practices](14_Best_Practices.md), [File Formats](13_File_Formats.md) |
| [text-spec-execution-firewall.md](../.github/agent-contracts/text-spec-execution-firewall.md) | Fail-closed invariant state and exact action authorization, linked by the Orchestrator | [Workflow Engine](03_Workflow_Engine.md), [Observability Reference](OBSERVABILITY.md) |

## Orchestrator mapping

| Stable heading or contract | What it implements | Documentation |
|---|---|---|
| [Invocation semantics](../.github/agents/text-spec-orchestrator.agent.md#invocation-semantics) | Meaning of NEW/RESUME controls, automatic NEW when research is absent or state mismatches, immediate text-source discovery, and prohibition on code execution | [Tutorial: clean NEW run](12_End_to_End_Tutorial.md#step-1-start-a-clean-new-run), [Best practice 1](14_Best_Practices.md#1-enter-through-the-orchestrator) |
| [Non-skippable phase gates](../.github/agents/text-spec-orchestrator.agent.md#non-skippable-phase-gates) | Gate ledger, transitions, and dependency blocking | [Tutorial: gate sequence](12_End_to_End_Tutorial.md#gate-sequence-at-a-glance), [Best practice 19](14_Best_Practices.md#19-enforce-phase-gates) |
| [Domain neutrality](../.github/agents/text-spec-orchestrator.agent.md#domain-neutrality) | Source-derived vocabulary and cross-domain behavior | [Tutorial fixture](12_End_to_End_Tutorial.md#current-workspace-fixture), [Best practice 22](14_Best_Practices.md#22-test-domain-neutrality) |
| [Three-layer Artifact Contract](../.github/agents/text-spec-orchestrator.agent.md#three-layer-artifact-contract) | Specification, downstream, and operational actor separation | [Tutorial: baseline](12_End_to_End_Tutorial.md#step-5-build-the-extraction-only-baseline), [Best practice 4](14_Best_Practices.md#4-keep-the-three-actor-layers-separate) |
| [Four namespaces](../.github/agents/text-spec-orchestrator.agent.md#four-namespaces) | `OBL-*`, `IRQ-*`, `UNIT-*`, and `KQ-*` ownership | [File Formats: Identifier namespaces](13_File_Formats.md#identifier-namespaces) |
| [Published knowledge-question roles](../.github/agents/text-spec-orchestrator.agent.md#published-knowledge-question-roles) | Target-facing versus specification-reader questions | [Q&A Specification Manual](QA-SPEC-MANUAL.md#the-two-kq-roles), [Best practice 9](14_Best_Practices.md#9-select-the-correct-published-question-role) |
| [Source discovery and evidence](../.github/agents/text-spec-orchestrator.agent.md#source-discovery-and-evidence) | Non-reserved textual candidate inventory, one-goal semantic scope, bounded-EOF source identity, snapshot equations, evidence membership, obligation grounding, location rules, and synthesis boundary | [Tutorial: source snapshot](12_End_to_End_Tutorial.md#step-2-take-an-exact-source-snapshot), [Best practices 3–6](14_Best_Practices.md#3-infer-scope-from-semantics-not-filenames) |
| [Field Instantiation Contract](../.github/agents/text-spec-orchestrator.agent.md#field-instantiation-contract) | Adaptive source-supported topics, fields, cardinality, enumerations, placeholders, and relationship-specific completeness | [Tutorial: extraction-only baseline](12_End_to_End_Tutorial.md#step-5-build-the-extraction-only-baseline), [File Formats](13_File_Formats.md#markdown-field-instantiation-contract), [Best practices 10–12](14_Best_Practices.md#10-define-completeness-before-generating-content) |
| [Defaults, user answers, and source conflicts](../.github/agents/text-spec-orchestrator.agent.md#defaults-user-answers-and-source-conflicts) | Conservative defaults and explicit override records | [Best practice 18](14_Best_Practices.md#18-handle-defaults-and-overrides-conservatively) |
| [Allowed writes and fresh runs](../.github/agents/text-spec-orchestrator.agent.md#allowed-writes-and-fresh-runs) | Reserved generated ownership, verified NEW cleanup, uninterrupted research-first initialization, exact snapshot creation, local partial-state repair, and rollback base | [File Formats](13_File_Formats.md#persistent-format-policy), [Best practices 3 and 20](14_Best_Practices.md#3-infer-scope-from-semantics-not-filenames) |
| [Workflow](../.github/agents/text-spec-orchestrator.agent.md#workflow) | Baseline, resolution, planning, production, assembly, and publication order | [End-to-End Tutorial](12_End_to_End_Tutorial.md) |
| [Final chat response](../.github/agents/text-spec-orchestrator.agent.md#final-chat-response) | Minimal success handoff after postpublication pass | [Tutorial: transactional publication](12_End_to_End_Tutorial.md#step-14-publish-transactionally) |
| [Fail-Closed Execution Firewall link](../.github/agents/text-spec-orchestrator.agent.md#fail-closed-execution-firewall) | Requires the linked contract before every guarded action and blocks when it cannot be read | [Workflow Engine: Run invariants](03_Workflow_Engine.md#run-invariants-and-action-guards) |
| [Text Spec Fail-Closed Execution Contract](../.github/agent-contracts/text-spec-execution-firewall.md#fail-closed-execution-firewall) | Authoritative Run Invariant Block, research-first bootstrap, managed-path and snapshot equations, local initialization repair, durable action guards, exact write authorization, and success authorization | [Workflow Engine: Run invariants](03_Workflow_Engine.md#run-invariants-and-action-guards), [Run Health](09_Run_Health.md), [Debugging: contradictory READY](10_Debugging.md#copilot-reports-ready-with-zero-kqs-or-incomplete-coverage), [Observability Reference](OBSERVABILITY.md#run-invariant-block-and-action-authorization) |
| [VS Code permission modes and optional host boundary](../.github/agent-contracts/text-spec-execution-firewall.md#vs-code-permission-modes-and-optional-host-continuation-boundary) | Separates Default/Bypass tool permissions, internal Critic verdicts, and the optional Advanced Autopilot three-loop outer limit | [Workflow Engine: outer and inner loops](03_Workflow_Engine.md#outer-autopilot-loop-and-inner-critic-loop), [Critic and Convergence](06_Critic_and_Convergence.md#two-different-limits-that-both-use-the-number-three), [Debugging](10_Debugging.md#advanced-autopilot-stops-after-three-automatic-loops) |
| [Guarded run initialization, source identity, and scope](../.github/agent-contracts/text-spec-execution-firewall.md#guarded-run-initialization-source-identity-and-scope) | Uninterrupted research-first NEW transaction, RESUME freshness, exact managed snapshot path, path/block/count/row/EOF/text equations, one-goal selection, evidence membership, single-run checks, and automatic repair of uncommitted state | [Shared Memory](04_Shared_Memory.md#four-lifecycle-roles-plus-one-rollback-checkpoint), [Observability](07_Observability.md#source-snapshot-scope-and-inventory), [Best practice 3](14_Best_Practices.md#3-infer-scope-from-semantics-not-filenames) |

## Investigator mapping

| Stable heading or contract | What it implements | Documentation |
|---|---|---|
| [Evidence boundaries](../.github/agents/text-spec-investigator.agent.md#evidence-boundaries) | Exact initialization precheck, verified source use, exact operators, and non-execution boundary | [Best practices 3–6](14_Best_Practices.md#3-infer-scope-from-semantics-not-filenames) |
| [BASELINE mode](../.github/agents/text-spec-investigator.agent.md#baseline-mode) | `INITIALIZATION_NOT_READY` refusal unless exact research/snapshot and persisted baseline guard exist, followed by extraction-only Artifact Contract, obligations, source-explicit shape facts, constraints, and candidate gaps | [Tutorial: extraction-only baseline](12_End_to_End_Tutorial.md#step-5-build-the-extraction-only-baseline), [File Formats: Research Markdown](13_File_Formats.md#research-markdown) |
| [RESOLVE_BATCH mode](../.github/agents/text-spec-investigator.agent.md#resolve_batch-mode) | Evidence-grounded resolution of a supplied internal query | [Tutorial: genuine decisions](12_End_to_End_Tutorial.md#step-8-resolve-only-genuine-decisions), [Best practice 7](14_Best_Practices.md#7-prefer-grounded-synthesis-over-unnecessary-questions) |
| [PRODUCE_BATCH mode](../.github/agents/text-spec-investigator.agent.md#produce_batch-mode) | ID-preserving KQ/UNIT generation and completion ledgers | [Tutorial: Q&A production](12_End_to_End_Tutorial.md#step-10-produce-the-question---answer-knowledge-base), [Best practices 10–14](14_Best_Practices.md#10-define-completeness-before-generating-content) |
| [`DOWNSTREAM_TARGET_QUESTION`](../.github/agents/text-spec-investigator.agent.md#downstream_target_question) | Verbatim respondent-facing question and answer semantics | [Q&A Specification Manual](QA-SPEC-MANUAL.md#downstream_target_question), [Best practice 9](14_Best_Practices.md#9-select-the-correct-published-question-role) |
| [`SPEC_KNOWLEDGE_QUERY`](../.github/agents/text-spec-investigator.agent.md#spec_knowledge_query) | Specification-reader Q&A for non-question-native work and question-native schema/reference knowledge | [Tutorial: Q&A production](12_End_to_End_Tutorial.md#step-10-produce-the-question---answer-knowledge-base), [Best practice 9](14_Best_Practices.md#9-select-the-correct-published-question-role) |
| [Quality rules](../.github/agents/text-spec-investigator.agent.md#quality-rules) | Placeholder rejection, list expansion, batch reduction, and cross-unit effects | [Best practices 11–16](14_Best_Practices.md#11-instantiate-every-required-field) |
| [Response discipline](../.github/agents/text-spec-investigator.agent.md#response-discipline) | Mode-specific output and no file editing | [File inventory](13_File_Formats.md#file-inventory) |

## Interrogator mapping

| Stable heading or contract | What it implements | Documentation |
|---|---|---|
| [When an IRQ is allowed](../.github/agents/text-spec-interrogator.agent.md#when-an-irq-is-allowed) | Material-gap threshold and rejection of unnecessary clarification | [Tutorial: genuine decisions](12_End_to_End_Tutorial.md#step-8-resolve-only-genuine-decisions), [Best practice 7](14_Best_Practices.md#7-prefer-grounded-synthesis-over-unnecessary-questions) |
| [Query requirements](../.github/agents/text-spec-interrogator.agent.md#query-requirements) | Evidence, route, duplicate, and resolution-condition fields | [File Formats: Identifier namespaces](13_File_Formats.md#identifier-namespaces) |
| [Response format](../.github/agents/text-spec-interrogator.agent.md#response-format) | Research-only `IRQ-*` proposal shape | [File Formats: Research Markdown](13_File_Formats.md#research-markdown) |

## Critic mapping

| Stable heading or contract | What it implements | Documentation |
|---|---|---|
| [Baseline review](../.github/agents/text-spec-critic.agent.md#baseline-review) | Deterministic `INITIALIZATION_NOT_READY` refusal for missing/malformed bootstrap state, then admission of extraction-only source inventory, Artifact Contract, obligations, explicit shape facts, constraints, and candidate gaps | [Tutorial: baseline Critic loop](12_End_to_End_Tutorial.md#step-7-run-the-baseline-critic-loop) |
| [Internal resolution review](../.github/agents/text-spec-critic.agent.md#internal-resolution-review) | Acceptance or revision of `IRQ-*` resolutions | [Tutorial: genuine decisions](12_End_to_End_Tutorial.md#step-8-resolve-only-genuine-decisions) |
| [Production review](../.github/agents/text-spec-critic.agent.md#production-review) | Pair decisions and field, placeholder, enumeration, and scale gates | [Tutorial: production Critic loop](12_End_to_End_Tutorial.md#step-11-run-the-production-critic-loop), [Best practice 15](14_Best_Practices.md#15-make-the-critic-decide-not-advise) |
| [Source and default integrity](../.github/agents/text-spec-critic.agent.md#source-and-default-integrity) | Conflict, override, quantity, variant, and readiness safeguards | [Best practices 6 and 18](14_Best_Practices.md#6-preserve-exact-logical-force) |
| [Final review](../.github/agents/text-spec-critic.agent.md#final-review) | Complete draft, manifest, coverage, consistency, and phase checks | [Tutorial: FINAL review](12_End_to_End_Tutorial.md#step-13-perform-final-review), [Best practice 19](14_Best_Practices.md#19-enforce-phase-gates) |
| [Postpublication review](../.github/agents/text-spec-critic.agent.md#postpublication-review) | Equality, repeat checks, and transactional publication result | [Best practice 20](14_Best_Practices.md#20-publish-transactionally) |
| [Fail-Closed Review Firewall](../.github/agents/text-spec-critic.agent.md#fail-closed-review-firewall) | Valid review modes, missing-evidence rejection, final audit counts, exact `READY` equivalence, and postpublication equivalence | [Workflow Engine: Run invariants](03_Workflow_Engine.md#run-invariants-and-action-guards), [Run Health](09_Run_Health.md), [Debugging: contradictory READY](10_Debugging.md#copilot-reports-ready-with-zero-kqs-or-incomplete-coverage), [Observability Reference](OBSERVABILITY.md#final-review-dashboard) |

## Runtime artifact mapping

| Runtime artifact | Governing contract | Documentation |
|---|---|---|
| Readable non-reserved textual candidates | Orchestrator “Source discovery and evidence” plus guarded semantic scope | [Tutorial workspace fixture](12_End_to_End_Tutorial.md#current-workspace-fixture), [Source Markdown](13_File_Formats.md#source-markdown) |
| `.spec-workflow/research.md` | First authoritative NEW artifact: Run Invariant Block, phase ledger, action authorizations, source evidence, contracts, decisions, and manifest | [Research Markdown](13_File_Formats.md#research-markdown), [Observability Reference](OBSERVABILITY.md#run-invariant-block-and-action-authorization) |
| `.spec-workflow/source-snapshot.md` | Second NEW artifact: exact-path source identity, bounded-EOF rows, structural equations, and RESUME freshness checks; a copy without matching research is uncommitted | [Source-snapshot Markdown](13_File_Formats.md#source-snapshot-markdown), [Shared Memory](04_Shared_Memory.md#spec-workflowsource-snapshotmd), [Observability](07_Observability.md#source-snapshot-scope-and-inventory) |
| `.spec-workflow/draft.md` | Orchestrator “Assemble the Q&A specification” | [Draft Markdown](13_File_Formats.md#draft-markdown) |
| `.spec-workflow/prior-spec.md` | Orchestrator initialization checkpoint and transactional rollback contract | [Persistent format policy](13_File_Formats.md#persistent-format-policy), [Best practice 20](14_Best_Practices.md#20-publish-transactionally) |
| `spec.md` | Orchestrator final/postpublication contracts and Critic repeat checks; the file may be absent before successful publication | [Published spec.md](13_File_Formats.md#published-specmd) |
| `outgoing/**` | Reserved legacy generated namespace; NEW deletes and verifies it but normal specification production does not recreate it | [Best practice 3](14_Best_Practices.md#3-infer-scope-from-semantics-not-filenames), [Observability lifecycle](07_Observability.md#the-artifact-lifecycle) |

## Frontmatter mapping

| Frontmatter field | Current use | Documentation |
|---|---|---|
| `name` and `description` | VS Code agent identity and picker description | [Model Configuration](11_Model_Configuration.md#current-configuration-at-a-glance) |
| `argument-hint` | Explains bare control utterances for the Orchestrator | [Tutorial step 1](12_End_to_End_Tutorial.md#step-1-start-a-clean-new-run) |
| `target: vscode` | Restricts these definitions to the VS Code custom-agent format | [Model Configuration](11_Model_Configuration.md) |
| `tools` | Grants only the operations each role requires | [Model Configuration](11_Model_Configuration.md#current-configuration-at-a-glance) |
| `agents` | Restricts Orchestrator delegation to the three workers | [Model Configuration](11_Model_Configuration.md#current-configuration-at-a-glance) |
| `user-invocable` | Makes only the Orchestrator a normal user entry point | [Model Configuration](11_Model_Configuration.md#current-configuration-at-a-glance) |
| `disable-model-invocation` | Prevents accidental use of the Orchestrator as another model's subagent | [Model Configuration](11_Model_Configuration.md#current-configuration-at-a-glance) |

## Maintenance workflow

When an agent contract changes:

1. Identify the stable heading that owns the behavior.
2. Update that agent file.
3. Update every documentation row that links to the heading.
4. Update the WHAT, HOW, and WHY explanation in the owning chapter.
5. Update failure modes and checklists if acceptance behavior changed.
6. Test both a question-native and a non-question-native fixture, each through verified research-first NEW cleanup, exact managed paths, complete snapshot equations, and local orphan-state recovery.
7. Confirm that RESUME changes to NEW when any candidate path or logical line changes.
8. Avoid documenting transient line numbers, generated IDs, or model output wording as stable implementation facts.

## Mapping failure modes

| Failure | Why it is risky | Correction |
|---|---|---|
| Documentation cites line numbers | Prompt edits immediately stale the claim | Link to a stable filename and heading. |
| Same rule is described differently in several chapters | Users cannot tell which behavior is authoritative | Treat agent prompt as implementation and link explanations back to its owning heading. |
| A current task's vocabulary enters the generic mapping | Future tasks inherit accidental domain rules | Describe contracts generically and label examples non-normative. |
| Runtime output is treated as the agent contract | A bad run appears to redefine intended behavior | Map to `.agent.md` headings; use outputs only as fixtures. |
| Generated cleanup or snapshot behavior has no mapping | Stale-domain reuse can look like valid continuation | Map initialization to the execution contract and to shared-memory and observability chapters. |
| Research-first ordering, exact managed paths, or repair behavior is omitted | A partial bootstrap can be mistaken for a valid run | Map the initialization transaction, snapshot equations, and `REPAIR_RUN_INITIALIZATION` path explicitly. |
| Frontmatter is documented as provider-neutral pseudocode | Users configure the wrong platform | State that these are VS Code GitHub Copilot custom agents. |

## Documentation coverage checklist

- [ ] Every agent file has a top-level ownership row.
- [ ] Every linked agent contract has an ownership row and is excluded from task evidence.
- [ ] Every stable behavioral heading has an owning documentation section.
- [ ] Links are relative and do not claim fixed line numbers.
- [ ] Runtime artifacts are distinguished from agent configuration.
- [ ] NEW cleanup, research-first ordering, exact managed paths, snapshot equations, local repair, scope, evidence membership, and RESUME freshness map to the execution contract.
- [ ] Internal JSON transport is not described as persistent state.
- [ ] Both question roles are documented.
- [ ] Completeness and phase-gate rules map to the Critic.
- [ ] Publication and rollback map to Orchestrator plus Critic.
- [ ] Examples are explicitly non-normative.

## Related reading

- [11. Model Configuration](11_Model_Configuration.md)
- [12. End-to-End Tutorial](12_End_to_End_Tutorial.md)
- [13. File Formats](13_File_Formats.md)
- [14. Best Practices](14_Best_Practices.md)
- [16. Agent Loops Explained Like You Are 10](16_Agent_Loops_Explained_for_Age_10.md)
