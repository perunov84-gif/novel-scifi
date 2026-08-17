# Project Memory

**Role:** Operational memory index (Tier B — working memory)  
**Question:** Where are we right now?  
**Not:** a second book brief, a canon bible, or a store of chapter prose.

This file holds high-signal operational state and **pointers** to the files that own facts. If a fact will still matter after the current act, it belongs in `BOOK_BRIEF.md`, `book/canon/`, `book/characters/`, `book/world/`, `book/plot/`, `book/timeline/`, or `book/research/`—not as a duplicated paragraph here.

Interface law: [`../docs/architecture/060_BOOK_CORE.md`](../docs/architecture/060_BOOK_CORE.md).  
Intent source: [`BOOK_BRIEF.md`](BOOK_BRIEF.md).  
Filesystem homes: [`../docs/architecture/050_MEMORY_IMPLEMENTATION.md`](../docs/architecture/050_MEMORY_IMPLEMENTATION.md).

---

## Information kinds

Every entry below is labeled. Agents must not collapse these kinds.

| Kind | Meaning | Authority |
| --- | --- | --- |
| **Approved** | Human-committed for its class | Treat as true for that class until superseded |
| **Derived** | Index, pointer, or compression of a source | Valid only while it matches the source; the source wins |
| **Pending** | Awaiting a human decision | Not truth. Do not invent a resolution |
| **Temporary** | Session-only or not yet written back | Zero after the session unless promoted through protocol |

Lean rule: replace blobs with paths. Chat is **Temporary** until written here or into a source file.

---

## 1. Purpose

**Derived** from framework law.

Operational dashboard for this novel: current stage, approved vs open decisions, active constraints as pointers, and indexes into memory directories. Kernel agents read it when current project state matters. Kernel agents may append only lean flags (focus, open questions, unpromoted facts, risks, next gate). They must not dump research notes, act maps, or canon paragraphs into this file.

---

## 2. Current project state

| Kind | State |
| --- | --- |
| **Derived** | Novel concept is not yet captured in `BOOK_BRIEF.md`. Author fields there are empty. |
| **Derived** | Memory directories exist and contain no story files yet: `book/canon/`, `book/characters/`, `book/world/`, `book/plot/`, `book/timeline/`, `book/research/`. |
| **Pending** | Production path is closed until the human fills intent and sets brief Canon status to `Approved`. |

---

## 3. Current development stage

**Derived** from [`../docs/architecture/040_WORKFLOW.md`](../docs/architecture/040_WORKFLOW.md) §4.2.

| Field | Value |
| --- | --- |
| Backbone stage | Book Brief (concept development) |
| Brief Canon status | Not `Approved` (field empty in `BOOK_BRIEF.md`) |
| Next production stage after brief approval | Architect |

Do not start mass architecture, research Constraints as binding production law, outlining, or drafting while this row remains unapproved.

---

## 4. Approved decisions

**Approved:** none for this novel.

Do not record chat agreements here as if they were decisions. A decision exists when the human has written it into the winning artifact (`BOOK_BRIEF.md`, approved plot, approved canon, or an accepted Constraint).

---

## 5. Active constraints

| Kind | Constraint | Home |
| --- | --- | --- |
| **Derived** (process) | Do not invent missing brief, canon, or plot facts | Framework kernel; see Book Core |
| **Pending** | Book-level hard limits (POV, rating, content boundaries, world limits) | `BOOK_BRIEF.md` §18 — empty |

When the author records constraints in the brief, point at that section. Do not paste the constraint text here.

---

## 6. Open questions

**Pending** until the author supplies intent.

- What is this book? (title, working title, genre, premise, logline, core concept)
- Who is it for, in what language, at what tone and reader experience?
- What setting, protagonist, main conflict, central question, and ending direction apply?
- What hard constraints and inspirations bind the work?
- When will the human set brief Canon status to `Approved`?

Do not answer these in this file. Answers belong in `BOOK_BRIEF.md`.

---

