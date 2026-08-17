# Architect — Inputs

**Agent:** Architect  
**Audience:** Operators assembling an Architect session; other agents checking whether Architect was entitled to run  
**Companion files:** [`SYSTEM.md`](SYSTEM.md) · [`OUTPUTS.md`](OUTPUTS.md)

Inputs are files with statuses, not “whatever is in the thread” ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §5).

---

## Required inputs

| Input | Why it is required | Production-path status |
| --- | --- | --- |
| `book/BOOK_BRIEF.md` | Premise, audience, Theme, tone, POV policy, Style intent, and hard Constraints are the contract architecture must serve | `Approved` for production-path architecture. If `Draft`, Architect may only help the human pressure-test options and must not emit a binding act map. |
| `book/PROJECT_MEMORY.md` | Current focus, open questions, risks, next gate | Must exist. May be thin at first architecture. |

For a **structural amendment** (Architect re-entry), also required:

| Input | Why |
| --- | --- |
| Existing `book/architecture/*` for the span | The live act map being changed |
| The artifact that forced re-entry | Review report `S`/`X` defects, or an Outline file that cannot realize the turn, or a human note recorded in project memory |

---

## Optional inputs

| Input | When to load |
| --- | --- |
| Prior architecture Drafts or `*.alt.md` candidates | Comparing options the human has not chosen |
| Existing `book/research/*` **Constraint** lines | A turn would assert grounded or speculative-technical claims |
| Existing `book/canon/` slices | World Rules, cast, or Timeline already approved and must not be contradicted |
| Series canon | Only if a series module is enabled |
| Approved outlines for the span | Amendment after scenes already exist; used to measure cascade cost, not as a second act map |
| Neighbor act architecture | Amending one act; load the adjacent turns |

