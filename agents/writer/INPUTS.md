# Writer — Inputs

**Agent:** Writer  
**Audience:** Operators assembling a Writer session; Review checking that the draft was entitled to exist  
**Companion files:** [`SYSTEM.md`](SYSTEM.md) · [`OUTPUTS.md`](OUTPUTS.md)

Inputs are files with statuses ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §5).

---

## Required inputs

| Input | Why | Production-path status |
| --- | --- | --- |
| `book/BOOK_BRIEF.md` | POV, Style, Theme, hard Constraints | `Approved` |
| Approved scene contracts for the chapter | Goal, conflict, turn, payoff, hooks, claims | Outline span `Approved` |
| Relevant character sheets | Voice, capabilities, who-knows-what | Approved for facts treated as true |
| Timeline slice | Clock | Must match planned/accepted events for this chapter |
| World/location Rules the scene occupies | What prose may not contradict | Approved world files as applicable |
| Open Threads the scene might plant, pay, or contradict | Reader promises | `book/canon/threads.md` (or equivalent) as it exists |
| Style constraints from the brief (and style pack if enabled) | Diction/POV policy | Brief outranks packs |

`book/PROJECT_MEMORY.md` is required operational context (flags, risks, unpromoted facts).

For a **revision** session, also required: the Review report (or human defect list) being applied.

---

## Optional inputs

| Input | When |
| --- | --- |
| Neighbor chapters | Prior ending / next opening |
| Research notes for in-scene claims | Always when the outline lists claims |
| Latest review report | Revision; also useful when continuing a chapter after a partial Review |
| Diff against last acceptable commit | Targeted revision ([`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §10.6) |
| Dialogue-only pass extras | Relationship state, Secrets (who can say what) |

Research before every scene is **soft**. If the outline claims section is “none,” research files may be omitted.

---

## Authoritative inputs

| Kind | Store | Binds Writer to |
| --- | --- | --- |
| **Source — intent / style** | Approved brief | POV, rating, Theme, anti-goals |
| **Source — scene intent** | Approved outline for this chapter | Turn and payoff |
| **Source — canon** | Approved canon slices | Facts, clock, Secrets |
| **Source — grounding** | Accepted Constraints | Technical/historical claims |
| **Source — defects (revision)** | Review report repair list | What must change |
| **Process law** | Specs + this contract | No self-approval, no silent retcon |

If outline and architecture disagree on an **act** turn, stop—do not average in prose. Architecture wins until human amends; the outline should have been blocked already.

---

## Non-authoritative inputs

| Kind | Store | Use |
| --- | --- | --- |
| **Derived** | `PROJECT_MEMORY.md` | Flags and focus; canon wins if they disagree |
| **Generated** | Prior Draft wording on the live path | Starting text for revision; not canon |
| **Generated** | `*.alt.md` | Candidate only until human replaces the live file |
| **Temporary** | Chat | Zero unless written back |
| **Illustrative** | `examples/` | Never this book’s events or borrowed “voice paragraph” as fact |
| **Review paraphrase** | Chat restatement of the chapter | Not wording authority |

Taste Notes in a Review report are **not** binding unless the human says so. Blocker/Major items are binding for the revision pass.

---

## Source documents vs generated content

| Class | Writer treats as |
| --- | --- |
| Approved brief, outline, canon, Constraints | Source |
| The chapter file being written | Generated. Claims inside need flags if reusable |
| Neighbor chapters | Generated production; match continuity but do not treat them as a second canon |
| Unpromoted header flags | Not source. Do not build a subplot on them until human decides |

Do not import example chapter text into the live book.

---

## Input validation

1. Outline for this chapter is `Approved`. Brief `Approved`.
2. POV in the contract matches brief policy.
3. In-state on the outline is consistent with sheets and Timeline, or the conflict is already a named blocker (then stop).
4. Each outline claim has a Constraint or is non-technical.
5. Revision sessions have defect IDs; do not “general polish” as a fake apply-fixes.
6. One live path: confirm slug/`ch-NN` is not duplicated.
7. This session is not also Review.

---

## Missing-input handling

| Missing item | Handling |
| --- | --- |
| No approved outline | **Refuse.** Recovery: write/approve contracts first ([`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §9). |
| Brief `Draft` | Stop production-path draft. |
| Character sheet missing for POV | Stop. Do not invent a voice as canon. |
| Location/Rules missing for an on-page place the outline requires | Stop or flag-and-stop; do not invent gravity plating. |
| Timeline slice missing | Stop if the outline names a clock; do not guess Day numbers. |
| Constraints missing for a hard claim | Do not invent the spec. Cut the claim or wait for Research. |
| Review report missing on a “revision” request | Stop or treat as a new draft pass only if the human said so; do not invent defect IDs. |
| Operator wants Writer to “outline then draft then check” | Refuse blended session. |

---

## Examples

### Example A — Valid first draft

**Loaded:** approved brief; approved `book/outline/ch-01.md`; `aya-okoro.md`; `helios-station.md`; Timeline E-04; threads; Constraint C-R2; project memory.

**Valid:** draft `book/chapters/ch-01-the-warm-hatch.md` with `Status: Draft`; flag any new reusable fact.

**Invalid:** add a sister and create `kira.md`; or set `Status: Approved`.

### Example B — Valid revision

**Loaded:** same as A plus `book/reviews/ch-01-review-01.md` verdict `Pass-with-fixes`, `P-001` inventory stall.

**Valid:** edit the live chapter to address P-001 only (plus any Blockers listed); keep `Status: Draft`.

**Invalid:** regenerate the chapter with a new turn; or add a Review file saying `Pass`.

### Example C — Fusion core vs Constraint (stop)

Outline/research: core is cold; batteries dump heat. Operator: “make the fusion core rumble.”

**Valid:** refuse; cite Constraint + canon. Escalate if the human wants a retcon protocol first.

---

## References

- [`SYSTEM.md`](SYSTEM.md)
- [`../outline/OUTPUTS.md`](../outline/OUTPUTS.md)
- [`../research/OUTPUTS.md`](../research/OUTPUTS.md)
- [`../review/OUTPUTS.md`](../review/OUTPUTS.md) — revision input
- [`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §5, §7
- [`../../docs/architecture/020_MEMORY_SYSTEM.md`](../../docs/architecture/020_MEMORY_SYSTEM.md) §22
