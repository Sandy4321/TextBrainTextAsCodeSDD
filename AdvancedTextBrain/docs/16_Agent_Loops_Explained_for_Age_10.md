# Agent Loops Explained Like You Are 10

## What this chapter teaches

Imagine that three mission cards are lying on a desk:

- one asks for a specification for a cartoon film implemented in Python;
- one describes a school detective game;
- one describes a robot toy for a clinic.

A careless robot might stir all three cards together and invent a Python cartoon about a detective robot in a clinic. That would sound creative, but it would not follow the requested task.

This project uses a small agent team to avoid that mistake:

- the **Orchestrator** is the captain;
- the **Investigator** reads the evidence;
- the **Interrogator** asks only necessary internal questions;
- the **Critic** checks each important result.

The repeated pattern—**prepare, check, repair, and check again**—is an agent loop.

This chapter uses the source and implementation files present in this workspace on 2026-07-19. Replaceable `.spec-workflow/**` output is not treated as stable implementation evidence.

## The “code” for these loops is Markdown

The current Jira ticket mentions Python, but this agent framework is not itself a Python program. There is no `while` statement controlling the agents.

The loop is written as textual instructions in four `.agent.md` files and one linked execution-contract `.md` file. For example, the Orchestrator says that `REVISE` keeps the current phase open, triggers a targeted correction, and sends the result back to the Critic.

