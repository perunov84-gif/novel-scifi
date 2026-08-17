# Research — Completion Checklist

**Agent:** Research  
**Audience:** The human author, and any later agent confirming grounding may be consumed  
**Companion files:** [`OUTPUTS.md`](OUTPUTS.md) · [`SYSTEM.md`](SYSTEM.md)

Use this list before treating a Research run as finished. Unchecked items mean Outline/Writer must not treat new lines as binding Constraints.

Mark items only from repository files.

---

## How to use this checklist

- Production-path consumers need **Ready to proceed** plus human acceptance of Constraint language.
- Architect may have listed questions while architecture was still `Draft`; Research can still complete notes. Those notes do not approve architecture.
- Mid-pipeline Research uses the same checklist; then return to the calling stage (Outline, Writer, or Review) in a **new** session.

---

## Preconditions

- [ ] `book/BOOK_BRIEF.md` was loaded; hard limits are visible.
- [ ] Production-path binding notes require brief `Status: Approved`.
- [ ] A named question list exists (Architect `K-*`/`Q-*`, outline claims, Writer/Review flag, or human topic).
- [ ] Existing `book/research/` files for this topic were read; this run updates the same path.
- [ ] The session named **Research** as the primary role; it did not also draft or self-review prose.
- [ ] `examples/` was not used as this book’s sources.

---

## Work product

- [ ] A stable file exists under `book/research/` for the topic.
- [ ] Questions are restated with IDs.
- [ ] Findings are separated into **Source**, **Constraint**, **Conjecture**, and **Rejected** as applicable.
- [ ] No unlabeled paragraph is carrying a binding rule.
- [ ] Each Constraint is a testable present-tense rule.
- [ ] Uncertainty is stated where evidence is thin.
- [ ] Impacted canon paths are listed (including files that would be created on promotion).
- [ ] Header includes `Status: Draft` (agent), `Owner:`, `Stage: Research`, and question IDs.
- [ ] Optional canon sentences, if any, are marked `Proposal:` and not written into `book/canon/`.

---

## Role boundaries

- [ ] No `book/canon/` silent edits.
- [ ] No act map or scene-contract rewrite.
- [ ] No chapter prose rewrite.
- [ ] No Review verdict (`Pass` / `Fail` / `Pass-with-fixes`).
- [ ] Brief themes and hard limits were not overridden.
- [ ] Conjecture was not labeled Constraint to unblock a draft.

---

## Memory and canon

- [ ] Research remains non-canon until human promotion ([`../../docs/architecture/020_MEMORY_SYSTEM.md`](../../docs/architecture/020_MEMORY_SYSTEM.md) §10).
- [ ] Conflicts with existing Constraints, brief, or approved canon are named as blockers, not averaged.
- [ ] `PROJECT_MEMORY.md` has lean flags (promotion pending, constraint risk), not a paste of the note.
- [ ] Chat-only findings were written into the note or treated as never happened.

---

## Evidence hygiene

- [ ] Sources are not overclaimed.
- [ ] Model recollection without a Source is Conjecture or omitted.
- [ ] No invented citations.
- [ ] Copyrighted source text is not dumped as original research prose.
- [ ] Author stipulations are labeled as such (they can become Constraints if the human wants them binding).

---

## Handoff

- [ ] Handoff packet is writable from files: role, paths, status, which Constraints await human accept, uncertainty, next gate.
- [ ] Outline/Writer are told not to use Conjecture as clock, capability, or World Rule.
- [ ] Review fact-check is pointed at this topic file when the span’s claims use it.
- [ ] If this was a side path, the calling stage is named (e.g. return to Writer after C-R2 accept).

---

## Ready to proceed

Treat the Research **session** as complete when **Preconditions**, **Work product**, **Role boundaries**, **Memory and canon**, **Evidence hygiene**, and **Handoff** are checked.

Treat Constraints as **binding on later agents** only when the human has also:

- [ ] Accepted the Constraint language (note or equivalent recorded decision).
- [ ] Either promoted story-world facts into canon, struck them, or left them explicitly deferred in project memory.

If evidence is insufficient for a load-bearing plot claim, **do not proceed** as if the claim were grounded. Escalate ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §16).

---

## References

- [`OUTPUTS.md`](OUTPUTS.md)
- [`../architect/CHECKLIST.md`](../architect/CHECKLIST.md) — typical question source
- [`../outline/CHECKLIST.md`](../outline/CHECKLIST.md) — must not outline technical claims against Conjecture
- [`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.3, §5.3
- [`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §18.2
