# User Guide

Operator manual for the AI Book Framework.

This guide explains how to run the system day to day inside Cursor. It assumes you have completed [`QUICK_START.md`](QUICK_START.md). Normative law lives in [`docs/specifications/`](docs/specifications/); if this guide and a specification ever disagree, the specification wins.

---

## 1. What you are operating

The framework is a **literary operating system** implemented as a git repository:

- **Knowledge** — specs, guides, prompts, templates, examples
- **Memory** — brief, project memory, canon, research (under `book/` when populated)
- **Production** — architecture notes, outlines, chapters, reviews
- **Intelligence** — specialized agents and Cursor rules (added in later sprints)
- **Operations** — workflow gates, quality standards, changelog, commits

AI models are workers. You are the executive producer. The repository is the product and the process record.

Deep design: [`docs/specifications/001_ARCHITECTURE.md`](docs/specifications/001_ARCHITECTURE.md).  
Mission and principles: [`docs/specifications/000_PROJECT_VISION.md`](docs/specifications/000_PROJECT_VISION.md).

---

## 2. Mental model

### 2.1 Artifacts over chat

If a decision must survive the next session, it belongs in a file. Chat may accelerate thinking; only markdown (and git history) create institutional memory.

### 2.2 Downward authority

Unless you explicitly revise upstream documents:

```text
Book Brief → Architecture → Outline → Chapter prose
```

Canon and project memory constrain all generative stages. Review files defects; it does not silently rewrite canon.

### 2.3 Role purity

Do not ask one undifferentiated chat to invent world rules, draft the chapter, and certify its own quality. Separate stages:

| Stage | Job |
| --- | --- |
| Brief | Capture intent, tone, audience, hard constraints |
| Architect | Acts, turns, thematic spine |
| Research | Grounding and speculative consistency constraints |
| Outline | Scene contracts (goal, conflict, turn, payoff) |
| Writer | Prose under constraints |
| Review | Adversarial continuity and craft adjudication |

Summary reference: [`WORKFLOW.md`](WORKFLOW.md).

### 2.4 Human gates

You personally approve:

1. Book brief readiness for production
2. Architectural direction for the novel (or act)
3. Outline spans before mass drafting
4. Chapter accept / revise / reject after review
5. Canon retcons

---

## 3. Sprint 1 operating reality

Sprint 1 ships the **kernel documentation and directory shape**. You should use it to:

- Align collaborators on vision and architecture
- Practice artifact-first habits in Cursor
- Prepare for agent contracts, rules, and book templates in later sprints

You should **not** expect Sprint 1 alone to provide a complete one-command novel pipeline. Agent files and book production files are intentionally deferred.

Track releases in [`CHANGELOG.md`](CHANGELOG.md).

---

## 4. Recommended reading order for new operators

1. [`QUICK_START.md`](QUICK_START.md)
2. [`README.md`](README.md)
3. [`WORKFLOW.md`](WORKFLOW.md)
4. [`docs/specifications/000_PROJECT_VISION.md`](docs/specifications/000_PROJECT_VISION.md) — sections on principles, non-goals, success criteria
5. [`docs/specifications/001_ARCHITECTURE.md`](docs/specifications/001_ARCHITECTURE.md) — memory system, document hierarchy, naming, quality standards
6. This user guide (remaining sections)

---

## 5. Repository tour

### 5.1 Root operator surfaces

| File | Use when |
| --- | --- |
| `README.md` | You need the public overview and map |
| `QUICK_START.md` | You need the shortest path |
| `USER_GUIDE.md` | You are running or teaching the process |
| `WORKFLOW.md` | You need the stage order at a glance |
| `PROJECT.md` | You need project identity metadata |
| `CHANGELOG.md` | You need version history |
| `LICENSE` | You need license terms |
| `AGENTS.md` | You need the agent index (populated later) |

### 5.2 Framework knowledge

