# Outline — Completion Checklist

**Agent:** Outline  
**Audience:** The human author, and Writer/Review confirming the span may be drafted or audited  
**Companion files:** [`OUTPUTS.md`](OUTPUTS.md) · [`SYSTEM.md`](SYSTEM.md)

Use this list before treating an Outline run as finished. Unchecked items mean Writer must not mass-draft the span.

Mark items only from repository files.

---

## How to use this checklist

- Writer’s production path requires **Ready to proceed** *and* human `Approved` on this span.
- Re-outline after Review uses the same list; defect IDs being addressed should appear in the outline or project memory.
- Research may be invoked from a red claims item; that is a new session, then return here.

---

## Preconditions

- [ ] `book/BOOK_BRIEF.md` is `Approved` and was loaded.
- [ ] Architecture for **this span** is `Approved` (not `Draft`).
- [ ] `book/PROJECT_MEMORY.md` was read.
- [ ] Relevant canon slices for on-page characters, locations, threads, and timeline window were loaded **or** explicitly deferred with flags.
- [ ] Claims that need grounding have accepted Constraints, or those claims were removed/deferred.
- [ ] Session named **Outline** and a **span**; no chapter draft in this session.
- [ ] `examples/` was not used as this book’s plot.

---

## Work product

- [ ] Outline files exist under `book/outline/` on stable paths.
- [ ] Every scene has viewpoint, location, timeline anchor, goal, conflict, turn, and payoff.
- [ ] Chapter plan names `ch-NN-slug` targets, scene membership, chapter job, and clock.
- [ ] One live plan per chapter number.
- [ ] Arcs and Threads (plant / pay / avoid) are named where the scene touches them.
- [ ] Continuity hooks: in-state and out-state are stated.
- [ ] Dependencies on prior scenes/events are named.
- [ ] Claims list is present (including “none”).
- [ ] Structural constraints from architecture are not violated (e.g. betrayal placement, no-FTL).
- [ ] Header: `Status: Draft` (agent), `Owner:`, `Stage: Outline`, `Span:`, architecture pointer.

---

## Role boundaries

- [ ] No `book/chapters/*` prose.
- [ ] No competing act map under `book/architecture/` or inside the outline.
- [ ] No canon promotions.
- [ ] No research note bodies (questions only).
- [ ] Outline `Status` not set to `Approved` by the agent.
- [ ] Open Architect questions were not answered by implication.

---

## Memory and canon

- [ ] Scene-local new facts are flagged, not written as present-tense canon.
- [ ] Who-knows-what matches character sheets (or the conflict is a named blocker).
- [ ] Planned events are listed for Timeline; canon `timeline.md` was not silently rewritten as approved history.
- [ ] `PROJECT_MEMORY.md` is lean (span status, questions, next gate).
- [ ] Chat-only beats were written into the outline or discarded.

---

## Handoff

- [ ] Handoff packet is writable from files: role, paths, status, Writer red/green, open “do not invent” items, human gate.
- [ ] Writer is explicitly blocked until human approval of this span.
- [ ] Review will have a contract to audit (scene IDs, payoffs, claims).
- [ ] If old drafts exist against a previous contract, they are marked invalid until revision.

---

## Ready to proceed

Treat the Outline **session** as complete when **Preconditions**, **Work product**, **Role boundaries**, **Memory and canon**, and **Handoff** are checked.

Treat the **pipeline** as ready for Writer on this span only when the human has also:

- [ ] Set this outline span to `Approved`.
- [ ] Promoted or struck any facts the scenes will treat as true.
- [ ] Accepted any last-minute Research Constraints those claims require.

If the span needs a different act turn than approved architecture, **do not proceed**. Escalate to the human for Architect re-entry ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §14).

---

## References

- [`OUTPUTS.md`](OUTPUTS.md)
- [`../architect/CHECKLIST.md`](../architect/CHECKLIST.md) — upstream gate
- [`../writer/CHECKLIST.md`](../writer/CHECKLIST.md) — must refuse unapproved spans
- [`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.7–§4.8, §6
- [`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §18.2
