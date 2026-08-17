# 050 — Memory Implementation

**Document type:** Subsystem implementation  
**Status:** Canonical for the filesystem memory layer; subordinate to `001_ARCHITECTURE` and `020_MEMORY_SYSTEM`  
**Audience:** Operators maintaining book continuity, agent-contract authors, and framework maintainers  
**Depends on:** [`../specifications/000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md), [`../specifications/001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md), [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md)  
**Companions:** [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md), [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md), [`040_WORKFLOW.md`](040_WORKFLOW.md)  
**Agent contracts:** [`../../agents/architect/SYSTEM.md`](../../agents/architect/SYSTEM.md), [`../../agents/research/SYSTEM.md`](../../agents/research/SYSTEM.md), [`../../agents/outline/SYSTEM.md`](../../agents/outline/SYSTEM.md), [`../../agents/writer/SYSTEM.md`](../../agents/writer/SYSTEM.md), [`../../agents/review/SYSTEM.md`](../../agents/review/SYSTEM.md)  
**Scope:** Filesystem implementation of the Memory System. Markdown files plus Git are sufficient. This document does not introduce a database, vector store, templates, prompts, scripts, or live story files.

---

## 1. Purpose

This document implements the Memory System as directories and conventions under `book/`.

[`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) defines *what memory is*: tiers, fact classes, promotion, retrieval, and authority. This document defines *where those facts live on disk* in this repository, who may read or write them, and how Git records change.

The framework remains fully usable with Markdown files and Git. No database is required. No agent may treat chat, embeddings, or Git history as a substitute for the current files under `book/`.

This is an implementation of the existing architecture. It does not change memory tiers, agent roles, workflow gates, or the authority stack. Where [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) and [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md) name nested conceptual homes (for example `book/canon/characters/`), this repository uses the mapped paths in §2. Authority rules in `020` remain binding.

**Intended audience:** the human author operating the novel; Architect, Research, Outline, Writer, and Review sessions that must load and update memory; maintainers who later add templates or indexes.

---

## 2. Directory structure

### 2.1 Implemented tree

```text
book/
├── BOOK_BRIEF.md
├── PROJECT_MEMORY.md
├── canon/
├── characters/
├── world/
├── plot/
├── timeline/
└── research/
```

`BOOK_BRIEF.md` and `PROJECT_MEMORY.md` already exist. The six directories are the Sprint 4 filesystem layer. They start empty. Agents and humans create Markdown files inside them when production needs a home, using the names in §17. Do not invent extra top-level memory folders.

Production trees named in [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) (`book/chapters/`, `book/reviews/`, `book/manuscript/`) are **not** created by this implementation. Generated prose and review reports still belong there when those trees exist. They are not canon.

### 2.2 Correspondence to `020_MEMORY_SYSTEM`

| Memory tier ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §7) | Question | Conceptual home in `020` / `030` | Filesystem home in this implementation |
| --- | --- | --- | --- |
| **A — Intent** | What are we trying to make? | `book/BOOK_BRIEF.md` | `book/BOOK_BRIEF.md` |
| **B — Working memory** | Where are we right now? | `book/PROJECT_MEMORY.md` | `book/PROJECT_MEMORY.md` |
| **C — Canon overlay** | What is approved universe truth that is not a single character, place, or event? | `book/canon/` (glossary, threads, cross-cutting facts) | `book/canon/` |
| **C — Characters** | Who is this person, and what is true of them? | `book/canon/characters/` | `book/characters/` |
| **C — World** | What is true of the setting? | `book/canon/world/` | `book/world/` |
| **C — Timeline** | When do events occur relative to each other? | `book/canon/timeline.md` | `book/timeline/` |
| **Plot (act-level structure)** | What is the designed pressure at act scale? | `book/architecture/*` | `book/plot/` act-level files (`act-01.md`, spine, arcs) |
| **Plot (scene-level derived)** | What is the scene contract? | `book/outline/*` | `book/plot/` scene-contract files (`ch-07.md`) |
| **D — Research** | What grounding constrains invention? | `book/research/` | `book/research/` |
| **E — Production prose / reviews** | What has been drafted and judged? | `book/chapters/`, `book/reviews/` | Not part of this memory layer; still Production when created |

**Resolution rule:** when an agent contract or architecture document names a conceptual path in the middle column, operate on the filesystem path in the right column. Do not create a second copy under the conceptual nested path. One fact class, one directory.

**Why the layout is shallow:** [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) prefers predictable names and limited nesting. Character, world, plot, and timeline registries sit beside `canon/` so grep, `@`-references, and Git diffs stay obvious. Canon remains the overlay of *approved universe truth*; it is not a dump of copies from the other directories.

### 2.3 Responsibility of each directory

| Path | Holds | Does not hold |
| --- | --- | --- |
| `book/BOOK_BRIEF.md` | Approved high-level concept: premise, audience, tone, themes, POV policy, hard constraints | Chapter prose; a second plot; research dumps |
| `book/PROJECT_MEMORY.md` | Current focus, open questions, unpromoted flags, risks, next human gate, pointers | Bible-length lore; generated chapters; a second copy of canon paragraphs |
| `book/canon/` | Approved cross-cutting universe facts: glossary, threads/promises, general laws not owned by one character or location | Research notes; Draft proposals; character sheets; location files; chapter prose |
| `book/characters/` | Character profiles and approved character facts | World encyclopedias; act maps; research sources |
| `book/world/` | Locations, factions, cultures, systems, technology, setting rules | Character sheets; timeline event files; research archives |
| `book/plot/` | Approved plot architecture, arcs, conflicts, story logic; Outline’s scene-contract files as **derived** artifacts | Canon world facts; research; finished chapter prose |
| `book/timeline/` | Canonical chronology: one file per event or dated slice | Scene contracts; character biographies copied in full |
| `book/research/` | Findings, sources, evidence, uncertainty, constraints, rejections | Story-world truth (until a human promotes a fact elsewhere) |

### 2.4 Worked path examples (do not create these files in this sprint)

These names illustrate the mapping. They are pedagogical, not live canon.

| Conceptual reference in `020` / `030` | Filesystem file |
| --- | --- |
| `book/canon/characters/aya-okoro.md` | `book/characters/aya-okoro.md` |
| `book/canon/world/helios-station.md` | `book/world/helios-station.md` |
| Location Hatch 17 | `book/world/hatch-17.md` |
| Helios system frame | `book/world/helios-system.md` |
| Act I architecture | `book/plot/act-01.md` |
| Scene contracts for chapter 7 | `book/plot/ch-07.md` |
| Timeline row “first contact” | `book/timeline/001-first-contact.md` |
| Research note on orbital decay | `book/research/orbital-decay.md` |
| Glossary / threads | `book/canon/glossary.md`, `book/canon/threads.md` |

---

## 3. Authority levels

Authority follows [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §5 and [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §3. The filesystem does not create a new stack. It stores each class in one place.

| Level (high → low for this book) | Filesystem objects | Agents may |
| --- | --- | --- |
| **Human recorded decisions** | Status fields, promotions, deprecations, brief amendments | Never impersonate |
| **Intent (Tier A)** | `book/BOOK_BRIEF.md` when `Approved` | Read. Do not silently rewrite. |
| **Act-level plot** | Approved `book/plot/act-*.md` and related architecture files | Outline specializes; Writer obeys; neither replaces the turn |
| **Scene intent** | Approved `book/plot/ch-*.md` (derived, then promoted to source *for scene intent only*) | Writer obeys; never outranks act-level plot |
| **Canon (Tier C)** | `Approved` files in `book/canon/`, `book/characters/`, `book/world/`, `book/timeline/` | Read. Propose. Human promotes. |
| **Research constraints (Tier D)** | Human-accepted **Constraint** lines in `book/research/` | Bind invention; still not story-world canon until promoted |
| **Working memory (Tier B)** | `book/PROJECT_MEMORY.md` | Lean flags and pointers only |
| **Generated prose (Tier E)** | `book/chapters/*` when that tree exists | Claims, not truth |
| **Temporary context** | Cursor chat, unsaved buffers | Zero after the session unless written back |
| **Chat / examples** | Chat; `examples/` | Never authority for this book |

`BOOK_BRIEF.md` is authoritative for intent. Agents must not silently rewrite it. Changing premise, POV policy, or hard constraints is a human amendment.

Approved canon wins over chapter wording until the human runs the canon-change protocol ([`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §25).

Research is **not** automatically canon. A **Constraint** may bind invention while the story-world sentence still waits in a character, world, timeline, or canon file.

---

## 4. File ownership

Ownership means who may create or edit a *class* of file as part of their role. It does not mean exclusive read access. The human owns every approval.

| File class | Owner (writes live files) | Proposer | Approver |
| --- | --- | --- | --- |
| `BOOK_BRIEF.md` | Human | Architect may pressure-test options in chat or a separate candidate; must not patch the brief as a side effect | Human |
| `PROJECT_MEMORY.md` | Human (full edit). Kernel agents (lean flags only) | — | Human for any statement treated as a decision |
| `book/canon/*` | Human | Any kernel agent may identify a candidate and record a proposal **outside** this directory (see §6) | Human |
| `book/characters/*` | Human for `Approved` truth | Architect (cast function), Research (grounding), Outline (scene-local flags) | Human |
| `book/world/*` | Human for `Approved` truth | Architect (what plot needs), Research (what must stay consistent) | Human |
| `book/timeline/*` | Human for `Approved` events | Architect, Research, Outline (Planned pointers) | Human |
| `book/plot/act-*.md` and other act-level architecture | Architect (`Draft`). Human sets `Approved` | Architect | Human |
| `book/plot/ch-*.md` and other scene contracts | Outline (`Draft`). Human sets `Approved` | Outline | Human |
| `book/research/*` | Research | Research | Human accepts **Constraint** language; promotion of story facts is a later human step |
| Chapter prose | Writer | Writer | Human after Review (never Writer, never Review) |
| Review reports | Review | Review | Human accept of the chapter is separate |

**Ownership isolation:**

- Architect must not rewrite Outline’s `ch-*.md` files to “make a scene work.”
- Outline must not rewrite Architect’s `act-*.md` files.
- Research must not edit character, world, timeline, or canon files as truth.
- Writer must not edit any Tier C directory or `BOOK_BRIEF.md`.
- Review must not rewrite authoritative memory to match a convenient draft.
- No agent may rewrite unrelated memory (for example, Research cleaning character sheets “while here”).
- No agent may perform broad cleanup of the memory system.
- No agent may delete canonical information because it looks outdated. Flag it for human review (§19).

---

## 5. Source vs derived vs temporary vs generated information

Kinds match [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §3.2. The filesystem must keep them distinguishable.

| Kind | Definition | Where it lives | Authority |
| --- | --- | --- | --- |
| **Source** | Human-committed contracts and registries meant to be true | Approved brief; `Approved` files in `canon/`, `characters/`, `world/`, `timeline/`; human-accepted research **Constraint** lines; approved `book/plot/act-*.md` | High, per fact class in §3 |
| **Derived** | Compressions or specializations of sources | `PROJECT_MEMORY.md` pointers; `book/plot/ch-*.md` (derived from architecture + canon; becomes source *for scene intent* once `Approved`); review reports | Valid only while they match sources; scene outlines never outrank act turns |
| **Temporary** | Session-only material | Cursor chat, unsaved buffers, operator recollection | Zero unless written back the same day |
| **Generated** | Prose or candidate text produced under contracts | Chapter drafts; `*.alt.md`; `Proposal:` sections; research **Conjecture** | Not canon. Wording authority only for the active draft file |

**Rules that keep kinds from collapsing:**

1. Temporary context must not automatically become persistent memory. Write back decisions, flags, defects, and gate changes, then discard the chat.
2. Generated text must not automatically become canon. A sentence in a chapter is a claim ([`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §13).
3. Research must not automatically become world knowledge. Copying a finding into `book/world/` is a promotion (§6), not a save.
4. Derived information must remain distinguishable from approved facts. Do not paste a canon paragraph into `PROJECT_MEMORY.md`. Point at the source file.

**Example:** Writer drafts “The plating hummed under her boots.” That line is generated. If `book/world/helios-station.md` says spin gravity only, the line is a claim that fails continuity. It does not update the world file.

---

## 6. Canon promotion

Canon promotion is an operational protocol, not software. Do not implement it as a script, bot, or status machine in this sprint.

Nothing is canon because it was generated, researched, or discussed. It is canon because a human approved it into the authoritative file for that fact class ([`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §15, §26).

### 6.1 Protocol (mandatory order)

1. **Agent identifies a candidate fact.** Reusable, testable, needed again (character capability, location rule, event time, glossary term, unpaid promise).
2. **Agent records its source.** Research note path, chapter header, architecture question, or author stipulation. Invented citations are forbidden ([`agents/research/SYSTEM.md`](../../agents/research/SYSTEM.md)).
3. **Agent marks uncertainty.** Use **Conjecture**, ranges, missing data, or `Uncertainty:` on the flag. Do not label a guess as **Constraint**.
4. **Agent proposes promotion.** Flag the producing artifact and add a lean pointer in `PROJECT_MEMORY.md`. Optionally draft a `Proposal:` block in the *target* domain file (`characters/`, `world/`, `timeline/`) with `Status: Draft`. Do **not** write `book/canon/` until the human approves. Research records impacted paths in the research note and stops.
5. **Human reviews it.** Check brief constraints, existing canon, and whether the fact belongs in characters, world, timeline, or canon.
6. **Human approves or rejects.** Reject: strike from prose if needed; mark research **Rejected** if that was the origin; clear the flag. Defer: leave the flag. Approve: continue.
7. **Only after approval may the information become canonical.** Human (or a human-directed edit) writes the present-tense fact into exactly one home, sets `Status: Approved`, and removes duplicate wording from project memory (leave a pointer if useful).
8. **Git records the change.** Commit the promotion as its own logical unit when possible (`Canon: …`). History is the audit trail, not a second canon.

Research can create findings in `book/research/`. Research cannot perform step 7.

### 6.2 Where a promoted fact goes

| Fact class | Destination |
| --- | --- |
| Intent / POV / rating / no-FTL as a project rule | `book/BOOK_BRIEF.md` (human amends intent; not a canon-directory write) |
| Character identity, voice, who-knows-what, capability | `book/characters/<slug>.md` |
| Location, faction, culture, technology, setting rule | `book/world/<slug>.md` |
| Event time, sequence, who was present | `book/timeline/<nnn>-<slug>.md` |
| Spelling, neologism, consistent name | `book/canon/glossary.md` |
| Foreshadow, clue, unpaid reader promise | `book/canon/threads.md` |
| Universe law not owned by one file above | `book/canon/<slug>.md` |

Do not copy the same approved sentence into two directories. Point instead.

### 6.3 Example (correct)

Research writes `book/research/comms-lag.md`: **Constraint:** no FTL; Helios-to-Earth lag ≥ 4 minutes at this plot distance. Impacted path: `book/world/helios-station.md`.  
`PROJECT_MEMORY.md`: `Promotion pending: comms lag ≥ 4 min → world/helios-station.md`.  
Human approves and adds the present-tense sentence to `book/world/helios-station.md` with `Status: Approved`.  
Git commit: `Canon: Helios–Earth comms lag`.  
Outline requires the wait. Writer cannot use instant Earth video. Review fails the chapter if it does.

### 6.4 Example (incorrect)

Chat: “Let’s say Aya is allergic to latex.” Next week’s Writer session puts her in latex gauntlets. The allergy was never promoted. Treat the chat as never happened, or run this protocol the same day onto `book/characters/aya-okoro.md`.

---

## 7. Human approval

Human approval is required to ([`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §26, [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §13):

- Set `book/BOOK_BRIEF.md` to `Approved` and amend intent
- Set act-level `book/plot/` architecture to `Approved`
- Set scene-contract `book/plot/ch-*.md` spans to `Approved` before mass drafting
- Promote research or generated claims into `canon/`, `characters/`, `world/`, or `timeline/`
- Change canon, including retcons
- Deprecate or discard a previously approved fact
- Accept a chapter into the accepted line
- Freeze an act or manuscript

Agents may fill `Draft` text and `Proposal:` sections. Until the human promotes, Writer and Review treat the proposal as untrue for continuity—except Review may record that a draft *assumes* the proposal and that the chapter must not be accepted until canon is decided.

No agent sets `Status: Approved` on brief, architecture, outline, canon, or chapters.

---

## 8. Agent read permissions

All five kernel agents may **read the memory required for their task**. They must not load the entire bible by default. Retrieval and assembly are in §15–§16.

| Agent | Must be allowed to read | Must not treat as truth |
| --- | --- | --- |
| **Architect** | Approved brief; project memory; existing plot files for the span; existing research **Constraint** lines; existing canon slices the amendment must respect | `examples/`; unapproved architecture as binding for mass outline; chat |
| **Research** | Approved brief; project memory questions; existing research notes for the topic; architecture/outline slices that created the question; existing canon slices that might collide | Conjecture as Constraint; example-novel physics |
| **Outline** | Approved brief; approved act-level plot for the span; relevant character, world, timeline, and thread slices; research constraints for claims in span; neighboring scene contracts; reviews that forced re-outline | Draft architecture as frozen law; prose as a second outline |
| **Writer** | Approved brief; approved scene contracts; on-page character sheets; location/world rules; timeline window; threads the scene may plant or pay; research constraints for in-scene claims; neighbor chapters; latest review if revising | Unpromoted flags as canon; Draft outlines on the production path |
| **Review** | Brief constraints and theme; project memory risks; scene contracts; the draft (and diff if revising); on-page sheets; timeline neighbors; world file; threads; listed research notes; prior reports for the chapter | Writer’s “make this succeed” prompt; Conjecture as a fact-check pass; examples as this book’s facts |

**Read-before-write** ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §7.4): before outlining or drafting a scene, consult brief constraints, relevant character files, the timeline slice, open threads the scene might touch, world/location files the scene occupies, and the latest review constraints for that chapter or act.

If a required approved input is missing, the agent stops and names the path. It does not invent a substitute from `examples/` or from chat.

---

## 9. Agent write permissions

Write access is constrained by responsibility ([`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §8). No agent may rewrite unrelated memory.

### 9.1 Permission matrix

Legend:

| Code | Meaning |
| --- | --- |
| **R** | Read when the task needs that slice |
| **W** | Create or edit files in this tree as the owning role (`Draft` unless noted) |
| **P** | Propose only: flags, `Proposal:` / `Status: Draft` on a *target domain* file; never set `Approved`; never treat the proposal as live canon |
| **F** | Lean flag or pointer in `PROJECT_MEMORY.md` and/or the producing artifact header |
| **—** | Must not write |

| Location | Architect | Research | Outline | Writer | Review | Human |
| --- | --- | --- | --- | --- | --- | --- |
| `book/BOOK_BRIEF.md` | R | R | R | R | R | **W** (amend + approve) |
| `book/PROJECT_MEMORY.md` | R+F | R+F | R+F | R+F | R+F | **W** |
| `book/canon/` | R | R | R | R | R | **W** (promote, amend, deprecate) |
| `book/characters/` | R+P | R+P | R+P (scene-local flags) | R | R | **W** (promote, amend, deprecate) |
| `book/world/` | R+P | R+P | R+P | R | R | **W** (promote, amend, deprecate) |
| `book/timeline/` | R+P | R+P | R+P (`Planned` pointers) | R | R | **W** (promote, amend, deprecate) |
| `book/plot/` act-level (`act-*.md`, spine, arcs) | **R+W** | R | R | R | R | Approve / reject |
| `book/plot/` scene contracts (`ch-*.md` and related derived artifacts) | R | R | **R+W** | R | R | Approve / reject |
| `book/research/` | R | **R+W** | R | R | R | Accept **Constraint** language; reject notes |
| `book/chapters/` (when present) | — | — | — | **W** | R | Accept / reject after Review |
| `book/reviews/` (when present) | — | — | — | — | **W** | Read; chapter accept is separate |

### 9.2 Matrix invariants (must remain true)

- **Research can create research findings** in `book/research/` (**W**).
- **Research cannot promote research into canon.** No **W** on `canon/`, `characters/`, `world/`, or `timeline/` as truth. Only **P** plus human step 7 in §6.
- **Architect can propose structural changes** by writing `Draft` act-level files under `book/plot/` (**W** on act-level only). Architect does not set `Approved` and does not promote world/cast needs into canon.
- **Outline can create or modify outline-related derived artifacts** (`book/plot/ch-*.md` and related scene contracts). Outline does not replace act turns and does not edit canon as truth.
- **Writer creates generated prose** on the live chapter path. Writer **cannot silently change canon** (canon columns are **R** only).
- **Review evaluates but does not silently rewrite authoritative memory.** Review writes reports only. Canon, brief, plot, and research bodies are **R** (plus **F** on working memory).
- **Human approval is required for canon promotion.**

### 9.3 Write examples

| Allowed | Forbidden |
| --- | --- |
| Research creates `book/research/orbital-decay.md` with Source / Constraint / Conjecture / Rejected and impacted paths | Research edits `book/world/helios-station.md` to “save a step” |
| Architect writes `book/plot/act-02.md` as `Status: Draft` and lists `K-*` world needs | Architect sets `act-02.md` to `Approved` or silently changes the brief’s POV policy |
| Outline updates `book/plot/ch-07.md` after a human-confirmed `S` defect | Outline patches `book/plot/act-02.md` so the scene can betray early |
| Writer flags `Unpromoted: Aya mentions a sister` in the chapter header and project memory | Writer adds `book/characters/kira.md` and proceeds as if she were canon |
| Review files `C-014` that timeline and chapter clocks disagree | Review “fixes” `book/timeline/001-first-contact.md` to match the draft |

---

## 10. Memory update workflow

This is the operational loop for any memory write. It implements [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §19 and [`040_WORKFLOW.md`](040_WORKFLOW.md) §5.6 without new software.

```text
Detect → Classify → Write in the owning tree → Flag if reusable and unapproved
    → Human promote / strike / defer → Cascade dependents → Commit
```

1. **Detect** the new or changed statement (draft, research, architecture question, review defect, human instruction).
2. **Classify** the fact class (intent, act structure, scene intent, character, world, event, research constraint, working-memory flag). If two classes are involved, update each home separately.
3. **Write in the owning tree** using §4 and §9. Do not scatter untitled notes.
4. **Flag** reusable facts that are not yet `Approved` (producing header + lean `PROJECT_MEMORY.md` pointer).
5. **Human** promote, strike, or defer (§6–§7).
6. **Cascade:** if canon or architecture changed, update timeline, threads, dependent scene contracts, and already-drafted scenes that assert the old fact. Run Review continuity on the affected span.
7. **Commit** a logical unit (§14). Clear resolved flags from project memory or replace them with pointers.

**Lean B rule:** if a fact will still matter after the current act, it does not stay as a paragraph in `PROJECT_MEMORY.md`. Promote it, then keep a pointer.

**Example:** Outline needs Hatch 17 to be warm. Research already constrained heat to battery dump. Human promotes the tell into `book/world/hatch-17.md`. Outline’s `book/plot/ch-01.md` lists the in-state. Writer dramatizes warmth without inventing gravity plating. Project memory does not retell the physics.

---

## 11. Conflict handling

A **conflict** is a disagreement between agents or artifacts about *which contract should win*. Handle it as [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §14 and [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §20. Do not blend answers in prose.

1. Identify the fact class.
2. Apply the authority stack in §3.
3. Prefer the more specific **approved** artifact in the same tier (approved `book/plot/ch-07.md` beats a vague sentence in `act-02.md` for *scene* intent; approved `act-02.md` wins if the *act turn* itself conflicts).
4. If still ambiguous, **fail loudly**. Name both paths. File a defect or an open question. Stop.
5. Human updates the winning file, then cascade (§10).

Agents do not vote. A majority of chat sessions is not evidence.

**Worked conflict:** `book/plot/ch-12.md` ends with a betrayal. Approved `book/plot/act-02.md` says Act II earns trust and the betrayal is the Act III turn. Outline is wrong until the human amends architecture. Writer must not hint betrayal as a compromise. Review files `S` and `X` if prose already did.

**Worked conflict:** Architect wants a mid-novel POV shift. `BOOK_BRIEF.md` forbids it. Brief wins. Architect stops and escalates; it does not hide the shift in a “flexible act map.”

---

## 12. Contradiction handling

A **contradiction** is two statements that cannot both be true as *current* story-world or process truth. Fail loudly. Do not average facts (“the reactor is half-cold”).

| Pattern | Winner | Filesystem action |
| --- | --- | --- |
| Chapter vs `Approved` character/world/timeline/canon file | Canon home | Revise prose, or human runs retcon protocol |
| Two `Approved` files in `characters/`, `world/`, `timeline/`, or `canon/` disagree | None until human picks | Human merges into one home; mark the other `Deprecated` with `Superseded-by:` |
| Research **Constraint** vs approved canon | None until human picks | Human updates the losing file; Research does not silently edit canon |
| `PROJECT_MEMORY.md` vs canon | Canon | Replace the dashboard blob with a pointer |
| Scene contract vs approved act-level plot on the turn itself | Act-level plot | Amend `ch-*.md` or human amends `act-*.md` first |
| Git history vs current file | Current file | History explains *how* it changed (§20); it is not live memory |
| `examples/` vs live `book/` | Live book | Never import example facts |

**Potentially outdated canon** is not a license to delete. If `book/world/hatch-17.md` still says the hatch is cold after an accepted chapter heated it, flag `Stale canon: hatch-17 vs accepted ch-01` in `PROJECT_MEMORY.md` and file a Review `C` defect. The human updates the world file or reverts the prose.

**Example:** `book/timeline/001-first-contact.md` says docking Day 12 04:00. Chapter 6 says “three hours after docking” at a clock that cannot match. Stop. Either amend the timeline file (human) or fix the chapter. Do not leave both `Approved`.

---

## 13. Version control

Git is the version history. The filesystem under `book/` is the current project state.

- Agents must **never** use Git history as a substitute for current memory. Load the live files.
- Git history **may** be used to investigate how a decision changed (blame, log, diff). That investigation informs a human edit to the live file. It does not resurrect deleted wording as canon.
- In-file `Status` and `Last-reviewed` record editorial state. They are not a parallel version database.
- Do not keep `timeline-final-final.md` or `act-01-copy.md` as a versioning scheme. One live path per ID, plus Git.

Future structured snapshots (JSON exports, indexes) must include a commit hash and remain derived from these files ([`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §21).

---

## 14. Git workflow

Git practice for memory follows [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §10 and [`040_WORKFLOW.md`](040_WORKFLOW.md) §7.

| Situation | Practice |
| --- | --- |
| **Revisions** | Edit the live file in place. Git records previous wording. Do not create `aya-okoro-rev3.md`. |
| **Reverts** | `git revert` (or restore the last good commit for that path), then confirm the filesystem matches intended truth. Run continuity Review on dependents if canon or plot reverted. |
| **Rejected changes** | Do not merge the branch. If the text already landed, revert it. Record **Rejected** in the research note or an architecture “Rejected” section so agents do not revive it. Clear the project-memory flag. |
| **Alternate drafts** | `*.alt.md` next to the live path, or `exp/` branches. Candidates have no authority until the human replaces the live file and, if facts changed, updates canon. Never two live files for one ID. |
| **Canonical changes** | Human promotion or retcon on the single home; cascade; commit focused on the change (`Canon: …`, `Plot: amend Act II turn`). |
| **Branches** | Hold variants. After merge, `book/` states the chosen truth. Branches are not memory. |
| **Commit size** | One concern per commit: brief lock, one research topic, one canon promotion, one act file, one scene-contract span. |
| **Do not commit** | Secrets, huge binary dumps, caches, personal scratch that would confuse later agents |

**Example commit set:** `Research: comms lag constraints` → (human) `Canon: Helios–Earth comms lag` → `Plot: ch-09 requires four-minute wait`. Do not mix those with a chapter prose dump.

Framework edits and novel-memory edits stay on separate commits ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §17.1). This document is framework track; `book/characters/aya-okoro.md` is novel track.

---

## 15. Retrieval strategy

Retrieval is **named-file and sliced**, not “load the novel” ([`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §22).

**v1 search:** operator `@`-references; grep inside `book/`; glossary for spellings; numeric prefixes in `book/timeline/` for clock order.

**Minimum retrieval before outlining or drafting a scene:**

1. `book/BOOK_BRIEF.md` constraints
2. Relevant files in `book/characters/`
3. Timeline slice in `book/timeline/` for the scene’s clock
4. Open threads in `book/canon/threads.md` the scene might plant, pay, or contradict
5. Latest review constraints for that chapter or act (when reviews exist)
6. Location/rules files in `book/world/` the scene occupies
7. Research constraints in `book/research/` for technical or historical claims the scene will make
8. Approved plot files: act-level for the span, plus the scene contract itself

**Anti-pattern:** stuffing all chapters into context so nothing is forgotten. That overflows the window and treats prose as canon.

**Anti-pattern:** searching Git log instead of reading `book/world/hatch-17.md`.

---

## 16. Context assembly

Context assembly is the runtime application of retrieval. Default load order for any agent is in [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §7, with paths resolved through §2.2.

**Compression stack** (prefer the higher, smaller layer unless the task is a local prose pass):

```text
Brief → act-level plot → scene contract → chapter
         Canon slices (characters, place, clock, threads)
         Research constraints for claims in span
```

Always include `book/PROJECT_MEMORY.md` so unpromoted flags and freeze risks are visible.

**Do not load:** `examples/` as this book’s facts; the full manuscript for a single-chapter task; unrelated acts; previous session transcripts.

**Example (Writer, chapter 7):** brief POV and constraints; project memory; `book/plot/ch-07.md`; `book/plot/act-02.md` only if the chapter’s act job is in doubt; `book/characters/` sheets for characters on-page; `book/timeline/` files covering the chapter’s clock (and neighbor events); `book/world/` file for the location; `book/canon/threads.md` items the scene might pay or plant; research notes listed on the scene’s claims; neighbor chapter files if they exist; latest ch-07 review if this is a revision.

**Example (Review, chapter 7):** the same slices, plus the draft and a diff against the last acceptable commit when revising. Do not inherit a Writer prompt that says “make this chapter succeed.”

---

## 17. Naming conventions

New memory files use **lowercase**, filesystem-safe, stable, descriptive, predictable Markdown names.

Existing top-level contracts keep their established names: `BOOK_BRIEF.md`, `PROJECT_MEMORY.md` ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §16.1). Do not rename them.

### 17.1 Rules

- Use **kebab-case** ASCII: `aya-okoro.md`, `hatch-17.md`.
- No spaces, no path punctuation (`/ \ : * ? " < > |`).
- Prefer `[a-z0-9-]+.md`.
- Filenames are **stable IDs**. If a display name changes, keep the slug and record aliases inside the file.
- Do not encode status in the filename (`-final`, `-wip`, `-copy`). Use a `Status:` header.
- Do not use `misc`, `temp`, `notes2`, or `new-new`.
- One live file per ID.

### 17.2 Patterns by directory

| Directory | Pattern | Examples (illustrative; do not create in this sprint) |
| --- | --- | --- |
| `characters/` | `<given>-<family>.md` or a single stable slug | `book/characters/aya-okoro.md` |
| `world/` | `<place-or-domain>.md` | `book/world/hatch-17.md`, `book/world/helios-system.md` |
| `plot/` act-level | `act-<NN>.md`; `thematic-spine.md`; `arcs.md` as needed | `book/plot/act-01.md` |
| `plot/` scene contracts | `ch-<NN>.md` (chapter-scoped) | `book/plot/ch-07.md` |
| `timeline/` | `<NNN>-<event-slug>.md` zero-padded for order | `book/timeline/001-first-contact.md` |
| `research/` | `<topic>.md` | `book/research/orbital-decay.md` |
| `canon/` | `glossary.md`, `threads.md`, or a kebab-case law slug | `book/canon/glossary.md` |

Act numbers are two digits (`act-01.md`). Timeline prefixes are three digits so lexical sort matches chronology (`001-` before `010-`). Chapter numbers match the production chapter ID (`ch-07.md` pairs with future `book/chapters/ch-07-….md`).

**Alternates:** `ch-07.alt.md` or a file on an `exp/` branch. Not a second live contract.

---

## 18. File organization rules

1. **One home per fact.** If Aya’s allergy matters, it lives on `book/characters/aya-okoro.md`, not also as a paragraph in world and project memory.
2. **Pointers, not copies.** Derived files cite paths (`See book/world/hatch-17.md`).
3. **Split when grep fails.** One file per major character, location, faction, system, research topic, act, chapter span, or timeline event. Do not grow a single `world.md` wiki.
4. **Do not nest further memory roots.** No `book/world/canon/…` and no parallel `book/canon/world/`. Subfolders inside `world/` are allowed later for a large system pack (for example `book/world/systems/station-power.md`) without creating a second authority.
5. **Empty directories are valid.** Create a file when the next production need arrives, using these names ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §3.2, [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md) §31.2). Missing Culture files are acceptable until a scene needs them. Missing brief is not.
6. **Headers on major artifacts** ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §5.4): `Status:` Draft | Review | Approved | Deprecated; `Owner:`; `Stage:`; `Last-reviewed:`.
7. **Present tense for current canon.** Mark hypotheses `Hypothesis:` or research **Conjecture**. Mark secrets `Author secret:` / `Reader-known:` / `Character-known:`.
8. **Research notes** must label **Source**, **Constraint**, **Conjecture**, or **Rejected**, and must list impacted filesystem paths from §2.
9. **`book/canon/` is approved-only.** Draft proposals belong in project memory, research, or a `Status: Draft` domain file under `characters/`, `world/`, `timeline/`, or `plot/`—not as unapproved essays in `canon/`.
10. **No agent-wide tidy passes.** Do not reorganize another role’s tree as a courtesy.

---

## 19. Archiving

Archiving preserves discoverability. It is not deletion.

| Object | Archive method |
| --- | --- |
| Superseded canon fact | Keep the file or section. Set `Status: Deprecated`. Add `Superseded-by:` path or ID. Git keeps the old wording. |
| Rejected research | Keep the note. Label **Rejected** so later agents do not revive it. |
| Rejected plot branch | Unmerged `exp/` branch, or `*.alt.md` quarantined; live `book/plot/` states the chosen design. |
| Accepted then replaced chapter wording | Git history of the live chapter path. Do not stockpile `ch-07-old.md` in memory directories. |
| Frozen act | Human freeze recorded in project memory and optional Git tag (`act-1-frozen`). Files stay in place. Unfreeze is a recorded human decision. |

**Deletion is rare.** Do not delete canonical information because it appears outdated. Flag it for human review (`Stale:` in `PROJECT_MEMORY.md` plus a Review `C` or `X` defect). The human deprecates, amends, or confirms the fact.

Do not archive by copying live files into `book/canon/archive/` or `book/old/`. Those folders are forbidden memory forks.

---

## 20. Revision history

| Need | Use |
| --- | --- |
| What is true **now** | Read the live file under `book/` |
| When a fact appeared | `git log` / blame on that path |
| What a retcon replaced | Deprecated section + `Superseded-by:` + the commit message `Canon: …` |
| Whether a chapter claim was ever promoted | Chapter header flags, `PROJECT_MEMORY.md` history in Git, then the destination canon file |
| Editorial pass state | `Last-reviewed:` and review report IDs — not a new memory file |

Revisions of prose and of memory are edits to live paths ([`030_BOOK_MODEL.md`](030_BOOK_MODEL.md) §27). Review reports remain separate production artifacts (`book/reviews/` when that tree exists) so adjudication is not mixed into canon files.

If Git history and the live file disagree, the live file wins. Use history only to explain the disagreement to the human who will edit the live file.

---

## 21. Future database / vector-store migration

The core OS has **no mandatory database**. Markdown plus Git is sufficient ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §2.1, [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §27).

If a later module adds a database or vector store:

| Rule | Requirement |
| --- | --- |
| Source of truth | Files under `book/` remain authoritative. Indexes are derived. |
| Mapping | Index keys must be the filesystem slugs in §17 (`aya-okoro`, `hatch-17`, `001-first-contact`). |
| Rebuild | Indexes must regenerate from files plus commit hash. |
| Writes | Tools may suggest patches to files. Updating an embedding is not a canon edit. |
| Retrieval | Return file paths and spans. Do not return citation-free blobs as if they were canon. |
| Conflict | If index and file disagree, the file wins; the index is stale ([`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §24). |
| Privacy | Unpublished manuscripts stay local-friendly; hosted indexes are an operator choice with a privacy cost. |
| Promotion | No automated “embed then promote.” §6 remains a human protocol. |

Vector retrieval may **assist** §15. It must not replace named-file retrieval of the brief, the scene contract, and the scene’s canon slices.

A migration that required agents to write `book/canon/characters/` *and* `book/characters/` would be a fork. Future tools must use this implementation’s paths.

---

## 22. Validation criteria

### 22.1 This document is complete when

- The six directories and two existing contracts are mapped to `020` tiers and `030` entities.
- Authority, ownership, information kinds, promotion, and human gates match `010`, `020`, and the five `SYSTEM.md` contracts.
- The permission matrix encodes the invariants in §9.2.
- Conflict, contradiction, Git, retrieval, assembly, naming, organization, archiving, history, and index migration are specified with examples.
- No software, templates, prompts, or live story files are required for the protocol to be operable.

### 22.2 This implementation is correct when

| Check | Pass condition |
| --- | --- |
| Directories | `book/canon/`, `book/characters/`, `book/world/`, `book/plot/`, `book/timeline/`, `book/research/` exist |
| SoT | A reusable fact has one approved home; duplicates are pointers |
| Path resolution | Conceptual `020`/`030` homes resolve through §2.2; nested `book/canon/characters/` is not also created |
| Brief | Agents do not silently rewrite `BOOK_BRIEF.md` |
| Research | Findings appear in `book/research/`; promotion into canon requires a human |
| Plot roles | Architect writes act-level `book/plot/` files; Outline writes `ch-*.md` derived contracts; neither silently edits the other’s live files |
| Writer | Generated prose does not edit Tier C directories |
| Review | Defects are filed; canon is not rewritten to match the draft |
| Temporary | Chat-only facts are treated as never happened unless written through §6 the same day |
| Git | Live files are current memory; history is investigation only |
| Resume | An operator returning after weeks can reconstruct state from `book/` without chat logs |
| Index (future) | Any database or vector module rebuilds from these files and loses to them on conflict |

### 22.3 Operator smoke test (no extra files required)

1. Directories in §2.1 exist beside `BOOK_BRIEF.md` and `PROJECT_MEMORY.md`.
2. A Research session would write only under `book/research/`.
3. A Writer session would refuse to edit `book/canon/` or `book/characters/`.
4. A promotion candidate is a flag, not an `Approved` canon sentence.
5. Grep for a slug such as `hatch-17` would have exactly one intended home once that file exists: `book/world/hatch-17.md`.

---

## 23. References

- [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) — memory law this filesystem implements
- [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) — who may read and write
- [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md) — entities stored in these directories
- [`040_WORKFLOW.md`](040_WORKFLOW.md) — when memory is updated
- [`../specifications/000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md) — continuity as infrastructure; artifacts over chat
- [`../specifications/001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) — tiers, authority stack, Git, naming
- [`../../agents/architect/SYSTEM.md`](../../agents/architect/SYSTEM.md)
- [`../../agents/research/SYSTEM.md`](../../agents/research/SYSTEM.md)
- [`../../agents/outline/SYSTEM.md`](../../agents/outline/SYSTEM.md)
- [`../../agents/writer/SYSTEM.md`](../../agents/writer/SYSTEM.md)
- [`../../agents/review/SYSTEM.md`](../../agents/review/SYSTEM.md)
- [`../../WORKFLOW.md`](../../WORKFLOW.md) — operator backbone

---

## Document Control

| Field | Value |
| --- | --- |
| ID | `050_MEMORY_IMPLEMENTATION` |
| Title | Memory Implementation — AI Book Framework |
| Layer | Subsystem implementation |
| Depends on | `000_PROJECT_VISION`, `001_ARCHITECTURE`, `020_MEMORY_SYSTEM` |
| Enables | Canon file creation in later production, continuity review against real paths, future `003_MEMORY_MODEL` without a second filesystem |
| Maintenance rule | Update when directory mapping, promotion protocol, or the permission matrix changes; remain consistent with `020_MEMORY_SYSTEM` and do not fork nested `book/canon/characters/` trees |

---

*End of document.*
