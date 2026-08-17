# Review — Inputs

**Agent:** Review  
**Audience:** Operators assembling a Review session; later agents checking the report was entitled  
**Companion files:** [`SYSTEM.md`](SYSTEM.md) · [`OUTPUTS.md`](OUTPUTS.md)

Inputs are files with statuses ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §5).

---

## Required inputs

| Input | Why | Status |
| --- | --- | --- |
| The draft (or span) under review | Object of adjudication | Typically `Draft` chapter on the live path |
| Outline contracts it must satisfy | Structure and scene intent | `Approved` on production path |
| `book/BOOK_BRIEF.md` | POV, Style, Theme, hard limits | `Approved` |
| Canon slices the draft touches | Continuity oracle | Approved facts |
| Prior review reports for the same chapter | Re-review and ID continuity | As they exist |

`book/PROJECT_MEMORY.md` is required for unpromoted flags and freeze risks.

---

## Optional inputs

| Input | When |
| --- | --- |
| Diff against last acceptable version | Revision passes; often more informative than a cold reread |
| Research notes for fact checking | When the outline lists claims or prose asserts grounded facts |
| Neighbor chapters | Continuity of clock, inventory, knowledge, hanging Threads |
| Architecture spine | Theme/structure (`T`/`S`) beyond the single scene |
| Human taste notes already recorded | Distinguish new `Note` items from settled taste |

---

## Authoritative inputs

| Kind | Store | Binds Review to |
| --- | --- | --- |
| **Source — intent** | Approved brief | What “theme drift” and POV breaks mean |
| **Source — scene intent** | Approved outline | Whether the turn/payoff occurred |
| **Source — structure (macro)** | Approved architecture | Whether an act turn was stolen |
| **Source — canon** | Approved canon | Continuity |
| **Source — grounding** | Accepted Constraints | Fact check |
| **Object under test** | Chapter wording | Not canon; claims to test |
| **Process law** | Specs + this contract | Taxonomy, no silent rewrite |

If outline and architecture conflict, do not blend. File `S`/`X`, name both sources, escalate ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §14).

---

## Non-authoritative inputs

| Kind | Store | Use |
| --- | --- | --- |
| **Derived** | `PROJECT_MEMORY.md` | Flags; canon wins if they disagree |
| **Generated** | Chapter prose | Test object |
| **Temporary** | Chat, including Writer’s self-assessment | Zero authority |
| **Illustrative** | `examples/` | Defect-writing *format* only |
| **Conjecture** | Research labeled Conjecture | Not a Pass for facts |
| **Taste of the previous session** | Unrecorded | Ignore unless in a file |

Writer commentary in the chapter header (flags) **is** an input: unpromoted facts should become `X` (and `C` if they contradict canon).

---

## Source documents vs generated content

| Class | Review treats as |
| --- | --- |
| Brief, approved outline, architecture, canon, Constraints | Source (oracles) |
| Chapter Draft | Generated. Wording to judge; facts are claims |
| This report | Derived quality record; not canon |
| `*.alt.md` | Review only if the operator named it; never confuse with the live path |

Do not use example reviews as this book’s defect list.

---

## Input validation

1. Draft path exists and is the live `ch-NN` (or the named span).
2. Outline contract exists; on production path it should be `Approved`. If missing, that is already an `X` Blocker—not a reason to invent a contract from the prose.
3. Brief loaded.
4. Canon slices for on-page entities loaded **or** the miss is itself a process defect.
5. Fact-check pass has research notes **or** the pass states “no technical claims” / “insufficient research → Fail or side path.”
6. Session is not the drafting session.
7. Context does not include a goal of making the draft succeed.

---

## Missing-input handling

| Missing item | Handling |
| --- | --- |
| No draft | Stop. No Pass. |
| No outline on production-path chapter | File `X` Blocker; verdict `Fail`. Do not reconstruct intent from vibes. |
| Brief missing | Stop. Cannot judge POV/Theme/constraints. |
| Canon missing for an on-page character | Continuity pass cannot clear who-knows-what; `C`/`X` Blocker. |
| Research missing for a hard claim | Do not Pass the claim. Verdict `Fail` or require Research side path ([`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.13). |
| Diff missing on a tiny polish | Allowed; full-file review. Prefer diff when the operator said “revision.” |
| Operator asks Review to “just fix the sentences” | Refuse apply. File `R` defects; Writer applies. |

---

## Examples

### Example A — Valid chapter review inputs

**Loaded:** `ch-01-the-warm-hatch.md`; approved `book/outline/ch-01.md`; brief; Aya sheet; Helios world file; Timeline E-04; `orbital-decay.md` Constraints; project memory with sister flag.

**Valid:** all four passes in one report.

**Invalid:** also opening the Writer contract and rewriting the hatch scene in the same session.

### Example B — Worked conflict (architecture vs prose)

Outline says ch-12 ends in betrayal. Approved architecture says betrayal is Act III.

**Valid input use:** architecture + outline + draft. **Valid output:** `S` and `X`; Writer must not split the difference ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §14).

### Example C — Missing research (valid Fail)

Prose claims a seven-day deorbit. No Constraint file.

**Valid:** do not invent orbital mechanics in the review. `Fail` or demand Research. Conjecture in chat is not an input.

---

## References

- [`SYSTEM.md`](SYSTEM.md)
- [`../writer/OUTPUTS.md`](../writer/OUTPUTS.md) — object under test
- [`../outline/OUTPUTS.md`](../outline/OUTPUTS.md) — contract
- [`../research/OUTPUTS.md`](../research/OUTPUTS.md) — fact-check corpus
- [`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §5, §7
- [`../../docs/architecture/020_MEMORY_SYSTEM.md`](../../docs/architecture/020_MEMORY_SYSTEM.md) §23
