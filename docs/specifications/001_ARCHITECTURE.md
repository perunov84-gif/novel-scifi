# 001 — Architecture

**Document type:** Foundational specification  
**Status:** Canonical  
**Audience:** Framework maintainers, agent authors, Cursor rule authors, and advanced users extending the system  
**Depends on:** `000_PROJECT_VISION.md`  
**Scope:** Complete system architecture of the AI Book Framework—repository layout, knowledge and memory models, agent topology, Cursor/Git integration, templates, modules, conventions, workflows, and quality standards  

---

## 1. Purpose of This Document

This specification defines **how the AI Book Framework is built and how its parts relate**. Where `000_PROJECT_VISION.md` answers *why* the system exists, this document answers *what the system is made of*, *where truth lives*, *how work flows*, and *what contracts every subsystem must honor*.

The architecture is **repository-native**: folders, filenames, markdown contracts, agent specs, and Cursor rules are the primary runtime surfaces. There is no mandatory external database in the core OS. The repository *is* the state machine.

This document describes the **target production architecture**. The current repository may still be a Stage 1 skeleton; implementers must treat this file as the blueprint that subsequent specs, agents, and modules converge toward—not as optional aspiration.

---

## 2. Architectural Overview

### 2.1 System metaphor

The framework is a **literary operating system** with six cooperating layers:

1. **Kernel** — repository law, naming conventions, source-of-truth rules, workflow gates  
2. **Knowledge** — specifications, guides, prompt library, templates, examples  
3. **Memory** — book brief, project memory, canon registries, continuity state  
4. **Production** — outlines, chapters, revisions, manuscript assembly  
5. **Intelligence** — specialized agents, Cursor rules, role prompts  
6. **Operations** — git history, changelogs, quality gates, development workflow  

### 2.2 Design invariants

These invariants are non-negotiable across all modules:

- **Artifacts over chat:** durable files beat ephemeral conversation.  
- **One source of truth per fact class:** contradictions are defects.  
- **Role purity:** agents do not silently assume adjacent roles.  
- **Downward authority:** Brief → Architecture → Outline → Prose, unless a human elevates a revision.  
- **Human gates:** intent, structure, and acceptance remain human-controlled.  
- **Diffability:** every meaningful change must be git-reviewable.  
- **Replaceable models:** process contracts survive vendor/model churn.  

### 2.3 Logical architecture diagram

```text
┌─────────────────────────────────────────────────────────────────┐
│                        HUMAN AUTHOR (executive)                  │
└───────────────────────────────┬─────────────────────────────────┘
                                │ approvals / revisions / taste
┌───────────────────────────────▼─────────────────────────────────┐
│                     CURSOR IDE RUNTIME SURFACE                   │
│   agents/  ·  .cursor/rules/  ·  chat+edit  ·  multi-file ctx   │
└───────┬─────────────────┬─────────────────┬─────────────────────┘
        │                 │                 │
        ▼                 ▼                 ▼
┌───────────────┐ ┌───────────────┐ ┌─────────────────────────────┐
│ KNOWLEDGE OS  │ │ MEMORY OS     │ │ PRODUCTION OS               │
│ docs/         │ │ book/ brief   │ │ book/ outline, chapters,    │
│ prompts/      │ │ memory, canon │ │ reviews, manuscript         │
│ templates/    │ │ research      │ │                             │
│ examples/     │ │               │ │                             │
└───────┬───────┘ └───────┬───────┘ └──────────────┬──────────────┘
        │                 │                        │
        └─────────────────┴────────────┬───────────┘
                                       ▼
                              ┌────────────────┐
                              │  GIT HISTORY   │
                              │  audit + branches
                              └────────────────┘
```

### 2.4 Pipeline architecture (canonical)

```text
BOOK_BRIEF
  → ARCHITECT (structure / thematic spine)
    → RESEARCH (grounding / speculative consistency)
      → OUTLINE (scene contracts)
        → WRITER (prose under constraints)
          → REVIEW (quality + continuity adjudication)
            ↺ REVISION → Outline and/or Writer (explicit loop)
```

Side paths (mid-draft research, targeted continuity repair) are allowed only if they **write back** into memory/canon and do not create parallel truth.

---

## 3. Repository Structure

### 3.1 Target top-level tree

The production target structure is:

```text
novel-scifi/   (or any book project rooted on this framework)
├── AGENTS.md
├── README.md
├── PROJECT.md
├── QUICK_START.md
├── USER_GUIDE.md
├── WORKFLOW.md
├── CHANGELOG.md
├── .gitignore
├── .cursor/
│   └── rules/
│       ├── README.md
│       ├── 00-framework-core.mdc
│       ├── 10-continuity.mdc
│       ├── 20-style-voice.mdc
│       ├── 30-workflow-gates.mdc
│       └── 40-review-quality.mdc
├── agents/
│   ├── README.md
│   ├── architect.md
│   ├── research.md
│   ├── outline.md
│   ├── writer.md
│   └── review.md
├── docs/
│   ├── specifications/
│   │   ├── 000_PROJECT_VISION.md
│   │   ├── 001_ARCHITECTURE.md
│   │   ├── 002_WORKFLOW.md
│   │   ├── 003_MEMORY_MODEL.md
│   │   ├── 004_AGENT_CONTRACTS.md
│   │   ├── 005_QUALITY_GATES.md
│   │   └── 006_NAMING_AND_LAYOUT.md
│   ├── guides/
│   │   ├── authoring.md
│   │   ├── continuity.md
│   │   └── revision.md
│   └── decisions/
│       └── ADR-NNNN-title.md
├── prompts/
│   ├── README.md
│   ├── architect/
│   ├── research/
│   ├── outline/
│   ├── writer/
│   └── review/
├── templates/
│   ├── BOOK_BRIEF.template.md
│   ├── PROJECT_MEMORY.template.md
│   ├── CHAPTER.template.md
│   ├── SCENE_OUTLINE.template.md
│   ├── REVIEW_REPORT.template.md
│   ├── CHARACTER_SHEET.template.md
│   └── RESEARCH_NOTE.template.md
├── examples/
│   ├── minimal-project/
│   └── sci-fi-sample-snippets/
├── book/
│   ├── BOOK_BRIEF.md
│   ├── PROJECT_MEMORY.md
│   ├── architecture/
│   ├── research/
│   ├── outline/
│   ├── chapters/
│   ├── reviews/
│   ├── canon/
│   └── manuscript/
├── modules/                    # future / optional
│   ├── genre-scifi/
│   ├── style-packs/
│   └── export/
└── tools/                      # future / optional
    ├── assemble_manuscript.*
    └── continuity_lint.*
```

### 3.2 Stage awareness

Early repositories may contain only a subset (brief, memory, workflow stubs, empty agent folder). **Absence of a folder does not repeal the architecture.** When a capability is needed, it must be introduced under the names and responsibilities defined here.

### 3.3 Root file responsibilities

| File | Responsibility |
| --- | --- |
| `README.md` | Public entry: what this repo is, how to start, links to guides |
| `PROJECT.md` | Project identity metadata (title working name, repo URL, stage) |
| `QUICK_START.md` | Shortest path to first useful action |
| `USER_GUIDE.md` | Human operator manual |
| `WORKFLOW.md` | Operator-facing stage summary (may point to formal `002_WORKFLOW`) |
| `AGENTS.md` | Index/pointer to `agents/` specifications |
| `CHANGELOG.md` | Framework and/or project-significant changes |

Root docs are **operator surfaces**. Deep law lives in `docs/specifications/`.

---

## 4. Folder Responsibilities

### 4.1 `docs/` — Knowledge and law

`docs/` holds the framework’s institutional knowledge:

- **`docs/specifications/`** — normative specs (`000+`). Numbered, canonical, change-controlled.  
- **`docs/guides/`** — explanatory how-tos; helpful but subordinate to specs if conflict arises.  
- **`docs/decisions/`** — Architecture Decision Records (ADRs) capturing significant design choices and rejected alternatives.

**Rule:** If a guide disagrees with a specification, the specification wins until formally amended.

### 4.2 `book/` — Active novel state

`book/` is the live production workspace for the current novel (or current volume in a series layout). It contains intent, memory, structure, research outputs, chapters, and reviews.

**Rule:** Creative deliverables for the active book live under `book/`, not under `docs/`.

### 4.3 `agents/` — Role definitions

Each agent file defines mission, inputs, outputs, forbidden behaviors, and handoff expectations for one role. `agents/README.md` indexes roles and usage.

### 4.4 `.cursor/rules/` — Persistent runtime constraints

Cursor rules enforce framework law during agent/editor sessions: continuity habits, workflow gates, style constraints, review standards. Rules are adapters from specs into the IDE runtime.

### 4.5 `prompts/` — Prompt library

Versioned, role-scoped prompt assets used by humans or agents for repeatable operations (e.g., “continuity sweep for chapter N”, “act outline expansion”). Prompts are not a substitute for agent contracts; they are reusable procedures.

### 4.6 `templates/` — Blank contracts

Starter documents with required sections and placeholders. Copy into `book/` (or elsewhere) rather than editing templates in place for a specific novel—unless intentionally evolving the framework template itself.

### 4.7 `examples/` — Pedagogical fixtures

Minimal samples demonstrating correct shape of artifacts. Examples are non-normative for a live novel’s canon but normative for *format expectations*.

### 4.8 `modules/` — Optional extensions

Genre packs, style packs, export pipelines, series tooling. Modules may add files and prompts but must not invent a second kernel or second memory authority.

### 4.9 `tools/` — Optional automation

Scripts and linters that validate repository invariants, assemble manuscripts, or check continuity metadata. Tools are servants of the file contracts, not owners of truth.

---

## 5. Document Hierarchy

### 5.1 Authority stack (highest to lowest)