| Path | Role |
| --- | --- |
| `docs/specifications/` | Canonical contracts (`000`, `001`, …) |
| `docs/guides/` | Practical guides subordinate to specs |
| `docs/decisions/` | ADRs for significant design choices |

### 5.3 Extensibility trees

| Path | Role |
| --- | --- |
| `prompts/` | Reusable task playbooks per role |
| `templates/` | Blank schemas for briefs, chapters, reviews, etc. |
| `examples/` | Teaching fixtures; never live canon |
| `modules/` | Optional genre, style, export, series packs |
| `tools/` | Optional linters and manuscript assemblers |

Each of these directories includes a README describing what belongs there and what must not.

### 5.4 Runtime and production trees

| Path | Role |
| --- | --- |
| `agents/` | Role contracts (later sprint) |
| `.cursor/rules/` | Persistent Cursor constraints (later sprint) |
| `book/` | Active novel state (content in later sprints) |

---

## 6. How to run a Cursor session (future-ready discipline)

Use this session pattern even before specialized agent files exist. When agent specs arrive, the same pattern maps onto them directly.

### 6.1 Start of session

1. Name the stage and role (“Outline Act II”, “Review chapter 7 continuity”).
2. Open or `@`-reference required upstream files.
3. Confirm preconditions (for example: brief approved before architecture; outline approved before drafting).
4. State the output file path you expect the model to write or update.

### 6.2 During session

1. Prefer edits to durable files over long chat essays.
2. When a new reusable fact appears, flag it for canon/memory instead of leaving it only in prose.
3. If inputs conflict, stop and resolve the documents—do not paper over contradictions in stylish prose.
4. Keep scope narrow: one stage, one span (scene, chapter, or act slice).

### 6.3 End of session

1. Verify the intended files changed.
2. Update working memory with current focus, open questions, and risks (when `book/PROJECT_MEMORY.md` is in use).
3. Commit a logical unit with a clear message (see §8).
4. Note the next stage entrance criteria before closing.

---

## 7. Continuity habits

Continuity failure is the primary novel-scale defect. Architecture defines memory tiers; operators must feed them.

### 7.1 Intent memory

The book brief holds premise, genre, audience, themes, tone, POV policy, and hard boundaries. Change it deliberately. Downstream work may need re-validation after brief changes.

### 7.2 Working memory

Project memory stays **short and current**: active conflicts, recent decisions, open questions. Long-term facts belong in canon registries, not an endless scratchpad.

### 7.3 Canon memory

Characters, timeline, world rules, open threads, and glossary entries are source-of-truth for story facts. Prose that invents a reusable fact must either update canon or lose the fact.

### 7.4 Research memory

Research notes distinguish grounded constraints from speculation. Link research outputs to the canon files they affect.

Full model: Architecture §7 (Memory System) in [`001_ARCHITECTURE.md`](docs/specifications/001_ARCHITECTURE.md).

---

## 8. Git practice for novel work

Git is part of the craft loop, not only engineering ceremony.

### 8.1 Commit small, reviewable units

Good examples:

- Update brief: lock close-third POV on the protagonist
- Outline chapters 12–14 escalation
- Draft chapter 07 (work in progress)
- Review chapter 07: file continuity defects
- Canon: fix Station Arrival timeline

Avoid mixing unrelated framework refactors with manuscript hot paths in one commit.

### 8.2 Branch for experiments

Use branches for alternate endings or competing plot directions. After you choose, merge the winner and update canon/brief/outline so memory matches reality. Branches hold variants; memory states the chosen truth.

### 8.3 Diff-driven revision

When revising, inspect the diff against the last acceptable version. Review both prose quality and accidental canon changes.

---

## 9. Quality standards operators should enforce

### 9.1 Stage gates (practical)

| Before you… | You need… |
| --- | --- |
| Approve architecture | A coherent, constrained brief |
| Mass-outline an act | Approved architectural direction for that span |
| Draft a chapter | Scene contracts for that chapter |
| Call a chapter done | Review against brief, outline, continuity, and craft |
| Retcon a fact | Explicit canon (and maybe brief) updates |

