# 010 — Agent System

**Document type:** Subsystem architecture  
**Status:** Canonical for this subsystem; subordinate to `001_ARCHITECTURE`  
**Audience:** Framework maintainers, future agent-contract authors, and operators who invoke roles in Cursor  
**Depends on:** [`../specifications/000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md), [`../specifications/001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md)  
**Companions:** [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md), [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md), [`040_WORKFLOW.md`](040_WORKFLOW.md)  
**Scope:** Conceptual architecture of specialized writing agents. This document does not create agent files under `agents/`.

---

## 1. Purpose

The Agent System is the intelligence layer of the AI Book Framework. It defines how specialized roles consume book contracts, produce durable artifacts, and stop when they lack authority or evidence.

This subsystem exists so that novel production does not collapse into a single chat that invents world rules, drafts prose, and certifies its own quality. Agents are workers with contracts. The human author is the executive producer. The repository is the system of record.

The Agent System must:

- Keep creative generation and quality adjudication in separate roles.
- Make every stage leave inspectable files.
- Give each role a bounded memory read/write surface.
- Fail loudly on missing inputs, canon conflicts, and skipped gates.
- Scale from a five-role kernel to optional specialists without creating a second source of truth.

Normative layout, authority stack, and kernel role names are defined in [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §8. This document specifies how those roles behave, when they split, and how they collaborate.

---

## 2. Design principles

1. **Role purity over file count.** A responsibility has one owner. That owner may be a kernel agent hosting several compatible capability passes, not thirteen separate agent files on day one.
2. **Artifacts over chat.** If a decision must survive the next session, an agent writes it to a file. Chat has zero authority.
3. **Downward authority.** Brief constrains architecture. Architecture constrains outline. Outline constrains prose. Canon and research constraints bind every generative stage. See [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md).
4. **Propose, do not silently promote.** Generative agents may suggest new canon. Only a human-approved memory write makes it true.
5. **Review is adversarial, not decorative.** The Review agent files defects. It does not “fix” a chapter by regenerating it and calling the result approved.
6. **One primary role per session.** Multi-role work is a sequence of handoffs, not a blended personality.
7. **Fail loudly.** Missing brief, unapproved outline, or contradictory canon is a stop condition, not a prompt to invent a patch in prose.
8. **Replaceable models.** Contracts bind to files and gates, not to a vendor, temperature, or chat UI.
9. **Human gates are structural.** Agents prepare options. Humans commit direction. Mandatory gates are listed in §13 and in [`040_WORKFLOW.md`](040_WORKFLOW.md).
10. **Extensibility without parallel kernels.** New agents plug into this topology. They do not invent a second brief, a second canon, or a bypass around review.

---

## 3. Agent responsibilities

### 3.1 Kernel agents (v1 implementation identity)

When agent contract files are added in a later sprint, the kernel is exactly these five roles. Names match [`../../AGENTS.md`](../../AGENTS.md) and [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §8.1.

| Kernel agent | Primary job | Must not |
| --- | --- | --- |
| **Architect** | Design acts, major turns, escalation, and thematic spine; declare what the story structure needs from world and cast | Deliver finished chapter prose; treat unapproved architecture as binding for mass drafting |
| **Research** | Produce grounding and speculative-consistency constraints; distinguish sourced fact from conjecture | Override brief themes; promote unchecked invention into canon |
| **Outline** | Write scene contracts: goal, conflict, turn, payoff, POV, and continuity hooks | Write full prose chapters; invent a second plot authority that contradicts approved architecture |
| **Writer** | Draft and revise prose under outline, voice, and canon | Redefine architecture; silently retcon canon; mark a chapter `Approved` |
| **Review** | Adjudicate continuity, structure, character, pacing, prose, theme, and process; file defects with taxonomy and severity | Approve on the human’s behalf; rewrite canon; regenerate a chapter as a substitute for a defect report |

These five are the only agents required to run the canonical pipeline:

```text
Book Brief → Architect → Research → Outline → Writer → Review → Revision
```

### 3.2 Capability roles (conceptual, not automatic agent files)

The framework must support the following capabilities. Support does **not** mean each name becomes a separate `agents/*.md` file.

| Capability role | Default home | Output class |
| --- | --- | --- |
| Project Manager | Human author following [`040_WORKFLOW.md`](040_WORKFLOW.md); optional later orchestrator that only sequences work | Stage plan, gate checklist, session brief |
| Knowledge Keeper | Memory discipline in every agent, plus Review continuity passes; optional later Lore Keeper | Canon integrity, promotion queue, stale-fact flags |
| Researcher | Research kernel agent | Research notes and constraints |
| World Builder | Architect (what the plot needs) + Research (what must stay consistent) + human-approved canon writes | World, location, faction, culture, technology, and rules files |
| Character Designer | Architect (cast and arc function) + Outline (scene-level pressure) + canon character sheets | Character and relationship files |
| Plot Designer | Architect (acts and turns) + Outline (scenes) | Architecture docs and scene contracts |
| Chapter Writer | Writer | Chapter drafts |
| Dialogue Specialist | Writer voice/dialogue pass | Revised dialogue inside the same chapter files |
| Editor | Review (find line/copy/structure issues) + Writer (apply fixes) | Review reports and revised prose |
| Continuity Checker | Review continuity pass | Continuity defects |
| Fact Checker | Research (constraint set) + Review (claims in prose) | Constraint violations and research defects |
| Critic | Review craft/theme pass | Craft and theme defects; taste items marked `Note` |
| Publisher | Human + future export module; not a prose author | Assembled manuscript, front/back matter, export package |

### 3.3 When to combine responsibilities

Combine capability roles into a kernel agent when **all** of the following are true:

1. They produce or consume the **same artifact class** (for example, both Continuity Checker and Critic write `book/reviews/*`).
2. They share the **same authority direction** (both constrain the Writer; neither invents canon as truth).
3. Splitting would add a handoff without a measured quality gain.
4. The project’s complexity still fits one context assembly recipe (current chapter + neighbors + relevant canon slices).

**Required combinations in v1:**

- Plot Designer is not a third plot agent. Act design lives in Architect; scene design lives in Outline. Two files, one downward authority line.
- Chapter Writer and default Dialogue Specialist live in Writer. Dialogue is a pass, not a competing draft authority.
- Continuity Checker, Fact Checker, Editor-as-finder, and Critic live in Review as named passes of one adjudication role.
- Knowledge Keeper is a duty of every agent (read the right files, flag new facts) rather than a sixth required kernel file.

### 3.4 When to separate responsibilities

Split a capability into its own agent contract when **any** of the following is true:

1. **Generation vs adjudication would mix.** Writer and Review never share a session that both drafts and certifies the same chapter.
2. **Invention vs grounding would mix.** Research must not be asked to “make the science support this cool scene” in the same pass that drafts the scene.
3. **A defect class is systemic.** If dialogue, timeline, or technical claims repeatedly land as `Major`/`Blocker` after ordinary Review, add a specialist pass with its own contract.
4. **Canon volume exceeds one Review context.** A Lore Keeper / Knowledge Keeper agent that only audits and proposes canon updates becomes justified.
5. **World or cast work is a production track of its own** (series bible, large ensemble, invented science system) and is crowding Architect/Outline sessions.
6. **Manuscript assembly is routine.** Publisher/export may become an agent that reads accepted chapters and writes `book/manuscript/`, never rewriting story facts.

### 3.5 Boundaries that never collapse

| Boundary | Rule |
| --- | --- |
| Writer ↔ Review | The drafter does not adjudicate. The adjudicator does not replace the draft with an unreviewed regeneration and call it done. |
| Outline ↔ Writer | Scene intent is contracted before prose. Writer may flag outline defects; Writer may not “fix” structure only in prose. |
| Architect ↔ Outline | Outline may specialize beats. Outline may not change act turns or thematic spine without an architecture amendment. |
| Research ↔ Canon | Research writes constraints and notes. Research does not declare story-world truth until a human promotes facts into canon. |
| Review ↔ Canon | Review files defects. Review does not silently edit character sheets or the timeline to match a convenient draft. |
| Any agent ↔ Brief | No agent amends `book/BOOK_BRIEF.md` intent without human direction. |
| Publisher ↔ Writer | Export packaging does not rewrite plot, canon, or accepted prose except mechanical formatting. |

---

## 4. Agent lifecycle

An agent run is a bounded session, not a standing process with hidden state.

```text
Invoke → Load contract → Assemble context → Check gates
    → Execute → Write artifacts → Handoff or escalate → Close
```

| Phase | Required behavior |
| --- | --- |
| **Invoke** | The operator names one primary role and one span (scene, chapter, act slice, or named research question). |
| **Load contract** | Read the role’s future `agents/<role>.md` when it exists; until then, obey this document plus [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §8. |
| **Assemble context** | Load only the files listed for that role and span (§7). Do not substitute `examples/` for live canon. |
| **Check gates** | Refuse to proceed if entrance criteria in [`040_WORKFLOW.md`](040_WORKFLOW.md) are red. |
| **Execute** | Produce the role’s outputs. Do not perform adjacent roles “while you are here.” |
| **Write artifacts** | Update the canonical output paths. Do not scatter untitled alternates. |
| **Handoff or escalate** | Point to the next role and files, or stop for the human. |
| **Close** | Leave no authority in chat. If a decision was made, it is already in a file. |

Re-running an agent on the same inputs must update the same output paths (git-visible diffs). Experimental alternates use a git branch or a clearly named `*.alt.md` candidate pending human choice. See [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §8.5.

---

## 5. Agent inputs

Inputs are files with statuses, not “whatever is in the thread.”

| Agent | Required inputs | Optional inputs |
| --- | --- | --- |
| **Architect** | Approved `book/BOOK_BRIEF.md`; current `book/PROJECT_MEMORY.md` | Prior architecture drafts; research constraints that already exist; series canon if a series module is enabled |
| **Research** | Approved brief; the question list (from memory, architecture, or outline); any existing `book/research/` notes for that topic | Architecture and outline slices that create the question; relevant canon |
| **Outline** | Approved brief; approved architecture for the span; relevant canon; research constraints that the span will claim | `PROJECT_MEMORY.md`; neighboring outlines; prior review defects that forced re-outline |
| **Writer** | Approved brief; approved scene contracts for the chapter; relevant character sheets, timeline slice, world rules, open threads; style constraints from the brief (and style pack if enabled) | Neighbor chapters; latest review report if this is a revision; research notes for claims in-scene |
| **Review** | The draft (or span) under review; the outline contracts it must satisfy; brief constraints; canon slices the draft touches; prior review reports for the same chapter | Diff against last acceptable version; research notes for fact checking |
| **Publisher (when present)** | Human-accepted chapters; brief metadata; style/export module settings | Front/back matter drafts |

**Hard input rule:** If a required input is missing, `Draft`, or internally contradictory, the agent stops and names the blocker. It does not infer a substitute from an example novel or from chat memory.

---

## 6. Agent outputs

Outputs are path-stable artifacts under `book/` (and review/export trees defined in [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §3).

| Agent | Writes | Does not write |
| --- | --- | --- |
| **Architect** | `book/architecture/*` (act map, thematic spine, major turns, open design questions) | Finished chapters; approved canon as a side effect of a cool idea |
| **Research** | `book/research/*` notes with sourced-vs-conjecture labels and a list of canon files that would be impacted if promoted | Silent edits to character sheets or timeline |
| **Outline** | `book/outline/*` scene contracts for the requested span | Chapter prose; a competing act map |
| **Writer** | `book/chapters/ch-NN-slug.md` drafts and requested revision passes; flags for new facts in the chapter header and/or `PROJECT_MEMORY.md` | `Status: Approved` on a chapter; canon promotions |
| **Review** | `book/reviews/*` reports with defect IDs, taxonomy, severity, and verdict `Pass` / `Fail` / `Pass-with-fixes` | Canon “fixes” that hide the defect; human acceptance |
| **Any generative agent** | Lean updates to `book/PROJECT_MEMORY.md` for current focus, open questions, and unpromoted fact flags | A second copy of a canon fact restated as if it were a new truth |

Alternates (`*.alt.md` or branch-only files) are candidates. They are not live canon or live manuscript until the human merges them into the canonical path.

---

## 7. Agent context

Context is assembled, not dumped. Novels exceed model windows; the architecture relies on hierarchical compression defined in [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md) and retrieval rules in [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §22–23.

**Default assembly order for any agent:**

1. Applicable Cursor rules (when present).
2. The agent’s own contract (when present).
3. `book/BOOK_BRIEF.md` + `book/PROJECT_MEMORY.md`.
4. Stage inputs for the span (architecture, outline, research, chapter, review).
5. Canon **slices** the span can touch (named characters, locations, timeline window, open threads), not the entire bible.
6. Optional prompts under `prompts/<role>/` for the specific job.

**Do not load:**

- `examples/` as if they were this book’s facts.
- The full manuscript for a single-chapter draft.
- Unrelated act outlines.
- Chat transcripts from previous sessions.

**Example (Writer, chapter 7):** load brief constraints; project memory; `book/outline/` contracts for ch-07; character sheets for characters on-page; `timeline.md` rows covering the chapter’s clock; world file for the location; `threads.md` items the scene might pay or plant; ch-06 ending and ch-08 opening if they exist; the latest ch-07 review if this is a revision.

---

## 8. Agent memory access

Memory tiers and write rules are defined in [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md). Agents must obey this access matrix.

| Agent | Brief (A) | Working memory (B) | Canon (C) | Research (D) | Production (E) |
| --- | --- | --- | --- | --- | --- |
| Architect | Read | Update focus and open design questions | Read; **propose** new world/cast needs | Read | Write architecture; read outlines |
| Research | Read | Flag questions and constraint risks | Read; **propose** promotions | Write notes | Read architecture/outline that created the question |
| Outline | Read | Update span status and questions | Read; flag scene-local new facts | Read constraints | Write outlines |
| Writer | Read | Flag new facts and leftover threads | Read only | Read constraints for in-scene claims | Write chapter drafts |
| Review | Read | Flag unresolved defects and freeze risks | Read only | Read for fact-check pass | Write review reports; read chapters/outlines |
| Human | Write approvals and amendments | Edit freely | **Promote, amend, deprecate** | Accept constraint language | Accept/reject production artifacts |

**Read-before-write rule:** before outlining or drafting a scene, the responsible agent consults brief constraints, relevant character sheets, the timeline slice, open threads the scene might touch, and the latest review constraints for that chapter or act.

**Prose is never canon authority.** If a chapter invents a reusable fact, the Writer flags it and the human either promotes it into canon or strikes it. Review treats an unpromoted reusable fact as a process defect (`X`) and, if it contradicts canon, a continuity defect (`C`).

---

## 9. Agent communication

Agents do not negotiate in a private channel. They communicate through files and a single human operator.

| Channel | Allowed use | Authority |
| --- | --- | --- |
| Canonical files under `book/` | Contracts, canon, drafts, reviews | Authoritative once status and authority stack agree |
| `PROJECT_MEMORY.md` | High-signal now-state and flags | Operational, not a long-term fact store |
| Cursor chat | Clarifying questions, option listing, patch application | None unless written back |
| Git commit messages / diffs | Audit trail of what changed | Historical evidence, not a substitute for canon |
| `*.alt.md` or `exp/` branches | Competing options | Candidate only |

**Handoff packet (minimum):**

1. Role that just finished.
2. Output paths and their `Status`.
3. Preconditions now green or still red for the next role.
4. Open questions the next role must not invent away.
5. Human gate required before proceeding, if any.

**Example handoff (Outline → Writer):** “Span ch-07 scene contracts are `Approved` in `book/outline/ch-07.md`. Writer may draft `book/chapters/ch-07-the-gate.md`. Do not introduce a new faction; open thread T-12 may be planted, not paid. Human already approved this outline span.”

---

## 10. Agent dependencies

Runtime artifact dependencies follow [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §15.2.

```text
BOOK_BRIEF
 ├── Architect depends on Brief
 ├── Research depends on Brief (+ questions from Architect/Outline/Writer)
 ├── Outline depends on Brief + approved Architecture (+ Research when claims need grounding)
 ├── Writer depends on Brief + approved Outline + Canon slices + Research constraints
 ├── Review depends on Draft + Outline + Brief + Canon (+ Research for fact check)
 └── Revision depends on Review defects; may loop to Writer or Outline (or Architect if structure is wrong)
```

| Dependency | Type | If missing |
| --- | --- | --- |
| Brief before architecture approval | Hard | Architect produces at most a non-binding sketch and stops for human brief work |
| Approved architecture before mass outline of that span | Hard | Outline refuses the span |
| Research before every scene | Soft | Required only when the scene asserts grounded or speculative-technical claims |
| Approved outline before chapter draft | Hard on the production path | Writer refuses |
| Review before chapter `Done` | Hard | Chapter remains a draft |
| Publisher/export tools | Soft | Human can assemble markdown manually |

Changing an upstream artifact invalidates dependents until impact review. Agents must not keep drafting against a superseded outline.

---

## 11. Agent hierarchy

Hierarchy is authority, not prestige.

```text
Human author (executive producer, final editor, moral owner)
    └── Project Manager capability (human in v1; optional sequencer later)
            ├── Architect
            ├── Research
            ├── Outline
            ├── Writer ── optional Dialogue Specialist pass
            └── Review ── optional Continuity / Fact / Line-edit / Critic passes
                    └── Publisher / export (after human acceptance)
```

Rules:

- No kernel agent outranks the human.
- Architect outranks Outline on structure. Outline outranks Writer on scene intent.
- Review outranks Writer on whether a draft may be called ready for human acceptance. Review does not outrank the brief or approved architecture; if Review believes those are wrong, it files a process/structure defect and escalates.
- Optional specialists never outrank the kernel role they split from. A Dialogue Specialist cannot declare the chapter done. A Continuity Checker cannot rewrite the timeline as a side effect of a failed check.
- Publisher sits after acceptance. It cannot reopen story authority.

---

## 12. Agent orchestration

v1 orchestration is **human-directed**. There is no required custom orchestrator binary. Cursor is the runtime surface. [`040_WORKFLOW.md`](040_WORKFLOW.md) is the stage machine. Cursor rules (later sprint) are adapters that refuse skipped gates.

**Orchestration procedure:**

1. The operator names the stage and role.
2. The operator `@`-references required files.
3. The agent checks entrance criteria.
4. The agent writes outputs to known paths.
5. If a human gate is next, the agent stops.
6. If the next stage is same-authority continuation (for example, Writer applying `Pass-with-fixes` after Review), the operator starts a new session with that role.

**Forbidden orchestration patterns:**

- One session that outlines, drafts, and self-reviews the same chapter.
- An agent advancing `Status:` from `Draft` to `Approved` on brief, architecture, outline, or chapter.
- Background multi-agent loops that mutate canon without a human commit.
- Using a “Project Manager” prompt to generate story facts.

When a later orchestrator agent exists, its only jobs are: checklists, file-presence checks, recommended next role, and refusal to skip gates. It does not write prose, canon, or review verdicts.

---

## 13. Human approval points

Agents may prepare text. The following commitments are human-only. They match [`000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md) §12.6 and [`040_WORKFLOW.md`](040_WORKFLOW.md).

| Gate | What the human accepts | Agent behavior until then |
| --- | --- | --- |
| Book brief for production | Premise, audience, tone, themes, POV policy, hard constraints | No mass architecture or drafting |
| Architectural direction | Act map, major turns, thematic spine for the novel or named span | Outline may not treat the span as binding |
| Outline span before mass drafting | Scene contracts for the chapters about to be written | Writer refuses those chapters |
| Chapter accept / revise / reject | Whether the reviewed draft enters the accepted line | Writer and Review must not set `Approved` |
| Canon change / retcon | New or altered story-world truth | Agents only propose and flag |
| Act freeze / manuscript freeze | That a span is done enough to stop casual edits | Publisher may assemble only from frozen/accepted files |
| Publication / export release | What leaves the repository | Publisher agent (if any) waits |

Taste, ethics, and disclosure remain human even when Review is `Pass`.

---

## 14. Conflict resolution between agents

When two agents (or two artifacts they own) disagree, do not blend the answers in prose.

1. **Identify the fact class:** intent, structure, canon fact, research constraint, scene intent, prose wording, or process law.
2. **Apply the authority stack** in [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §5 and the memory rules in [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md).
3. **Prefer the more specific approved artifact in the same tier** (approved scene outline beats a vague architecture note for scene intent; approved architecture beats outline if the turn itself conflicts).
4. **If still ambiguous, fail loudly.** File a defect or an open question. Name both sources.
5. **Human updates the winning artifact**, then cascade downstream (re-outline, revise, or retcon protocol).

**Worked conflict:** Outline says chapter 12 ends with a betrayal. Approved architecture says Act II earns trust and the betrayal is the Act III turn. Outline is wrong until the human amends architecture. Writer must not “split the difference” by hinting betrayal. Review files `S-…` (structure) and `X-…` (process) if prose already did.

**Worked conflict:** Writer’s draft states Helios Station has a working fusion core. Research constraint says the core is cold and the plot depends on scavenged batteries. Research constraint + canon win. Writer revises. If the author prefers the fusion core, the human changes research/canon/architecture first.

Agents do not vote. Majority of sessions is not evidence.

---

## 15. Quality control

Quality is a system property, not a politeness pass. Standards live in [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §18. Agents apply them as follows.

| Control | Owner | Mechanism |
| --- | --- | --- |
| Gate entrance/exit | Orchestration + each agent | Refuse red gates; write required artifacts |
| Defect taxonomy | Review | `C` continuity, `S` structure, `H` character, `P` pacing, `R` prose, `T` theme, `X` process |
| Severity | Review | `Blocker`, `Major`, `Minor`, `Note` |
| Role-boundary control | Every agent | Forbidden-behavior list in §3 |
| Continuity control | Writer flags; Review adjudicates; human promotes canon | Memory write rules |
| Voice control | Writer under brief/style pack; Review prose pass | Anti-slop stance unless a style pack allows a mode |
| Human acceptance | Human | Chapter is not done without it |

Review verdicts:

- **Pass** — no Blocker/Major remain; Minors are listed; human may still reject on taste.
- **Fail** — Blocker or unaddressed Major; Writer or Outline must return.
- **Pass-with-fixes** — specified Minors/Majors with a bounded repair list; re-review those items.

Writer quality control is **conformance**: scene goals met, canon respected, new facts flagged. Writer does not self-issue `Pass`.

---

## 16. Failure handling

| Failure | Detection | Agent response |
| --- | --- | --- |
| Missing required input | File absent or empty required sections | Stop; name the path and the gate |
| Unapproved upstream | `Status: Draft` on brief/architecture/outline | Stop for production-path work; do not pretend approval |
| Canon contradiction | Two approved files disagree, or prose disagrees with canon | Stop invention; file defect; do not silently pick a winner |
| Hallucinated source | Research cannot point to a note or distinguish conjecture | Label as conjecture or delete the claim |
| Scope bleed | Agent starts doing the next role | Stop; hand off |
| Model collapse / generic prose | Review `R` defects; human taste | Revision pass with style constraints; do not lower the gate |
| Partial write | Session dies mid-file | Resume on the same path; prefer git to reconstruct last good state |
| Wrong-path alternate | Extra untitled drafts | Delete or quarantine as `*.alt.md`; never leave two live chapters for one number |

**Escalation to human is mandatory** when: a retcon is needed; brief constraints conflict with a desired scene; two approved artifacts conflict; ethical or content-boundary questions appear; or the agent would have to invent a missing plot turn.

Recovery of production state is a workflow concern: see [`040_WORKFLOW.md`](040_WORKFLOW.md) (mistake recovery) and git history in [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §10.

---

## 17. Agent isolation

Isolation prevents role contamination and canon forks.

1. **Session isolation.** One primary role, one span, one output tree.
2. **Context isolation.** Review does not inherit a Writer prompt that says “make this chapter succeed.” Review loads the draft and its constraints.
3. **Memory isolation.** Temporary chat context is discarded at session end. Only files persist. See [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §17.
4. **Authority isolation.** Optional specialists cannot widen their write set.
5. **Experiment isolation.** Plot alternatives live on `exp/` branches or `*.alt.md` files. They do not leak into `book/canon/` until chosen.
6. **Framework vs novel isolation.** Agents writing the book do not “improve” framework specs as a side effect of a chapter session. Framework changes are a separate track ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §17.1).
7. **Example isolation.** `examples/` never enters the live authority stack.

Isolation is not a ban on reading upstream files. It is a ban on writing outside the role’s output set and on carrying unrecorded decisions between sessions.

---

## 18. Agent extensibility

A new agent is legal only if it declares all of the following (the contract anatomy in [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §8.2):

1. Mission (one paragraph).
2. Required and optional inputs.
3. Outputs and paths.
4. Acceptance criteria.
5. Forbidden behaviors.
6. Handoff to a named next role.
7. Escalation conditions.
8. Subsystem home: Intent, Continuity/Memory, Structure, Production, Quality, or Operations/Export.
9. Mapping from any capability name in §3.2.

**Allowed extensions:** Dialogue Specialist, Line Editor, Continuity Auditor, Lore Keeper, genre-research specialists, export/Publisher.

**Disallowed extensions:** a second Architect, a “Creative Director” that overrides the brief from chat, a self-approving Writer, a canon store outside `book/`, an agent that publishes without a human gate.

Modules ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §14) may add optional agents. They may not replace `BOOK_BRIEF` or memory authority, and they may not bypass review.

---

## 19. Future scalability

| Scale axis | Agent-system response |
| --- | --- |
| Longer novels | Keep kernel roles; slice context; add Continuity Auditor when timeline/POV load demands it |
| Series | Shared series canon; volume-level briefs; spoiler boundaries so agents on book 1 cannot ingest book 3 secrets |
| Teams | Same roles mapped to people; branch-scoped PRs; no new kernel |
| Genre packs | Extra research/review rubrics and optional specialists; same five-role backbone |
| Model churn | Contracts stay file-shaped; prompts version in `CHANGELOG.md` |
| Automation | Validators and orchestrators check files; they do not become owners of truth |
| Evaluation | Continuity regression and outline-coverage tools consume artifacts this system already requires |

The north star remains unchanged: specialized junior staff, human executive control, durable handoffs. See [`000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md) §14.

---

## 20. Example: end-to-end agent workflow

Illustrative only. Names below are pedagogical fixtures, not live canon.

**Book:** working title *Helios Wake*. **Protagonist:** Aya Okoro, salvage engineer. **Inciting need:** a sealed deck on Helios Station still has power.

1. **Human (Project Manager capability)** fills `book/BOOK_BRIEF.md` (close-third on Aya, no omniscient station AI narrator, hard constraint: no faster-than-light travel). Human sets `Status: Approved`.
2. **Architect** reads brief and memory. Writes `book/architecture/act-map.md`: Act I boarding and scarcity; Act II map of the dead core; Act III choice to save a rival crew or the station archive. Lists open question: “What still has power, and why is that a moral problem?” Human approves architecture.
3. **Research** writes `book/research/orbital-decay.md`: constraint — station cannot be “falling in a week” without a defined orbit; constraint — sealed deck power must be batteries, residual reactor, or a lie. Marks conjecture vs sourced orbital mechanics. Lists impacted canon files: `world/helios-station.md`, `timeline.md`.
4. **Human (Knowledge Keeper duty)** promotes accepted constraints into `book/canon/world/helios-station.md` and a first `timeline.md`. Aya sheet created at `book/canon/characters/aya-okoro.md`.
5. **Outline** writes scene contracts for chapter 1: goal (reach the hatch), conflict (air and authority), turn (the hatch is warm), payoff (Aya now owns a secret). Status `Approved` by human for ch-01 only.
6. **Writer** drafts `book/chapters/ch-01-the-warm-hatch.md`. Mid-draft it invents a sister. Writer does **not** promote her. It flags `New fact (unpromoted): Aya mentions a sister, Kira`. Updates `PROJECT_MEMORY.md` with that flag.
7. **Human** rejects Kira as a cast member. Writer pass removes the mention. No character sheet is created.
8. **Review (continuity + craft + fact-check passes in one report)** files `P-001` (opening stalls on inventory) as Minor and verifies the hatch heat matches battery-residual language in canon. Verdict `Pass-with-fixes`.
9. **Writer** applies P-001 on the same chapter path.
10. **Review** re-checks P-001. Verdict `Pass`.
11. **Human** accepts chapter 1. Working memory updates current focus to chapter 2. Git commit records the accepted draft.

No agent skipped a gate. No agent treated chat as canon. Dialogue polish, if needed later, is a Writer pass on the same file, followed by Review of the diff.

---

## 21. Acceptance and validation criteria

### 21.1 This document is complete when

- Kernel agents, capability roles, combine/separate rules, and never-collapse boundaries are specified.
- Lifecycle, inputs, outputs, context, memory access, communication, dependencies, hierarchy, and orchestration are specified.
- Human gates match [`040_WORKFLOW.md`](040_WORKFLOW.md).
- Conflict, quality, failure, isolation, extensibility, and scale behaviors are specified.
- An end-to-end example names real paths and stop conditions.
- No agent files are required for this document to be usable as a contract for later `agents/` work.

### 21.2 The Agent System is correctly implemented (later sprints) when

| Check | Pass condition |
| --- | --- |
| Kernel contracts | `agents/architect.md`, `research.md`, `outline.md`, `writer.md`, `review.md` exist and match §3.1 |
| Role purity | A Writer session cannot set chapter `Approved`; a Review session cannot omit a defect report in favor of silent regeneration |
| Gates | Mass drafting without approved outline is refused |
| Memory | New reusable facts are flagged; canon changes require human promotion |
| Handoffs | Each stage writes the paths in §6 |
| Isolation | Review context does not include “make this draft succeed” as a goal |
| Extensibility | Optional agents declare the §18 fields and map to §3.2 |
| Non-conflict | This subsystem does not contradict `001_ARCHITECTURE` authority stack or the five-role kernel |

### 21.3 Operator validation (usable before agent files exist)

A session that names one role, loads the files in §5, writes the files in §6, and stops at the next human gate is a valid Agent System run.

---

## 22. References

- [`../specifications/000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md) — human authorship, role separation, fail loudly
- [`../specifications/001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) — repository law, agent topology, quality taxonomy
- [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) — what agents may treat as true
- [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md) — entities agents read and write
- [`040_WORKFLOW.md`](040_WORKFLOW.md) — when each agent runs
- [`../../AGENTS.md`](../../AGENTS.md) — kernel role index
- [`../../WORKFLOW.md`](../../WORKFLOW.md) — operator stage summary
- [`../../USER_GUIDE.md`](../../USER_GUIDE.md) — session habits

---

## Document Control

| Field | Value |
| --- | --- |
| ID | `010_AGENT_SYSTEM` |
| Title | Agent System — AI Book Framework |
| Layer | Subsystem architecture |
| Depends on | `000_PROJECT_VISION`, `001_ARCHITECTURE` |
| Enables | Future `agents/*.md` contracts, Cursor role rules, prompt playbooks per role |
| Maintenance rule | Update when kernel roles, write permissions, gates, or isolation rules change; remain consistent with `001_ARCHITECTURE` |

---

*End of document.*