Python still matters to the requested specification. The Orchestrator must treat a technology named by an active source as domain evidence; it must not search for an existing Python program before it can write a specification. See [Invocation semantics](../.github/agents/text-spec-orchestrator.agent.md#invocation-semantics), especially lines 23–45.

## The three current mission cards

| File | What it actually asks for | Exact source location |
|---|---|---|
| [jira.md](../jira.md) | A complete specification for a cartoon film implemented in Python | lines 1–9 |
| [description1.txt](../input-spec/description1.txt) | A specification for a school detective game about a missing cardboard compass | lines 3 and 23 |
| [description2.txt](../input-spec/description2.txt) | A specification for a tabletop robot toy in a clinic waiting room | lines 3 and 31 |

The three files contain three different requested deliverables, so their instructions cannot simply be glued together. But the user explicitly called the two `.txt` files current input material. That means the agent must inspect them section by section: the Jira ticket selects the deliverable, useful story or design material may support that deliverable, and old game/toy output instructions remain excluded.

| Candidate | Current classification | Why |
|---|---|---|
| `jira.md` | `ACTIVE_GOAL_SOURCE` | It declares the current task and output. |
| Relevant propositions in `input-spec/description1.txt` | `IN_SCOPE_SOURCE_MATERIAL` | The missing-compass mystery, child-appropriate tone, and original-name rule can inform the cartoon. |
| Game logistics/directive in `input-spec/description1.txt` | Excluded embedded scope/directive | It requests a live detective game, not the selected film deliverable. |
| Relevant propositions in `input-spec/description2.txt` | `IN_SCOPE_SOURCE_MATERIAL` | The friendly imaginary robot-pet concept and non-startling expressions can inform the cartoon. |
| Hardware/directive in `input-spec/description2.txt` | Excluded embedded scope/directive | Batteries, cleaning, networking, tabletop controls, and the toy-spec instruction do not automatically constrain a film. |

The agent records every admitted and excluded section with a reason. Only admitted lines may create obligations or bound synthesis. The framework therefore avoids both mistakes: throwing away a useful designated input and turning every physical-game or hardware rule into a cartoon requirement.

This selection behavior is defined in [Invocation semantics](../.github/agents/text-spec-orchestrator.agent.md#invocation-semantics), lines 25–41, and [Source discovery and evidence](../.github/agents/text-spec-orchestrator.agent.md#source-discovery-and-evidence), lines 149–180.

## Meet the four-agent team

### The Orchestrator is the captain

The Orchestrator chooses the active goal, manages run state, calls the workers, and edits the allowed Markdown artifacts. Its frontmatter gives it `read`, `search`, `edit`, and `agent` tools and limits delegation to the three named workers. See [text-spec-orchestrator.agent.md](../.github/agents/text-spec-orchestrator.agent.md), lines 1–12.

### The Investigator is the evidence detective

The Investigator has read/search tools but no edit tool. It must examine only current-run snapshot members that the Orchestrator classified as in scope. It must flag a passed file that actually describes another goal. See [Evidence boundaries](../.github/agents/text-spec-investigator.agent.md#evidence-boundaries), lines 12–24.

It works in three modes:

- `BASELINE` builds an evidence map;
- `RESOLVE_BATCH` proposes answers to accepted internal questions;
- `PRODUCE_BATCH` proposes complete Question → Answer knowledge entries.

The same agent receives a different, bounded assignment in each mode. See [BASELINE mode](../.github/agents/text-spec-investigator.agent.md#baseline-mode), [RESOLVE_BATCH mode](../.github/agents/text-spec-investigator.agent.md#resolve_batch-mode), and [PRODUCE_BATCH mode](../.github/agents/text-spec-investigator.agent.md#produce_batch-mode), lines 26–129.

### The Interrogator is the careful question keeper

The Interrogator creates private `IRQ-*` records only when an important decision cannot be obtained from evidence, derivation, grounded synthesis, or a safe default. It does not write questions for the published knowledge base. See [When an IRQ is allowed](../.github/agents/text-spec-interrogator.agent.md#when-an-irq-is-allowed), lines 16–41.

### The Critic is the independent checker

The Critic reads and checks but does not edit. For BASELINE, it first requires a clean initialization, exact source-snapshot equality, one selected goal, valid evidence membership, and a single-run state. It rejects merged projects and facts backed by excluded paths. See [Baseline review](../.github/agents/text-spec-critic.agent.md#baseline-review), lines 14–20.

## Loop 0: NEW means clean the desk

Before the evidence loop begins, the captain must know whether this is NEW work or a real RESUME.

### Start words always mean NEW

These controls always start NEW:

- `run`
- `start`
- `go`
- `generate`
- `build`

A longer user instruction that selects or changes the goal also means NEW. The presence of an old ledger does not turn `run` into `resume`.

The rule is in [Invocation semantics](../.github/agents/text-spec-orchestrator.agent.md#invocation-semantics), line 25, and [Guarded run initialization, source identity, and scope](../.github/agent-contracts/text-spec-execution-firewall.md#guarded-run-initialization-source-identity-and-scope), lines 129–139.

### What the NEW reset removes

Think of `.spec-workflow/**` and the legacy `outgoing/**` directory as the captain’s erasable whiteboard. For NEW, the captain:

1. preserves human inputs, framework files, unknown files, and the current published artifact when one exists;
2. deletes every generated file under the two managed namespaces;
3. verifies that those stale files are gone;
4. inventories every non-reserved textual candidate, selects one active goal and anchor, and classifies candidate files or sections against it;
5. first creates and rereads `.spec-workflow/research.md` with that goal, one new run ID, and pending control records;
6. creates `.spec-workflow/source-snapshot.md` only at that exact path;
7. reads every source through EOF and checks the global candidate/path/block equation plus every numbered snapshot row; and
8. updates and rereads the research checks and baseline-call permission.

`MANAGED_ARTIFACT_PATH_CHECK` enforces step 6: a root-level, basename-only, or duplicate identity copy claiming the current RUN_ID fails the check.

The captain must replace state, never append a new run beneath an old run. If two run IDs, two current baselines, old domain paragraphs, or stale sidecar references survive, `SINGLE_RUN_STATE_CHECK` cannot pass.

The managed-file rule is in [Phase ownership firewall](../.github/agent-contracts/text-spec-execution-firewall.md#phase-ownership-firewall), lines 39–55. The complete NEW transaction is in [Guarded run initialization, source identity, and scope](../.github/agent-contracts/text-spec-execution-firewall.md#guarded-run-initialization-source-identity-and-scope), lines 129–139. The Orchestrator’s write boundary is in [Allowed writes and fresh runs](../.github/agents/text-spec-orchestrator.agent.md#allowed-writes-and-fresh-runs), lines 206–217.

### Why the reset matters for the current task

Suppose old research contains rules for some earlier project. If the captain merely adds “Python cartoon” at the top, later agents may still see the old actors, quantities, or constraints. A clean NEW reset makes those old statements disappear before the Investigator works.

The reset is not an instruction to delete every irrelevant-looking file. `jira.md`, both `.txt` files, agent definitions, and documentation remain on disk. The scope loop classifies them; it does not destroy them.

## The exact source snapshot is the team photograph

After cleaning the desk, the captain records what the source candidates look like now.

The file `.spec-workflow/source-snapshot.md` contains:

- the current `RUN_ID`;
- candidate paths in sorted order;
- exactly one `### SOURCE path` block per candidate;
- each file’s logical line count;
- each source line in a form such as `L000001|exact text`.

This project does not ask the language model to invent a SHA-256 value. A made-up hash would look official without proving anything. The exact line-addressed copy is the source identity supported by the current Markdown-only tools.

The captain reads in small enough chunks to reach the editor's real end of file. First, `SOURCE_CANDIDATE_COUNT`, the sorted candidate-path count, and the `### SOURCE path` block count must all be equal. Then, for each block, `LINE_COUNT`, the number of contiguous rows, the highest row number, and the live source line count must all be equal, and every row text must match the live file through EOF. If a 33-line file stops at `L000018`, the photograph is torn and the baseline door stays locked.

If the photograph exists but `.spec-workflow/research.md` does not, there is no captain's scoreboard and therefore no active run. The captain throws away that managed partial state and starts NEW automatically. It never tells the child to update a notebook that was never created or asks whether it should proceed.

The snapshot includes all textual candidates needed for change detection, even when a candidate is later classified as unrelated. An excluded candidate remains visible to the freshness check but contributes no evidence.

This format and its equality rule are defined in [Guarded run initialization, source identity, and scope](../.github/agent-contracts/text-spec-execution-firewall.md#guarded-run-initialization-source-identity-and-scope), line 135.

## RESUME means “prove nothing changed”

`resume`, `continue`, or `proceed` continues an existing run only when all of these still agree:

- one current run ID;
- one active goal;
- the exact candidate path set;
- every snapshotted logical line;
- all scope classifications;
- all admitted evidence memberships;
- the physical draft, checkpoint, and publication state.

If `jira.md` changes from another domain to “Python cartoon,” the old run is not resumed. If a source is added, removed, renamed, or edited, RESUME becomes NEW and records the mismatch reason. Old obligations and line references are not patched into the new task.

The important idea is:

> same files and same state → RESUME may continue  
> changed goal, source, or state → initialize NEW

The Orchestrator states this at [Invocation semantics](../.github/agents/text-spec-orchestrator.agent.md#invocation-semantics), line 25. The exact comparison and fallback are at [Guarded run initialization, source identity, and scope](../.github/agent-contracts/text-spec-execution-firewall.md#guarded-run-initialization-source-identity-and-scope), lines 131–139.

## Loop 1: select one goal and check the scope

This loop occurs inside NEW initialization before `.spec-workflow/research.md` is first written and before the snapshot is created. It is explained separately here so the semantic decision is easier to learn; it is not a later phase after the photograph.

The Orchestrator inventories every readable non-reserved textual candidate before deciding relevance. Fixed framework/generated trees are excluded by pattern; a `.txt` file outside those reserved trees does not become invisible merely because examples often use `.md`.

The semantic authority order is:

1. an explicit current user instruction;
2. a source that declares the current task or ticket goal and requested output;
3. the only remaining coherent goal-bearing source.

Filenames and modification times do not decide authority by themselves.

For the current files, the scope loop is:

> inventory `jira.md` and both descriptions  
> → identify the ticket’s Python-cartoon goal  
> → admit useful story/design propositions from the designated inputs  
> → exclude their incompatible live-game and physical-toy directives  
> → check that every included line supports the selected goal  
> → record `SOURCE_SCOPE_CHECK: PASS` only when the table is coherent

If two equally authoritative sources still request different deliverables, the result is `SOURCE_SCOPE_CONFLICT`. The captain asks one precise scope question; it does not blend the projects.

`EVIDENCE_MEMBERSHIP_CHECK` provides another lock. Every obligation, fact, location, and synthesis boundary must point to admitted snapshot lines. An excluded line cannot corroborate a claim merely because it sounds useful.

The rules are implemented in [Source discovery and evidence](../.github/agents/text-spec-orchestrator.agent.md#source-discovery-and-evidence), lines 151–180, and [Guarded run initialization, source identity, and scope](../.github/agent-contracts/text-spec-execution-firewall.md#guarded-run-initialization-source-identity-and-scope), lines 137–139.

## Loop 2: Investigator builds the baseline, Critic checks it

Only after `GUARD_CALL_BASELINE: PASS` may the Investigator receive the selected goal, snapshot membership, included source sections, and exclusions.

If the Investigator is accidentally called early, it must return `WORKER_RESULT_REJECTED: INITIALIZATION_NOT_READY` and `ALLOWED_NEXT_ACTION: REPAIR_RUN_INITIALIZATION`. The Critic makes the same refusal before semantic `BASELINE` review when research, snapshot, or the persisted guard is absent or malformed. Neither worker repairs files; the Orchestrator repairs initialization locally and reevaluates the guard without asking the user to rerun. See [Investigator initialization precheck](../.github/agents/text-spec-investigator.agent.md), line 12, and [Critic baseline review](../.github/agents/text-spec-critic.agent.md#baseline-review), line 16.

### What belongs in the baseline

The baseline contains:

- the selected goal and requested result;
- the candidate-source scope table;
- the three actor layers;
- exact source terminology;
- atomic `OBL-*` obligations;
- source-explicit values, structure, and constraints;
- same-goal conflicts and genuine gaps;
- possible resolution routes that have not yet been selected.

For the current active ticket, clear source facts include:

- the requested artifact is a complete specification (`jira.md:5-9`);
- the subject is a cartoon film (`jira.md:1-5`);
- Python is the required implementation technology (`jira.md:1-5`);
- the specification must cover what to build, why, and how to code it (`jira.md:7-9`).

The baseline may use the admitted missing-compass mystery proposition, including its fictional cardboard compass, as source material for the cartoon. It must not import the live game's participant count, physical compass-construction rules, rooms, prizes, or timing, nor the clinic's cleaning rules, robot buttons, or prototype budget. Those operational facts belong to excluded embedded scopes.

The baseline also must not contain final Q&A, selected story details, invented Python libraries, produced scenes, accepted coverage, or a draft. Zero planned KQs and zero covered obligations are expected before production.

These boundaries are in [Phase ownership firewall](../.github/agent-contracts/text-spec-execution-firewall.md#phase-ownership-firewall), lines 39–53, [BASELINE mode](../.github/agents/text-spec-investigator.agent.md#baseline-mode), lines 26–49, and [Complete durable baseline requirement](../.github/agent-contracts/text-spec-execution-firewall.md#complete-durable-baseline-requirement), lines 141–155.

### The baseline Critic loop

The Critic can return:

- `ACCEPT`: the evidence map is complete and coherent;
- `REVISE`: one or more named defects must be repaired;
- `BLOCKED`: no legal in-scope correction can make the baseline safe to use.

The loop is:

> Investigator proposes baseline  
> → Critic checks initialization, snapshot, scope, membership, and evidence  
> → Orchestrator repairs only named defects  
> → Critic checks again  
> → leave with `ACCEPT`, or stop honestly with `BLOCKED`

Examples of `REVISE` defects for this workspace would be:

- treating all three files as one project;
- citing an excluded description as a cartoon requirement;
- leaving an old run ID in the new research file;
- inventing a Python library as if Jira named it;
- writing final questions and answers during BASELINE.

The Critic contract is in [Baseline review](../.github/agents/text-spec-critic.agent.md#baseline-review), lines 14–20. The Orchestrator’s baseline call is in [Workflow: Baseline](../.github/agents/text-spec-orchestrator.agent.md#1-baseline), lines 221–236.

## Loop 3: ask only genuine internal questions

After baseline acceptance, the Interrogator may identify decisions that genuinely affect a useful specification.

For the current ticket, these are bad internal questions:

- “Should this use Python?”—the source already says Python.
- “Should this be a cartoon film?”—the source already says cartoon film.
- “Should the cartoon obey the clinic’s prototype budget?”—that budget belongs to an excluded project.

Possible genuine gaps include the exact cartoon content, visual approach, duration, target audience, animation method, and Python technology choices. However, the Interrogator creates an `IRQ-*` only when the missing decision cannot be safely handled by evidence, grounded synthesis, or a conservative source-compatible default.

The resolution loop works in batches of at most five:

> Interrogator proposes genuine IRQs  
> → Investigator proposes resolutions  
> → Critic accepts, revises, or blocks each one  
> → only accepted resolutions enter durable research

See [Workflow: Resolve genuine gaps only](../.github/agents/text-spec-orchestrator.agent.md#2-resolve-genuine-gaps-only), lines 238–246, [When an IRQ is allowed](../.github/agents/text-spec-interrogator.agent.md#when-an-irq-is-allowed), lines 16–41, and [Internal resolution review](../.github/agents/text-spec-critic.agent.md#internal-resolution-review), lines 22–37.

## Loop 4: plan, produce, and check the Q&A knowledge base

After accepted baseline and resolution states, the Orchestrator maps every mandatory obligation to planned knowledge. A planned KQ contains a topic and mappings, not its final question and answer.

For this task, planned topics might need to cover the source-required “what,” “why,” and “how to code” dimensions. Exact subdivisions must come from accepted scope and resolution records; the framework must not copy the detective-game or robot-toy section lists.

The Orchestrator sends at most five planned entries in one production batch. It supplies:

- the active goal and admitted sources;
- actor layers;
- applicable source-required fields;
- obligation mappings;
- accepted resolutions;
- a compact index of already accepted topics.

It does not paste every old answer simply to make the next answer imitate them. Full prior entries are passed only for a real dependency or overlap.

Each proposed pair receives:

- `KQ_DECISION`;
- `UNIT_DECISION` when applicable;
- a field-instantiation check;
- a placeholder check;
- an enumeration-completeness check;
- an ordered-scale check when applicable.

If a batch is too large to complete without `...` or deferred instructions, the Investigator returns `BATCH_TOO_LARGE`, and the Orchestrator retries with fewer entries.

The production loop is:

> Investigator produces complete pairs  
> → Critic checks each pair  
> → accepted pairs enter memory  
> → revised pairs receive targeted repairs  
> → repeat batches until mandatory coverage is complete

See [Workflow: Plan knowledge coverage](../.github/agents/text-spec-orchestrator.agent.md#3-plan-knowledge-coverage), lines 248–250; [Workflow: Produce in independent batches](../.github/agents/text-spec-orchestrator.agent.md#4-produce-in-independent-batches), lines 252–277; [PRODUCE_BATCH mode](../.github/agents/text-spec-investigator.agent.md#produce_batch-mode), lines 68–129; and [Production review](../.github/agents/text-spec-critic.agent.md#production-review), lines 39–101.

## Six gates stop shortcuts

Think of the workflow as a hallway with six locked doors:

1. `BASELINE_GATE`
2. `RESOLUTION_GATE`
3. `PRODUCTION_GATE`
4. `DRAFT_GATE`
5. `FINAL_GATE`
6. `POSTPUBLICATION_GATE`

A friendly sentence such as “the draft looks ready” does not unlock a door. Exact accepted values are required.

The draft is assembled only from admitted production content. The final Critic checks the entire candidate, including source scope, snapshot equality, traceability, coverage, consistency, and the literal Question/Answer envelope. Only `FINAL_GATE: READY` permits publication. After writing, POSTPUBLICATION checks the physical file against the accepted draft again.

If publication verification fails, the previous published state is restored. If no previous publication existed, the failed new file is removed. A success chat response is allowed only after `POSTPUBLICATION_GATE: PASS`.

See [Non-skippable phase gates](../.github/agents/text-spec-orchestrator.agent.md#non-skippable-phase-gates), lines 47–79; [Workflow: Assemble the Q&A specification](../.github/agents/text-spec-orchestrator.agent.md#5-assemble-the-qa-specification), lines 279–313; [Final review](../.github/agents/text-spec-critic.agent.md#final-review), lines 116–166; and [Postpublication review](../.github/agents/text-spec-critic.agent.md#postpublication-review), lines 168–172.

## The inner Critic loop and the outer VS Code clock

These are two different control systems.

The inner workflow loop is:

> make a proposal → Critic checks → repair a named defect → Critic checks again

The optional outer host loop belongs to VS Code. Only when the permission level is Autopilot and `chat.autopilot.advanced.enabled` is `true` does VS Code 1.124 apply its maximum of three automatic continuations. That host number is not a Critic-call count.

The framework also has a different “three”: if the identical normalized Critic defect survives three completed targeted repairs with no changed evidence or state, it becomes `BLOCKED: NON_CONVERGING`.

Never add those numbers together:

- a host continuation does not increment `SAME_DEFECT_REVISION_COUNT`;
- a host stop does not mean the specification failed;
- a Critic `REVISE` is not a request for user permission;
- an explicitly reported host stop preserves the ledger so a valid `resume` can continue.

See [Full-run authorization and automatic convergence](../.github/agent-contracts/text-spec-execution-firewall.md#full-run-authorization-and-automatic-convergence), lines 27–31; [VS Code permission modes and optional host continuation boundary](../.github/agent-contracts/text-spec-execution-firewall.md#vs-code-permission-modes-and-optional-host-continuation-boundary), lines 33–37; and the durable counter fields and update rule at lines 78–79 and 121.

## A healthy current run, told as a short story

1. You select **Text Spec Orchestrator** and say `run`.
2. The captain classifies the invocation as NEW.
3. It preserves human and framework files, clears only managed generated namespaces, and verifies the reset.
4. It inventories candidates, identifies `jira.md` as the active Python-cartoon goal source, and section-classifies both `.txt` descriptions.
5. It creates and rereads `.spec-workflow/research.md` first with the goal, one run ID, pending controls, and the scope table.
6. It creates the exact-path snapshot second and verifies candidate/path/block plus every line/row/EOF/text equation.
7. It persists and rereads `GUARD_CALL_BASELINE: PASS`.
8. The Investigator proposes a baseline using only admitted Jira and designated-input lines.
9. The Critic checks initialization, source freshness, scope, membership, and baseline evidence.
10. Any `REVISE` result triggers a targeted correction and another same-mode review.
11. Genuine missing decisions are resolved through the Interrogator → Investigator → Critic loop.
12. The Orchestrator plans Q&A coverage for all mandatory cartoon-specification obligations.
13. The Investigator produces at most five complete entries per batch.
14. The Critic admits, revises, or blocks each entry.
15. Only accepted entries enter the candidate draft.
16. The whole candidate receives final review.
17. Only `READY` permits the publication write.
18. The written result must pass postpublication comparison before success is reported.

At no point does the framework execute Python or render the film. It creates the Question → Answer specification knowledge needed for a later implementation step.

## Signs that the loop is unhealthy

| Bad sign | Why it fails |
|---|---|
| Two `RUN_ID` values appear in current research | NEW was appended instead of replacing old state. |
| A removed source path remains in the current baseline | Source freshness or single-run checking failed. |
| Live-event logistics or physical-toy requirements appear as film obligations without an explicit adaptation reason | Incompatible embedded scope was merged into the selected goal. |
| An excluded file is cited as evidence | `EVIDENCE_MEMBERSHIP_CHECK` should fail. |
| A stored hash is trusted even though no trusted hash tool produced it | It does not prove source identity. |
| `resume` continues after Jira or an input file changed | Snapshot equality was not checked. |
| Baseline contains final questions, answers, or implementation selections | Later-phase content leaked into evidence extraction. |
| The agent offers “pick one” for a correctable `REVISE` | It voluntarily broke the Critic loop. |
| Success is reported before postpublication `PASS` | The physical publication was never verified. |

## Tiny glossary

| Word | Simple meaning |
|---|---|
| Agent | A model following one specialized Markdown role |
| NEW | Start a clean run with a new identity and fresh snapshot |
| RESUME | Continue only after proving source and state equality |
| Reset | Erase managed generated state without deleting human inputs |
| Candidate source | A readable text file considered during scope classification |
| Active goal | The one selected deliverable this run is specifying |
| Source scope | The table explaining which source lines belong to that goal |
| Source snapshot | The exact line-addressed picture of candidate text for this run |
| Evidence membership | Proof that a claim uses only admitted snapshot lines |
| Baseline | An extraction-only evidence map |
| Obligation | A requirement grounded in exact active-source wording |
| IRQ | A private question about a genuinely missing decision |
| KQ | A published specification-knowledge Question → Answer entry |
| Critic loop | Propose, check, repair a named defect, and check again |
| Gate | A rule that prevents dependent work from starting too early |
| Manifest | The research-only map from accepted records to draft blocks |
| Rollback | Restore the previous published state after a failed disk check |
| Host loop | VS Code’s outer automatic-continuation decision |
| Non-convergence | The same Critic defect surviving three completed repairs |

## Check your understanding

### Why can’t the agent treat every sentence in all three files as one requirement list?

Because their embedded directives request different deliverables. The Jira selects the Python cartoon film. Relevant story/design propositions in the designated inputs can be admitted, while live-game and physical-toy directives are excluded before obligation extraction.

### Why does the agent classify a designated `.txt` file section by section?

Designation says the file matters, but it does not make an incompatible embedded output instruction control the run. Section classification keeps useful evidence and rejects only the conflicting scope.

### Why isn’t Python ignored as “implementation”?

The source explicitly names Python, so it is domain evidence that the specification must cover. The framework refuses to execute the downstream implementation; it does not erase implementation requirements from the specification.

### What happens if `jira.md` changes before `resume`?

Exact snapshot equality fails. The old obligations and line references cannot continue. The framework initializes NEW with a source-change reason.

### Why not store a hash invented by the model?

The current agent has no trusted hash tool. The line-addressed source snapshot can be checked directly; a model-generated digest cannot.

### Does `REVISE` mean “ask the user whether to continue”?

No. It means repair the named defect and invoke the same Critic mode again. User input is reserved for a genuine reviewed human decision or an actual VS Code tool confirmation.

### Does `FINAL_GATE: READY` allow a success message immediately?

No. The candidate still must be written and the physical result must pass POSTPUBLICATION comparison.

## Actual implementation map

The headings are the stable links. The line numbers below were checked against the current files on 2026-07-19 and should be refreshed after prompt edits.

| Responsibility | Stable implementation heading | Current lines |
|---|---|---:|
| Start, NEW/RESUME classification, active-goal authority | [Invocation semantics](../.github/agents/text-spec-orchestrator.agent.md#invocation-semantics) | 23–45 |
| Six gates and dependencies | [Non-skippable phase gates](../.github/agents/text-spec-orchestrator.agent.md#non-skippable-phase-gates) | 47–79 |
| Candidate inventory, source admission, snapshot instruction | [Source discovery and evidence](../.github/agents/text-spec-orchestrator.agent.md#source-discovery-and-evidence) | 149–180 |
| Markdown write boundary and NEW cleanup | [Allowed writes and fresh runs](../.github/agents/text-spec-orchestrator.agent.md#allowed-writes-and-fresh-runs) | 206–217 |
| Investigator/Critic baseline loop | [Workflow: Baseline](../.github/agents/text-spec-orchestrator.agent.md#1-baseline) | 221–236 |
| IRQ resolution loop | [Workflow: Resolve genuine gaps only](../.github/agents/text-spec-orchestrator.agent.md#2-resolve-genuine-gaps-only) | 238–246 |
| Coverage planning | [Workflow: Plan knowledge coverage](../.github/agents/text-spec-orchestrator.agent.md#3-plan-knowledge-coverage) | 248–250 |
| Production batching and pair review | [Workflow: Produce in independent batches](../.github/agents/text-spec-orchestrator.agent.md#4-produce-in-independent-batches) | 252–277 |
| Draft assembly and manifest | [Workflow: Assemble the Q&A specification](../.github/agents/text-spec-orchestrator.agent.md#5-assemble-the-qa-specification) | 279–313 |
| Final review, publication, and rollback | [Workflow: Final review and publication](../.github/agents/text-spec-orchestrator.agent.md#6-final-review-and-publication) | 315–323 |
| Automatic Critic convergence | [Full-run authorization and automatic convergence](../.github/agent-contracts/text-spec-execution-firewall.md#full-run-authorization-and-automatic-convergence) | 27–31 |
| Optional outer host boundary | [VS Code permission modes and optional host continuation boundary](../.github/agent-contracts/text-spec-execution-firewall.md#vs-code-permission-modes-and-optional-host-continuation-boundary) | 33–37 |
| Baseline phase ownership and managed namespaces | [Phase ownership firewall](../.github/agent-contracts/text-spec-execution-firewall.md#phase-ownership-firewall) | 39–55 |
| Ordered current-run scoreboard | [Mandatory Run Invariant Block](../.github/agent-contracts/text-spec-execution-firewall.md#mandatory-run-invariant-block) | 57–127 |
| NEW reset, snapshot, scope, and resume freshness | [Guarded run initialization, source identity, and scope](../.github/agent-contracts/text-spec-execution-firewall.md#guarded-run-initialization-source-identity-and-scope) | 129–139 |
| Canonical extraction-only baseline | [Complete durable baseline requirement](../.github/agent-contracts/text-spec-execution-firewall.md#complete-durable-baseline-requirement) | 141–155 |
| All action permission guards | [Exact action guards](../.github/agent-contracts/text-spec-execution-firewall.md#exact-action-guards) | 157–357 |
| Guard failure behavior | [Fail-closed behavior](../.github/agent-contracts/text-spec-execution-firewall.md#fail-closed-behavior) | 359–370 |
| Investigator source boundary | [Evidence boundaries](../.github/agents/text-spec-investigator.agent.md#evidence-boundaries) | 12–24 |
| Investigator baseline output | [BASELINE mode](../.github/agents/text-spec-investigator.agent.md#baseline-mode) | 26–49 |
| Investigator production output | [PRODUCE_BATCH mode](../.github/agents/text-spec-investigator.agent.md#produce_batch-mode) | 68–129 |
| Genuine internal-question rules | [When an IRQ is allowed](../.github/agents/text-spec-interrogator.agent.md#when-an-irq-is-allowed) | 16–41 |
| Critic baseline admission | [Baseline review](../.github/agents/text-spec-critic.agent.md#baseline-review) | 14–20 |
| Critic production admission | [Production review](../.github/agents/text-spec-critic.agent.md#production-review) | 39–101 |
| Critic whole-candidate review | [Final review](../.github/agents/text-spec-critic.agent.md#final-review) | 116–166 |
| Critic written-file comparison | [Postpublication review](../.github/agents/text-spec-critic.agent.md#postpublication-review) | 168–172 |

## Related reading

- [Agentic AI for Beginners](01_Agentic_AI_for_Beginners.md)
- [Workflow Engine](03_Workflow_Engine.md)
- [Interrogation Algorithm](05_Interrogation_Algorithm.md)
- [Critic and Convergence](06_Critic_and_Convergence.md)
- [Fresh Run, Source Scope, and Cleanup](17_Fresh_Run_Source_Scope_and_Cleanup.md)
- [Agent-to-Documentation Mapping](15_Code_Documentation_Mapping.md)
