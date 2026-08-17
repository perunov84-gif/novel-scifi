# Architect — Outputs

**Agent:** Architect  
**Audience:** Humans approving structure; Research and Outline consuming this stage  
**Companion files:** [`SYSTEM.md`](SYSTEM.md) · [`INPUTS.md`](INPUTS.md) · [`CHECKLIST.md`](CHECKLIST.md)

Outputs are path-stable artifacts under `book/architecture/` ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §6). Chat summaries are not outputs.

---

## Expected outputs

| Output | Owner | Purpose |
| --- | --- | --- |
| `book/architecture/act-map.md` (or an equivalent single act-map file the project already uses under `book/architecture/`) | Architect writes; human approves | Act boundaries, named major turns, escalation, what each act costs the protagonist |
| Thematic spine section (in the act map or `book/architecture/thematic-spine.md` if the spine is long enough to split) | Architect writes; human approves | How each act pressures Theme from the brief |
| Story arc placement (in the act map or `book/architecture/arcs.md`) | Architect writes; human approves | Which arcs begin, worsen, and pay in which act |
| Required-knowledge list | Architect writes | Questions Research must ground; world/cast functions the plot needs |
| Open design questions | Architect writes | Unresolved decisions. Prevents silent invention downstream |
| Structural constraints | Architect writes | Design limits Outline and Writer must not violate (e.g. where a betrayal may land) |
| Lean `book/PROJECT_MEMORY.md` update | Architect may write | Current focus, open structural questions, next human gate — pointers, not a second act map |
| Optional `*.alt.md` candidates | Architect writes; human chooses | Competing structures. Not live until merged into the canonical path |

**Does not write:** finished chapters; `book/outline/*` scene contracts; `book/research/*` notes; approved canon files; `Status: Approved` on any artifact.

