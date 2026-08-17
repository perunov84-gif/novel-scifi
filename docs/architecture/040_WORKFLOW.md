# 040 — Workflow

**Document type:** Subsystem architecture  
**Status:** Canonical for this subsystem; subordinate to `001_ARCHITECTURE`  
**Audience:** Operators running a novel in Cursor, and future rule/agent authors encoding gates  
**Depends on:** [`../specifications/000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md), [`../specifications/001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md)  
**Companions:** [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md), [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md), [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md)  
**Scope:** End-to-end novel production workflow from project creation to export. AI does not run this pipeline unsupervised.

---

## 1. Purpose

The Workflow subsystem is the stage machine of the AI Book Framework. It defines what happens, in what order, with which agents, against which memory, and where a human must decide.

The operator-facing summary remains:

```text
Book Brief → Architect → Research → Outline → Writer → Review → Revision
```

That backbone is defined in [`../../WORKFLOW.md`](../../WORKFLOW.md) and [`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §2.4. This document expands it into operational stages (initialization through export), iteration loops, git practice, draft/revision handling, and recovery.

**Non-goal:** an autonomous “write the novel” run. Agents propose artifacts. Humans approve intent, structure, outline spans, chapter acceptance, canon changes, and publication.

---

## 2. Design rules

1. **Do not skip upstream approvals for convenience.**
2. **Every stage writes files.** Chat is not a stage exit.
3. **Review is a gate**, not a compliment pass.
4. **Side paths must write back** to memory/canon ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §2.4).
5. **One primary agent role per session** ([`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §2).
6. **Fail loudly** on missing inputs or contradictions.
7. **Git records** narrative and process evolution; branches hold variants; memory states chosen truth after merge.

---

## 3. Backbone vs operational stages

Operational stages nest inside the backbone. They do not replace it.

| Backbone stage | Operational stages in this document |
| --- | --- |
| Project setup | 1. Project initialization |
| Book Brief | 2. Concept development |
| Architect | 5. Character development (cast/arcs), 6. Plot development; world needs identified |
| Research | 3. Research, 4. World building (constraints → proposed canon) |
| Outline | 7. Outline, 8. Chapter planning |
| Writer | 9. Drafting, 10. Dialogue refinement, 15. Revision (apply fixes) |
| Review | 11. Editing (find), 12. Continuity checking, 13. Fact checking, 14. Critique |
| Revision loop | 15. Revision (may return to Outline or Architect) |
| After acceptance | 16. Final editing, 17. Manuscript preparation, 18. Publishing/export |

**Order note:** Architect precedes Research on the production path so worldbuilding serves the story rather than drowning it. Research may start as soon as questions exist (soft dependency) but cannot override the brief and cannot lock World as canon before the human accepts architecture for that span. World building and character development are **canon-writing activities** constrained by Architect and Research, not extra plot authorities.

