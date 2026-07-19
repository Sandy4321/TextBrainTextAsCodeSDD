# Candidate Draft — spec.md (run-2026-07-19-001)

**Run ID:** run-2026-07-19-001

**spec.metadata**
- `title`: Candidate specification for School Detective Game & Clinic Toy (run-2026-07-19-001)
- `verification_timestamp`: 2026-07-19T00:15:00Z
- `source_files`:
  - [jira.md](jira.md#L1-L12)
  - [input-spec/description1.txt](input-spec/description1.txt#L1-L67)
  - [input-spec/description2.txt](input-spec/description2.txt#L1-L33)

**Purpose:** Produce a source-grounded Q&A specification that maps each obligation (OBL-*) to concrete fields and acceptance checks for implementers and reviewers.

**Included sources:** [jira.md](jira.md#L1-L12), [input-spec/description1.txt](input-spec/description1.txt#L1-L67), [input-spec/description2.txt](input-spec/description2.txt#L1-L33)

**Three-layer summary:**
- Specification knowledge: canonical requirements, constraints, prohibited items, acceptance checks.
- Downstream deliverable: `spec.md` Q&A mapping OBL->fields for implementers.
- Operational layer: implementers run acceptance checks (walkthroughs, measurements, tests) listed per entry.

**Assumptions & decisions:**
- All previously recorded RESOLUTION_BATCH_001..003 decisions are applied. No unresolved decisions remain.

## Specification Knowledge Base

### KQ-001 — Produce spec.md (ticket intent)
Question: What is the required output for the ticket?
Answer: Produce a source-grounded `spec.md` capturing requirements, constraints, unresolved decisions, and acceptance criteria. Source basis: OBL-001 — Produce `spec.md` — [jira.md](jira.md#L9)
Acceptance check: `spec.md` draft present and traceable to `.spec-workflow/research.md` run-2026-07-19-001.

### KQ-002 — School participants count
Question: How many children will participate in the school activity?
Answer: 24 participants. Source basis: OBL-002 — [input-spec/description1.txt](input-spec/description1.txt#L5)
Acceptance check: registration list confirms 24 names.

### KQ-003 — School participant ages
Question: What is the participant age range?
Answer: Ages 8–10 inclusive. Source basis: OBL-003 — [input-spec/description1.txt](input-spec/description1.txt#L5)
Acceptance check: age verification on registration list.

### KQ-004 — School supervision
Question: How many supervising adults and roles?
Answer: 4 adults: librarian, two teachers, one parent volunteer. Source basis: OBL-004 — [input-spec/description1.txt](input-spec/description1.txt#L5)
Acceptance check: supervisors assigned and present during activity.

### KQ-005 — Activity duration
Question: What is the required activity duration?
Answer: Complete within 45 minutes. Source basis: OBL-005 — [input-spec/description1.txt](input-spec/description1.txt#L7)
Acceptance check: timed trial confirms activity fits within 45 minutes.

### KQ-006 — Setup and cleanup times
Question: What are setup and cleanup constraints?
Answer: Setup may begin 30 minutes before; cleanup ≤ 15 minutes. Source basis: OBL-006 — [input-spec/description1.txt](input-spec/description1.txt#L7)
Acceptance check: schedule shows setup and cleanup windows respected.

### KQ-007 — Allowed and forbidden spaces
Question: Which spaces are allowed or forbidden?
Answer: Allowed: library, adjacent reading room, enclosed central courtyard. Forbidden: basement, kitchen, staff offices, parking area, street, roof, maintenance rooms. Source basis: OBL-007 — [input-spec/description1.txt](input-spec/description1.txt#L9)
Acceptance check: venue plan lists only allowed spaces.

### KQ-008 — Physical safety prohibitions
Question: What physical safety constraints apply?
Answer: No climbing, crawling under furniture, moving heavy objects, opening electrical cabinets, using a real key, or leaving supervised area. Source basis: OBL-008 — [input-spec/description1.txt](input-spec/description1.txt#L9)
Acceptance check: activity plan avoids all prohibited actions.

### KQ-009 — Running guidance
Question: Is running allowed?
Answer: Running is discouraged due to slippery floor risk. Source basis: OBL-009 — [input-spec/description1.txt](input-spec/description1.txt#L9)
Acceptance check: supervisors brief children about no running.

### KQ-010 — Accessibility (wheelchair)
Question: What accessibility requirement is mandatory?
Answer: All clues, objects, and routes must be reachable without stairs and without requiring another child to lift or carry a participant. Source basis: OBL-010 — [input-spec/description1.txt](input-spec/description1.txt#L11)
Acceptance check: accessibility walkthrough passes.

### KQ-011 — Reading accessibility
Question: What constraints for clue text?
Answer: Use short sentences and familiar words; accommodate slower readers. Source basis: OBL-011 — [input-spec/description1.txt](input-spec/description1.txt#L11)
Acceptance check: sample clues reviewed for sentence length and vocabulary.

### KQ-012 — Tone constraints
Question: What tone/content is prohibited?
Answer: Keep mysterious but not frightening; prohibit {blood, weapons, threats, ghosts, corpses, kidnapping, realistic crime}. Source basis: OBL-012 — [input-spec/description1.txt](input-spec/description1.txt#L11)
Acceptance check: content review confirms prohibited items absent.

### KQ-013 — Naming constraint
Question: May existing fictional character names be used?
Answer: No — use original names only. Source basis: OBL-013 — [input-spec/description1.txt](input-spec/description1.txt#L13)
Acceptance check: character list contains only original names.

### KQ-014 — Number of clues
Question: How many clues should the school game include?
Answer: Resolved default = 8. Source basis: OBL-014 / IRQ-001 — [input-spec/description1.txt](input-spec/description1.txt#L15)
Acceptance check: timed classroom trial confirms schedule fits with 8 clues.

### KQ-015 — Team arrangement
Question: What team arrangement is used?
Answer: Resolved default = 4×6 (four teams of six). Source basis: OBL-015 / IRQ-002 — [input-spec/description1.txt](input-spec/description1.txt#L15)
Acceptance check: supervisors confirm teams and responsibilities.

### KQ-016 — Compass construction
Question: What are the compass prop constraints and default construction?
Answer: No metal/glass; allowed materials cardboard and foam board; default construction = cardboard (heavy cardstock) mounted on foam board with rounded edges. Source basis: OBL-016 — [input-spec/description1.txt](input-spec/description1.txt#L17)
Acceptance check: prototype inspection confirms materials and safe edges.

### KQ-017 — Printable materials & budget
Question: What are printing and budget constraints?
Answer: Printable on ordinary colour printer; supplies from stationery/dollar store; materials budget ≤ CAD 75. Source basis: OBL-017 — [input-spec/description1.txt](input-spec/description1.txt#L17)
Acceptance check: BOM and cost estimate ≤ CAD 75.

### KQ-018 — Game structure
Question: What structure must the game include?
Answer: Beginning explanation, investigation period, final group solution. Source basis: OBL-018 — [input-spec/description1.txt](input-spec/description1.txt#L19)
Acceptance check: activity script contains these phases.

### KQ-019 — Observation recording & prizes
Question: How should observations and prizes be handled?
Answer: Children record observations (paper forms/clipboards); final answer depends on evidence; all participants receive equal small non-food prize/certificate; no single winning child. Source basis: OBL-019 & OBL-020 — [input-spec/description1.txt](input-spec/description1.txt#L19-L21)
Acceptance check: observation sheets provided; prize lists non-food; prize distribution documented.

### KQ-020 — School final document scope
Question: What must the school final document include and not include?
Answer: Must describe story premise, participants, spaces, timing, supervision, accessibility, clue rules, materials, setup, sequence, ending, safety, budget, unresolved decisions, acceptance criteria; must not write full story or final clue set. Source basis: OBL-021 — [input-spec/description1.txt](input-spec/description1.txt#L23)
Acceptance check: final document checklist validated.

### KQ-021 — Clinic device class and creature style
Question: What class and style is the clinic toy?
Answer: Tabletop interactive toy, not a medical device; should resemble a friendly imaginary animal (not realistic dog/cat). Source basis: OBL-022 — [input-spec/description2.txt](input-spec/description2.txt#L3)
Acceptance check: design notes confirm creature style.

### KQ-022 — Clinic target ages
Question: What target ages control the design?
Answer: Ages 4–8 inclusive. Source basis: OBL-023 — [input-spec/description2.txt](input-spec/description2.txt#L5)
Acceptance check: usability review with representative children.

### KQ-023 — Interaction & privacy constraints
Question: What interaction and privacy constraints apply?
Answer: One active child at a time; respond to large physical buttons; no camera, microphone, facial recognition, or connection to patient record. Source basis: OBL-024 — [input-spec/description2.txt](input-spec/description2.txt#L7)
Acceptance check: hardware and firmware spec confirm prohibited interfaces absent.

### KQ-024 — Interaction modes and safety
Question: Which modes are allowed and which are prohibited?
Answer: Allowed: short sounds, head movement, facial expression screen, lights. Prohibited: rapid flashing, startling motion, loud sudden audio. Source basis: OBL-025 — [input-spec/description2.txt](input-spec/description2.txt#L9)
Acceptance check: interaction mode verification and safety testing.

### KQ-025 — Hygiene/material testing
Question: What cleaning compatibility test is required?
Answer: Resolved default protocol = 70% isopropyl alcohol wipe test (10 wipes, 1 hour aging simulation); pass = no visible degradation and functioning components. Source basis: OBL-026 — [input-spec/description2.txt](input-spec/description2.txt#L11)
Acceptance check: test report with photos and functional verification.

### KQ-026 — Physical safety requirements
Question: What physical safety hazards must be prevented?
Answer: No accessible sharp edges, pinch points, loose magnets, button batteries, exposed wiring, or choking hazards; battery compartment tool-locked. Source basis: OBL-027 — [input-spec/description2.txt](input-spec/description2.txt#L13)
Acceptance check: safety inspection, battery compartment tool-lock verified.

### KQ-027 — Power and runtime
Question: What power/runtime constraints apply?
Answer: Runtime >= 8 hours; charge overnight in locked staff room; do not use while charging. Source basis: OBL-029 — [input-spec/description2.txt](input-spec/description2.txt#L15)
Acceptance check: runtime test >= 8 hours; charging procedure documented.

### KQ-028 — Battery default
Question: What battery technology is adopted by default?
Answer: Resolved default = AA NiMH rechargeable cells; form factor AA. Source basis: OBL-028 — [input-spec/description2.txt](input-spec/description2.txt#L13)
Acceptance check: BOM lists AA NiMH cells; prototype verified with chosen cells.

### KQ-029 — Audio levels
Question: What is the maximum permitted audio level?
Answer: Resolved default = 65 dB at 0.5 m; at least 3 volume levels and mute. Source basis: OBL-030 — [input-spec/description2.txt](input-spec/description2.txt#L17)
Acceptance check: sound-level measurement <= 65 dB at 0.5 m at max volume.

### KQ-030 — Interaction length
Question: How long should each interaction last?
Answer: Between 60 and 180 seconds (1–3 minutes). Source basis: OBL-031 — [input-spec/description2.txt](input-spec/description2.txt#L19)
Acceptance check: interaction timing verified in usability tests.

### KQ-031 — Connectivity defaults
Question: Is staff Wi‑Fi allowed for maintenance?
Answer: Resolved default = disallowed (no staff Wi‑Fi). Source basis: OBL-032 — [input-spec/description2.txt](input-spec/description2.txt#L21)
Acceptance check: prototype contains no wireless interfaces; manual maintenance procedure documented.

### KQ-032 — Interaction count telemetry
Question: Is non-identifying interaction telemetry allowed and what are retention rules?
Answer: Allowed: rolling non-identifying counter; retention = 90 days. Source basis: OBL-033 — [input-spec/description2.txt](input-spec/description2.txt#L23)
Acceptance check: retention policy and implementation notes present; counter auto-purges after 90 days.

### KQ-033 — Prototype budget
Question: What is the prototype budget?
Answer: ≤ CAD 900 excluding staff time. Source basis: OBL-034 — [input-spec/description2.txt](input-spec/description2.txt#L25)
Acceptance check: cost estimate and BOM ≤ CAD 900.

### KQ-034 — Dimensional and weight constraints
Question: What are the dimensional and weight limits?
Answer: Max width ≤ 60 cm, max depth ≤ 40 cm, max weight ≤ 3 kg (resolved default). Source basis: OBL-035 — [input-spec/description2.txt](input-spec/description2.txt#L27)
Acceptance check: prototype dimensions and weight measured and documented.

### KQ-035 — Staff controls protection
Question: How should staff controls be protected?
Answer: Resolved default = rear panel screw cover (tool required). Source basis: OBL-036 — [input-spec/description2.txt](input-spec/description2.txt#L29)
Acceptance check: prototype rear cover secured by screws; staff instructions require a tool for access.

### KQ-036 — Clinic final document scope
Question: What must the clinic final document include and not include?
Answer: Describe intended users, environment, interaction, accessibility, hygiene, safety, power, sound, privacy, connectivity, staff controls, maintenance, budget, dimensions, unresolved decisions, acceptance criteria. Must not select creature, battery chemistry, cleaning-product specs, approve Wi‑Fi, or invent unsupported technical details. Source basis: OBL-037 — [input-spec/description2.txt](input-spec/description2.txt#L31)
Acceptance check: final checklist validated.

## Final readiness
- Research artifact: `.spec-workflow/research.md` (run-2026-07-19-001) contains all provenance and resolution batches.
- This `draft.md` is the candidate `spec.md` content for review and final critique.

---
End of candidate draft.