1. **Human author decisions** recorded into artifacts (brief amendments, canon updates)  
2. **Foundational specs** (`000`, `001`, then workflow/memory/agent/quality specs)  
3. **Active book contracts** (`book/BOOK_BRIEF.md`, architecture docs, approved outlines)  
4. **Canon / memory registries** (character sheets, timeline, world rules)  
5. **Cursor rules** (runtime enforcement of the above)  
6. **Agent specs** (role behavior implementing contracts)  
7. **Prompts / templates / examples / guides** (supporting materials)  
8. **Chat messages** (ephemeral; zero authority unless written back to files)

### 5.2 Specification numbering scheme

| Range | Purpose |
| --- | --- |
| `000–049` | Foundational OS (vision, architecture, workflow, memory, agents, quality, naming) |
| `050–099` | Cross-cutting technical specs (git conventions, export, evaluation harnesses) |
| `100–199` | Genre module specs (sci-fi pack, etc.) |
| `200–299` | Style and voice pack specs |
| `300+` | Experimental / optional |

Filenames: `NNN_SHORT_NAME.md` in `SCREAMING_SNAKE` for the short name.

### 5.3 Conflict resolution algorithm

When two durable documents conflict:

1. Identify the fact class (intent, structure, canon fact, prose, process law).  
2. Apply authority stack for that class.  
3. Prefer the more specific approved artifact *within the same authority tier* (e.g., approved scene outline beats a vague architecture note for scene intent).  
4. If still ambiguous: **fail loudly**—open a defect/review note; do not silently invent reconciliation in prose.  
5. Resolve by human update to the winning artifact, then cascade downstream.

### 5.4 Document lifecycle states

Recommended header metadata for major artifacts:

- `Status:` Draft | Review | Approved | Deprecated  
- `Owner:` human role (Author / Editor)  
- `Stage:` which workflow stage consumes/produces it  
- `Last-reviewed:` date or commit reference  

Agents must not treat `Draft` architecture as binding for mass chapter generation without human elevation to `Approved`.

---

## 6. Knowledge System

### 6.1 Definition

The **knowledge system** is the framework’s instructional and normative corpus: what operators and agents need to know to run the OS correctly. It is distinct from **memory** (state of *this* novel).

### 6.2 Knowledge classes

| Class | Location | Nature |
| --- | --- | --- |
| Vision & mission | `docs/specifications/000_*` | Normative philosophy |
| Architecture | `docs/specifications/001_*` | Normative structure |
| Workflow law | `docs/specifications/002_*` + `WORKFLOW.md` | Normative process |
| Operator guides | `docs/guides/`, `USER_GUIDE.md` | Explanatory |
| Decisions | `docs/decisions/` | Historical rationale |
| Prompts | `prompts/` | Procedural |
| Templates | `templates/` | Structural defaults |
| Examples | `examples/` | Illustrative |
| Module docs | `modules/*/README.md` + module specs | Extension knowledge |

### 6.3 Knowledge acquisition path for new operators

1. `README.md` → `QUICK_START.md`  
2. `000_PROJECT_VISION.md` (why)  
3. `001_ARCHITECTURE.md` (what)  
4. `WORKFLOW.md` / workflow spec (how stages run)  
5. Fill `book/BOOK_BRIEF.md` using template  
6. Read relevant agent specs before invoking roles  

### 6.4 Knowledge acquisition path for agents

Agents should load, in order of priority for a task:

1. Applicable Cursor rules  
2. Own agent contract  
3. `BOOK_BRIEF` + `PROJECT_MEMORY` + relevant canon  
4. Stage inputs (architecture/outline/prior review)  
5. Only then: optional prompts/examples  

Agents must not substitute examples for the active book’s canon.

### 6.5 Knowledge change control

- Spec changes that alter contracts require changelog entries.  
- ADRs for significant architectural pivots.  
- Template changes should be backward-compatible or accompanied by migration notes.  
- Deprecated knowledge must be marked, not silently deleted mid-project without migration.

---

## 7. Memory System

### 7.1 Definition

The **memory system** is the durable state of the novel: what is true, what was promised, what changed, and what remains open. Memory is the primary defense against continuity collapse.

### 7.2 Memory tiers

#### Tier A — Intent memory

- `book/BOOK_BRIEF.md`  
- Premise, genre, audience, themes, tone, POV policy, hard constraints, non-goals for the book  

Intent memory answers: *What are we trying to make?*

#### Tier B — Working memory

- `book/PROJECT_MEMORY.md`  
- Rolling high-signal state: current act focus, active conflicts, recent decisions, open questions, risks  

Working memory answers: *Where are we right now?*

#### Tier C — Canon memory

Under `book/canon/` (target):

- `characters/` — sheets, relationships, voice notes  
- `timeline.md` — chronology and scene dating  
- `world/` — settings, factions, technology/magic systems, rules  
- `threads.md` — open mysteries, foreshadowing ledgers, unpaid bills to the reader  
- `glossary.md` — terms, neologisms, consistent spellings  

