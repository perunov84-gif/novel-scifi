# Review — Outputs

**Agent:** Review  
**Audience:** Writer (repair), Outline/Architect (escalation), human (accept/reject)  
**Companion files:** [`SYSTEM.md`](SYSTEM.md) · [`INPUTS.md`](INPUTS.md) · [`CHECKLIST.md`](CHECKLIST.md)

Outputs live under `book/reviews/` ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §6).

---

## Expected outputs

| Output | Owner | Purpose |
| --- | --- | --- |
| `book/reviews/ch-NN-review-01.md` (then `review-02` for re-review) | Review writes | Durable adjudication |
| Defect list with IDs, taxonomy, severity, location, recommended revision | Review writes | Executable repair / escalation |
| Named passes (editing, continuity, fact, critique) | Review writes | Role completeness without extra agents |
| Verdict `Pass` / `Fail` / `Pass-with-fixes` | Review writes | Gate status — **not** human acceptance |
| Lean `PROJECT_MEMORY.md` flags | Review may write | Unresolved defects, freeze risks, next gate |

**Does not write:** silent chapter patches; canon “fixes”; human `Approved` on the chapter; research note bodies; a regenerated chapter as the review artifact.

Span-level reports (e.g. `book/reviews/act-02-continuity-pass.md`) are allowed for freeze-oriented passes ([`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §16.3).

---

## Output format

Markdown. Numbered reports so IDs remain traceable:

```text
book/reviews/ch-07-review-01.md
book/reviews/ch-07-review-02.md
```

Recommended shape:

1. Header metadata.
2. **Scope** — path, outline-ref, passes run.
3. **Verdict** — one of the three values, with a one-sentence reason.
4. **Defects** — table or list: ID, class, severity, location, description, recommended owner (Writer / Outline / Architect / Research / Human), recommended action.
5. **Unpromoted facts** — confirm flagged or file `X`.
6. **Re-review instructions** — which IDs must be rechecked.
7. **Notes** — taste only, labeled `Note`.

Illustrative rewrite snippets, if any, must be marked `Illustration only — not applied`.

---

## Required metadata

| Field | Review sets | Notes |
| --- | --- | --- |
| `Status:` | `Review` (the report) | The **chapter** stays `Draft` until human accept |
| `Owner:` | Author | |
| `Stage:` | Review | |
| `Target:` | Chapter or span path | |
| `Verdict:` | `Pass` / `Fail` / `Pass-with-fixes` | |
| `Passes:` | e.g. Editing, Continuity, Fact, Critique | |

Defect IDs follow [`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §16.7: `C-014`, `P-003`, etc.

---

## Quality requirements

| Verdict | Meaning ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §15) |
| --- | --- |
| **Pass** | No Blocker/Major remain; Minors listed; human may still reject on taste |
| **Fail** | Blocker or unaddressed Major; Writer and/or Outline (or Architect) must return |
| **Pass-with-fixes** | Bounded repair list; re-review those items |

Every defect must be:

- classed (`C` continuity, `S` structure, `H` character, `P` pacing, `R` prose, `T` theme, `X` process);
- severitized (`Blocker`, `Major`, `Minor`, `Note`);
- locatable (heading, paragraph cue, or diff hunk);
- actionable (who does what).

Unpromoted reusable facts → `X`; if they contradict canon → also `C`.

Fact pass must not treat Conjecture as clearance.

Anti-quality: “Great job!” with no passes; rewriting the chapter in the review file as if it were the live draft; `Pass` despite a live Blocker.

---

## Validation

| Check | Pass |
| --- | --- |
| Path | `book/reviews/` with stable numbering |
| Chapter file | Untouched by this session (except the human later) |
| Canon | Untouched |
| Verdict | Exactly one of the three strings |
| IDs | Unique in this report; re-review traces old IDs |
| Isolation | No Writer apply in this session |
| Escalation | Act-turn problems point at Architect/human, not a prose-only fix |

---

## Downstream consumers

| Consumer | What they take | What they must not take |
| --- | --- | --- |
| **Writer** | Repair list for `Fail` / `Pass-with-fixes` | Permission to self-Pass; permission to change act turns in prose |
| **Outline** | `S` scene-intent defects | A Review-authored outline |
| **Architect** | Escalated act-turn defects after human direction | A Review-authored act map |
| **Research** | New evidence questions | A fact-check Pass without notes |
| **Human** | Verdict + residual Minors/Notes for accept/reject | Agent-granted publication |

After Writer revises, **re-review** is a new Review session on `review-02` (or next index). Circular ownership is avoided: Review never applies the fix; Writer never writes the verdict.

---

## Examples

### Example — sufficient defect + verdict (pedagogical)

```markdown
Status: Review
Owner: Author
Stage: Review
Target: book/chapters/ch-01-the-warm-hatch.md
Verdict: Pass-with-fixes
Passes: Editing, Continuity, Fact, Critique

## Defects
- P-001 | Pacing | Minor | Opening inventory before Hatch 17 | Writer | Cut or pressure-bind the inventory so the walk has a clock.
- X-001 | Process | Major | Header flags sister Kira; no human promote/strike | Human + Writer | Do not accept chapter until sister is struck or a sheet exists.

## Fact
- Hatch heat language matches Constraint C-R2 (battery dump). No defect.

## Re-review
- Recheck P-001 and X-001. Do not treat Kira as canon.
```

**Handoff:** Review finished. `book/reviews/ch-01-review-01.md` verdict `Pass-with-fixes`. Next: human sister decision; Writer applies P-001; then Review `review-02`. Chapter remains `Draft`. Review did not edit prose or canon.

### Example — invalid output

The Review session rewrites `ch-01-the-warm-hatch.md`, deletes the sister, sets `Status: Approved`, and writes no report.

---

## References

- [`CHECKLIST.md`](CHECKLIST.md)
- [`../writer/README.md`](../writer/README.md) — repair consumer
- [`../outline/README.md`](../outline/README.md)
- [`../architect/README.md`](../architect/README.md)
- [`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §6, §15
- [`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §16.3, §18.3–§18.4
