# 030 — Book Model

**Document type:** Subsystem architecture  
**Status:** Canonical for this subsystem; subordinate to `001_ARCHITECTURE`  
**Audience:** Framework maintainers, template authors, and operators structuring a novel project  
**Depends on:** [`../specifications/000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md), [`../specifications/001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md)  
**Companions:** [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md), [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md), [`040_WORKFLOW.md`](040_WORKFLOW.md)  
**Scope:** Conceptual data model of a novel. Genre-agnostic. Usable without a database.

---

## 1. Purpose

The Book Model names the entities a novel project must be able to represent, how those entities relate, and where they live when the runtime is a git repository of markdown files.

The model must work for science fiction, fantasy, thriller, literary, romance, and other long-form fiction without rewriting the kernel. Genre packs may add fields (a magic system, a tech tree). They may not add a second novel root or a second canon authority.

This document distinguishes three layers that operators and agents must not collapse:

| Layer | What it is | Example |
| --- | --- | --- |
| **Conceptual model** | Entities and relationships that exist whether or not a file has been created yet | “Aya is a Character who participates in Event E-12” |
| **Markdown files** | The v1 implementation of the model | `book/canon/characters/aya-okoro.md` |
| **Future structured data** | Optional schemas, YAML front matter, or databases derived from files | `character.id = aya-okoro` in a rebuildable index |

Agents reason about the conceptual model. They read and write markdown. They must not require a database. See [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §27.

---

## 2. Novel

A **Novel** is one complete narrative work produced under this framework (one volume). A future series module may group multiple Novels under shared series canon; each volume still has its own brief, architecture, and production tree.

**Conceptual identity:** one Book Brief, one accepted manuscript line, one working-memory dashboard.

**Markdown:** the `book/` tree for the active volume ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §4.2).

**Future structured data:** a `novel_id`, title, language, and volume number in `PROJECT.md` or a manifest. Not required for v1.

The Novel is not the framework repository. Framework specs live in `docs/`. Live story state lives in `book/`.

---

## 3. Book metadata

**Purpose:** identify the project to humans, git remotes, and export tools without mixing identity with plot.

| Field | Typical home |
| --- | --- |
| Working title, genre label, language | `book/BOOK_BRIEF.md` |
| Repository URL, framework stage | `PROJECT.md` |
| Status of the volume (concept, drafting, frozen) | Brief and/or project memory |
| Author name, pen name, intended markets | Brief (human-owned) |

Metadata is not canon. Changing the working title does not change Helios Station.

**Example:** `PROJECT.md` holds the git remote. The brief holds `Title: Helios Wake (working)`. Export later reads both.

---

## 4. Premise

**Premise** is the one-sentence (or short-paragraph) statement of the story’s dramatic engine: who faces what, in what world, with what stakes.

**Home:** `book/BOOK_BRIEF.md`  
**Authority:** intent memory (Tier A).  
**Consumers:** Architect (structure must serve it), Review (theme/structure defects if the book abandons it).

**Example:** “A salvage engineer on a dying orbital station finds a sealed deck that still has power—and a reason someone wanted it forgotten.”

Premise is conceptual. It is not a plot outline. Multiple plot designs may serve one premise until architecture is approved.

---

## 5. Theme

**Themes** are the questions or claims the book intends to dramatize (not slogans pasted into dialogue).

**Home:** brief (required); architecture (thematic spine—how acts pressure the theme).  
**Relationship:** Theme constrains Architect and Review (`T` defects). It does not invent locations.

**Example:** Brief lists `Theme: what we owe the dead when resources are finite.` Architecture states Act III forces Aya to choose archive versus living crew.

---

## 6. Setting

**Setting** is the chosen frame of world + time + social situation in which scenes occur. It is the author’s cut of the World for this Novel.

**Home:** brief (high-level); `book/canon/world/` (detail).  
**Relationship:** Setting ⊆ World. A World may contain unused continents; Setting is what this book may on-page.

**Example:** World includes the entire Earth–Moon system. Setting for *Helios Wake* is Helios Station and a shuttle, over three weeks.

---

## 7. World

**World** is the story-universe container: physical laws as the book uses them, history relevant to the plot, and indexes to locations, factions, cultures, technology, and rules.

**Home:** `book/canon/world/` plus `glossary.md` for names.  
**Markdown pattern:** one file per major domain object (`helios-station.md`), not a single unreadable wiki dump.