Canon memory answers: *What is true in the story world?*

#### Tier D — Research memory

Under `book/research/`:

- Topic notes, source lists, speculative consistency memos, “do not contradict” technical constraints  

Research memory answers: *What grounding constrains invention?*

#### Tier E — Production memory

- Outlines, chapter headers/status, review reports, revision logs  

Production memory answers: *What has been planned, drafted, and judged?*

### 7.3 Memory write rules

1. **Generative agents may propose canon; they may not silently promote it.** New facts that will be reused must be written into Tier C (or flagged in Tier B until promoted).  
2. **Prose is not canon authority.** If a chapter invents a fact, either update canon or strike the fact.  
3. **Retcons require explicit edits** to canon + brief/architecture if intent changes.  
4. **PROJECT_MEMORY stays short and current.** Long-term facts belong in canon registries, not in an ever-growing working-memory blob.  
5. **Deletion is rare; deprecation is preferred** for facts that were true in draft history but superseded—mark superseded-by references.

### 7.4 Memory read rules

Before drafting or outlining a scene, agents must consult at least:

- Book brief constraints  
- Relevant character sheets  
- Timeline slice  
- Open threads that the scene might touch  
- Latest review constraints for that chapter/act  

### 7.5 Memory failure modes (and architectural responses)

| Failure | Symptom | Response |
| --- | --- | --- |
| Chat amnesia | Reinvented facts | Force file reads; refuse to proceed if Tier A/B missing |
| Canon fork | Two folders disagree | Authority rules + review defect |
| Memory bloat | Unreadable PROJECT_MEMORY | Split into canon; keep working memory lean |
| Hidden retcon | Chapter changes past | Continuity review + timeline repair |
| Orphan research | Notes never linked | Require research outputs to declare impacted canon files |

### 7.6 Series memory (future-compatible)

For multi-volume work, architecture anticipates:

- `series/canon/` shared across volumes  
- `book/` or `volumes/vN/` for volume-specific state  
- Spoiler boundary rules for agents working on earlier volumes  

Series layout is a future module; the memory *tiers* remain the same.

---

## 8. Agent System

### 8.1 Role topology

| Agent | Primary job | Must not |
| --- | --- | --- |
| Architect | Act structure, thematic spine, major turns, escalation logic | Draft polished chapter prose as final deliverable |
| Research | Grounding, references, speculative consistency constraints | Override brief themes or invent unchecked canon as final truth |
| Outline | Scene-level contracts: goal, conflict, turn, payoff | Write full prose chapters |
| Writer | Prose under outline + voice + canon constraints | Redefine architecture or silently retcon canon |
| Review | Adversarial quality/continuity/craft adjudication | “Fix” by regenerating wholesale without filing defects |

Optional future agents (Line Editor, Continuity Auditor, Lore Keeper) must map cleanly onto Quality or Memory subsystems without collapsing role purity.

### 8.2 Agent contract anatomy

Each file in `agents/` should define:

1. **Mission** — one paragraph  
2. **Inputs (required / optional)** — file paths and statuses  
3. **Outputs** — exact artifacts produced  
4. **Acceptance criteria** — what “done” means  
5. **Forbidden behaviors** — role boundary enforcement  
6. **Handoff** — next agent and required artifacts  
7. **Escalation** — when to stop and ask the human  
8. **Reference prompts** — links into `prompts/<role>/`  

### 8.3 Invocation model

Agents are invoked through Cursor using their specs + rules + relevant files in context. The architecture does not require a custom orchestrator binary in v1. Orchestration is:

- human-directed stage progression, plus  
- workflow docs/rules that prevent skipping gates, plus  
- artifact checklists that make incomplete handoffs obvious.

### 8.4 Multi-agent collaboration rules

- One primary agent role per task session when possible.  
- If multiple roles are needed, sequence them with file handoffs.  
- Reviewer must see the Writer’s output and the constraints that bound it.  
- Writer must not mark chapters `Approved`; only human + review process may.  

### 8.5 Agent state and idempotency

Re-running an agent on the same inputs should update the same output paths deliberately (with git-visible diffs), not scatter alternate untitled files. If experimental alternates are needed, use git branches or clearly named `*.alt.md` candidates pending human choice.

---

## 9. Cursor Integration

### 9.1 Why Cursor is the runtime

Cursor provides the operational loop the architecture assumes:

- multi-file context packing  
- agent chat with edit capabilities  
- persistent rules  
- inline human correction  
- tight git coupling  

### 9.2 Integration surfaces

| Surface | Role in architecture |
| --- | --- |
| `.cursor/rules/` | Always-on constraints and checklists |
| `agents/*.md` | Role contracts loaded/referenced per task |
| `@` file references | Explicit context assembly for stage inputs |
| Inline edit / apply | Human-gated mutation of artifacts |
| Notepads / docs (if used) | Must not become a second memory store; promote to `book/` |

