# 060 — Book Core

**Document type:** Subsystem architecture  
**Status:** Canonical for this subsystem; subordinate to `001_ARCHITECTURE`  
**Audience:** Human authors operating a novel in this repository; kernel agent sessions; framework maintainers  
**Depends on:** [`../specifications/000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md), [`../specifications/001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md)  
**Companions:** [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md), [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md), [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md), [`040_WORKFLOW.md`](040_WORKFLOW.md), [`050_MEMORY_IMPLEMENTATION.md`](050_MEMORY_IMPLEMENTATION.md)  
**Agent contracts:** [`../../agents/architect/SYSTEM.md`](../../agents/architect/SYSTEM.md), [`../../agents/research/SYSTEM.md`](../../agents/research/SYSTEM.md), [`../../agents/outline/SYSTEM.md`](../../agents/outline/SYSTEM.md), [`../../agents/writer/SYSTEM.md`](../../agents/writer/SYSTEM.md), [`../../agents/review/SYSTEM.md`](../../agents/review/SYSTEM.md)  
**Live contracts:** [`../../book/BOOK_BRIEF.md`](../../book/BOOK_BRIEF.md), [`../../book/PROJECT_MEMORY.md`](../../book/PROJECT_MEMORY.md)  
**Scope:** The Book Core is the interface between the human author, the book memory system, and the five kernel agents. This document does not add directories, templates, prompts, scripts, or story content.

---

## 1. Book Core purpose

The Book Core is the control plane of a novel project. It is not a sixth kernel agent, not a second canon store, and not a replacement for the Memory System or the Agent System.

It exists so that:

- the human author’s concept has exactly one authoritative home (`book/BOOK_BRIEF.md`);
- current operational state has exactly one dashboard (`book/PROJECT_MEMORY.md`);
- durable story knowledge has exactly one filesystem (`book/` memory directories defined in [`050_MEMORY_IMPLEMENTATION.md`](050_MEMORY_IMPLEMENTATION.md));
- kernel agents load the right files, distinguish kinds of information, and stop when they lack authority;
- generated artifacts return through Review to a human gate before they can change what is true.

Book Core answers: *How does a human-owned idea become agent-usable state without silent invention?*

Memory law remains [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md). Filesystem homes remain [`050_MEMORY_IMPLEMENTATION.md`](050_MEMORY_IMPLEMENTATION.md). Role purity remains [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md). Stage order remains [`040_WORKFLOW.md`](040_WORKFLOW.md). Entity types remain [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md). This document binds those contracts at the book-project boundary.

---

## 2. Control flow

```text
Human Author
      ↓
BOOK_BRIEF.md
      ↓
PROJECT_MEMORY.md
      ↓
Book Memory
      ↓
Agents
      ↓
Generated Artifacts
      ↓
Review
      ↓
Human Approval
```

This is **authority and feedback**, not a substitute for the production backbone:

```text
Book Brief → Architect → Research → Outline → Writer → Review → Revision
```

| Arrow | Meaning |
| --- | --- |
| Human → Brief | The author writes intent. Empty fields stay empty. |
| Brief → Project memory | Working memory points at the brief. It does not copy it. |
| Project memory → Book memory | Flags and indexes send agents to `canon/`, `characters/`, `world/`, `plot/`, `timeline/`, `research/`. |
| Book memory → Agents | Agents read slices required by role and span ([`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §7–§8). |
| Agents → Generated artifacts | Architect writes act-level plot; Research writes notes; Outline writes scene contracts; Writer writes chapters; Review writes reports. |
| Generated artifacts → Review | Quality adjudication against brief, plot, canon, and Constraints. |
| Review → Human approval | Verdicts prepare a gate. Humans accept, reject, promote, or amend. Approved changes write back to brief, memory, or production files. |

Downstream layers do not silently rewrite upstream layers. A chapter cannot amend the brief. A review cannot promote canon. Project memory cannot outrank the brief or approved canon.

---

## 3. Human-author interface

The human author is the executive producer, final editor, and moral owner ([`../specifications/000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md) §5.1).

**Primary surfaces:**

| Surface | Job |
| --- | --- |
| `book/BOOK_BRIEF.md` | Write and lock intent |
| `book/PROJECT_MEMORY.md` | See current state; confirm next gate |
| Memory directories | Promote, amend, or deprecate story-world truth |
| Cursor diffs | Accept or reject agent patches |
| Git | Record chosen history |

**Rules for the human interface:**