**Future structured data:** `world_id`, linked IDs for child entities. Optional.

World is canon once approved. Generated travelogue in a chapter does not expand World until promoted ([`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §15).

---

## 8. Locations

A **Location** is a place where Events can occur (station module, city, ship, room class).

**Home:** `book/canon/world/` (per location or grouped).  
**Relationships:** Location belongs to World; Events and Scenes reference Locations; Characters have current or typical Location (derived from Timeline, not duplicated as a second clock).

**Required conceptual fields:** name, where it sits relative to other places, constraints on access, sensory/operational facts that prose must not contradict.

**Example:** `inner-ring-hatch-17` is dark, warm when batteries dump heat, requires a salvage credential Aya has.

---

## 9. Characters

A **Character** is an agent in the story (human, AI, creature, or other) with identity, wants, and a voice that prose must respect.

**Home:** `book/canon/characters/<stable-slug>.md`  
**Relationships:** Characters participate in Events; appear in Scenes; belong to Factions and Cultures optionally; hold Relationships with other Characters; are bound by Rules (what they can know and do).

**Stable ID vs display name:** the slug does not change when a nickname changes. Aliases live on the sheet ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §16.4).

**POV policy** lives in the brief; which Character is viewpoint in a Scene lives on the outline contract.

**Example sheet facts:** Aya Okoro; salvage engineer; close-third POV; does not know the core is cold until ch-11; voice: clipped, tool-first metaphors.

---

## 10. Factions

A **Faction** is a group with interests, resources, and typical methods (corp, crew, government, church, swarm).

**Home:** `book/canon/world/` (faction files) with pointers from character sheets.  
**Relationships:** Characters may belong to Factions; Conflicts often run through Factions; Locations may be controlled by Factions.

Unused factions stay out of Setting. Do not create a faction file because a Writer needed a passing uniform; flag and decide.

---

## 11. Cultures

A **Culture** is a pattern of language, custom, value, and taboo that Characters can inhabit.

**Home:** world canon.  
**Relationships:** Characters and Factions may participate in Cultures; Style/voice may draw on Culture without replacing the brief’s voice policy.

Genre packs (especially sci-fi and fantasy) often deepen this entity. Literary contemporary novels may keep it light. The entity still exists so agents have a home when needed.

---

## 12. Technology

**Technology** is the set of tools, infrastructures, and capabilities the World makes available (or withholds).

**Home:** world canon and, when claims need grounding, research constraints.  
**Relationships:** Rules limit Technology; Scenes must not use capabilities the World has not established without a promotion or a lie labeled as such.

**Example:** no FTL; comms lag ≥ 4 minutes; cutting torches exist; medical nanites do not.

Fantasy projects may leave this entity thin and thicken Magic systems instead. The slot remains so mixed-genre work (science-fantasy) can fill both.

---

## 13. Rules

**Rules** are binding World laws: physical, institutional, magical, or informational (who can know what).

**Homes:** brief (project constraints), world canon (story laws), research **Constraint** (grounding).  
**Relationships:** Rules constrain Characters’ capabilities, Technology, Magic/science systems, and valid Events.

**Example rule:** “No one on Helios can receive real-time Earth video.” An Event that needs a live Earth feed is illegal until Rules change.

---

## 14. Magic or science systems

Where applicable, a **system** is a structured subset of Rules + Technology (or magic analogues) with costs, limits, and tells.

**Home:** `book/canon/world/` (for example `systems/station-power.md` or `systems/thaumaturgy.md`).  
**Soft dependency:** Research before locking a system that makes real-world or internal-consistency claims ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §15.3).

**Not applicable** is a valid state. A literary novel does not invent a fake system file. The model still knows the slot so sci-fi and fantasy packs can require it.

**Example (science):** station power = residual batteries + cold reactor; heat at Hatch 17 is a tell, not magic.  
**Example (fantasy, same slot):** blood-price conjuring cannot raise the dead; necromancy is rumor.

---

## 15. Plot

**Plot** is the designed sequence of pressure: what happens to pursue the Premise through Conflicts to a resolution.

**Homes:** architecture (macro), outline (micro), timeline (when).  
**Relationships:** Plot is realized as Story Arcs, Chapters, Scenes, and Events. Plot is not identical to Timeline (order of telling may differ from order of occurrence).

Architect owns Plot at act scale. Outline owns Plot at scene scale. See [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §3.3.

---

## 16. Story arcs

A **Story Arc** is a through-line with a beginning state, pressures, and an intended end state (protagonist arc, relationship arc, mystery arc, world-change arc).

**Home:** architecture (arc list and act placement); character sheets (personal arc notes); `threads.md` for mystery arcs.

**Relationships:** Arcs span Chapters; Scenes should declare which Arc they serve; Review `S`/`H`/`T` defects fire when a Scene serves none or contradicts an Arc’s contracted direction.

**Example:** Aya competence → complicity → moral choice. Rival-crew trust arc pays in Act III, not chapter 2.

---

## 17. Chapters

A **Chapter** is a reader-facing prose unit with a production file.

**Markdown:** `book/chapters/ch-NN-slug.md` ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §16.2).  
**Status:** Draft | Review | Accepted (human) | Frozen with its act.

**Relationships:** Chapter contains one or more Scenes; implements Outline contracts for that chapter number; may depict multiple Events.

**Conceptual vs file:** the Chapter entity can exist in the outline before the prose file exists. Absence of `ch-07-*.md` does not delete the planned Chapter.

---

## 18. Scenes

A **Scene** is the atomic dramatic unit: a stretch of action with a goal, conflict, turn, and payoff (the outline contract).

**Markdown:** scene blocks inside `book/outline/` files (chapter-scoped or act-scoped). Templates will standardize fields in a later sprint; the conceptual fields are already required by workflow.

**Required conceptual fields:**

- Viewpoint Character (per brief POV policy)
- Location and Timeline anchor
- Goal, conflict, turn, payoff
- Arcs and Threads touched (plant / pay / avoid)
- Claims that need Research constraints
- Continuity hooks (what must be true going in; what is true going out)

**Relationships:** Scene belongs to a Chapter; depicts Events; is constrained by Canon and Outline authority.

Writer prose that changes the turn without amending the Scene contract is a process defect.

---

## 19. Events

An **Event** is something that occurs in the World (docking, a death, a message arrival), independent of how many pages depict it.

**Home:** `book/canon/timeline.md` rows, with pointers to Scenes that show the Event.

**Relationships:** Events have time, Location, participating Characters; they change Character state, World state, or information state (Secrets revealed).

Off-page Events still belong on the Timeline if later Scenes depend on them.

**Example:** Event `E-04 Station Arrival` at Day 12 04:00, Helios dock, Aya + crew. Depicted in ch-01. Referenced by ch-06 clock.

---

## 20. Timeline

The **Timeline** is the ordered set of Events (and planned Events) for the Novel.

**Home:** `book/canon/timeline.md`  
**Relationships:** orders Events; Scenes must attach to a slice; Chapters may be told out of chronological order only if the Timeline still makes the occurrence order explicit.

Nonlinear narrative is allowed. Hidden clocks are not. If chapter 20 is a flashback to before chapter 1, the Timeline says so.

---

## 21. Relationships

A **Relationship** is a typed link between Characters (or Character and Faction): trust, kinship, debt, command, romance, rivalry.

**Home:** character sheets as the index; optional dedicated sections when the graph is large.  
**Relationships (meta):** Relationships change via Events; Outline should state the in/out state for relationship-critical Scenes.

**Who-knows-what** is part of Relationship and Character knowledge, not a free-floating vibe ([`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §7).

**Example:** `Aya — Marek: professional distrust until E-19; reader-known, Aya-not-known: Marek filed the sealed-deck order.`

---

## 22. Conflicts

A **Conflict** is opposed want or force that generates Scene pressure (person vs person, vs institution, vs environment, vs self).

**Homes:** brief (central conflict), architecture (escalation), outline (scene conflict field), project memory (conflicts currently in play).

**Relationships:** Conflicts involve Characters and/or Factions and/or Rules; they are not identical to plot events. A fight scene with no Conflict field is a structure defect.

---

## 23. Secrets

A **Secret** is information whose known-to set is not universal.

**Home:** character sheets and `book/canon/threads.md`, labeled `Author secret` / `Reader-known` / `Character-known` as needed ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §16.7).