### 9.3 Rule pack design

Recommended rule layers:

1. **Framework core** — artifacts-over-chat, authority stack, fail loudly  
2. **Continuity** — mandatory memory reads/writes  
3. **Style/voice** — project voice constraints (may be book-specific overrides)  
4. **Workflow gates** — refuse mass drafting without approved brief/outline  
5. **Review quality** — defect language, specificity, no vague praise  

Rules should cite specs by ID rather than re-authoring philosophy at length.

### 9.4 Context budgeting strategy

Because novels exceed context windows:

- Prefer **structured memory** over stuffing all chapters.  
- Load the **current chapter + neighbors + relevant canon slices**, not the entire manuscript.  
- Use outlines as compression of future/past intent.  
- Keep `PROJECT_MEMORY.md` high-signal.  
- For continuity sweeps, run chapter-scoped or act-scoped jobs.

### 9.5 Human-in-the-loop patterns in Cursor

- Author edits brief/architecture directly when intent changes.  
- Author uses diffs to accept/reject agent patches.  
- Author may ask agents to propose options as separate candidate sections, then merge chosen options into canonical files.  
- Author never relies on unexported chat as the only record of a decision.

---

## 10. Git Integration

### 10.1 Git as narrative black box

Git provides:

- history of canon and prose evolution  
- branches for alternate plot experiments  
- blame for “when did this fact appear?”  
- PR-like review discipline for collaborators  

### 10.2 Branching philosophy

Recommended model:

- `main` / `trunk` — stable approved manuscript line (or framework trunk in framework repos)  
- `develop` — integration line for active drafting  
- `feature/act-N-...` or `draft/ch-0N-...` — scoped work  
- `exp/plot-...` — speculative divergences; merge only after human choice and canon updates  

Do not use branches as a substitute for memory files. Branches hold variants; memory states the chosen truth after merge.

### 10.3 Commit discipline

Commits should be small enough to review:

- “Update brief: constrain POV to close third on protagonist”  
- “Add outline for ch12–ch14 escalation”  
- “Draft ch07 prose (wip)”  
- “Review ch07: continuity defects filed”  
- “Canon: timeline fix for Station Arrival”  

Avoid mega-commits that mix brief changes, five chapters, and unrelated rule edits.

### 10.4 What not to commit

- Secrets, API keys, private publisher credentials  
- Huge binary research dumps without need  
- Generated caches if tools create them (ignore via `.gitignore`)  
- Personal scratch that confuses agents (keep scratch clearly named or local)

### 10.5 Tags and releases (optional)

For framework repos: tag framework versions.  
For novel repos: optional tags at act completion or submission milestones (`act-1-frozen`, `beta-reader-1`).

### 10.6 Diff-driven review

Review agents and humans should prefer inspecting diffs for revision passes: *what changed* relative to last approved version is often more informative than re-reading an entire chapter cold.

---

## 11. Prompt Library

### 11.1 Purpose

The prompt library (`prompts/`) stores **repeatable procedures** that encode known-good instructions for specific jobs. It reduces improvisation and improves reproducibility across sessions and models.

### 11.2 Library layout

```text
prompts/
  README.md
  architect/
    act-design.md
    thematic-spine.md
  research/
    topic-deep-dive.md
    consistency-constraints.md
  outline/
    scene-expansion.md
    act-outline.md
  writer/
    chapter-draft.md
    scene-rewrite.md
    voice-pass.md
  review/
    continuity-audit.md
    craft-review.md
    pacing-audit.md
```

### 11.3 Prompt document standard

Each prompt file should include:

- Intent  
- Required context files  
- Preconditions (e.g., brief approved)  
- Instruction body  
- Output schema / destination path  
- Stop conditions / escalation  
- Anti-patterns  

### 11.4 Prompts vs agents vs rules

| Mechanism | Longevity | Scope |
| --- | --- | --- |
| Rules | Persistent session law | Always-on constraints |
| Agents | Role identity | Whole-stage behavior |
| Prompts | Task procedures | Single job playbooks |

A Writer agent may call different prompts for first draft vs voice pass. The agent contract still governs boundaries.

### 11.5 Prompt versioning

Breaking prompt changes should be noted in `CHANGELOG.md`. Active novels may pin to prompt filenames; avoid silent in-place meaning changes without notice.

---

## 12. Templates

### 12.1 Role of templates

Templates define the **minimum viable schema** for artifacts so agents and humans produce interoperable documents.

### 12.2 Core templates (required set)

| Template | Becomes |
| --- | --- |
| `BOOK_BRIEF.template.md` | `book/BOOK_BRIEF.md` |
| `PROJECT_MEMORY.template.md` | `book/PROJECT_MEMORY.md` |
| `SCENE_OUTLINE.template.md` | outline entries |
| `CHAPTER.template.md` | `book/chapters/...` |
| `REVIEW_REPORT.template.md` | `book/reviews/...` |
| `CHARACTER_SHEET.template.md` | `book/canon/characters/...` |
| `RESEARCH_NOTE.template.md` | `book/research/...` |