1. Edit the brief directly when intent changes. Do not leave the real decision in chat.
2. Empty brief fields are allowed. They are not an invitation for agents to complete the novel concept.
3. Agents may list options. The author chooses. The author (or a human-directed edit the author has accepted) writes the winning text into the brief.
4. Setting brief Canon status to `Approved` is a human-only production gate ([`040_WORKFLOW.md`](040_WORKFLOW.md) §4.2, [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §13).
5. Taste, ethics, and disclosure remain human even when Review is `Pass`.

**Example (correct):** The author types a premise into §7 of the brief, sets Canon status to `Approved`, and commits. Architect then reads that file.

**Example (incorrect):** An agent “helpfully” fills Genre, Themes, and Ending direction because they were blank. That is a silent rewrite of intent.

---

## 4. BOOK_BRIEF authority

`book/BOOK_BRIEF.md` is **AUTHOR-OWNED**, **HUMAN-EDITABLE**, and **AUTHORITATIVE**.

It is Tier A intent memory ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §7.2, [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §3.1). It answers: *What are we trying to make?*

| Property | Rule |
| --- | --- |
| Ownership | Human author only for live text and Canon status |
| Agent read | Required when the task concerns the book itself |
| Agent write | Forbidden as a side effect of any kernel session |
| Empty fields | Mean “not provided.” Agents must not invent values |
| Conflict | Brief wins over architecture, outline, prose, research, and project memory for intent, audience, themes, tone, and hard limits |
| Status | Canon status `Draft` \| `Review` \| `Approved` \| `Deprecated`. Missing or not `Approved` blocks production-path mass architecture, outlining, and drafting |

The twenty-one sections of the live brief are the concept schema: title, working title, genre, subgenre, target audience, language, premise, logline, core concept, themes, tone, intended reader experience, setting, main protagonist, main conflict, central question, ending direction, important constraints, inspirations/references, author notes, canon status.

POV policy, rating, content boundaries, book non-goals, and hard world limits belong in **Important constraints** unless the author records them in another brief section. They are still brief-owned. They are not automatically world canon until promoted where a story-world sentence is required ([`050_MEMORY_IMPLEMENTATION.md`](050_MEMORY_IMPLEMENTATION.md) §6.2).

Inspirations are not canon and not a plagiarism workflow ([`../specifications/000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md) §16.2).

Schema sentences printed in the brief (for example, the note under Important constraints) are framework instruction. They are not story content. Only author-supplied values count as intent.

---

## 5. PROJECT_MEMORY role

`book/PROJECT_MEMORY.md` is the operational memory index. It is Tier B working memory. It answers: *Where are we right now?*

It **must not** duplicate the brief. It **must not** become a dumping ground.

| It holds | It does not hold |
| --- | --- |
| Current stage and next human gate | A restated premise, theme list, or logline |
| Approved vs pending vs derived vs temporary labels | Bible-length lore |
| Pointers into `canon/`, `characters/`, `world/`, `plot/`, `timeline/`, `research/` | Copies of those files |
| Lean flags: unpromoted facts, open questions, freeze risks | Chapter prose, research note bodies, act maps |
| Change log of operational updates | Git history as a substitute for live files |

**Length rule** ([`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §4): if a fact will still matter after the current act, promote it to the authoritative home, then leave a pointer.

**Conflict rule:** if project memory disagrees with the brief or approved canon, the brief or canon wins. Fix the dashboard.

---

## 6. Information kinds at the Book Core

Kinds match [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §3.2 and [`050_MEMORY_IMPLEMENTATION.md`](050_MEMORY_IMPLEMENTATION.md) §5. Book Core requires every operational statement to be distinguishable.

| Kind | Book Core home | Example (pedagogical, not live canon) |
| --- | --- | --- |
| **Approved** | Brief when Canon status is `Approved`; `Approved` files in canon/characters/world/timeline; accepted research **Constraint** lines; approved `book/plot/act-*.md` | Brief hard limit “No FTL.” |
| **Derived** | `PROJECT_MEMORY.md` indexes; approved `book/plot/ch-*.md` (source *for scene intent only*) | Memory row: “See `book/world/helios-station.md` — comms lag.” |
| **Pending** | Flags, `Proposal:` blocks, `Status: Draft` domain files, unanswered brief fields | “Promotion pending: sister mentioned in ch-01.” |
| **Temporary** | Cursor chat, unsaved buffers | “Let’s make the rival sympathetic” said only in chat |

Approved outlines are a special derived class: once the human approves them, they are source for scene intent and still lose to approved act-level plot on the turn itself.

Generated chapter prose is not approved canon. It is a claim ([`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §13).

---

## 7. Relationship to canon

Canon is the approved overlay of story-world truth ([`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §15, [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md) §25).

Filesystem ([`050_MEMORY_IMPLEMENTATION.md`](050_MEMORY_IMPLEMENTATION.md) §2):

| Fact class | Home |
| --- | --- |
| Cross-cutting glossary, threads, universe laws | `book/canon/` |
| Character truth | `book/characters/` |
| World, location, faction, system, setting rules | `book/world/` |
| Event chronology | `book/timeline/` |

**Book Core rules:**

1. The brief constrains what canon may become. A no-FTL brief forbids an FTL world file until the human amends the brief first.
2. The brief is not a character sheet, location file, or timeline. High-level setting and protagonist in the brief are intent. Detail is promoted into memory directories.
3. Project memory indexes canon. It does not store canon paragraphs.
4. Agents propose. Humans promote (§13, and [`050_MEMORY_IMPLEMENTATION.md`](050_MEMORY_IMPLEMENTATION.md) §6).
5. Prose never outranks approved canon until the human runs the canon-change protocol.

Conceptual paths such as `book/canon/characters/` in `020`/`030` resolve to `book/characters/` through [`050_MEMORY_IMPLEMENTATION.md`](050_MEMORY_IMPLEMENTATION.md) §2.2. Do not create a second nested copy.

---

## 8. Relationship to agent system

The kernel remains exactly five roles ([`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §3.1): Architect, Research, Outline, Writer, Review.

Book Core does not add a “Book Core agent.” Every kernel agent is a *client* of Book Core:

| Agent | Book Core duty |
| --- | --- |
| **Architect** | Serve the approved brief; list missing intent instead of inventing it; propose structure under `book/plot/` act-level files; lean-flag questions in project memory |
| **Research** | Obey brief hard limits; write notes under `book/research/`; never “research away” themes; never promote canon |
| **Outline** | Specialize approved architecture into scene contracts; refuse a span that needs a different brief or act turn |
| **Writer** | Draft under approved scene contracts, brief style/POV, and canon slices; flag new facts; never edit the brief or canon as truth |
| **Review** | Adjudicate artifacts against the brief and memory; file defects; never rewrite the brief or canon to make a draft succeed |

Role purity, one primary role per session, and never-collapse boundaries in [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §3.5 remain binding. Book Core adds the **context contract** in §12–§13 so those sessions start from the same files.

---

## 9. Relationship to book model

[`030_BOOK_MODEL.md`](030_BOOK_MODEL.md) names entities. Book Core says which live contract owns the *intent* instance versus the *canon* instance.

| Entity | Intent / index | Authoritative detail when approved |
| --- | --- | --- |
| Novel | `book/` tree | One brief, one working-memory dashboard, one accepted manuscript line |
| Premise, Theme, Style, Constraints | `BOOK_BRIEF.md` | Same file |
| Setting (author’s cut) | Brief §13 | `book/world/` |
| Main protagonist (concept) | Brief §14 | `book/characters/<slug>.md` |
| Main conflict / central question | Brief §§15–16 | `book/plot/` + threads |
| World, Locations, Factions, Rules | — | `book/world/`, `book/canon/` |
| Plot / Story arcs | — | `book/plot/act-*.md` then `ch-*.md` |
| Events / Timeline | — | `book/timeline/` |
| Research | — | `book/research/` |
| Drafts / Revisions | Project memory flags | Chapter files + git + reviews when those trees exist |

Agents reason about entities. They must not treat an empty brief protagonist field as a Character instance, and must not treat a chapter mention as a Character instance until promotion.

---

## 10. Relationship to workflow

Book Core sits at workflow stage **concept development** and then remains in force for every later stage ([`040_WORKFLOW.md`](040_WORKFLOW.md) §4.2 and §6).

| Workflow fact | Book Core enforcement |
| --- | --- |
| Brief before architecture approval | Hard. Architect may only pressure-test options while brief Canon status is not `Approved` |
| Human gates | Brief approval, architecture approval, outline-span approval, canon promotion, chapter accept, freeze, export |
| Side paths | Mid-draft research is allowed only if it writes back to `book/research/` and flags project memory |
| Revision | Defect class chooses the return path: Writer, Outline, Architect, or human canon/brief amendment |
| Isolation | Framework edits and novel-memory edits stay on separate commits ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §17.1) |

Kernel agents become available **according to gates**, not all at once after the author types a title. Initialization in §22 states the sequence.

---

## 11. Context loading

Context is assembled, not dumped ([`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §7, [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §22–§23, [`050_MEMORY_IMPLEMENTATION.md`](050_MEMORY_IMPLEMENTATION.md) §15–§16).

**Default Book Core load order for any kernel session that touches this novel:**

1. Applicable Cursor rules, when present.
2. The agent’s own contract set under `agents/<role>/`.
3. `book/BOOK_BRIEF.md` when the task concerns the book itself.
4. `book/PROJECT_MEMORY.md` when current project state matters (default: **yes** — flags and freeze risks).
5. Relevant memory directories for the span, resolved through [`050_MEMORY_IMPLEMENTATION.md`](050_MEMORY_IMPLEMENTATION.md) §2.2.
6. Stage inputs (plot, research, chapter, review) required by the role.
7. Optional `prompts/<role>/` playbooks when present. They do not override contracts.

**Do not load:**

- `examples/` as this book’s facts;
- the full manuscript for a single-chapter job;
- unrelated acts;
- previous chat transcripts as authority;
- Git history as a substitute for the live file.

**Compression stack:**

```text
Brief → act-level plot → scene contract → chapter
         Canon slices (characters, place, clock, threads)
         Research constraints for claims in span
         PROJECT_MEMORY.md (risks and unpromoted flags)
```

If a required approved input is missing or a required brief field is empty and the task needs it, **stop and name the path**. Do not infer a substitute.

---

## 12. Agent access

Access follows [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §8 and the permission matrix in [`050_MEMORY_IMPLEMENTATION.md`](050_MEMORY_IMPLEMENTATION.md) §9. Book Core restates only the interface rows.

| Location | Architect | Research | Outline | Writer | Review | Human |
| --- | --- | --- | --- | --- | --- | --- |
| `BOOK_BRIEF.md` | Read | Read | Read | Read | Read | **Write** (amend + set Canon status) |
| `PROJECT_MEMORY.md` | Read + lean flag | Read + lean flag | Read + lean flag | Read + lean flag | Read + lean flag | **Write** |
| Canon / characters / world / timeline | Read; propose only | Read; propose only | Read; scene-local flags | Read only | Read only | Promote / amend / deprecate |
| `book/plot/` act-level | **Write Draft** | Read | Read | Read | Read | Approve |
| `book/plot/` scene contracts | Read | Read | **Write Draft** | Read | Read | Approve |
| `book/research/` | Read | **Write** | Read | Read | Read | Accept Constraint language |

No kernel agent sets `Approved` on the brief, plot, canon, or chapters. No kernel agent silently patches `BOOK_BRIEF.md`.

---

## 13. Agent context contract

Every kernel agent, at the start of work, must:

1. **Read `BOOK_BRIEF.md`** when the task concerns the book itself.
2. **Read `PROJECT_MEMORY.md`** when current project state matters (treat as default).
3. **Read relevant memory directories** for the span (`canon/`, `characters/`, `world/`, `plot/`, `timeline/`, `research/`).
4. **Read relevant architecture documents** when changing framework behavior (this is a separate track from novel production).
5. **Never assume missing information.** Empty brief fields and empty directories are stop conditions for any claim that needs them.
6. **Never silently invent canon.** Flag instead.
7. **Clearly distinguish** assumptions from approved facts (label **Pending** / **Temporary** vs **Approved**).
8. **Ask for human approval** when a decision would change authoritative project information (brief, approved plot, canon, accepted Constraints, chapter Done).

If the session would have to invent a missing plot turn, POV policy, or story fact, escalate. Fail loudly.

### 13.1 Architect

**When:** after the human has entered concept; production-path work requires brief Canon status `Approved`.

**Must load:** this contract set; approved (or, at initialization, the current) `BOOK_BRIEF.md`; `PROJECT_MEMORY.md`; existing `book/plot/` act-level files for the span; existing research **Constraint** lines that would invalidate a turn; canon slices only if an amendment must respect them.

**Book Core behaviors:**

- Identify missing brief information and list it. Do not fill the brief.
- Propose project structure as `Draft` act-level files under `book/plot/` (conceptual `book/architecture/` resolves here).
- Pressure-test structural implications as **options** only while the brief is not `Approved`.
- Update project memory with focus, open design questions, and next gate — pointers, not a second act map.

**Must not:** silently rewrite the brief; mark architecture `Approved`; promote world/cast needs into canon; write scene contracts or chapters.

**Escalate when:** desired structure violates a brief hard limit; required intent fields are empty; two approved artifacts conflict.

### 13.2 Research

**When:** a named question list exists (from memory, Architect `K-*`/`Q-*` IDs, outline claims, Writer flags, Review, or the human). Soft dependency: not every scene needs Research ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §15.3).

**Must load:** brief hard limits and themes; `PROJECT_MEMORY.md` questions; existing `book/research/` notes for the topic; the slice of plot that created the question; existing canon that might collide.

**Book Core behaviors:**

- Treat brief limits as Constraints that must not be “researched away.”
- Write labeled notes (Source / Constraint / Conjecture / Rejected) with impacted filesystem paths.
- Flag promotion pending in project memory. Do not edit canon as truth.

**Must not:** override brief themes; promote findings; draft prose; relabel Conjecture as Constraint to save a beat.

**Escalate when:** evidence contradicts the brief; a load-bearing claim has no honest source or Conjecture label.

### 13.3 Outline

**When:** architecture for the span is `Approved`. Human must still approve the outline span before mass drafting.

**Must load:** approved brief; `PROJECT_MEMORY.md`; approved act-level `book/plot/` for the span; character, world, timeline, and thread slices; research Constraints for claims in span; neighboring scene contracts; reviews that forced re-outline.

**Book Core behaviors:**

- Specialize beats. Do not replace the act map or the brief.
- Declare in-state, out-state, threads, and claims. Flag scene-local new facts.
- Update project memory with span status and questions only.

**Must not:** write chapters; amend act turns in the scene file; treat Draft architecture as frozen law; invent a POV the brief forbids (if POV is still empty, stop).

**Escalate when:** a scene needs a different act turn or a brief amendment.

### 13.4 Writer

**When:** the outline span is `Approved`.

**Must load:** brief constraints and style/POV; `PROJECT_MEMORY.md`; approved scene contracts; on-page character sheets; location/world rules; timeline window; threads the scene may plant or pay; research Constraints for in-scene claims; neighbor chapters if they exist; latest review if this is a revision.

**Book Core behaviors:**

- Conform to the contract. Flag every new reusable fact in the chapter header and as a lean memory pointer.
- Treat unpromoted flags as **Pending**, not canon.
- Stop if the brief lacks a constraint the scene would have to guess (POV, content boundary, hard world limit).

**Must not:** edit `BOOK_BRIEF.md`; edit canon directories; set chapter `Approved`; self-issue `Pass`; change the contracted turn in prose alone.

**Escalate when:** the turn cannot be executed honestly; a retcon is required; two approved artifacts conflict.

### 13.5 Review

**When:** a draft (or named span) exists to adjudicate. Never in the same session that drafted it.

**Must load:** brief POV, theme, and constraints; `PROJECT_MEMORY.md` risks and unpromoted flags; scene contracts; the draft and diff if revising; on-page sheets; timeline neighbors; world file; threads; listed research notes; prior reports for the chapter.

**Book Core behaviors:**

- File defects with taxonomy and severity ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §18.3–§18.4).
- Treat unpromoted reusable facts as process `X` and, if they contradict canon, continuity `C`.
- Flag unresolved Blockers and freeze risks in project memory. Do not paste the report.

**Must not:** rewrite the brief, canon, or chapter as the review; approve on the human’s behalf; use Conjecture as a fact-check pass; outrank the brief or approved architecture. If those are wrong, file `S`/`T`/`X` and escalate.

**Escalate when:** two approved artifacts conflict; the “fix” would be a retcon; ethical or content-boundary questions appear.

---

## 14. Change proposals

Agents propose. Humans dispose.

A **change proposal** is required when an agent believes authoritative project information should change: brief intent, approved act-level plot, approved scene intent, approved canon, or accepted Constraint language.

**Proposal form (minimum):**

1. What should change (quote or path).
2. Why (defect ID, missing field, structural collision).
3. What stays in force until the human decides.
4. Suggested replacement text, clearly labeled `Proposal:` — not applied to the live brief.
5. Downstream cascade if accepted (which plot, timeline, or chapters would need impact review).

**Allowed proposal channels:**

- Chat, if the operator asked for options — **Temporary** until written back.
- `Proposal:` section on a `Status: Draft` domain file under `characters/`, `world/`, `timeline/`, or `plot/`.
- Lean pointer in `PROJECT_MEMORY.md`.
- `*.alt.md` or `exp/` branch for competing structure or prose.

**Forbidden:** patching `BOOK_BRIEF.md` “while here”; editing approved canon to match a convenient draft; treating a proposal as live truth in the next Writer session.

**Example (correct):** Architect finds the brief forbids a mid-novel POV shift that the desired Act II turn needs. It stops, names both sources, and proposes either amending Important constraints or keeping the single POV. It does not edit the brief.

**Example (incorrect):** Writer’s draft is stronger in first person. Writer changes brief Tone and Important constraints to match the draft.

---

## 15. Book Brief change protocol

Agents must never silently modify `BOOK_BRIEF.md`.

```text
1. Human edits BOOK_BRIEF.md directly.
   — or —
2. Agent identifies a possible required change.
3. Agent proposes the change (§14). Does not patch the brief.
4. Human accepts or rejects it.
5. If accepted, the human edits BOOK_BRIEF.md.
6. Git records the change.
7. Agents use the new version (the live file). Downstream artifacts are invalid until impact review.
```

**After a brief amendment:**

- If Canon status remains `Approved`, treat the new text as binding immediately.
- If the change invalidates architecture, outlines, or chapters, fail loudly on those dependents until cascade ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §15.4).
- Update `PROJECT_MEMORY.md` with a pointer and the next gate — not a copy of the new premise.
- Commit the brief change as its own logical unit when possible (`Brief: …`).

**Rejection:** leave the brief unchanged. Record **Rejected** in architecture or research if an option must not be revived. Clear the pending flag.

**Empty-field rule:** proposing a value for a blank field is allowed. Writing that value into the brief is not allowed until the human accepts and the human (or human-directed accepted edit) enters it.

---

## 16. Project Memory update protocol

`PROJECT_MEMORY.md` is an index. Only important operational state belongs there.

### 16.1 Who may update it

| Actor | May write | Circumstances |
| --- | --- | --- |
| **Human** | Any section | Decisions, stage changes, warnings, log lines treated as operational truth |
| **Architect** | Lean flags only | Current focus, open design questions, next gate after an Architect run |
| **Research** | Lean flags only | Question pointers, constraint risks, promotion pending |
| **Outline** | Lean flags only | Span status, questions Writer must not invent away, next gate |
| **Writer** | Lean flags only | Unpromoted facts, leftover threads, next gate = Review |
| **Review** | Lean flags only | Unresolved Blocker/Major IDs, freeze risks, next gate |

No agent performs a “cleanup rewrite” of the whole file. No agent pastes canon, research notes, or chapter summaries.

### 16.2 What to add

- A pointer (`See book/world/hatch-17.md`).
- A flag (`Unpromoted: …` / `Stale canon: …` / `Promotion pending: …`).
- A gate (`Next: human approve ch-07 outline span`).
- A log row when operational state actually changed.

### 16.3 What not to add

- Restated brief sections.
- Full character biographies.
- Act maps.
- Chat transcripts.
- Temporary brainstorming that was not selected.

### 16.4 After promotion or resolution

Remove the duplicate wording. Leave a pointer if useful. Add a change-log row. The source file remains the home.

**Example:** Writer flags `Unpromoted: glove liner is heated`. Human strikes it from prose. Architect/Writer session clears the flag and logs “flag cleared; not promoted.” No world file is created.

---

## 17. Human approval

Human approval at Book Core matches [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §13, [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §26, and [`040_WORKFLOW.md`](040_WORKFLOW.md) §6.

| Gate | Human accepts | Until then |
| --- | --- | --- |
| Brief for production | Concept fields the author considers sufficient; Canon status `Approved` | No mass architecture or drafting |
| Architectural direction | Act map, major turns, thematic spine | Outline must not treat the span as binding |
| Outline span | Scene contracts for chapters about to be written | Writer refuses those chapters |
| Canon promotion / retcon | Story-world truth | Agents only propose and flag |
| Chapter accept / revise / reject | Whether the reviewed draft enters the accepted line | Writer and Review must not set `Approved` |
| Act or manuscript freeze | Span closed to casual edits | Publisher/export waits |
| Brief amendment | New intent text in `BOOK_BRIEF.md` | Agents keep using the previous live file |

The human may reject a Review `Pass` on taste. The human may edit any file directly. Direct human edits still need git and, if they change truth, a cascade.

---

## 18. Canon promotion

Canon promotion is the protocol in [`050_MEMORY_IMPLEMENTATION.md`](050_MEMORY_IMPLEMENTATION.md) §6. Book Core places it on the control flow after agents and before later sessions treat a fact as true.

Mandatory order:

1. Agent identifies a reusable candidate fact.
2. Agent records its source (research path, chapter header, architecture question, author stipulation). Invented citations are forbidden.
3. Agent marks uncertainty (Conjecture, range, `Uncertainty:`).
4. Agent proposes: producing-artifact flag + lean `PROJECT_MEMORY.md` pointer; optional `Proposal:` on a `Status: Draft` domain file. **Do not** write `book/canon/` until the human approves.
5. Human reviews against the brief and existing canon.
6. Human approves, rejects, or defers.
7. Only after approval does the human write present-tense truth into exactly one home and set `Status: Approved`.
8. Git records the promotion (`Canon: …`).

Research can create findings. Research cannot perform step 7. Writer cannot create character files as a side effect of a mention.

Intent-class facts (POV, rating, no-FTL as a **project** rule) promote into `BOOK_BRIEF.md` via the brief change protocol, not into `book/canon/`.

---

## 19. Revision handling

Revision is a workflow loop ([`040_WORKFLOW.md`](040_WORKFLOW.md) §4.15, [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md) §27), not a pile of `final-final` files.

| Defect class | Return path | Book Core write |
| --- | --- | --- |
| Prose / local craft (`R`, many `P`/`H`) | Writer on the same chapter path | Lean memory flag if facts changed |
| Scene intent (`S` at scene scale) | Outline amends `book/plot/ch-*.md`, then Writer | Span status in project memory |
| Act turn / spine (`S` at act scale) | Human confirms; Architect amends `book/plot/act-*.md`; cascade | Next gate = re-approve architecture |
| Continuity vs canon (`C`) | Revise prose **or** human retcon protocol | Never “fix” canon in Review |
| Process (`X`) unpromoted fact | Human promote/strike/defer | Clear or replace the flag |
| Theme vs brief (`T`) | Human amends brief **or** prose/outline restores the brief | Brief protocol if intent changes |

One live path per chapter number. Git holds previous wording. `*.alt.md` remains a candidate until the human replaces the live file.

If revision needs a brief change, stop the production loop and run §15 before rewriting dependents.

---

## 20. Git versioning

Git is the novel’s black box recorder ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §10, [`040_WORKFLOW.md`](040_WORKFLOW.md) §7, [`050_MEMORY_IMPLEMENTATION.md`](050_MEMORY_IMPLEMENTATION.md) §13–§14).

Book Core rules:

- The **live files** under `book/` are current memory. Agents must not treat `git log` as the brief or as canon.
- Git **may** be used to investigate how a decision changed. That investigation informs a human edit to the live file.
- Brief changes, canon promotions, plot amendments, and chapter drafts are separate logical commits when possible.
- Branches hold variants (`exp/plot-…`, `draft/ch-NN-…`). After merge, `book/` states the chosen truth. Branches are not memory.
- Do not version by renaming (`BOOK_BRIEF-final.md`, `PROJECT_MEMORY-copy.md`). One live path plus git.
- Framework-track commits (this document) stay isolated from novel-track commits (brief text, character files).

Agents always use the current working-tree / HEAD version of `BOOK_BRIEF.md` and `PROJECT_MEMORY.md` after the human has saved.

---

## 21. Recovery

Recovery follows [`040_WORKFLOW.md`](040_WORKFLOW.md) §9. Book Core special cases:

| Mistake | Recovery |
| --- | --- |
| Agent rewrote `BOOK_BRIEF.md` without human request | Revert that path from git. Treat the agent text as never approved. Log a warning in project memory |
| Concept existed only in chat | Treat as never happened, or the human writes it into the brief the same day |
| Project memory duplicated the brief | Delete the blob; leave a pointer. Brief remains source |
| Invented genre/POV because fields were empty | Strike the invention from plot/prose. Leave the brief empty until the human fills it |
| Canon fork | Fail loudly; human picks one home; deprecate the other with `Superseded-by:` |
| Wrong plot branch merged | Revert; restore plot/canon to intended truth; Review the span |
| Session died mid-flag | Resume the same files; prefer last good git state |

**Always recoverable if** small commits exist and intent was in the brief. **Not recoverable by agents alone:** unpublished intent the human never wrote down.

---

## 22. Project initialization

A new novel project begins in this repository’s `book/` tree. Do not copy `examples/` into `book/` as if it were canon ([`040_WORKFLOW.md`](040_WORKFLOW.md) §4.1).

**Minimum process:**

1. **Human opens** `book/BOOK_BRIEF.md`.
2. **Human enters** the initial concept into the sections they know. Unknown sections stay empty.
3. **Human saves** the document (and should commit it).
4. **Architect reads** the brief and `PROJECT_MEMORY.md`.
5. **Architect identifies** missing information and lists it (open questions). Architect does not fill the brief.
6. **Architect proposes** project structure as `Draft` act-level files under `book/plot/` once the brief is `Approved` for production. If the brief is not yet `Approved`, Architect may only pressure-test options and must stop before a binding act map.
7. **Human approves** architectural direction for the novel or named span (sets plot `Approved`). Human also sets brief Canon status to `Approved` when intent is locked—this may precede step 6.
8. **Research, Outline, Writer, and Review become available according to workflow gates** — not all at once. Research when questions need grounding; Outline after approved architecture; Writer after approved outline span; Review after a draft exists.

**Initialization validation:**

- `BOOK_BRIEF.md` and `PROJECT_MEMORY.md` exist.
- Memory directories exist (may be empty).
- No example-novel facts were imported.
- Project memory lists the next human gate and does not contain a fake premise.

**Worked initialization (pedagogical names; not live canon):**

The author writes a premise and “No FTL; close-third on the salvage engineer” into the brief, sets Canon status `Approved`, and commits `Brief: lock Helios Wake constraints`. Architect reads the brief, lists `Q-01 What still has power?`, writes `book/plot/act-01.md` as Draft, and flags the next gate. Human approves architecture. Research may now write `book/research/` notes. Outline waits for that approval. Writer is still red.

---

## 23. Project completion

Completion is a human freeze and export decision ([`040_WORKFLOW.md`](040_WORKFLOW.md) §4.16–§4.18). Book Core does not auto-publish.

| Step | Book Core state |
| --- | --- |
| Act freeze | Human records freeze in project memory; optional git tag (`act-1-frozen`). Files stay in place |
| Manuscript assembly | Reads accepted/frozen chapters only. Must not rewrite plot or canon. Must not export `Author secret:` / author-notes that the human marked private |
| Export / publication | Human-only gate |
| Brief after completion | Remains the intent record. Further changes are a new edition: brief protocol + cascade |
| Memory after completion | Do not delete canon to “clean up.” Deprecate if a later edition retcons. Project memory’s last milestone becomes the freeze/export the human accepted |
| Agents after freeze | Casual Writer/Architect edits are closed until the human unfreezes in project memory and git |

A novel is complete for framework purposes when: brief still describes the book that was made; accepted chapters exist; Review + human accept have closed the production path the author intended to ship; freeze is recorded; export (if any) was a human decision.

---

## 24. Worked examples

Illustrative only. Names are pedagogical fixtures, not live canon. Same through-line style as [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §20.

### 24.1 Correct Book Core path

1. Human fills premise and constraints in `BOOK_BRIEF.md`; Canon status `Approved`.
2. `PROJECT_MEMORY.md` next gate: approve architecture. No duplicated premise paragraph.
3. Architect writes `book/plot/act-01.md` Draft; flags `Q-01`.
4. Human approves architecture.
5. Research writes `book/research/comms-lag.md`; memory: `Promotion pending → book/world/helios-station.md`.
6. Human promotes the lag sentence into `book/world/helios-station.md`.
7. Outline writes `book/plot/ch-01.md`; human approves the span.
8. Writer drafts the chapter; flags an unpromoted sister; does not create `book/characters/kira.md`.
9. Human rejects the sister. Writer removes the mention. Flag cleared.
10. Review files a pacing Minor; verdict `Pass-with-fixes`. Writer repairs. Re-review `Pass`. Human accepts.

No agent edited the brief. No agent treated chat as canon.

### 24.2 Incorrect Book Core path

Chat agrees the book is “darker.” Outline writes a betrayal as the Act I turn. Writer drafts it. Review “fixes” the world file so the station has FTL to help the ending. The brief still says no FTL and never mentioned betrayal-in-Act-I.

Defects: silent brief drift (`X`); architecture outranked (`S`); illegal canon write by Review; project memory never flagged a proposal. Recovery: revert world and prose; restore brief; Architect/Outline only after human intent is explicit in `BOOK_BRIEF.md`.

### 24.3 Empty field

Brief §6 Language is empty. Export tooling must not assume English. Writer must not invent a constructed language and treat it as approved. Stop and ask, or write English only if the human has already been writing the brief itself in English *and* the human confirms Language—still a human edit to §6, not an agent patch.

---

## 25. Acceptance and validation criteria

### 25.1 This document is complete when

- Control flow from human author through brief, project memory, book memory, agents, artifacts, review, and human approval is specified.
- Brief authority, project-memory index role, information kinds, and relations to canon, agents, book model, and workflow are specified.
- Context loading, agent access, and per-role context contracts exist for all five kernel agents.
- Brief change protocol, memory update protocol, promotion, revision, git, recovery, initialization, and completion are specified with examples.
- No TODOs, no story content presented as this book’s canon, no second memory filesystem.

### 25.2 Book Core is correctly implemented when

| Check | Pass condition |
| --- | --- |
| Brief | `BOOK_BRIEF.md` is marked author-owned, human-editable, authoritative; agents do not silently rewrite it |
| Empty intent | Unfilled brief fields are empty; no invented title, cast, or plot in the live brief |
| Memory index | `PROJECT_MEMORY.md` points; it does not duplicate the brief |
| Kinds | Approved / derived / pending / temporary are distinguishable |
| Five agents | Architect, Research, Outline, Writer, Review each have a defined Book Core context relationship |
| Gates | Production-path drafting cannot start on an unapproved empty brief |
| Promotion | New facts are flags until the human writes the home file |
| Git | Live files are current state; history is investigation |
| Resume | An operator returning after weeks can reconstruct state from `book/` without chat logs |

---

## 26. References

- [`../specifications/000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md) — human authorship, artifacts over chat, fail loudly
- [`../specifications/001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) — authority stack, memory tiers, agent topology, git
- [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) — roles, lifecycle, gates, isolation
- [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) — fact classes, promotion, retrieval
- [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md) — entities the brief and memory instantiate
- [`040_WORKFLOW.md`](040_WORKFLOW.md) — stages, loops, initialization, freeze, export
- [`050_MEMORY_IMPLEMENTATION.md`](050_MEMORY_IMPLEMENTATION.md) — filesystem homes and permission matrix
- [`../../book/BOOK_BRIEF.md`](../../book/BOOK_BRIEF.md) — live intent contract
- [`../../book/PROJECT_MEMORY.md`](../../book/PROJECT_MEMORY.md) — live operational index
- [`../../WORKFLOW.md`](../../WORKFLOW.md) — operator backbone
- [`../../agents/architect/SYSTEM.md`](../../agents/architect/SYSTEM.md)
- [`../../agents/research/SYSTEM.md`](../../agents/research/SYSTEM.md)
- [`../../agents/outline/SYSTEM.md`](../../agents/outline/SYSTEM.md)
- [`../../agents/writer/SYSTEM.md`](../../agents/writer/SYSTEM.md)
- [`../../agents/review/SYSTEM.md`](../../agents/review/SYSTEM.md)

---

## Document Control

| Field | Value |
| --- | --- |
| ID | `060_BOOK_CORE` |
| Title | Book Core — AI Book Framework |
| Layer | Subsystem architecture |
| Depends on | `000_PROJECT_VISION`, `001_ARCHITECTURE` |
| Enables | Author-facing brief/memory operation, kernel session context assembly, later Cursor rules for brief/memory gates |
| Maintenance rule | Update when brief schema, working-memory index rules, agent context contracts, or promotion/approval protocols change; remain consistent with `020`, `050`, and the five `SYSTEM.md` contracts |

---

*End of document.*
