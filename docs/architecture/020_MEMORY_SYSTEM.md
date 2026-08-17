# 020 — Memory System

**Document type:** Subsystem architecture  
**Status:** Canonical for this subsystem; subordinate to `001_ARCHITECTURE`  
**Audience:** Framework maintainers, agent-contract authors, and operators who maintain book continuity  
**Depends on:** [`../specifications/000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md), [`../specifications/001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md)  
**Companions:** [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md), [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md), [`040_WORKFLOW.md`](040_WORKFLOW.md)  
**Scope:** Knowledge and memory architecture for a novel project. Framework instructional docs (`docs/`, `prompts/`, `templates/`) are the Knowledge OS, not this book’s memory.

---

## 1. Purpose

The Memory System is the durable state of the novel: what is true, what was promised to the reader, what was decided, what remains open, and what is only a draft.

Naive AI usage loses this state at the end of a chat. The framework prevents that loss by treating continuity as infrastructure. If a fact must be true in chapter 30, it must live in a file that chapter 30’s agents can read.

This subsystem exists to:

- Give every fact class exactly one authoritative home.
- Distinguish source documents, derived information, temporary context, and generated content.
- Define how agents read, propose, and update memory.
- Make retcons explicit, reviewable, and cascaded.
- Remain fully usable as markdown in git, with optional future indexes (databases, vector stores) that never become a second source of truth.