Responsible agents below use kernel names. Capability roles map as in [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §3.2.

---

## 4. Stage specifications

Each stage lists: objective, inputs, responsible agent(s), outputs, required memory, validation, human approval.

### 4.1 Project initialization

| Field | Contract |
| --- | --- |
| **Objective** | Create a repository-native novel workspace that already obeys framework law. |
| **Inputs** | This framework clone or a project derived from it; operator identity. |
| **Responsible agent(s)** | Human (Project Manager capability). No generative agent is required. |
| **Outputs** | Git remote/identity in `PROJECT.md` as used; `book/` present; operator has read vision, architecture, and this workflow. |
| **Required memory** | None yet. Do not copy `examples/` into `book/` as if it were canon. |
| **Validation** | Root operator docs exist; `book/BOOK_BRIEF.md` and `book/PROJECT_MEMORY.md` exist as the live homes (fill comes next). |
| **Human approval** | Human confirms they will run gates; no AI self-start. |

### 4.2 Concept development

| Field | Contract |
| --- | --- |
| **Objective** | Lock what the book is trying to be: Premise, audience, Theme, tone, POV policy, Style intent, Constraints. |
| **Inputs** | Author idea; optional comparable titles as *non-canon* notes. |
| **Responsible agent(s)** | Human owns thesis. Architect may help pressure-test structure implications **only as options**. No Writer mass-draft. |
| **Outputs** | `book/BOOK_BRIEF.md` with required intent sections filled; `Status: Draft` then `Approved`. |
| **Required memory** | Tier A is created here. Project memory may list open concept questions only. |
| **Validation** | Constraints are explicit; non-goals for the book are stated; POV policy is unambiguous. |
| **Human approval** | **Mandatory.** Production architecture and drafting wait on `Approved` brief. |

### 4.3 Research

| Field | Contract |
| --- | --- |
| **Objective** | Produce grounding and speculative-consistency **Constraints** the story must respect. |
| **Inputs** | Approved brief; question list from architecture, outline, or flagged prose; existing research notes. |
| **Responsible agent(s)** | Research. Human directs questions and accepts constraint language. |
| **Outputs** | `book/research/*` with Source / Constraint / Conjecture / Rejected labels and impacted canon paths. |
| **Required memory** | Brief; questions in project memory; relevant World/Character slices if they already exist. |
| **Validation** | Conjecture is not labeled Constraint. Each note names impacted files. |
| **Human approval** | Human accepts which Constraints bind; promotion into canon is a separate Knowledge Keeper/human step. |

### 4.4 World building

| Field | Contract |
| --- | --- |
| **Objective** | Instantiate Setting, World, Locations, Factions, Cultures, Technology, Rules, and systems **needed by this Novel**. |
| **Inputs** | Approved brief; architecture’s “world needs”; Research Constraints. |
| **Responsible agent(s)** | Architect (what plot needs) + Research (what must stay consistent). Human promotes canon. Not the Writer. |
| **Outputs** | Approved files under `book/canon/world/` (and glossary as terms appear). |
| **Required memory** | Tiers A, C (world), D. Project memory holds only unpromoted world flags. |
| **Validation** | Unused encyclopedia pages are not required. Every on-page Location the next outline span needs exists or is explicitly deferred with a flag. |
| **Human approval** | **Mandatory** before treating world files as canon for Outline/Writer. |

### 4.5 Character development

| Field | Contract |
| --- | --- |
| **Objective** | Design the cast, Relationships, wants, voice notes, and who-knows-what needed for the next span. |
| **Inputs** | Approved brief; architecture arcs; world Rules that limit capabilities. |
| **Responsible agent(s)** | Architect (function in Plot/Arcs) + human. Outline later specializes scene pressure. Not a second Plot Designer. |
| **Outputs** | `book/canon/characters/<slug>.md` sheets; relationship and secret labels. |
| **Required memory** | Brief, architecture, world Rules, timeline as it exists. |
| **Validation** | Stable slugs; POV characters match brief; secrets labeled Author/Reader/Character. |
| **Human approval** | **Mandatory** for sheets that Writer will treat as true. |

### 4.6 Plot development

| Field | Contract |
| --- | --- |
| **Objective** | Design acts, major turns, escalation, Story Arcs, and thematic spine. |
| **Inputs** | Approved brief; known Research Constraints; existing canon if any. |
| **Responsible agent(s)** | Architect. |
| **Outputs** | `book/architecture/*` (act map, turns, arcs, open design questions). `Status: Draft` then `Approved` for the span. |
| **Required memory** | Tier A; relevant C/D; project memory for open structural questions. |
| **Validation** | Escalation is stated; Act turns are named; questions are listed rather than silently invented. |
| **Human approval** | **Mandatory** for the novel or named act span before mass outline of that span. |

World and character work (§4.4–4.5) may interleave with plot development as long as architecture remains the structural authority and canon promotions stay human.

### 4.7 Outline

| Field | Contract |
| --- | --- |
| **Objective** | Write Scene contracts for a span: goal, conflict, turn, payoff, POV, continuity hooks. |
| **Inputs** | Approved brief; approved architecture for the span; relevant canon; Research Constraints for claims the span will make. |
| **Responsible agent(s)** | Outline. |
| **Outputs** | `book/outline/*` for the span. |
| **Required memory** | A, C slices, D as needed, architecture, project memory. |
| **Validation** | Every Scene has goal/conflict/turn/payoff; Threads to plant or pay are named; no competing act map. |
| **Human approval** | **Mandatory** for a span before mass drafting that span. |

### 4.8 Chapter planning

| Field | Contract |
| --- | --- |
| **Objective** | Group Scenes into Chapters, name files, and state chapter-level purpose and clock. |
| **Inputs** | Approved outline span (or the same session’s scene contracts if the operator outlines per chapter). |
| **Responsible agent(s)** | Outline (same kernel role; narrower job). |
| **Outputs** | Chapter list with `ch-NN-slug` targets, scene membership, Timeline anchors. |
| **Required memory** | Outline + Timeline. |
| **Validation** | One live path per chapter number; Events the chapter depicts exist or are added to the Timeline as `Planned`. |
| **Human approval** | Covered by outline-span approval unless the operator only approved scenes and not grouping—then approve the chapter plan before drafting. |

### 4.9 Drafting

| Field | Contract |
| --- | --- |
| **Objective** | Produce Chapter prose that satisfies Scene contracts, voice policy, and canon. |
| **Inputs** | Approved outline/chapter plan; brief; canon slices; research constraints for in-scene claims; neighbor chapters if they exist. |
| **Responsible agent(s)** | Writer. |
| **Outputs** | `book/chapters/ch-NN-slug.md` with `Status: Draft`; unpromoted-fact flags; lean project-memory flags. |
| **Required memory** | Retrieval minimum in [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §22. |
| **Validation** | Scene goals met or gaps flagged; no silent retcon; no `Approved` status set by Writer. |
| **Human approval** | Not yet. Human may line-edit at any time but chapter **Done** waits for Review + human accept. |

### 4.10 Dialogue refinement

| Field | Contract |
| --- | --- |
| **Objective** | Improve spoken/voiced exchange without changing contracted turns or canon. |
| **Inputs** | Current chapter Draft; character voice notes; Relationship state. |
| **Responsible agent(s)** | Writer (Dialogue Specialist pass). Split to a specialist agent only if dialogue is a systemic Major defect class. |
| **Outputs** | Edits on the **same** chapter path; diff-reviewable. |
| **Required memory** | Character sheets, Secrets (who can say what). |
| **Validation** | Information leaks match Secrets; voice distinguishable; plot turn unchanged. |
| **Human approval** | Not a separate gate; changes are included in the later chapter accept. Human may request this pass. |

### 4.11 Editing

| Field | Contract |
| --- | --- |
| **Objective** | Find line, copy, clarity, and local structure issues. **Finding is Review. Applying is Writer or human.** |
| **Inputs** | Draft; brief Style; outline (to avoid “fixes” that change the turn). |
| **Responsible agent(s)** | Review (Editor-as-finder). Writer applies. |
| **Outputs** | Review report section with `R`/`S` defects and severities. |
| **Required memory** | Brief style; chapter; outline. |
| **Validation** | Defects are specific and locatable. No wholesale silent regenerate-as-edit. |
| **Human approval** | Human may add taste Notes; acceptance still comes after the full Review verdict and human gate. |

### 4.12 Continuity checking

| Field | Contract |
| --- | --- |
| **Objective** | Detect breaks in facts, Timeline, inventory, geography, and who-knows-what. |
| **Inputs** | Draft; canon slices; Timeline; Threads; neighbor chapters. |
| **Responsible agent(s)** | Review (Continuity Checker pass). |
| **Outputs** | `C` defects; process `X` if unpromoted reusable facts remain. |
| **Required memory** | Tier C slices + production files for the span. |
| **Validation** | Every reusable new fact is flagged or already in canon. Clock matches Timeline. |
| **Human approval** | If the “fix” is a retcon, human must run canon-change protocol ([`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §25) instead of patching only prose. |

### 4.13 Fact checking

| Field | Contract |
| --- | --- |
| **Objective** | Test in-prose claims against Research Constraints and approved World Rules. |
| **Inputs** | Draft; research notes listed for the Scene; world/system files. |
| **Responsible agent(s)** | Review (Fact Checker pass) using Research outputs. Research may be re-invoked if a new question appears (write back). |
| **Outputs** | Defects for constraint violations; new research questions in project memory if needed. |
| **Required memory** | Tiers D and C (rules/systems). |
| **Validation** | Conjecture is not used as a pass. Missing research for a hard claim is a fail or a required side-path. |
| **Human approval** | Human decides whether to amend Constraints/canon or revise prose. |

### 4.14 Critique

| Field | Contract |
| --- | --- |
| **Objective** | Adversarial craft and theme adjudication: pacing, character motivation, thematic drift, exposition. |
| **Inputs** | Draft; brief Theme; architecture spine; outline payoffs. |
| **Responsible agent(s)** | Review (Critic pass). |
| **Outputs** | `H`, `P`, `T`, and remaining `S` defects; taste as `Note`. |
| **Required memory** | Brief, architecture, outline, chapter. |
| **Validation** | Critique is specific and actionable. Critic does not approve the chapter. |
| **Human approval** | Taste Notes may be dismissed by the human; Blocker/Major craft issues must be resolved or explicitly accepted as residual risk in the accept decision. |

Editing, continuity, fact check, and critique may be **one Review report with named passes** ([`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §3.3). They must not be a Writer self-review in the same session that drafted the chapter.

### 4.15 Revision

| Field | Contract |
| --- | --- |
| **Objective** | Repair defects. Update Outline/Canon/Architecture when the defect is not merely wording. |
| **Inputs** | Review report; current Draft; upstream contracts. |
| **Responsible agent(s)** | Writer for prose; Outline if scene intent is wrong; Architect if an act turn is wrong; human for canon. |
| **Outputs** | Updated files on canonical paths; defects marked addressed; possible re-review request. |
| **Required memory** | The defect’s class determines reads (canon for `C`, architecture for `S`, etc.). |
| **Validation** | Structural problems are not “fixed” only in prose. Re-review covers targeted IDs. |
| **Human approval** | Required if Revision needs a canon or brief change. Otherwise human later accepts the chapter. |

### 4.16 Final editing

| Field | Contract |
| --- | --- |
| **Objective** | After chapters in a span are accepted, perform a freeze-oriented polish: consistency of glossary, remaining Minors, voice pass across the span. |
| **Inputs** | Accepted chapters for the span; glossary; style policy; open Minor list. |
| **Responsible agent(s)** | Review (span pass) + Writer (apply) + human. |
| **Outputs** | Polished accepted files; updated glossary; freeze notes in project memory. |
| **Required memory** | Canon + accepted production tree for the span. |
| **Validation** | No new unpromoted facts. Threads planted in the span are logged. |
| **Human approval** | **Mandatory** for act freeze / span freeze. |

### 4.17 Manuscript preparation

| Field | Contract |
| --- | --- |
| **Objective** | Assemble front matter, chapter order, and a single manuscript artifact from **accepted/frozen** files. |
| **Inputs** | Frozen/accepted chapters; brief metadata; optional export module settings. |
| **Responsible agent(s)** | Human + Publisher/export capability when it exists. Not Writer inventing new scenes. |
| **Outputs** | `book/manuscript/` (or module export target) assembled from sources of truth. |
| **Required memory** | Metadata, accepted chapters, glossary for spellings. |
| **Validation** | Assembly does not rewrite Plot or Canon. Order matches the chapter index. |
| **Human approval** | **Mandatory** before treating the assembly as a release candidate. |

### 4.18 Publishing / export

| Field | Contract |
| --- | --- |
| **Objective** | Produce delivery formats (print markdown, DOCX, EPUB, or publisher package) and decide what leaves the repo. |
| **Inputs** | Approved manuscript assembly; human disclosure/ethics decisions. |
| **Responsible agent(s)** | Human. Export tools/Publisher agent may format only. |
| **Outputs** | Export artifacts as the operator chooses; changelog/tag optional (`beta-reader-1`). |
| **Required memory** | None beyond the frozen manuscript line. Do not re-open canon from an export script. |
| **Validation** | No unpublished secrets (author notes, `Author secret` sections) leak into the export. |
| **Human approval** | **Mandatory.** The framework never auto-publishes ([`000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md) §9). |

---

## 5. Iteration loops

The pipeline is linear only at first pass. These loops are explicit and required.

### 5.1 Concept loop

Brief Draft → human revision → `Approved`. Stop production-path Architect approval until green.

### 5.2 Structure loop

Architect Draft → human → `Approved`. If Outline or Review later proves a turn is wrong, return here; do not hide the change in a Scene.

### 5.3 Research / world loop

Question → Research note → human Constraint accept → optional canon promotion → Outline/Writer resume. Mid-draft research is allowed if it writes back.

### 5.4 Outline loop

Scene contracts Draft → human span approval. Review `S` defects may reopen Outline for that chapter without reopening the whole act—unless the turn is an act turn.

### 5.5 Draft–Review–Revise loop (main production loop)

```text
Writer draft
  → Review (edit + continuity + fact + critique passes)
      → Fail or Pass-with-fixes → Writer (and/or Outline/Architect) Revision
          → targeted re-Review
              → Pass → Human accept / reject / request another cycle
```

A chapter may cycle many times. Word count is not an exit criterion.

### 5.6 Canon loop

Unpromoted fact → human promote/strike/defer → cascade ([`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §19 and §25) → continuity Review on dependents.

### 5.7 Act freeze loop

Accepted chapters in act → final editing → human freeze → later edits require an explicit unfreeze decision in memory and git.

### 5.8 Forbidden loops

- Writer drafts and Review-passes in one blended session.
- Export formatting that “smoothes” continuity errors.
- Chat-only revision that never hits the chapter file.

---

## 6. Human decision points (explicit)

AI must stop and wait at least at:

1. Brief approved for production
2. Architecture approved for the novel or named span
3. Outline span approved before mass drafting
4. Canon promotions and retcons
5. Chapter accept / revise / reject after Review
6. Act or manuscript freeze
7. What is exported or published
8. Any conflict between two approved artifacts ([`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §14)

Optional human decisions (not skipable if they change truth): style-pack enablement, whether to split a specialist agent, whether an `exp/` plot branch merges.

The human may also interrupt any stage, edit files directly, and reject a `Pass` on taste.

---

## 7. How Git fits

Git is the novel’s black box recorder ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §10).

| Practice | Workflow use |
| --- | --- |
| Small commits | One concern: brief lock, outline span, ch-07 draft, ch-07 review, canon timeline fix |
| `develop` / trunk | Integration line for active drafting (framework repos may use `develop` for OS work) |
| `draft/ch-NN-…` | Scoped prose work |
| `exp/plot-…` | Competing plot; merge only after human choice **and** memory updates |
| `fix/continuity-…` | Retcon and dependent repairs |
| Diff-driven Review | Prefer inspecting what changed since last acceptable version |
| Tags | Optional `act-1-frozen`, `beta-reader-1` |
| Do not commit | Secrets, huge binary dumps, caches, personal scratch that confuses agents |

**Branches are not memory.** After merge, `book/canon/` and the brief must state the chosen truth. Do not leave two live `ch-07` files.

Framework refactors and manuscript hot paths stay on separate commits/branches ([`001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) §17.1).

---

## 8. Drafts and revisions

- **One live chapter path.** `book/chapters/ch-07-slug.md` is the Draft. Git history is previous Drafts. `ch-07-slug.alt.md` is a candidate until the human replaces the live file.
- **Status lives in the file header**, not in proliferating filenames (`-final`, `-final2`).
- **Review reports** are append-only enough to trace defect IDs (`book/reviews/ch-07-review-01.md`, then `review-02` for re-review).
- **Revision that changes scene intent** updates the outline in the same change set or immediately after, before calling the prose done.
- **Accepted** means human accept after Review `Pass` (or human-accepted residual Minors). Writer never sets this.
- **Frozen** means the span is closed to casual edits; unfreeze is a recorded human decision.

---

## 9. Recovering from mistakes

| Mistake | Recovery |
| --- | --- |
| Drafted without approved outline | Stop. Write or approve Scene contracts. Diff the prose against new contracts; revise or quarantine as `*.alt.md`. |
| Silent retcon in prose | File `C`/`X`. Run canon-change protocol **or** revert prose. Do not leave disagreement. |
| Wrong plot branch merged | `git` revert or reverse merge; restore canon/outline to the intended truth; Review the span. |
| Canon fork (two files disagree) | Fail loudly; human picks the home; deprecate the other ([`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) §20). |
| Session invented facts only in chat | Treat as never happened, or write them through the promotion protocol the same day. |
| Over-scope chapter ruined voice | Revert to last acceptable commit; smaller Writer pass; Review the diff. |
| Research error discovered late | Amend Constraint; cascade Timeline/World; continuity Review dependents; do not “just add a footnote in ch-22.” |
| Accidental framework edit during book work | Revert the framework files; keep book commits separate. |

**Always recoverable if:** files were committed in small units and canon was not only in chat. **Not recoverable by agents alone:** the human’s lost unpublished intent that was never written down.

---

## 10. Example: idea to finished chapter

Pedagogical fixture, not live canon. Same *Helios Wake* through-line as [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §20.

1. **Initialization.** Operator opens the framework in Cursor, confirms `book/` homes exist, sets `PROJECT.md` identity as needed.
2. **Concept.** Human writes the premise (sealed warm hatch; no FTL; close-third Aya) into `book/BOOK_BRIEF.md` and sets `Approved`.
3. **Plot development.** Architect writes a three-act map. Human approves architecture.
4. **Research.** Research writes orbital-decay and power-source Constraints; names `helios-station.md` and `timeline.md` as impacts.
5. **World building.** Human promotes station Rules (spin gravity, cold reactor, battery heat tell) into `book/canon/world/helios-station.md`.
6. **Character development.** Human approves `aya-okoro.md` (no sister).
7. **Outline + chapter planning.** Outline writes ch-01 scene contract (goal/conflict/turn/payoff) and Timeline event `E-04 Station Arrival`. Human approves the ch-01 span.
8. **Git.** Commit brief, architecture, research, canon, outline as separate logical units (or stacked small commits). Branch `draft/ch-01-the-warm-hatch`.
9. **Drafting.** Writer produces `book/chapters/ch-01-the-warm-hatch.md`, flags an invented sister, updates project memory.
10. **Human** rejects the sister. Writer removes the mention (Revision inside drafting).
11. **Dialogue refinement.** Writer pass tightens hatch-radio dialogue without changing the turn.
12. **Review.** One report runs editing, continuity, fact check (heat = batteries), and critique (inventory stall `P-001` Minor). Verdict `Pass-with-fixes`.
13. **Revision.** Writer fixes `P-001` on the same path.
14. **Re-review.** `Pass`.
15. **Human** accepts ch-01. Project memory: focus ch-02; no unpromoted facts. Commit. Merge branch to the integration line if that is the operator’s git practice.
16. **Later.** After more accepted chapters, final editing and act freeze. Manuscript assembly reads accepted files only. Export waits for a human publish decision.

If step 12 had found the hatch heat contradicting a cold-only station without batteries, Review would **Fail**, Research/World would be amended or the image cut, and Writer would not “split the difference.”

---

## 11. Session micro-workflow

Every Cursor session, regardless of stage ([`../../USER_GUIDE.md`](../../USER_GUIDE.md) §6):

1. Name role and span.
2. Load required files.
3. Confirm entrance criteria.
4. Produce or update the output paths.
5. Flag memory promotions; fail on conflicts.
6. Stop at the next human gate.
7. Commit a logical unit.

Until `agents/*.md` files exist, this document plus [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) is the contract for those sessions.

---

## 12. Acceptance and validation criteria

### 12.1 This document is complete when

- All eighteen operational stages have objective, inputs, agents, outputs, memory, validation, and human approval.
- Stages nest in the canonical seven-stage backbone without creating a second plot authority.
- Iteration loops, human gates, git, drafts/revisions, and recovery are specified.
- An idea-to-chapter example names files, gates, and a failure path.
- Unsupervised autonomous generation is excluded.

### 12.2 The Workflow is correctly implemented when

| Check | Pass condition |
| --- | --- |
| Backbone | Brief → Architect → Research → Outline → Writer → Review → Revision is the production path |
| Gates | Mass draft without approved outline is refused; chapter Done requires Review + human accept |
| Memory write-back | Mid-draft research updates `book/research/` and flags canon |
| Role purity | Draft and Review are separate sessions |
| Git | Variants live on branches; chosen truth lives in `book/` after merge |
| Recovery | A silent retcon has a documented repair path |
| Export | Publisher cannot change canon or auto-release |
| Alignment | Human gates match [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) §13; entity homes match [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md) |

### 12.3 Chapter Done (production)

A chapter is done when [`../../USER_GUIDE.md`](../../USER_GUIDE.md) §14 holds: outline satisfied, brief/voice respected, canon aligned or deliberately updated, Review passed (or only residual Minor/Note by human standard), human accept, project memory updated, change committed.

---

## 13. References

- [`../specifications/000_PROJECT_VISION.md`](../specifications/000_PROJECT_VISION.md) — human gates, non-goals
- [`../specifications/001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md) — pipeline, git, quality gates
- [`../../WORKFLOW.md`](../../WORKFLOW.md) — operator backbone
- [`010_AGENT_SYSTEM.md`](010_AGENT_SYSTEM.md) — roles and isolation
- [`020_MEMORY_SYSTEM.md`](020_MEMORY_SYSTEM.md) — what must be true at each stage
- [`030_BOOK_MODEL.md`](030_BOOK_MODEL.md) — entities created per stage
- [`../../USER_GUIDE.md`](../../USER_GUIDE.md) — day-to-day operation

---

## Document Control

| Field | Value |
| --- | --- |
| ID | `040_WORKFLOW` |
| Title | Workflow — AI Book Framework |
| Layer | Subsystem architecture |
| Depends on | `000_PROJECT_VISION`, `001_ARCHITECTURE` |
| Enables | Cursor workflow-gate rules, operator checklists, future `002_WORKFLOW` specification |
| Maintenance rule | Update when stages, gates, or loops change; remain consistent with root `WORKFLOW.md` backbone |

---

*End of document.*
