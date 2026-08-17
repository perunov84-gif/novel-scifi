# Review — Completion Checklist

**Agent:** Review  
**Audience:** The human author, and Writer/Outline confirming the gate result is usable  
**Companion files:** [`OUTPUTS.md`](OUTPUTS.md) · [`SYSTEM.md`](SYSTEM.md)

Use this list before treating a Review run as finished. Unchecked items mean the pipeline must not treat the chapter as gated.

Mark items only from repository files.

---

## How to use this checklist

- Writer uses **Ready to proceed** to know whether to apply fixes, wait for Outline/Architect, or wait for human accept.
- Human accept is **never** implied by this list alone.
- Re-review uses the same list; additionally confirm prior IDs.

---

## Preconditions

- [ ] Session named **Review** and a span; this session did not draft the span.
- [ ] Draft (live path) was loaded.
- [ ] Outline contracts for the span were loaded (or missing outline was filed as `X` Blocker).
- [ ] Brief was loaded (POV, Style, Theme, hard limits).
- [ ] Canon slices for touched entities were loaded (or the miss is a filed defect).
- [ ] Research notes were loaded for claimed facts, or fact pass states insufficient evidence.
- [ ] `PROJECT_MEMORY.md` was read (unpromoted flags, risks).
- [ ] Context did not include a Writer goal to “make this chapter succeed.”
- [ ] `examples/` was not used as this book’s continuity.

---

## Work product

- [ ] A report exists under `book/reviews/` with a stable name (`ch-NN-review-01.md` or next index / span name).
- [ ] Exactly one verdict: `Pass` / `Fail` / `Pass-with-fixes`.
- [ ] Named passes were run or explicitly marked N/A with reason (editing, continuity, fact, critique).
- [ ] Every defect has ID, class (`C S H P R T X`), severity, location, owner, action.
- [ ] Unpromoted reusable facts were checked (`X`, and `C` if contradicting canon).
- [ ] `Note` items are labeled taste, not hidden Blockers.
- [ ] Re-review instructions name IDs (or “none — Pass”).
- [ ] Header includes `Stage: Review`, `Target:`, `Verdict:`.

---

## Role boundaries

- [ ] Chapter prose file was **not** patched in this session.
- [ ] Canon files were **not** edited to hide defects.
- [ ] Outline/architecture were **not** silently rewritten.
- [ ] No research note body was written (questions flagged only).
- [ ] Chapter `Status` was not set to `Approved` or `Accepted`.
- [ ] No regenerated chapter was substituted for the report.
- [ ] Illustrative snippets, if any, are labeled not applied.

---

## Verdict integrity

- [ ] `Pass` implies no remaining Blocker or Major.
- [ ] `Fail` has at least one Blocker or unaddressed Major, with a next owner.
- [ ] `Pass-with-fixes` has a bounded list and a re-review plan.
- [ ] Conjecture was not used to Pass a factual claim.
- [ ] If architecture vs outline vs prose conflict, both sources are named and the verdict does not “average” them.

---

## Memory and process

- [ ] `PROJECT_MEMORY.md` updated leanly (unresolved IDs, next gate) without duplicating the report.
- [ ] Chat-only judgments were written into the report or discarded.
- [ ] If Research is needed, that is a named side path—not a fake Pass.

---

## Handoff

- [ ] Handoff packet is writable from files: role, report path, verdict, open IDs, next role, human gate.
- [ ] If `Fail` / `Pass-with-fixes` and owner is Writer: Writer’s next session is a revision, not a self-review.
- [ ] If owner is Outline or Architect: human is named as the gate before those files change authority.
- [ ] If `Pass`: next gate is **human** accept/reject, not Publisher, not Writer certification.

---

## Ready to proceed

Treat the Review **session** as complete when **Preconditions**, **Work product**, **Role boundaries**, **Verdict integrity**, **Memory and process**, and **Handoff** are checked.

| Verdict | Pipeline may |
| --- | --- |
| `Fail` | Start Writer and/or Outline/Architect as named; do not accept the chapter |
| `Pass-with-fixes` | Start Writer on the listed IDs; then re-review; do not accept yet |
| `Pass` | Human accept / revise / reject; Writer must not set Approved |

A chapter is **Done** only when this gate has a qualifying result **and** the human accepts ([`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §12.3).

---

## References

- [`OUTPUTS.md`](OUTPUTS.md)
- [`../writer/CHECKLIST.md`](../writer/CHECKLIST.md) — must not self-Pass
- [`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.11–§4.15, §5.5, §6
- [`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §18.2–§18.4
- [`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §15