The memory *tiers* in [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §7 are binding. This document specifies principles, lifecycle, retrieval, conflict handling, and examples.

---

## 2. Memory principles

1. **One source of truth per fact class.** Duplication is a defect unless the copy is clearly derived and points at the source.
2. **Write for the next session.** Assume the next human or agent has no chat memory.
3. **Prose is not canon.** A sentence in a chapter is a claim. Canon is the approved registry of reusable truth.
4. **Promote deliberately.** New facts start as flags. They become canon only through the update protocol in §19–§26.
5. **Keep working memory lean.** `book/PROJECT_MEMORY.md` is a dashboard, not a bible.
6. **Prefer deprecation to silent deletion** when a fact was true in draft history and is now superseded.
7. **Fail loudly** on contradiction. Do not reconcile in stylish prose.
8. **Human promotion.** Agents propose; humans dispose for canon, brief, and other authority-tier changes ([`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §8 and §13).
9. **Derived caches are disposable.** Summaries, embeddings, and search indexes must be rebuildable from files.
10. **Series memory is the same tiers**, later rooted in a shared canon module; volume memory must not fork series truth.

---

## 3. Single Source of Truth

### 3.1 Authority by class

| Fact class | Authoritative store | Losing stores |
| --- | --- | --- |
| Intent (premise, audience, tone, POV, hard constraints) | `book/BOOK_BRIEF.md` when `Approved` | Chat, outlines that “feel darker,” style riffs |
| Current operational state | `book/PROJECT_MEMORY.md` | Old session summaries, stale review asides |
| Story-world truth | Approved files under `book/canon/` | Chapter prose, research hypotheses, examples |
| Grounding constraints | `book/research/*` items marked **Constraint** | Model conjecture, unsourced chat |
| Act-level structure | Approved `book/architecture/*` | Outline improvisation, Writer “improvements” |
| Scene intent | Approved `book/outline/*` for that span | Writer’s unrecorded change of goal/turn |
| Prose wording | Current chapter file on the accepted or active draft path | Review paraphrase, chat rewrite not applied |
| Process law | `docs/specifications/` (`000`, `001`, then later specs) | Guides, prompts, this folder if they ever conflict with `001` |
| Historical wording | Git history | Memory files that forgot a date |

If two durable documents conflict, follow [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §5.3: identify class, apply the authority stack, prefer the more specific approved artifact in the same tier, otherwise fail loudly and wait for a human edit to the winner.

### 3.2 Four information kinds

| Kind | Definition | Examples | Authority |
| --- | --- | --- | --- |
| **Source documents** | Human-committed contracts and registries meant to be true | Approved brief, approved canon sheets, research **Constraint** lines, approved architecture | High, per class above |
| **Derived information** | Compressions or indexes of sources | `PROJECT_MEMORY.md` focus list, outline (derived from architecture + canon), review reports, future search embeddings | Valid only while they match sources; outlines become source for *scene intent* once approved |
| **Temporary context** | Session-only material | Cursor chat, unsaved buffers, “notes to self” not in-repo | Zero after the session unless written back |
| **Generated content** | Model or human prose produced under contracts | Chapter drafts, candidate `*.alt.md` scenes, proposed canon paragraphs | Not canon; wording authority only for the active draft file |

Approved outlines are a special derived class: they are derived from architecture, then **promoted to source for scene intent**. They never outrank approved architecture on act turns.

---

## 4. Project memory

**Home:** `book/PROJECT_MEMORY.md`  
**Tier:** B — Working memory  
**Question it answers:** *Where are we right now?*

Project memory holds only high-signal operational state:

- Current act and chapter focus
- Active conflicts and stakes in play
- Recent decisions (pointers to the files that now own them, not a second copy of the decision)
- Open questions and unpromoted fact flags
- Risks (continuity, schedule, research gaps)
- Next human gate

**Length rule:** if a fact will still matter after the current act, it belongs in canon, brief, architecture, or outline—not here. Promote, then replace the blob with a pointer.

**Example entry:**

```text
Focus: ch-07 draft, Helios inner ring
Open: unpromoted fact — Aya’s glove liner is heated (flagged in ch-07 header)
Risk: timeline row “Station Arrival” disagrees with ch-06 clock (Review C-014)
Next gate: human accept/reject ch-07 after Review pass
```

---

## 5. Persistent knowledge

Persistent knowledge is everything that must survive session death.

| Persistence class | Location | Survives |
| --- | --- | --- |
| Framework knowledge (how to run the OS) | `docs/`, `prompts/`, `templates/`, `USER_GUIDE.md` | All novels |
| Book intent | `book/BOOK_BRIEF.md` | This novel |
| Book working state | `book/PROJECT_MEMORY.md` | This novel, until edited |
| Book canon | `book/canon/` | This novel (or series root, later) |
| Research archive | `book/research/` | This novel |
| Production record | outlines, chapters, reviews, git | This novel |

Framework knowledge is **not** this book’s canon. Agents must not copy example facts from `examples/` into live memory. See [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §6.

---

## 6. World knowledge

**Home (target):** `book/canon/world/`  
**Entity mapping:** [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md) §7–§14  
**Question:** *What is true about the story world?*

World knowledge includes setting frame, locations, factions, cultures, technology, and rules (including magic or science systems when the genre uses them).

**Authoritative form:** present-tense canon statements in dedicated files, for example `book/canon/world/helios-station.md`.

**Not authoritative:** a Writer’s atmospheric aside that “the station has always had gravity plating” if the world file says spin gravity only.

**Write path:** Research proposes constraints → Architect states what plot needs → human promotes into world files → Outline and Writer read slices.

---

## 7. Character knowledge

**Home (target):** `book/canon/characters/`  
**Stable ID:** filename slug (for example `aya-okoro.md`) even if the display name changes; aliases live inside the sheet.

A character sheet is the source for: identity, role in the plot, voice notes, known-vs-secret knowledge, capabilities, relationships pointers, and life-state relevant to the current volume.

**Who-knows-what is canon**, not a vibe. If Aya does not know the core is cold, prose that has her planning around a cold core is a continuity defect unless a scene taught her.

Relationships may be summarized on sheets and detailed in a relationship section or file; the sheet remains the index. See [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md) §9 and §21.

---

## 8. Timeline knowledge

**Home (target):** `book/canon/timeline.md`  
**Question:** *When do events occur relative to each other?*

Timeline knowledge is a chronology of **events** (story-world occurrences), not a list of files. Chapters and scenes *realize* events; they do not replace the clock.

Each timeline row should identify: event ID, in-world time or sequence, location, characters present, pointer to the scene/chapter that depicts it (if any), and status (`Planned`, `Drafted`, `Accepted`).

**Clock rule:** if chapter 6 says “three hours after docking” and the timeline says docking is Day 12 04:00, chapter 6’s clock must match or the timeline must be amended first.

---

## 9. Plot knowledge

**Homes:**

- Act-level plot: approved `book/architecture/*`
- Scene-level plot: approved `book/outline/*`
- Unpaid reader promises and mysteries: `book/canon/threads.md`

Plot knowledge is **intent and structure**, not wording. The Writer may change sentences. The Writer may not change the contracted turn without an outline (and possibly architecture) amendment.

**Threads** record foreshadowing, clues, prophecies, and capabilities promised to the reader. An unpaid thread at freeze is a defect.

---

## 10. Research knowledge

**Home:** `book/research/`  
**Question:** *What grounding constrains invention?*

Every research note must distinguish:

| Label | Meaning | May constrain prose? |
| --- | --- | --- |
| **Source** | External or author-provided reference | Only after distilled into **Constraint** or canon |
| **Constraint** | Rule the story must not violate | Yes |
| **Conjecture** | Model or author hypothesis | No, until promoted |
| **Rejected** | Considered and refused | No; kept so agents do not revive it blindly |

Research is not canon. A constraint may be *binding on invention* while still needing a canon sentence for story-world facts (“Helios’s main reactor is cold” belongs in the world file once the human accepts it).

Research outputs must list **impacted canon files**. Orphan notes that never name an impact are a process defect.

---

## 11. Style knowledge

**Homes, in order:**

1. Voice, POV, and diction constraints in `book/BOOK_BRIEF.md`
2. Optional book-level style notes the author keeps under `book/` (when a later template exists)
3. Optional style pack under `modules/style-packs/` — outranked by the brief
4. Review anti-slop stance in [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §18.5 unless a pack explicitly allows a mode

Style knowledge is **policy**, not a catalog of pretty sentences. Do not store “sample paragraphs to imitate” as if they were canon events.

---

## 12. Decisions

A **decision** is a committed choice that future sessions must not reopen casually.

| Decision type | Where it is recorded |
| --- | --- |
| Intent decision | Brief amendment |
| Structural decision | Architecture file |
| Scene-intent decision | Outline file |
| Canon decision | Canon file + optional one-line pointer in project memory |
| Process/framework decision | `docs/decisions/` ADR (framework track only) |
| Rejected alternative | Architecture or research “Rejected” section, or git branch not merged |

Chat agreement (“let’s make the rival sympathetic”) is not a decision until a file says so.

---

## 13. Facts

A **fact** is a reusable, testable statement about the story world, the timeline, or a character’s state.

Facts live in canon (or in research **Constraint** until promoted). They are written in the present tense for current truth.

**Non-facts:**

- A metaphor in prose
- A lie a character tells (record as *uttered lie* plus *actual truth* on the sheet or in secrets)
- A hypothesis in a research note

**Example:**

- Fact (canon): `Helios Station uses spin gravity. There is no gravity plating.`
- Non-fact until flagged/promoted: chapter line “The plating hummed under her boots.”

---

## 14. Constraints

Constraints are binding limits. They come from:

- Brief hard limits (POV, rating, themes to avoid, no-FTL, content boundaries)
- Research **Constraint** lines
- Approved world rules (what the science/magic system can do)
- Legal/ethical research hygiene ([`000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md) §16)

Constraints outrank a Writer’s desire for a spectacular beat. To violate a constraint, the human amends the brief, research, or canon first, then cascades.

---

## 15. Canon

**Canon** is the approved set of story-world truths: world, characters, timeline, glossary, and threads.

**Promotion rule:** nothing is canon because it was generated. It is canon because a human set the artifact to an approved state (or explicitly merged a proposal into an approved file).

**Prose vs canon:** if they disagree, canon wins until the human runs the canon-change protocol (§25).

**Glossary** (`book/canon/glossary.md`) is the source for spellings, neologisms, and consistent names. Chapters that invent a new term must flag it.

---

## 16. Draft information

Draft information is production state: outlines in `Draft`, chapters not yet accepted, review reports in progress, `*.alt.md` candidates.

| Draft object | Memory status |
| --- | --- |
| Draft outline | Not binding for mass Writer work |
| Draft chapter | Wording exists; facts inside are claims |
| Draft canon proposal | Not readable as truth by Writer/Review except as a flagged proposal |
| WIP header `Status: Draft` | Agents must not treat as frozen |

Git commits of drafts are history, not approval.

---

## 17. Temporary context

Temporary context includes Cursor chat, unrecalled tool traces, and operator memory.

**Rule:** if it is not in the repository, it is not yet real for the system ([`000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md) §13.1).

Agents may use chat to think. They must write back:

- decisions
- new reusable facts (as flags)
- defects
- gate status changes

At session close, temporary context is discarded. The next session reloads files (§23).

---

## 18. Memory lifecycle

```text
Create (Draft) → Review / human check → Approved
                    ↘ Rejected / not used
Approved → (in use) → Amended (with cascade)
                    → Deprecated (superseded-by pointer)
                    → Frozen (act or manuscript freeze)
```

Recommended headers on major artifacts ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §5.4):

- `Status:` Draft | Review | Approved | Deprecated
- `Owner:` human role
- `Stage:` workflow stage
- `Last-reviewed:` date or commit

**Deletion** is rare. Deprecated facts stay findable so agents do not resurrect them from an old chapter without seeing the supersession.

---

## 19. Updating memory

### 19.1 Who may write what

The access matrix in [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §8 is binding. Summary:

- **Writer** updates chapter files and may append flags to project memory. Writer does not edit canon as truth.
- **Review** writes review reports and may flag memory risks. Review does not “fix” canon to match a draft.
- **Architect / Outline / Research** write their production or research trees and may propose canon.
- **Human** approves brief, architecture, outline spans, chapters, and all canon promotions.

### 19.2 Update procedure (any new reusable fact)

1. Detect the fact in draft, research, or discussion.
2. Flag it in the producing artifact header and in `PROJECT_MEMORY.md`.
3. Human chooses: **promote**, **strike from prose**, or **defer** (flag remains).
4. If promote: edit the single authoritative file; set status; remove the duplicate wording from project memory, leaving a pointer if useful.
5. Cascade: timeline, threads, dependent outlines, and already-drafted scenes that touch the fact.
6. Commit as its own logical unit when possible (`Canon: …`).

### 19.3 Example

Writer drafts: “Aya’s sister taught her to read pressure gauges.”  
Flag: `Unpromoted: sister exists; taught gauges.`  
Human: no sister in this book.  
Update: prose revised; flag cleared; no `kira-*.md` file created.  
If human had said yes: create `book/canon/characters/` sheet, add relationship on Aya’s sheet, add timeline childhood event if needed, then keep or adjust the sentence.

---

## 20. Conflict resolution

Memory conflicts are a subclass of document conflicts.

| Pattern | Winner | Action |
| --- | --- | --- |
| Chapter vs approved canon | Canon | Revise prose or run §25 if the author wants the chapter |
| Outline vs approved architecture (turn/spine) | Architecture | Amend outline or human amends architecture first |
| Two canon files disagree | Fail loudly | Human merges into one home; deprecate the other |
| Research Constraint vs canon | Fail loudly | Human updates the losing file; do not hide in prose |
| Project memory vs canon | Canon | Fix the dashboard pointer |
| Guide vs specification | Specification | Framework track; not book canon |
| Example vs live book | Live book | Never import example facts |

Do not average facts (“maybe the reactor is half-cold”).

---

## 21. Versioning

Versioning is **git-first**.

- Commits record memory evolution.
- Branches hold variants; after merge, memory states the chosen truth ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §10.2).
- Tags may mark `act-1-frozen` or submission milestones.
- In-file `Status` and `Last-reviewed` record editorial version, not a parallel version database.
- Deprecated entries use `Superseded-by:` path or ID.

Do not keep `canon/timeline-final-final.md`. One `timeline.md`, plus git.

Future structured snapshots (JSON exports) are derived from these files and must include a commit hash.

---

## 22. Retrieval

Retrieval is **named-file and sliced**, not “load the novel.”

**Minimum retrieval before outlining or drafting a scene** ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §7.4):

1. Brief constraints
2. Relevant character sheets
3. Timeline slice for the scene’s clock
4. Open threads the scene might plant, pay, or contradict
5. Latest review constraints for that chapter or act
6. World/location/rules files the scene occupies
7. Research constraints for any technical or historical claim the scene will make

**Search practice (v1):** operator `@`-references paths; grep/headings inside canon files; glossary for names.

**Anti-pattern:** stuffing all chapters into context “so nothing is forgotten.” That both overflows the window and treats prose as canon.

---

## 23. Context assembly

Context assembly is the runtime application of retrieval. Agent order of load is in [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §7.

**Compression stack (always prefer the higher, smaller layer unless the task is a local prose pass):**

```text
Brief → Architecture → Outline → Chapter
         Canon slices (characters, place, clock, threads)
         Research constraints for claims in span
```

**Assembly recipe example (Review of ch-07):**

- Brief POV and constraint sections
- Outline contracts for ch-07
- `ch-07` draft and diff against last acceptable commit if revising
- Sheets for characters on-page
- Timeline rows spanning ch-06 through ch-08
- World file for the location
- Threads touched by the chapter
- Research notes listed in the outline’s “claims” section

`PROJECT_MEMORY.md` goes in every assembly because it names current risks and unpromoted flags.

---

## 24. Stale information

Information is stale when a newer approved artifact contradicts it, `Last-reviewed` is older than a dependent change, or project memory still lists a resolved item.

| Symptom | Response |
| --- | --- |
| Project memory repeats a canon paragraph | Replace with a pointer |
| Character sheet ignores an accepted chapter event | Update life-state; or file `C` defect |
| Timeline not updated after a drafted scene moved the clock | Blocker until timeline matches or prose reverts |
| Research Constraint superseded by a brief change | Mark research `Rejected` or amend constraint; cascade |
| Review report refers to a paragraph already cut | Close the defect as addressed; do not keep it live |

**Staleness check at act freeze:** every accepted chapter’s reusable facts exist in canon; threads planted in the act are logged; project memory no longer holds act-permanent facts.

---

## 25. Canon changes

A canon change is any amendment to approved story-world truth, including retcons.

**Protocol:**

1. Human states the change and the reason (taste, defect, new design).
2. Identify the single authoritative file (or split if two classes are involved, e.g. character + timeline).
3. Edit that file. If the old fact must remain discoverable, mark it `Deprecated` with `Superseded-by`.
4. Update dependents: architecture if plot logic changes; outlines if scene intent changes; chapters that already asserted the old fact; threads; glossary.
5. Run Review continuity on the affected span.
6. Record a git commit focused on the retcon (`Canon: …`).
7. Clear the item from project memory or replace with “retcon applied, continuity pass pending.”

Agents must not perform steps 2–4 as a silent side effect of a Writer session.

---

## 26. Human approval

Human approval is required to:

- Set brief, architecture, and outline spans to `Approved`
- Accept a chapter into the accepted line
- Promote research into canon
- Change canon (including retcons)
- Freeze an act or manuscript
- Discard a previously approved fact (deprecation)

Agents may fill a proposed canon paragraph and leave `Status: Draft` or a `Proposal:` section. Until the human promotes it, Writer and Review treat it as untrue for continuity—except Review may say “this draft assumes the proposal; do not accept the chapter until canon is decided.”

This matches the gates in [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §13 and [`040_WORKFLOW.md`](040_WORKFLOW.md).

---

## 27. Future database and vector-store integration

The core OS has **no mandatory database**. Markdown plus git is sufficient ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §2.1, §11.5).

If a later module adds a database or vector store:

| Rule | Requirement |
| --- | --- |
| Source of truth | Files under `book/` (and series canon root) remain authoritative |
| Index role | Search aid, similarity retrieval, “which sheets mention Helios” |
| Rebuild | Indexes must be regenerable from files + commit hash |
| Writes | Tools may suggest patches to files; they must not update embeddings as if that were a canon edit |
| Secrets | Unpublished manuscripts stay local-friendly; external hosted indexes are an operator choice with privacy cost |
| Agents | Retrieval still returns file paths and spans, not citation-free blobs |
| Failure | If index and file disagree, the file wins and the index is stale (§24) |

Vector retrieval may **assist** §22. It must not replace named-file retrieval of brief, outline, and the scene’s canon slices.

---

## 28. Worked examples

### 28.1 Correct promotion

Research note: `Constraint: No FTL. Helios-to-Earth comms lag is at least 4 minutes at this plot distance.`  
Human copies the lag number into `book/canon/world/helios-station.md`.  
Outline for the argument scene requires a 4-minute wait.  
Writer cannot have instant video from Earth.  
Review fails the chapter if it does.

### 28.2 Incorrect “memory”

Chat: “Let’s say Aya is allergic to latex.”  
Next week’s Writer session forgets and puts her in latex gauntlets.  
Correct pattern: allergy is a capability/constraint on `aya-okoro.md` the same day it is decided.

### 28.3 Derived vs source

`PROJECT_MEMORY.md` says “Aya does not trust Marek.”  
Aya’s sheet says `Trust in Marek: conditional, after ch-09`.  
The sheet wins. Memory should read `See aya-okoro.md — Marek trust.`

---

## 29. Acceptance and validation criteria

### 29.1 This document is complete when

- Tiers A–E are mapped to paths and questions.
- Source, derived, temporary, and generated kinds are distinguished.
- Authoritative stores are named per fact class.
- Agent read/write behavior is defined and matches [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md).
- Lifecycle, update, conflict, versioning, retrieval, assembly, staleness, retcon, and human approval are specified.
- Future indexes cannot become a second canon.
- Examples show promotion, failure, and derived-vs-source.

### 29.2 The Memory System is correctly implemented when

| Check | Pass condition |
| --- | --- |
| Homes | Brief, project memory, canon, research, production trees exist under the names in `001_ARCHITECTURE` |
| SoT | A reusable fact has one approved home; duplicates are pointers |
| Prose | Unpromoted reusable facts are flagged and caught in Review as `X` and/or `C` |
| Lean B | Project memory does not accumulate bible-length lore |
| Retcon | Canon changes edit registries and cascade; git shows a dedicated commit |
| Retrieval | Drafting sessions load slices, not the whole manuscript by default |
| Index (future) | Any DB/vector module rebuilds from files and loses to files on conflict |
| Resume | An operator returning after weeks can reconstruct state from `book/` without chat logs |

---

## 30. References

- [`../specifications/000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md) — continuity as infrastructure
- [`../specifications/001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) — memory tiers, write/read rules, authority stack
- [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) — who may read and write
- [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md) — entities stored in memory
- [`040_WORKFLOW.md`](040_WORKFLOW.md) — when memory is updated
- [`../../USER_GUIDE.md`](../../USER_GUIDE.md) §7 — operator continuity habits

---

## Document Control

| Field | Value |
| --- | --- |
| ID | `020_MEMORY_SYSTEM` |
| Title | Memory System — AI Book Framework |
| Layer | Subsystem architecture |
| Depends on | `000_PROJECT_VISION`, `001_ARCHITECTURE` |
| Enables | Canon layout, continuity review, future `003_MEMORY_MODEL` specification |
| Maintenance rule | Update when memory tiers, promotion rules, or index policy change; remain consistent with `001_ARCHITECTURE` §7 |

---

*End of document.*
