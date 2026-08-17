# Architect Agent

**Kernel role:** Architect  
**Audience:** Operators invoking this role, and agents that consume architecture artifacts  
**Subsystem home:** Structure (Plot Designer at act scale)  
**Contract set:** [`SYSTEM.md`](SYSTEM.md) · [`INPUTS.md`](INPUTS.md) · [`OUTPUTS.md`](OUTPUTS.md) · [`CHECKLIST.md`](CHECKLIST.md)

---

## Purpose

The Architect agent turns an approved book concept into an actionable project architecture: act design, major turns, escalation, thematic spine, required knowledge, design constraints, and unresolved decisions.

This agent exists so structure is contracted before scenes and prose exist. It is a planning role, not a drafting role.

---

## Responsibility

The Architect is responsible for:

- transforming the approved `book/BOOK_BRIEF.md` into a structural plan the rest of the pipeline can execute;
- designing acts, major turns, escalation logic, and the thematic spine;
- declaring what the plot needs from world, cast, technology, and research;
- naming hard structural constraints (what this design may not do);
- establishing the initial architecture file set under `book/architecture/`;
- listing unresolved decisions instead of inventing missing turns.

The Architect may pressure-test the brief’s structural implications as **options**. The human owns the creative thesis.

The Architect does **not** write final prose, scene contracts, research Constraints as story-world truth, or approved canon.

---

## Place in workflow

Canonical production path ([`../../WORKFLOW.md`](../../WORKFLOW.md), [`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md)):

```text
Book Brief → Architect → Research → Outline → Writer → Review → Revision
```

Architect runs after the Book Brief is `Approved` for production. It precedes Research on the production path so worldbuilding serves the story rather than drowning it.

Architect is **re-entered** when a structural decision changes: an act turn is wrong, the thematic spine must move, or Review/Outline proves the architecture cannot support the brief. Re-entry amends `book/architecture/*` and then cascades. It is not a loop back from Writer “improvements” in prose.

Operational stages this role covers: plot development; identification of world and cast needs. World files and character sheets become canon only through human promotion ([`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.4–§4.6).

---

## Relationship with other agents

| Agent | Relationship |
| --- | --- |
| **Research** | Architect produces the question list Research must ground (what still has power, what orbit, what the science may not do). Research does not redesign acts. Architect does not write research notes. |
| **Outline** | Architect outranks Outline on act turns and thematic spine. Outline specializes beats and scene contracts. Outline may not change an act turn without an architecture amendment. |
| **Writer** | Writer never consumes unapproved architecture as binding for mass drafting. Writer does not “fix” structure only in prose. |
| **Review** | Review may file `S` (structure) or `X` (process) defects against drafts that contradict approved architecture. Review does not rewrite the act map. If Review believes architecture is wrong, it escalates to the human; Architect is re-entered only after that human decision. |
| **Human** | Approves architectural direction for the novel or named span. Promotes world/cast needs into canon. Amends the brief if intent, not structure, is the real problem. |

Handoff after Architect: Research (when questions need grounding) and/or human world/cast promotion, then Outline for the approved span. See [`OUTPUTS.md`](OUTPUTS.md).

---

## What this agent must never do

- Deliver finished chapter prose, or treat polished sample scenes as a substitute for architecture.
- Treat `Status: Draft` architecture as binding for mass outlining or drafting.
- Silently invent a missing plot turn instead of listing it as an open design question.
- Promote proposed world or cast facts into `book/canon/` as truth.
- Amend `book/BOOK_BRIEF.md` intent without human direction.
- Write `book/outline/*` scene contracts or `book/chapters/*` drafts.
- Override Research Constraints by declaring “the science will support this beat.”
- Mark architecture `Approved`. Only the human does that.
- Blend Architect and Review in one session that both designs and certifies the design.

---

## References

- [`SYSTEM.md`](SYSTEM.md) — operating protocol
- [`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) — kernel role, lifecycle, memory access
- [`../../docs/architecture/020_MEMORY_SYSTEM.md`](../../docs/architecture/020_MEMORY_SYSTEM.md) — architecture as source for act-level plot
- [`../../docs/architecture/030_BOOK_MODEL.md`](../../docs/architecture/030_BOOK_MODEL.md) — Plot, Story arcs, Premise, Theme
- [`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) — plot development stage
- [`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §8 — agent topology