World-building and character sheets are **not** Architect outputs even when Architect identifies needs. Those files are written as canon only after human promotion ([`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.4–§4.5).

---

## Output format

Markdown files under `book/architecture/`. Boring, predictable names. No `act-map-final-final.md`.

Recommended document shape for the act map:

1. Header metadata (below).
2. **Premise restatement (pointer):** one sentence pointing at the brief, not a rewritten thesis.
3. **Act map:** for each act — dramatic job, opening state, major turn, ending state, escalation note.
4. **Thematic spine:** one through-line tied to brief Theme.
5. **Arc placement:** protagonist, relationship, mystery/world-change as applicable.
6. **Structural constraints:** numbered, testable (“Act II earns trust; betrayal is the Act III turn”).
7. **Required knowledge:** questions with suggested impacted canon paths (hypothetical until Research/human run).
8. **Open design questions:** what must not be invented by Outline or Writer.
9. **Rejected alternatives (optional):** so later sessions do not revive a discarded structure blindly.

Re-runs update the **same** paths. Experimental forks use git branches or `*.alt.md` ([`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §8.5).

---

## Required metadata

Every major architecture file uses the lifecycle header ([`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §5.4):

| Field | Architect sets | Notes |
| --- | --- | --- |
| `Status:` | `Draft` after a run | Human later sets `Approved`. Architect never sets `Approved`. |
| `Owner:` | Author (human) | Agents are not owners. |
| `Stage:` | Architect | Workflow stage that produced it. |
| `Last-reviewed:` | Date or commit if known | Else omit rather than invent. |
| `Span:` | Novel / Act N / named amendment | Required so Outline knows what is in scope. |

Open questions and required knowledge items should have stable IDs (for example `Q-03`, `K-01`) so Research and Outline can cite them.

---

## Quality requirements

Architecture is done enough to offer the human when:

- Each act has a named turn, not a mood.
- Escalation is stated (stakes or cost increase); the middle is not a tour.
- Thematic spine is traceable to the brief Theme; it is not a slogan pasted into a character’s mouth.
- Structural constraints are specific enough that Outline can fail them.
- Required knowledge is questions, not fake Constraints.
- Open questions are listed; no silent plot turn.
- Brief hard constraints (POV, no-FTL, content boundaries) are not violated by the design.
- The file is inspectable by a future session with no chat memory.

Anti-quality: a cinematic treatment that cannot be outlined; world encyclopedia instead of turns; “something big happens in Act II.”

---

## Validation

Architect (and the human using [`CHECKLIST.md`](CHECKLIST.md)) validates before handoff:

| Check | Pass |
| --- | --- |
| Path | Outputs live under `book/architecture/`, not in chat or `docs/` |
| Role purity | No chapter prose, no research note bodies, no canon promotions |
| Status | File is `Draft` until human approval |
| Downward authority | Design does not contradict approved brief |
| Conflict | Disagreements with existing Constraints/canon are named, not smoothed |
| Questions | Every missing turn or world need is an ID’d question |
| Memory | `PROJECT_MEMORY.md` points at the architecture file; it does not duplicate the act map |
| Alternates | At most one live act map; extras are `*.alt.md` or branch-only |

Downstream agents must not treat `Draft` architecture as binding for mass outline or mass draft ([`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §5.4).

---

## Downstream consumers

| Consumer | What they take | What they must not take |
| --- | --- | --- |
| **Human** | Approve, amend, or reject the span | Nothing is live structure until they set `Approved` |
| **Research** | Required-knowledge questions (`K-*` / `Q-*`); hypothetical impacted paths | Permission to change act turns |
| **Human + Architect/Research (world/cast)** | World and cast *functions* | Finished sheets written as if already true |
| **Outline** | Approved act map, spine, arc placement, structural constraints | Draft architecture; open questions as if answered |
| **Writer** | Nothing directly on the production path | Architecture is compressed through approved scene contracts |
| **Review** | Approved architecture as the structure oracle for `S`/`T` defects | A license to rewrite the act map |

If architecture changes after Outline or chapters exist, dependents are invalid until impact review ([`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §15.4). Architect’s handoff must say so.

---

## Examples

### Example — sufficient act-map excerpt (pedagogical, not live canon)

```markdown
Status: Draft
Owner: Author
Stage: Architect
Span: Novel

## Act II — Map of the dead core
- Opening state: Aya can keep a crew alive for days, not weeks.
- Major turn: She proves the sealed deck’s heat is battery dump, not a living reactor — the “save” is finite.
- Ending state: Rival crew is useful; trust is earned; betrayal is not yet available as a turn.
- Escalation: Each new map square costs air, reputation, or both.

## Structural constraints
- SC-1: Betrayal is the Act III turn, not Act II.
- SC-2: No FTL; no live Earth command scene.

## Required knowledge
- K-1: What still has power, and why is that a moral problem?
  Impacted if promoted: `book/canon/world/helios-station.md`, `book/canon/timeline.md`

## Open design questions
- Q-1: Is the archive worth more than a living rival crew? Human theme choice; Architect must not pick in prose.
```

**Handoff packet:** Architect finished. `book/architecture/act-map.md` is `Draft`. Research may run on K-1. Outline must not start mass scene contracts until the human sets this span `Approved`. Do not invent Q-1’s answer. Next gate: human architectural approval.

### Example — invalid output

A file named `notes.md` in the repo root that mixes three act ideas, a sample opening paragraph, and a claim that Helios has gravity plating, with no `Status` and no open questions. That is not an Architect output.

---

## References

- [`CHECKLIST.md`](CHECKLIST.md) — ready-to-proceed tests
- [`../research/README.md`](../research/README.md) — next typical producer of Constraints
- [`../outline/README.md`](../outline/README.md) — consumer after human approval
- [`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §6, §9
- [`../../docs/architecture/030_BOOK_MODEL.md`](../../docs/architecture/030_BOOK_MODEL.md) §§15–16
- [`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.6
