# Outline — Inputs

**Agent:** Outline  
**Audience:** Operators assembling an Outline session; other agents checking whether Outline was entitled to run  
**Companion files:** [`SYSTEM.md`](SYSTEM.md) · [`OUTPUTS.md`](OUTPUTS.md)

Inputs are files with statuses ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §5).

---

## Required inputs

| Input | Why | Production-path status |
| --- | --- | --- |
| `book/BOOK_BRIEF.md` | POV policy, Theme, constraints, premise | `Approved` |
| Approved `book/architecture/*` for the span | Act turns, spine, structural constraints, arc placement | `Approved` for mass outline of that span |
| Relevant `book/canon/` slices | Characters, locations, Rules, Timeline, Threads the span can touch | Approved sheets/files the Writer will treat as true; missing optional entities are allowed if deferred with a flag ([`../../docs/architecture/030_BOOK_MODEL.md`](../../docs/architecture/030_BOOK_MODEL.md) §33.2) |
| Research **Constraint** lines for claims the span will make | Grounding | Human-accepted Constraints when the scene asserts technical/historical claims |

`book/PROJECT_MEMORY.md` is required as operational context even when short.

---

## Optional inputs

| Input | When |
| --- | --- |
| Neighboring outline files | Sequence, in/out state continuity |
| Prior Review reports that forced re-outline | Defect IDs to satisfy |
| Existing Timeline rows | Attach scenes to Planned/Drafted events |
| Draft chapters (re-outline after prose exists) | Measure what must change; do not treat prose as the contract |
| Series canon | If enabled |

Research before every scene remains **soft**. If the span has no grounded claims, Constraints may be absent; the outline must say so.

---

## Authoritative inputs

| Kind | Store | Binds Outline to |
| --- | --- | --- |
| **Source — intent** | Approved brief | POV, rating, themes, hard limits |
| **Source — structure** | Approved architecture | Act turns, spine, structural constraints (`SC-*`) |
| **Source — canon** | Approved canon | Who-knows-what, Rules, clock, geography |
| **Source — grounding** | Accepted research Constraints | Claims the scene may make |
| **Source — scene intent (prior)** | Already-approved neighboring outlines | Adjacent in/out states |
| **Process law** | Specs + this contract | Role purity |

If approved architecture and a requested scene turn conflict, architecture wins until the human amends it.

---

## Non-authoritative inputs

| Kind | Store | Use |
| --- | --- | --- |
| **Derived** | `PROJECT_MEMORY.md` | Focus, flags, risks |
| **Derived** | Draft architecture | Not binding for mass outline |
| **Generated** | Chapter drafts, chat pitches | Symptoms that a contract is wrong; not a new spine |
| **Temporary** | Chat | Write back or discard |
| **Illustrative** | `examples/` | Format only |

A Writer “improvement” that changes the turn is **generated content**, not an authoritative restructure.

---

## Source documents vs generated content

| Class | Outline treats as |
| --- | --- |
| Approved brief, architecture, canon, Constraints | Source |
| Draft outline this session produces | Generated until human `Approved` (then source for scene intent only) |
| Chapters | Generated production; they implement contracts, they do not author them |
| Architect open questions still unanswered | Not source. Must remain questions or block the scene |

Do not copy example scene outlines from `examples/` as this book’s plot.

---

## Input validation

1. Brief `Approved`. Architecture for **this span** `Approved`.
2. Span is named (ch-01, ch-07–ch-09, Act I).
3. POV policy can be applied to each planned viewpoint character.
4. Structural constraints from architecture are in view (`SC-1` betrayal placement, no-FTL, etc.).
5. For each planned technical claim, either a Constraint exists or the claim is removed/deferred.
6. Canon slices for on-page characters and locations exist **or** are explicitly deferred with a flag—not silently invented as true.
7. No blended Writer request in this session.

---

## Missing-input handling

| Missing item | Handling |
| --- | --- |
| No approved brief | Stop. |
| Architecture `Draft` or absent | **Refuse** mass outline of the span ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §10). |
| Canon sheets missing for a POV character the brief requires | Stop or defer that chapter; do not invent a personality as canon. |
| Location file missing | Defer with flag, or stop if the scene cannot be specified without it. |
| Constraints missing for a hard claim | Do not invent the claim. Invoke Research or cut the claim. |
| Timeline empty | Outline may propose **Planned** event IDs for the human to add; Outline does not silently fill `timeline.md` as approved history. Prefer listing events in the outline for human/canon write. |
| Operator asks Outline to “write the chapter too” | Refuse. Hand off after approval. |

---

## Examples

### Example A — Valid ch-01 span

**Loaded:** approved brief (close-third Aya); approved act map (Act I boarding and scarcity); `aya-okoro.md`; `helios-station.md`; Constraint C-R2 on power/heat; project memory.

**Valid:** write `book/outline/ch-01.md` with goal/conflict/turn/payoff, plant of heat-as-secret, no new faction.

**Invalid:** also paste 2,000 words of opening prose; or schedule a live Earth video contrary to no-FTL / lag Constraints.

### Example B — Architecture conflict (stop)

Approved architecture: betrayal is Act III. Operator: “Outline ch-12 as the betrayal.”

**Valid:** stop; name `SC-1` vs request; escalate. Do not write a betrayal contract “provisionally.”

### Example C — Soft research skip (valid)

Span is two characters arguing about a debt. No World Rule claims.

**Valid:** outline without a research file; claims section says “none.”

**Invalid:** invent a new medical nanite capability “for the argument’s metaphor” without Research/canon.

---

## References

- [`SYSTEM.md`](SYSTEM.md)
- [`../architect/OUTPUTS.md`](../architect/OUTPUTS.md) — required upstream
- [`../research/OUTPUTS.md`](../research/OUTPUTS.md) — Constraints
- [`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §5
- [`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.7–§4.8
- [`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §15