### 12.3 Template design rules

- Required sections marked clearly.  
- Placeholders use obvious tokens (e.g., `{{TITLE}}`) or HTML comments.  
- No sample canon that could leak into a new book if copied carelessly—use clearly fake example tokens or keep examples in `examples/`.  
- Keep templates stable; prefer additive fields over renames.

### 12.4 Instantiation workflow

1. Copy template to destination path with canonical name.  
2. Fill required fields before invoking dependent agents.  
3. Set status metadata (`Draft` → `Approved` as appropriate).  
4. Commit the new artifact skeleton early so agents share structure.

---

## 13. Examples

### 13.1 Purpose

Examples teach shape and quality expectations without owning a user’s canon.

### 13.2 Example packages

- **`examples/minimal-project/`** — smallest coherent set of brief, memory, one outline scene, one chapter stub, one review stub.  
- **`examples/sci-fi-sample-snippets/`** — short illustrations of continuity notes, tech constraint memos, and review defect style.

### 13.3 Rules for examples

- Labeled as non-authoritative for live books.  
- Must obey naming and template schemas.  
- Should demonstrate fail-loudly patterns (e.g., a sample review filing a continuity defect).  
- Must not be so large that they bloat clones; prefer snippets and minimal projects.

### 13.4 Anti-example discipline

When documenting failure modes, keep anti-examples clearly marked (`BAD:`) so agents do not imitate them as style.

---

## 14. Future Modules

### 14.1 Module architecture principles

Modules plug into the kernel; they do not fork it.

A module may provide:

- additional prompts and templates  
- genre-specific review rubrics  
- canon schemas (e.g., tech tree, magic system)  
- export tools  
- optional agents  

A module may not:

- replace `BOOK_BRIEF` / memory authority  
- bypass review gates  
- require opaque binary state for core continuity  

### 14.2 Planned module families

#### Genre packs

- `modules/genre-scifi/` — hard/soft sci-fi constraints, tech plausibility checks, sense-of-wonder vs rigor dials  
- Future: fantasy, thriller, literary, romance packs with distinct rubrics  

#### Style packs

- Voice profiles, diction constraints, banned cliché lists, sentence rhythm targets  
- Per-book overrides live in `book/` and outrank generic packs  

#### Series module

- Shared canon root, volume inheritance, spoiler boundaries  

#### Export module

- Manuscript assembly to DOCX/EPUB/print markdown  
- Front matter / back matter templates  

#### Evaluation module

- Continuity regression checks  
- Outline coverage metrics  
- Review defect taxonomies and dashboards (file-based is enough initially)  

#### Collaboration module

- Role-based contribution guides for editor/researcher co-authors  
- Branch permissions conventions  

### 14.3 Module manifest (recommended)

Each module should include `MODULE.md` with:

- name/version  
- kernel compatibility  
- provided artifacts  
- conflicts/overrides policy  
- enablement instructions  

---

## 15. Dependency Graph

### 15.1 Spec dependency graph

```text
000_PROJECT_VISION
 └── 001_ARCHITECTURE
      ├── 002_WORKFLOW
      ├── 003_MEMORY_MODEL
      ├── 004_AGENT_CONTRACTS
      ├── 005_QUALITY_GATES
      └── 006_NAMING_AND_LAYOUT
           │
           ├──────────────► prompts/ templates/ examples/
           ├──────────────► agents/*
           └──────────────► .cursor/rules/*
```

### 15.2 Runtime artifact dependency graph

```text
BOOK_BRIEF
 ├── PROJECT_MEMORY (keeps brief constraints in view)
 ├── architecture/* (constrained by brief)
 │    └── outline/* (constrained by architecture + brief)
 │         └── chapters/* (constrained by outline + canon + brief)
 │              └── reviews/* (adjudicates chapters against all upstream)
 ├── research/* (feeds architecture/outline/writer constraints)
 └── canon/* (constrains outline/writer/review; updated from approved discoveries)
```

### 15.3 Soft vs hard dependencies

| Dependency | Type | Enforcement |
| --- | --- | --- |
| Brief before architecture approval | Hard | Workflow gates / rules |
| Research before all scenes | Soft | Required only when claims need grounding |
| Outline before chapter draft | Hard for production path | Rules + review |
| Review before “done” | Hard | Quality standards |
| Export tools | Soft | Optional module |

### 15.4 Change impact graph

- Changing **Brief** may invalidate architecture, outlines, and open chapters → require impact review.  
- Changing **Canon** may invalidate specific scenes → continuity audit on dependents.  
- Changing **Outline** after draft exists → revision task, not silent prose drift.  
- Changing **Rules/Agents** affects future sessions; re-review in-flight chapters if constraints tighten.

---

## 16. Naming Conventions

### 16.1 General rules

