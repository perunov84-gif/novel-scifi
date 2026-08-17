# Architect — Completion Checklist

**Agent:** Architect  
**Audience:** The human author, and any later agent confirming this stage may hand off  
**Companion files:** [`OUTPUTS.md`](OUTPUTS.md) · [`SYSTEM.md`](SYSTEM.md)

Use this list before treating an Architect run as finished. Unchecked items mean the work is not ready to proceed on the production path.

Mark items only from repository files, not from chat recollection.

---

## How to use this checklist

- The operator (or a later session named as a different role) reads `book/architecture/*` and this list.
- Production-path Outline must not start until **Ready to proceed** is all yes *and* the human has set architecture `Approved`.
- Research may start on listed questions while architecture is still `Draft` if the human wants grounding before approval; those notes still cannot override the brief.

---

## Preconditions

- [ ] `book/BOOK_BRIEF.md` exists, required intent sections are filled, and `Status: Approved` for production-path architecture.
- [ ] `book/PROJECT_MEMORY.md` exists and was read.
- [ ] The session named **Architect** as the primary role and named a **span**.
- [ ] Required inputs in [`INPUTS.md`](INPUTS.md) were loaded; `examples/` was not used as canon.
- [ ] If this is re-entry, the reason is recorded (Review defect ID, human decision, or conflicting approved artifacts) and the live architecture path is the one being edited.

---

## Work product

- [ ] Act map exists under `book/architecture/` on a stable path (not a uniquely named scratch file).
- [ ] Each act in the span has a dramatic job, a named major turn, and an ending state.
- [ ] Escalation is explicit (cost, stakes, or pressure increases).
- [ ] Thematic spine is written and points at Theme in the brief.
- [ ] Story arcs are placed at act scale (not scene-by-scene contracts).
- [ ] Structural constraints are testable statements Outline can violate or obey.
- [ ] Required knowledge is a question list with hypothetical impacted canon paths.
- [ ] Unresolved decisions are listed as open questions with IDs; no missing turn was filled by implication.
- [ ] Header metadata includes `Status: Draft` (unless the human already approved in a prior session), `Owner:`, `Stage: Architect`, and `Span:`.

---

## Role boundaries

- [ ] No finished chapter prose was written in this session.
- [ ] No `book/outline/*` scene contracts were written in this session.
- [ ] No `book/research/*` note bodies were written in this session (questions only, in architecture).
- [ ] No `book/canon/` file was edited as approved truth.
- [ ] Architecture `Status` was not set to `Approved` by the agent.
- [ ] Brief intent was not rewritten.

---

## Memory and canon

- [ ] New world/cast needs are labeled as proposals or questions, not present-tense canon.
- [ ] Existing research **Constraint** lines and approved canon were not contradicted; remaining conflicts are named as blockers.
- [ ] `PROJECT_MEMORY.md` was updated leanly (focus, open structural questions, next gate) without duplicating the act map.
- [ ] Temporary chat decisions that matter were written into architecture files.
- [ ] At most one live act map; unused options are `*.alt.md` or on an `exp/` branch.

---

## Brief and constraint conformance

- [ ] POV policy in the brief is not broken by the act design (for example, no sudden omniscient act).
- [ ] Hard constraints (rating, no-FTL, content boundaries, book non-goals) still hold.
- [ ] Premise is served; the architecture is not a different book with the same title.

---

## Handoff

- [ ] Handoff packet is writable from files: role, paths, statuses, red/green preconditions for Research and Outline, open questions, human gate.
- [ ] Research knows which `K-*` / `Q-*` items to take (or the list is empty for a non-grounded span).
- [ ] Outline is explicitly **not** cleared for mass scene contracts until human approval of this span.
- [ ] If chapters or outlines already exist against an old turn, the checklist records that they are invalid until cascade.

---

## Ready to proceed

Treat the Architect stage as ready to offer the human when **Preconditions**, **Work product**, **Role boundaries**, **Memory and canon**, **Brief and constraint conformance**, and **Handoff** are checked.

Treat the **pipeline** as ready for Outline of this span only when the human has also:

- [ ] Set architecture for the span to `Approved`.
- [ ] Directed Research on any blocking `K-*` items, or explicitly accepted the risk of outlining without them (soft dependency).
- [ ] Promoted any world/cast facts that Outline will be required to treat as true.

If any Blocker-level conflict remains (two approved artifacts disagree; brief vs design), **do not proceed**. Escalate to the human ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §14).

---

## References

- [`OUTPUTS.md`](OUTPUTS.md) — what “work product” means
- [`../research/CHECKLIST.md`](../research/CHECKLIST.md) — typical next evidence stage
- [`../outline/CHECKLIST.md`](../outline/CHECKLIST.md) — must not run mass outline on `Draft` architecture
- [`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.6, §5.2, §6
- [`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §18.2
