# Research — Inputs

**Agent:** Research  
**Audience:** Operators assembling a Research session; other agents checking whether Research was entitled to run  
**Companion files:** [`SYSTEM.md`](SYSTEM.md) · [`OUTPUTS.md`](OUTPUTS.md)

Inputs are files with statuses, not leftover chat ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §5).

---

## Required inputs

| Input | Why it is required | Status |
| --- | --- | --- |
| `book/BOOK_BRIEF.md` | Hard limits, genre frame, and themes Research must not “correct” | `Approved` for production-path notes that will bind Outline/Writer. If the brief is still `Draft`, Research may only collect non-binding background and must not emit Constraints as if production were open. |
| A **question list** | Research is not a wandering encyclopedia | From `book/PROJECT_MEMORY.md`, Architect required-knowledge IDs, Outline “claims” fields, Writer unpromoted/research flags, Review fact-check questions, or a human-written topic file |
| Existing `book/research/` notes for that topic, if any | Prevent forked truth | Read before writing |

The question list is required even if it is one sentence in project memory (for example, `K-1: What still has power?`).

---

## Optional inputs

| Input | When to load |
| --- | --- |
| Architecture slice that created the question | So the note serves the plot’s actual need, not a generic textbook chapter |
| Outline slice whose claims need grounding | Scene-level technical assertions |
| Relevant approved canon | Detect Constraint vs canon collision early |
| Chapter passage that asserted a claim (Review/Writer side path) | Quote the claim; do not revise the chapter in this session |
| Author-supplied sources (papers, notes, links recorded in-repo) | Prefer these over model recollection |
| Series canon | If a series module is enabled |

Research before every scene is **not** required. Optional inputs stay optional.

---

## Authoritative inputs

| Kind | Store | Binds Research to |
| --- | --- | --- |
| **Source — intent** | Approved brief | No-FTL, rating, content boundaries, what the book is not |
| **Source — prior grounding** | Existing research **Constraint** lines | Do not silently contradict; amend via human if wrong |
| **Source — canon** | Approved `book/canon/*` | Do not propose Constraints that ignore already-true world Rules without flagging the conflict |
| **Source — structure (context)** | Approved architecture for *why the question exists* | The question’s job; not a license to change turns |
| **Process law** | Specs + this contract | Labels, no canon promotion, ethics |

External references become authoritative for the book only after distillation into **Constraint** or canon—not by being mentioned in chat ([`../../docs/architecture/020_MEMORY_SYSTEM.md`](../../docs/architecture/020_MEMORY_SYSTEM.md) §3.1).

---

## Non-authoritative inputs

| Kind | Store | How Research may use it |
| --- | --- | --- |
| **Derived** | `PROJECT_MEMORY.md` | Question pointers and risks |
| **Generated** | Chapter prose, Outline claims, Architect `K-*` wording | The *question*. Prose numbers are claims to test, not facts. |
| **Temporary** | Chat, unrecorded browsing | Must be written into the note with a Source or Conjecture label |
| **Illustrative** | `examples/` | Format of a research note, never Helios physics for this book |
| **Model prior knowledge** | The model’s training | At best **Conjecture** until tied to a Source or an explicit author stipulation |

A Writer’s invented orbital period is **generated content**, not a Source.

---

## Source documents vs generated content

| Class | Research treats as |
| --- | --- |
| Approved brief; human-accepted Constraints; approved canon | Source (per class) |
| Author-provided reference listed in the note | **Source** (external). Still distill before it binds prose. |
| Architect questions, Outline claims, chapter sentences | Generated or derived *questions* |
| This session’s new Conjecture | Generated. Must not be labeled Constraint |
| Chat | Temporary. Zero after session unless written back |

Do not copy example research from `examples/` into `book/research/` as if it were this novel’s archive.

---

## Input validation

Before execute:

1. **Question exists and is scoped.** “Research space” is not a question. “Can Helios deorbit in seven days given X?” is.
2. **Brief hard limits visible.** A no-FTL book cannot take FTL papers as the Constraint set without escalation.
3. **Topic file identity.** If `book/research/orbital-decay.md` already exists, this run updates it; it does not create `orbital-decay-2.md`.
4. **Contradiction scan.** New work vs existing Constraints and canon is named up front.
5. **Role.** The operator did not also ask for a chapter draft or a Review Pass in this session.
6. **Ethics.** No request to ingest copyrighted fiction as original prose.

---

## Missing-input handling

| Missing item | Handling |
| --- | --- |
| No brief | Stop. |
| Brief `Draft` on production path | Stop emitting binding Constraints. Optional unlabeled background is not a substitute; say the gate is red. |
| No question list | Stop. Ask the human to name the question or point at Architect `K-*`. |
| Architecture not yet written | Allowed for early curiosity notes, but mark that production-path Constraints may be premature if plot needs are unknown. Prefer waiting for Architect questions on the canonical path. |
| Canon empty | Allowed. Impact list names files that *would* be created on promotion. |
| No external source available | Write **Conjecture** or author-stipulation **Constraint** clearly attributed to the author (“Author stipulation: comms lag ≥ 4 minutes”), never fake a journal. |
| Outline/Writer wants a number now | Do not invent a Constraint to unblock them. Hand back “insufficient evidence” and a Conjecture if useful as non-binding. |

Hard input rule: missing, wrongly Draft, or internally contradictory required inputs → stop and name the blocker.

---

## Examples

### Example A — Production-path question (valid)

**Loaded:** approved brief (no FTL); Architect `K-1`; empty canon.

**Question:** What can still power a sealed deck, and what heat tell is honest?

**Valid inputs:** brief, K-1, maybe a human link to battery thermal notes.

**Invalid input use:** treating a Writer draft’s “fusion core hum” as a Source that Research must support.

### Example B — Mid-Review side path (valid)

**Loaded:** Review asks whether “falling in a week” is allowed; existing `book/research/orbital-decay.md`; ch-09 sentence quoted.

**Valid:** Research updates the same note with Constraint vs Conjecture; lists `timeline.md` and `helios-station.md` as impacts.

**Invalid:** Research edits ch-09 to “a few months” and calls the science fixed.

### Example C — Missing question (invalid run)

Operator: “Research everything about space stations.”

**Valid response:** Stop. Demand a question list tied to this book’s architecture or claims.

---

## References

- [`SYSTEM.md`](SYSTEM.md)
- [`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §5
- [`../../docs/architecture/020_MEMORY_SYSTEM.md`](../../docs/architecture/020_MEMORY_SYSTEM.md) §3, §10
- [`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.3
- [`../architect/OUTPUTS.md`](../architect/OUTPUTS.md) — typical question source