- Prefer **predictable, boring names** over clever ones.  
- Use **kebab-case** for multi-word filenames in most content trees (`ch-07-the-gate.md`).  
- Use **SCREAMING_SNAKE** for top-level canonical contracts (`BOOK_BRIEF.md`, `PROJECT_MEMORY.md`).  
- Use **numeric prefixes** for ordered specs and ordered rules (`000_`, `10-`).  
- Avoid spaces and special characters.  

### 16.2 Chapter naming

Recommended:

```text
book/chapters/ch-01-slug.md
book/chapters/ch-02-slug.md
```

Optional status suffix only in working copies if needed (`ch-07-slug.wip.md`), but prefer YAML/frontmatter or header `Status:` inside the file to avoid rename churn.

### 16.3 Review naming

```text
book/reviews/ch-07-review-01.md
book/reviews/act-02-continuity-pass.md
```

### 16.4 Canon naming

```text
book/canon/characters/aya-okoro.md
book/canon/world/helios-station.md
book/canon/timeline.md
```

Character IDs should remain stable even if display names change; note aliases inside the sheet.

### 16.5 Prompt and template naming

- Prompts: verb-led or task-led (`continuity-audit.md`)  
- Templates: `THING.template.md`  

### 16.6 Branch naming

```text
draft/ch-12-escape
exp/ending-bittersweet
fix/continuity-timeline-station
chore/update-rules-voice
```

### 16.7 Language conventions inside documents

- Use consistent headings.  
- Prefer present-tense canon statements for true facts.  
- Mark hypotheses as `Hypothesis:` and secrets as `Author secret:` / `Reader-known:` where needed.  
- Use defect IDs in reviews (`C-014 continuity`, `P-003 pacing`) for traceability.

---

## 17. Development Workflow

### 17.1 Two development tracks

The repository may develop along two coupled tracks:

1. **Framework development** — improving OS specs, agents, rules, templates, modules  
2. **Novel production** — using the OS to write a book  

Do not casually mix unrelated framework refactors into manuscript hot paths without isolation (branches).

### 17.2 Framework development workflow

1. Identify pain from real writing (missing gate, weak template, ambiguous agent boundary).  
2. Check vision/architecture for principle alignment.  
3. Draft/update spec (and ADR if significant).  
4. Update agents/rules/templates/prompts to match.  
5. Validate against an example or active book slice.  
6. Changelog + commit.  

### 17.3 Novel production workflow (operator)

1. Initialize / update brief.  
2. Architect structure; human approve.  
3. Research as needed; write constraints into research/canon.  
4. Outline acts/scenes; human approve critical spans.  
5. Draft chapters with Writer.  
6. Review; file defects; revise.  
7. Update memory/canon continuously.  
8. Freeze acts when acceptance criteria met.  

### 17.4 Session workflow (micro)

A healthy Cursor session:

1. State the role and stage.  
2. Load required files explicitly.  
3. Confirm preconditions.  
4. Produce/update artifacts.  
5. Summarize diffs and remaining risks in working memory if decisions occurred.  
6. Commit logical units.  

### 17.5 Revision workflow

Revisions are first-class:

- Review report lists defects with severity and references.  
- Writer or human addresses defects.  
- Re-review targeted issues.  
- Update outline if structural fix required (do not only patch prose for structural problems).  

### 17.6 Contribution workflow (collaborators)

- Use branches.  
- Keep PRs scoped (one chapter, one canon domain, one framework concern).  
- Require continuity notes for canon-affecting changes.  
- Never merge silent retcons without memory updates.

---

## 18. Quality Standards

### 18.1 Quality as system property

Quality is not a final polish step alone. It emerges from intent clarity, structural soundness, continuity discipline, prose craft, and review rigor.

### 18.2 Gate standards by stage

| Stage | Entrance criteria | Exit criteria |
| --- | --- | --- |
| Brief | Project started | Required sections filled; constraints explicit; status Approved for production |
| Architect | Approved brief | Act map + thematic spine documented; open design questions listed |
| Research | Questions identified | Constraints/notes written; impacted canon listed |
| Outline | Approved architecture (for span) | Scene contracts complete for span; stakes/payoffs present |
| Writer | Approved outline slice + canon reads | Chapter draft meets scene goals; new facts flagged |
| Review | Draft exists | Defects filed with references; verdict Pass/Fail/Pass-with-fixes |
| Done (chapter) | Review pass | Human acceptance; memory updated |

### 18.3 Defect taxonomy (minimum)

- **Continuity (C):** facts, timeline, inventory, geography, who-knows-what  
- **Structure (S):** scene purpose missing, act turn weak, payoff absent  
- **Character (H):** motivation break, voice drift, relationship inconsistency  
- **Pacing (P):** stall, rush, misplaced exposition  
- **Prose (R):** clarity, imagery, filter overuse, generic diction (as defined by style pack)  
- **Theme (T):** thematic contradiction vs brief  
- **Process (X):** skipped gate, missing artifact, unrecorded retcon  

### 18.4 Severity levels

