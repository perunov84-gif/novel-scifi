# Outline — Outputs

**Agent:** Outline  
**Audience:** Humans approving spans; Writer and Review consuming scene contracts  
**Companion files:** [`SYSTEM.md`](SYSTEM.md) · [`INPUTS.md`](INPUTS.md) · [`CHECKLIST.md`](CHECKLIST.md)

Outputs live under `book/outline/` ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §6).

---

## Expected outputs

| Output | Owner | Purpose |
| --- | --- | --- |
| `book/outline/` files for the span (chapter-scoped e.g. `ch-07.md` or act-scoped with chapter sections) | Outline writes; human approves | Scene contracts |
| Chapter plan: `ch-NN-slug` targets, scene membership, chapter-level purpose and clock | Outline writes | Chapter planning ([`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.8) |
| Continuity hooks: in-state, out-state, Threads, Secrets | Outline writes | Writer retrieval and Review audit |
| Claims list (research) | Outline writes | What must be Constraint-backed |
| Dependencies | Outline writes | What prior scenes must have established |
| Scene-local fact flags | Outline writes | Unpromoted reusable facts |
| Lean `PROJECT_MEMORY.md` update | Outline may write | Span status, questions, next gate |
| Optional `*.alt.md` | Outline writes | Competing scene plans; not live |

**Does not write:** chapter prose; a competing act map; canon promotions; research note bodies; `Approved` status.

Timeline **Planned** events should be named in the outline (event ID, time, location, participants). The human (or a later canon edit) records them in `book/canon/timeline.md`. Outline must not treat a new timeline row as approved history by silently editing canon.

---

## Output format

Markdown under `book/outline/`. Prefer one file per chapter for drafting convenience (`ch-07.md`) unless the project already uses a consistent act-level file.

Each **Scene** block must include the conceptual fields in [`../../docs/architecture/030_BOOK_MODEL.md`](../../docs/architecture/030_BOOK_MODEL.md) §18:

- Viewpoint Character (per brief POV policy)
- Location and Timeline anchor
- Goal, conflict, turn, payoff
- Arcs and Threads touched (plant / pay / avoid)
- Claims that need Research Constraints
- Continuity hooks (true going in; true going out)

Chapter plan section: target filename `book/chapters/ch-NN-slug.md`, scene list, chapter dramatic job, clock.

Re-runs update the same paths. One live contract set per chapter number.

---

## Required metadata

| Field | Outline sets | Notes |
| --- | --- | --- |
| `Status:` | `Draft` | Human sets `Approved` for the span |
| `Owner:` | Author | |
| `Stage:` | Outline | |
| `Span:` | e.g. `ch-07` or `ch-01–ch-03` | |
| `Architecture-ref:` | Path to approved act map | Traceability |

Number scenes (`S-07a`, `S-07b`) so Review can cite them.

---

## Quality requirements

A span is fit to offer the human when:

- Every scene has goal, conflict, turn, and payoff—not a location tour.
- Stakes connect to architecture escalation for this act.
- POV matches the brief.
- Threads to plant or pay are named; unpaid freeze risks are not created casually.
- In-state does not contradict approved canon without a flagged retcon request.
- Hard claims point at Constraint IDs or are absent.
- Chapter slugs and numbers are unique; one live path per number.
- Writer could execute without inventing the turn.

Anti-quality: “they talk, then something happens”; a hidden act-turn change; a new faction invented as if canon.

---

## Validation

| Check | Pass |
| --- | --- |
| Path | `book/outline/`, stable names |
| No prose chapters | Role purity |
| No competing act map | Architecture still the structure oracle |
| Status | `Draft` until human |
| Architecture | Does not contradict approved turns/spine |
| Research | Conjecture not used as clock/capability |
| Continuity | In/out states and threads present |
| Memory | Lean project-memory pointers |

---

## Downstream consumers

| Consumer | What they take | What they must not take |
| --- | --- | --- |
| **Human** | Approve the span | Agent-granted approval |
| **Writer** | `Approved` contracts, slugs, flags, claims | `Draft` outline on production path |
| **Review** | The contract the draft must satisfy | Permission to rewrite the contract silently |
| **Research** | New questions from the claims list | A plot redesign |
| **Architect** | Only via escalation when a turn is wrong | Outline-authored act map |

Changing an approved outline after draft exists is a **revision task**, not silent prose drift ([`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §15.4). Handoff must say if Writer already drafted against the old contract.

---

## Examples

### Example — sufficient scene contract (pedagogical)

```markdown
Status: Draft
Owner: Author
Stage: Outline
Span: ch-01
Architecture-ref: book/architecture/act-map.md

## Chapter plan
- Target: `book/chapters/ch-01-the-warm-hatch.md`
- Clock: Day 12, after Event E-04 Station Arrival (Planned)
- Job: Put Aya at Hatch 17 with a secret the reader can feel.

## Scene S-01a
- POV: Aya (close third)
- Location: Helios dock → inner-ring Hatch 17
- Timeline: E-04 + same-day walk
- Goal: Reach the hatch and confirm salvage rights.
- Conflict: Thin air protocol vs crew authority stalling her.
- Turn: The hatch is warm.
- Payoff: Aya owns a secret; she does not yet know why.
- Arcs: Competence (start); complicity (not yet).
- Threads: Plant T-heat (reader may notice; Aya notices warmth only).
- Claims: Heat must match Constraint C-R2 (batteries / residual / lie). No FTL.
- In: Aya has salvage credential; does not know core is cold.
- Out: Hatch 17 is warm; she has not opened a political alliance.
- Do not: introduce a new faction; pay T-heat; invent a sister.
```

**Handoff:** Outline finished. `book/outline/ch-01.md` is `Draft`. Writer must not draft until human `Approved`. Do not invent Q-items still open in architecture. Next gate: human outline-span approval.

### Example — invalid output

A file that retells Act II as a new act map, plus a full chapter draft, plus `Status: Approved` set by the agent.

---

## References

- [`CHECKLIST.md`](CHECKLIST.md)
- [`../writer/README.md`](../writer/README.md) — next producer
- [`../review/README.md`](../review/README.md) — contract auditor
- [`../../docs/architecture/030_BOOK_MODEL.md`](../../docs/architecture/030_BOOK_MODEL.md) §18
- [`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §6, §9