### 9.2 Defect mindset

Prefer specific, actionable defects over vague praise or vague dislike. Architecture recommends taxonomy tags such as Continuity (C), Structure (S), Character (H), Pacing (P), Prose (R), Theme (T), and Process (X), with severities Blocker / Major / Minor / Note.

### 9.3 Anti-slop stance

Unless a style pack explicitly allows a mode, push for concrete specificity, character-particular diction, and motivated exposition. “Generically beautiful” prose that breaks canon is still a failure.

---

## 10. Using documentation and extensions correctly

### 10.1 Authority stack (operator view)

1. Your recorded decisions in book artifacts
2. Foundational specs (`000`, `001`, …)
3. Approved book contracts (brief, architecture, outlines)
4. Canon and memory registries
5. Cursor rules (when present)
6. Agent specs (when present)
7. Prompts, templates, examples, guides
8. Chat (no authority unless written back)

### 10.2 Templates and examples

- Templates define required shape for new artifacts.
- Examples teach format; they are never authority for your novel’s facts.
- Copy templates into `book/` paths when that sprint lands; do not turn the template library itself into your manuscript.

### 10.3 Modules

Genre packs, style packs, and export tools must plug into the kernel. They must not create a second brief, a second memory authority, or a bypass around review gates. See `modules/README.md`.

---

## 11. Common failure modes

| Failure | What it looks like | What to do instead |
| --- | --- | --- |
| Chat amnesia | Reinvented facts next session | Write decisions into brief/memory/canon |
| Role collapse | One chat invents, drafts, and self-approves | Sequence stages; separate review |
| Skipped brief | Pretty chapters with no thesis | Stop and complete intent contracts |
| Silent retcon | Chapter changes past without updates | Patch canon + timeline; file continuity defect |
| Memory bloat | Unreadable working memory | Promote stable facts to canon; keep working memory lean |
| Process theater | Docs exist but are ignored | Make stage exits require real files |

---

## 12. Collaboration notes

When more than one person touches the repository:

- Keep pull requests scoped (one chapter, one canon domain, or one framework concern).
- Require continuity notes for canon-affecting changes.
- Do not merge silent retcons.
- Prefer document ownership by domain (characters vs world vs timeline) as the book grows.

---

## 13. Ethics and authorship

- You own the creative thesis, publication decisions, and disclosure obligations for AI assistance in your market.
- Respect copyright and research ethics; do not treat the framework as a plagiarism pipeline.
- Honor content boundaries recorded in the brief; agents and collaborators must be able to discover them from files.

---

## 14. What “done” means for a chapter (target process)

A chapter is done when all of the following are true:

1. It satisfies its outline scene contracts.
2. It respects brief constraints and voice policy.
3. Continuity aligns with canon, or canon was deliberately updated.
4. Review has passed (or remaining items are only Minor/Note by your standard).
5. You, the human, accept it.
6. Working memory reflects any new open threads or decisions.
7. The change is committed in git.

Until review tooling and templates fully land, keep the same bar using manual checklists derived from Architecture §18.

---

## 15. Getting help inside the repo

| Question | Where to look |
| --- | --- |
| Why does this system exist? | `docs/specifications/000_PROJECT_VISION.md` |
| Where should a new file live? | `docs/specifications/001_ARCHITECTURE.md` |
| What is the stage order? | `WORKFLOW.md` |
| How do I start today? | `QUICK_START.md` |
| What shipped in this version? | `CHANGELOG.md` |
| What is licensed? | `LICENSE` |

---

## 16. Closing discipline

The framework rewards operators who treat writing like professional production without draining it of art: stabilize continuity and structure so you can take sharper creative risks where they matter.

Write the truth into the repository. Advance one gate at a time. Keep the human in charge.