- **Blocker:** must fix before proceeding to dependent work  
- **Major:** must fix before chapter/act freeze  
- **Minor:** fix in next revision pass  
- **Note:** advisory taste item  

### 18.5 Prose quality stance

The framework does not impose one aesthetic, but it does impose **anti-slop** expectations unless a style pack explicitly allows a mode:

- Prefer concrete sensory specificity over vague abstraction stacks.  
- Prefer character-specific diction over interchangeable “literary AI” gloss.  
- Exposition must be motivated by scene pressure.  
- Metaphors should be coherent within voice and world.  

### 18.6 Continuity quality stance

- Every reusable fact needs a home in canon/memory.  
- Reader promises (clues, foreshadow, prophecies, capabilities) are tracked.  
- Capability inflation (characters suddenly knowing/doing X) requires justification.  

### 18.7 Documentation quality stance

- Specs are precise, testable where possible, and free of decorative ambiguity.  
- Guides may be discursive; specs must be normative.  
- Examples must compile with naming/template rules.  

### 18.8 Definition of “production-grade” for this architecture

The architecture is production-grade when:

- a trained operator can run brief-to-reviewed-chapter without inventing structure,  
- continuity has an obvious home and update path,  
- agents have enforceable boundaries,  
- git history meaningfully explains evolution,  
- and quality gates can fail builds of a chapter the same way CI fails bad code—via explicit criteria, not vibes alone.

---

## 19. Security, Privacy, and Ethics (Architectural Implications)

- Keep credentials out of the repo.  
- Treat unpublished manuscripts as sensitive; be cautious with external tool sharing.  
- Research notes should distinguish sourced fact vs model conjecture.  
- Content boundaries in the brief are architectural constraints, not optional flavor.  
- The system must not provide workflows for plagiarized ingestion presented as original prose.

---

## 20. Performance and Scale Considerations

### 20.1 Context scale

As manuscripts grow, architecture relies on **hierarchical compression**: brief → architecture → outline → chapter, plus sliced canon. Full-manuscript context loading is a last resort, not a plan.

### 20.2 Team scale

File ownership and branch scope become more important as collaborators join. Canon domains (characters vs world vs timeline) can be owned separately.

### 20.3 Framework scale

Modules and numbered specs prevent a single sprawling README from becoming the architecture. New complexity must declare its home in the dependency graph.

---

## 21. Extension and Compatibility Policy

- Additive changes preferred.  
- Breaking contract changes require version notes and migration guidance.  
- Cursor feature changes may require rule/agent adapter updates without changing memory law.  
- External orchestrators (CI validators, future apps) must treat files as API.  

---

## 22. Mapping: Current Skeleton → Target Architecture

This repository may begin with a minimal set:

- root operator docs  
- `book/BOOK_BRIEF.md`, `book/PROJECT_MEMORY.md`  
- `agents/` stub  
- `.cursor/rules/` stub  
- `docs/specifications/000` and `001`  

The target architecture above is the destination map. Implementers should grow into the tree deliberately: **do not create empty labyrinths without owners**; create folders when the next production need arrives, using the names defined here.

---

## 23. Implementation Priorities (Architectural, Not a Task Tracker)

In order:

1. Stabilize brief + memory contracts  
2. Formalize workflow gates and agent contracts  
3. Establish outline/chapter/review templates  
4. Strengthen continuity canon layout  
5. Expand prompt library from real usage  
6. Add validators/tools  
7. Add modules (genre/style/export/series)  

Each priority should leave the dependency graph more executable.

---

## 24. Glossary (Architecture Terms)

| Term | Meaning |
| --- | --- |
| Kernel | Core repo law and workflow contracts |
| Artifact | Durable file with process meaning |
| Gate | Preconditions that must hold before a stage proceeds |
| Canon | Approved story-world truth |
| Working memory | Short, current operational state |
| Role purity | Agent stays within defined responsibilities |
| Handoff | Explicit artifact transfer between stages |
| Module | Optional extension pack |
| Defect | Filed quality failure with taxonomy/severity |
| Authority stack | Conflict resolution order for documents |

---

## 25. Closing

The AI Book Framework architecture turns novel production into an inspectable system: **knowledge teaches**, **memory remembers**, **agents execute roles**, **Cursor operates**, **git records**, **templates standardize**, **prompts repeat**, **modules extend**, and **quality gates protect the reader’s trust**.

If a proposed feature cannot name its folder, its authority tier, its dependencies, and its failure mode, it is not yet architectural—it is an idea waiting to be placed.

This document is the placement map.

---

## Document Control

| Field | Value |
| --- | --- |
| ID | `001_ARCHITECTURE` |
| Title | Architecture — AI Book Framework |
| Layer | Foundational |
| Depends on | `000_PROJECT_VISION` |
| Enables | Workflow, memory model, agent contracts, quality gates, naming, modules, tools |
| Maintenance rule | Update when repository layout, authority stack, subsystem boundaries, or integration model materially change |

---

*End of document.*