## 7. Pending human decisions

| Decision | Kind | Blocks |
| --- | --- | --- |
| Enter initial concept into `BOOK_BRIEF.md` | **Pending** | Architect production-path work |
| Set `BOOK_BRIEF.md` Canon status to `Approved` | **Pending** | Mass architecture, binding research Constraints, outline, draft |
| Approve first architectural span (after Architect Draft) | **Pending** (not yet proposed) | Mass outline of that span |

---

## 8. Canon references

**Derived** index. Not canon.

| Home | Holds | Files |
| --- | --- | --- |
| `book/canon/` | Approved cross-cutting universe facts: glossary, threads, laws not owned by one character or place | none yet |

---

## 9. Character references

**Derived** index. Not character sheets.

| Home | Holds | Files |
| --- | --- | --- |
| `book/characters/` | Character profiles and approved character facts | none yet |

Protagonist *intent* (if any) lives in `BOOK_BRIEF.md` §14. That field is empty. Do not create a character file from an empty brief.

---

## 10. World references

**Derived** index. Not world canon.

| Home | Holds | Files |
| --- | --- | --- |
| `book/world/` | Locations, factions, cultures, systems, technology, setting rules | none yet |

Setting *intent* (if any) lives in `BOOK_BRIEF.md` §13. That field is empty.

---

## 11. Plot references

**Derived** index. Not an act map.

| Home | Holds | Files |
| --- | --- | --- |
| `book/plot/` | Act-level architecture (`act-*.md`) and scene contracts (`ch-*.md`) | none yet |

Conceptual `book/architecture/` and `book/outline/` paths resolve here ([`../docs/architecture/050_MEMORY_IMPLEMENTATION.md`](../docs/architecture/050_MEMORY_IMPLEMENTATION.md) §2.2).

---

## 12. Timeline references

**Derived** index. Not a clock.

| Home | Holds | Files |
| --- | --- | --- |
| `book/timeline/` | Canonical chronology (one file per event or dated slice) | none yet |

---

## 13. Research references

**Derived** index. Not Constraints.

| Home | Holds | Files |
| --- | --- | --- |
| `book/research/` | Source / Constraint / Conjecture / Rejected notes | none yet |

Research is not story-world truth until the human promotes a fact into the correct canon home.

---

## 14. Current workflow state

**Derived** from the canonical backbone:

```text
Book Brief → Architect → Research → Outline → Writer → Review → Revision
```

| Gate | State |
| --- | --- |
| Book Brief approved for production | **Pending** |
| Architecture approved | Not started |
| Research Constraints accepted | Not started |
| Outline span approved | Not started |
| Chapter Review + human accept | Not started |
| Act / manuscript freeze | Not started |

Architect, Research, Outline, Writer, and Review become available for production-path work only as these gates turn green ([`../docs/architecture/040_WORKFLOW.md`](../docs/architecture/040_WORKFLOW.md)).

**Next human gate:** fill `BOOK_BRIEF.md` and set Canon status to `Approved`.

---

## 15. Last approved milestone

**Approved:** none.

This is not a framework-sprint log. Record novel milestones here only after the human accepts them (brief lock, act map, outline span, chapter accept, act freeze).

---

## 16. Important warnings

| Kind | Warning |
| --- | --- |
| **Derived** | `BOOK_BRIEF.md` author fields are empty. Never assume missing information. Never silently invent canon. |
| **Derived** | `examples/` and pedagogical names in architecture docs are not this book. |
| **Derived** | Git history is not live memory. Load the current files. |
| **Pending** | No POV policy, rating, or content boundary is recorded yet. Generative agents must stop rather than guess. |

---

## 17. Change log / memory updates

Newest first. Record operational updates and pointers, not story restatements.

| When | Kind | Update |
| --- | --- | --- |
| 2026-08-17 | **Derived** (process) | This file became the operational memory index under Book Core. No novel facts recorded. Brief remains empty. Next gate: human concept entry in `BOOK_BRIEF.md`. |