**Relationships:** Secrets constrain dialogue and POV; leaking a Secret in the wrong viewpoint is a continuity defect (`C`) and often a structure defect (`S`).

**Example:** Reader-known: Hatch 17 heat is battery dump. Aya-not-known until ch-04. Prose in close-third cannot have her “sense the reactor’s lie” before that.

---

## 24. Research

**Research** is the Novel’s grounding corpus: sources, constraints, conjectures, rejections.

**Home:** `book/research/`  
**Conceptual role:** constrains World, Rules, systems, and factual claims in prose. It is not World. Promotion copies accepted facts into canon ([`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §10).

Literary novels still use Research when they assert real places, dates, or professions. Invented-world novels use it for internal consistency memos.

---

## 25. Canon

**Canon** is the approved subset of World, Character, Timeline, Glossary, and Thread knowledge for this Novel (and later, series-shared canon).

**Home:** `book/canon/`  
**Relationship to other entities:** Canon is a *status and authority overlay*, not a separate story object. A Character draft sheet is not canon until approved.

Generated chapters can *propose* canon. They cannot *be* canon. See [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §3 and §15.

---

## 26. Drafts

A **Draft** is a versioned attempt at a production artifact (usually a Chapter, sometimes an outline or architecture option).

**Homes:** the canonical path (`book/chapters/ch-NN-slug.md`) for the active attempt; git history for prior wording; `*.alt.md` or `exp/` branches for competitors.

**Relationships:** Drafts implement Scene contracts; they contain claims that must be reconciled with Canon; Reviews attach to Drafts.

There is one live file per chapter number on the production path. Alternatives are isolated ([`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §17).

---

## 27. Revisions

A **Revision** is a deliberate change to a Draft in response to Review defects, human taste, or an upstream contract change.

**Homes:** edits on the same chapter path; review reports that list what must change; outline/architecture amendments when the defect is structural.

**Relationships:** Revision is a workflow loop ([`040_WORKFLOW.md`](040_WORKFLOW.md)), not a folder of `ch-07-rev3-final.md` copies. Git records rev3. The live file is the current Draft.

If structure is wrong, Revision returns to Outline (or Architect), not only to Writer.

---

## 28. Style

**Style** is the intended manner of prose: POV, tense, diction, rhythm, banned tics, and anti-slop expectations.

**Homes:** brief; optional book style notes; optional style pack (outranked by brief).  
**Relationships:** Style constrains Writer and Review (`R` defects). Style does not create Events.

Style is policy knowledge ([`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §11), not a Character.

---

## 29. Constraints

**Constraints** are non-negotiable limits on the Novel: content boundaries, POV hard rules, rating, research ethics, and World Rules the author refuses to break.

**Homes:** brief (project-level); research **Constraint**; canon Rules.  
**Relationships:** Constraints bind every generative agent. Violating them is a Blocker until the human amends the source.

**Example:** `No FTL.` `No sexual content involving minors.` `Close-third Aya only in Part I.`

---

## 30. Relationships between entities

```text
Novel
 ├── Book metadata
 ├── Premise, Theme, Style, Constraints      (Brief — intent)
 ├── Setting ──uses──► World
 │                    ├── Locations
 │                    ├── Factions
 │                    ├── Cultures
 │                    ├── Technology
 │                    ├── Rules
 │                    └── Magic/science systems
 ├── Characters ──Relationships──► Characters / Factions
 ├── Plot
 │    ├── Story arcs
 │    ├── Conflicts
 │    ├── Secrets / Threads
 │    ├── Chapters ──contain──► Scenes ──depict──► Events
 │    └── Outline contracts bind Scenes
 ├── Timeline ──orders──► Events
 ├── Research ──constrains──► World, Rules, factual claims
 └── Canon overlay (approved World/Character/Timeline/Threads)
        Drafts / Revisions realize Scenes as prose
```

**Authority along the graph** (must match [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §12.4 and [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §3):

- Brief (Premise, Theme, Style, Constraints) constrains Architecture (Plot, Arcs).
- Architecture constrains Outline (Scenes, Chapter plans).
- Outline constrains Drafts.
- Canon and Research Constraints constrain Outline, Writer, and Review.
- Timeline orders Events that Scenes must respect.
- Review does not rewrite Canon; it files defects against Drafts and contracts.

---

## 31. Conceptual model vs markdown vs structured data

### 31.1 Conceptual model (always)

Entities in §§2–29 exist as types. Operators may not yet have a Culture file. The type is still valid; the instance is optional until the story needs it.

### 31.2 Markdown files (v1 required implementation)

| Entity | Default markdown home |
| --- | --- |
| Novel | `book/` |
| Book metadata | `PROJECT.md` + brief |
| Premise, Theme, Style, Constraints | `book/BOOK_BRIEF.md` |
| Setting, World, Locations, Factions, Cultures, Technology, Rules, systems | `book/canon/world/` |
| Characters, Relationships | `book/canon/characters/` |
| Plot, Story arcs (macro) | `book/architecture/` |
| Chapters (prose) | `book/chapters/` |
| Scenes | `book/outline/` |
| Events, Timeline | `book/canon/timeline.md` |
| Conflicts (active) | architecture + outline + `PROJECT_MEMORY.md` |
| Secrets | sheets + `book/canon/threads.md` |
| Research | `book/research/` |
| Canon overlay | `book/canon/` + `Status: Approved` |
| Drafts / Revisions | chapter files + git + `book/reviews/` |

Absence of a folder does not repeal the entity ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §3.2). Create the file when the next production need arrives, using these names.

### 31.3 Future structured data (optional)

Allowed later, always derived or synchronized from files:

- YAML front matter (`id`, `status`, `pov`, `timeline_ids`)
- JSON/SQLite registries for linting continuity
- Vector indexes for retrieval
- Export DOCX/EPUB built from accepted chapters

**Forbidden:** a database row that is true while the markdown is false; a GUI wiki that agents cannot diff in git; renaming entities per genre (`Quest` instead of `Plot`) in a way that forks the kernel.

Genre packs add **fields**, not **parallel types**. Fantasy adds system cost tables under Rules/systems. Thriller adds clock/pressure fields on Scenes. Romance may require Relationship state on every Scene contract. The entity names above stay.

---

## 32. Examples (pedagogical, not live canon)

### 32.1 Minimal instance set for one chapter

- Novel: *Helios Wake* under `book/`
- Premise and Constraints in brief (approved)
- Character `aya-okoro` sheet (approved)
- Location `helios-station` world file (approved)
- Event `E-04` on the Timeline
- Scene contract in `book/outline/` for ch-01
- Draft `book/chapters/ch-01-the-warm-hatch.md`
- Review `book/reviews/ch-01-review-01.md`

That set is enough to draft and review chapter 1. Factions, Cultures, and a science-system file can wait until a Scene needs them.

### 32.2 Relationship walk

Scene ch-07 “The Gate” occurs at Location Hatch 17, depicts Event E-19, advances Story Arc “Aya complicity,” changes Relationship Aya–Marek, plants Secret S-3 for the reader, and must obey Rule “no FTL” plus Research Constraint on comms lag. Writer reads those slices only. Review checks each link.

### 32.3 Genre reuse

The same graph holds for a closed-room thriller (World is a house; Technology is locks and phones; no magic system file) and for epic fantasy (World is a continent; system file is required by a genre pack). Workflow stages do not change; see [`040_WORKFLOW.md`](040_WORKFLOW.md).

---

## 33. Acceptance and validation criteria

### 33.1 This document is complete when

- Every entity in the Sprint 2 list is defined with purpose, home, and relationships.
- The graph in §30 matches agent authority in [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) and memory authority in [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md).
- Conceptual / markdown / future-data layers are explicit.
- The model is usable with markdown only.
- Genre variation is additive fields, not a forked kernel.
- Examples instantiate real paths from `001_ARCHITECTURE` naming.

### 33.2 The Book Model is correctly implemented when

| Check | Pass condition |
| --- | --- |
| No database required | A novel can be produced with `book/` markdown and git |
| Naming | Chapter, review, character, and world paths match `001_ARCHITECTURE` §16 |
| Optional entities | Missing Culture/Faction files are valid until needed; missing Brief is not |
| Canon overlay | Draft character notes are not treated as Character canon |
| Nonlinear plots | Timeline still records occurrence order |
| Packs | Genre modules add fields under existing entities |
| Agents | Writer and Review can name which entities they read for a Scene |

---

## 34. References

- [`../specifications/000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md) — sci-fi first, principles general
- [`../specifications/001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) — `book/` layout, naming, templates
- [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) — who mutates which entities
- [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) — which instances are true
- [`040_WORKFLOW.md`](040_WORKFLOW.md) — when entities are created and frozen

---

## Document Control

| Field | Value |
| --- | --- |
| ID | `030_BOOK_MODEL` |
| Title | Book Model — AI Book Framework |
| Layer | Subsystem architecture |
| Depends on | `000_PROJECT_VISION`, `001_ARCHITECTURE` |
| Enables | Templates, canon file schemas, continuity linters, export mapping |
| Maintenance rule | Update when entities, homes, or layering rules change; do not fork types per genre |

---

*End of document.*
