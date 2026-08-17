# Writer — Completion Checklist

**Agent:** Writer  
**Audience:** The human author, and Review confirming the draft is ready to adjudicate  
**Companion files:** [`OUTPUTS.md`](OUTPUTS.md) · [`SYSTEM.md`](SYSTEM.md)

Use this list before treating a Writer run as finished. Unchecked items mean Review should not start—or should Fail on process (`X`).

Mark items only from repository files.

---

## How to use this checklist

- This checklist measures **conformance and handoff**, not literary merit. Review owns adjudication.
- A revision pass uses the same list plus **Defect application**.
- Passing this list does **not** mean the chapter is Done.

---

## Preconditions

- [ ] Brief is `Approved` and was loaded (POV and Style included).
- [ ] Outline span for this chapter is `Approved`.
- [ ] Canon slices for on-page characters, location/Rules, Timeline window, and relevant Threads were loaded.
- [ ] Research Constraints were loaded for every in-scene technical/historical claim (or claims section is “none”).
- [ ] Neighbor chapters loaded if they exist and this chapter must join them.
- [ ] If this is revision: Review report (or human list) with IDs was loaded.
- [ ] Session named **Writer** and a chapter span; not also Review.
- [ ] `examples/` was not used as this book’s facts or as substitute canon.

---

## Work product

- [ ] Live file exists at `book/chapters/ch-NN-slug.md` (one path per number).
- [ ] `Status: Draft` (not `Approved`, `Accepted`, or `Frozen`).
- [ ] Header includes outline-ref and POV.
- [ ] Prose implements the contracted goal, conflict, turn, and payoff **or** the file flags why it could not and stops.
- [ ] Unpromoted reusable facts are listed (or explicitly “none”).
- [ ] Claims used cite Constraint IDs where applicable.
- [ ] Alternates, if any, are `*.alt.md` or branch-only—not a second live chapter.

---

## Role boundaries

- [ ] No Review report was written.
- [ ] No self-verdict (`Pass`, “continuity OK,” “ready to publish”).
- [ ] No canon file edited as truth.
- [ ] No outline or architecture rewrite.
- [ ] No new research note body (flags/questions only).
- [ ] Contracted turn was not silently replaced.

---

## Memory and canon

- [ ] Who-knows-what in prose matches sheets (or a blocker was raised and work stopped).
- [ ] Timeline/clock in prose matches the slice (or work stopped).
- [ ] World Rules (e.g. no FTL, power/heat Constraints) are not violated.
- [ ] Secrets labeled Author/Reader/Character are not leaked through POV.
- [ ] `PROJECT_MEMORY.md` has lean flags only (new facts, leftover threads, next gate = Review).
- [ ] Chat-only inventions were written as flags or removed.

---

## Defect application (revision sessions only)

- [ ] Each targeted Blocker/Major (and listed Minor in a `Pass-with-fixes`) is addressed in the live file **or** explicitly blocked with a reason (e.g. needs Outline).
- [ ] Structural `S` defects that are act-turn problems were **not** “fixed” only in prose.
- [ ] Header or short list names the Review file and IDs addressed.
- [ ] No wholesale regenerate used to dodge the repair list unless the human requested a full rewrite on the same path.

---

## Handoff

- [ ] Handoff packet is writable from files: role, path, `Draft`, flags, claims, next role = Review.
- [ ] Review can load the draft **and** the constraints that bound it (outline, brief, canon slices, research).
- [ ] Human is not told the chapter is Done.

---

## Ready to proceed

Treat the Writer **session** as complete when **Preconditions**, **Work product**, **Role boundaries**, **Memory and canon**, **Handoff**, and (if applicable) **Defect application** are checked.

Treat the chapter as ready for **Review** when that session is complete.

Treat the chapter as **Done** only when Review has a qualifying verdict **and** the human accepts ([`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §12.3). Writer never checks that box.

If approved outline is missing, **do not proceed**. Refuse ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §10).

---

## References

- [`OUTPUTS.md`](OUTPUTS.md)
- [`../outline/CHECKLIST.md`](../outline/CHECKLIST.md) — upstream gate
- [`../review/CHECKLIST.md`](../review/CHECKLIST.md) — next gate
- [`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.9, §5.5, §12.3
- [`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §18.2