Research before all architecture is a **soft** dependency ([`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §15.3). Architect may run with an empty research tree if the brief does not yet require grounded claims. It must still list required knowledge for Research to pick up.

---

## Authoritative inputs

These may bind Architect when their status and class agree with the authority stack ([`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §5):

| Kind | Store | Binds Architect to |
| --- | --- | --- |
| **Source — intent** | Approved `book/BOOK_BRIEF.md` | Premise, Theme, POV, rating, content boundaries, book non-goals |
| **Source — grounding** | `book/research/*` lines labeled **Constraint** (human-accepted) | Turns that would violate grounding |
| **Source — canon** | Approved `book/canon/*` | Story-world facts already true |
| **Source — structure (prior)** | Approved `book/architecture/*` outside the span being amended | Adjacent turns that must still fit |
| **Process law** | `docs/specifications/` and this contract set | Role purity, fail-loudly, no silent promotion |

---

## Non-authoritative inputs

| Kind | Store | How Architect may use it |
| --- | --- | --- |
| **Derived** | `book/PROJECT_MEMORY.md` | Operational dashboard only. If it disagrees with brief or canon, the brief/canon wins. |
| **Derived** | Draft architecture, Draft outlines | Options and cascade hints, not frozen law |
| **Generated** | Chapter prose, chat pitches, `*.alt.md` | Evidence that a turn is painful in practice; never a reason to treat prose as the act map |
| **Temporary** | Cursor chat, unsaved buffers | Thinking aid. Write back or it did not happen. |
| **Illustrative** | `examples/` | Format only. Never this book’s facts. |
| **Guides / prompts** | `docs/guides/`, `prompts/` | Procedure. They lose to specifications and approved book contracts. |

Chat agreement (“let’s make the rival sympathetic in Act I”) is not an input until a file says so ([`../../docs/architecture/020_MEMORY_SYSTEM.md`](../../docs/architecture/020_MEMORY_SYSTEM.md) §12).

---

## Source documents vs generated content

| Class | Architect treats as |
| --- | --- |
| Approved brief, approved canon, accepted research **Constraint** | Source. Do not contradict; escalate instead. |
| Draft brief, draft canon proposals | Not production-path authority. |
| Architecture this session will write | Generated until the human sets `Approved`. |
| Outlines and chapters | Downstream generated/derived production. They do not outrank architecture on act turns. |

Do not infer a substitute brief from an example novel or from a chapter the Writer already drafted.

---

## Input validation

Before execute, Architect checks:

1. **Presence.** Required paths exist and are not empty shells without the brief’s required intent sections.
2. **Status.** Production-path work requires brief `Approved`. Draft brief → stop or non-binding sketch only.
3. **Internal consistency.** POV policy, constraints, and premise do not contradict each other inside the brief.
4. **Authority conflicts.** If an approved Constraint or canon file already forbids a turn the operator requested, name both sources; do not average them.
5. **Span clarity.** The operator named a span (whole novel vs Act II amendment). If not, stop and ask the human; do not invent the span.
6. **Example isolation.** No `examples/` file is being treated as live memory.

Validation failure is a red gate. Do not proceed into a fake-complete act map.

---

## Missing-input handling

| Missing item | Handling |
| --- | --- |
| No `BOOK_BRIEF.md` or required sections blank | Stop. Name the path and the gate. Do not invent premise, audience, or POV. |
| Brief `Draft` on production path | Stop for binding architecture. Optional: list structural *options* clearly labeled non-binding. |
| No `PROJECT_MEMORY.md` | Stop. The working-memory home must exist even if thin ([`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.1). |
| Research empty but a turn needs orbital mechanics / medicine / law | List required knowledge; do not fabricate **Constraint** lines. Hand off to Research. |
| Canon empty at first architecture | Allowed. Record world/cast *needs* as proposals and open questions. |
| Amendment requested but no existing architecture file | Stop. First pass must create the act map, not “amend” nothing. |
| Operator wants Architect to “just write chapter 1” | Refuse. Wrong role. Hand off after architecture exists and is approved, then Outline, then Writer. |

Hard input rule: if a required input is missing, `Draft` when approval is required, or internally contradictory, the agent stops and names the blocker ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §5).

---

## Examples

### Example A — First architecture (valid)

**Loaded:** `book/BOOK_BRIEF.md` (`Status: Approved`; close-third Aya; no FTL; theme: what we owe the dead when resources are finite); empty-ish `book/PROJECT_MEMORY.md`; no research yet.

**Valid:** Architect writes `book/architecture/act-map.md` with three acts, named turns, thematic spine, and open question “What still has power, and why is that a moral problem?”

**Invalid:** Architect also drafts `book/chapters/ch-01-*.md` “so the author can see the tone.”

### Example B — Constraint already exists (valid stop or redesign)

**Loaded:** brief approved; research note `Constraint: Helios-to-Earth comms lag ≥ 4 minutes`.

**Operator request:** Act I climax is a live Earth tribunal on video.

**Valid:** Architect refuses that turn, proposes an asynchronous tribunal or a local hearing, or escalates for the human to amend the Constraint.

**Invalid:** Architect writes the live tribunal and adds “maybe the lag is dramatic license.”

### Example C — Re-entry after Review (valid)

**Loaded:** approved `act-map.md` (betrayal is Act III); Review `S-012` on ch-12 prose that ends in betrayal; human confirms architecture should win.

**Valid:** Architect does not need to rewrite the act map. Handoff: Outline/Writer must restore Act II trust logic. If the human instead wants betrayal in Act II, Architect amends the act map first, then cascade.

**Invalid:** Architect “splits the difference” by making the betrayal ambiguous in a new architecture paragraph without changing the named turn.

---

## References

- [`SYSTEM.md`](SYSTEM.md) — gates and memory access
- [`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §5, §10
- [`../../docs/architecture/020_MEMORY_SYSTEM.md`](../../docs/architecture/020_MEMORY_SYSTEM.md) §3
- [`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.2, §4.6
- [`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §5, §15
