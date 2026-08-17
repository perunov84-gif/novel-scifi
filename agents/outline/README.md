# Outline Agent

**Kernel role:** Outline  
**Audience:** Operators invoking this role, and Writer/Review consuming scene contracts  
**Subsystem home:** Structure (Plot Designer at scene scale)  
**Contract set:** [`SYSTEM.md`](SYSTEM.md) · [`INPUTS.md`](INPUTS.md) · [`OUTPUTS.md`](OUTPUTS.md) · [`CHECKLIST.md`](CHECKLIST.md)

---

## Purpose

The Outline agent turns approved story architecture into structured narrative plans: chapter grouping, scene contracts, arc pressure, dependencies, and continuity requirements.

This agent exists so Writer receives a scene-level contract—goal, conflict, turn, payoff—before prose exists. Outline is a planning role, not a drafting role.

---

## Responsibility

The Outline agent is responsible for:

- transforming approved architecture (and brief) into scene-level narrative plans;
- story architecture at **scene** scale (not a second act map);
- placing Story Arcs across the requested span;
- chapter plans (`ch-NN-slug` targets, scene membership, clocks);
- scene objectives: viewpoint, location, timeline anchor, goal, conflict, turn, payoff;
- dependencies between scenes (what must be true going in; what is true going out);
- continuity requirements: Threads to plant/pay/avoid, Secrets, who-knows-what, research claims.

Outline does **not** produce final prose. It does not change act turns or thematic spine without an architecture amendment.

---

## Place in workflow

Canonical production path:

```text
Book Brief → Architect → Research → Outline → Writer → Review → Revision
```

Outline runs after architecture for the span is `Approved`. Research is required when the span asserts grounded or speculative-technical claims; otherwise it is a soft skip with the gap named.

**Revision loop:** Review `S` defects may reopen Outline for that chapter without reopening the whole act—unless the broken turn is an act turn, in which case Architect is re-entered first ([`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §5.4).

Operational stages: outline; chapter planning ([`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.7–§4.8). Both belong to this kernel role.

---

## Relationship with other agents

| Agent | Relationship |
| --- | --- |
| **Architect** | Architect outranks Outline on act turns and spine. Outline specializes beats. If a scene needs a different act turn, stop and escalate; do not hide it in a scene contract. |
| **Research** | Outline lists claims that need Constraints. If Constraints are missing for a hard claim, invoke Research (new session) or defer the claim. Outline does not write research notes. |
| **Writer** | Outline outranks Writer on scene intent. Writer may flag outline defects; Writer may not “fix” structure only in prose. Writer starts only after the human approves this outline span. |
| **Review** | Review adjudicates whether prose satisfied the contract. Review may demand re-outline. Review does not become the outline. |
| **Human** | Approves the outline span before mass drafting. |

Handoff after Outline: human span approval, then Writer. See [`OUTPUTS.md`](OUTPUTS.md).

---

## What this agent must never do

- Write full prose chapters, or “sample scenes” long enough to replace a draft.
- Invent a second plot authority that contradicts approved architecture.
- Change Act turns or thematic spine in the outline instead of requesting Architect re-entry.
- Promote new world/cast facts into canon as truth.
- Mark outline `Approved`.
- Treat `Draft` architecture as binding for mass scene contracts.
- Relabel Research Conjecture as a scene clock or capability.
- Blend Outline and Writer in one session that both contracts and drafts the same chapter.
- Leave two live chapter numbers with competing scene sets.

---

## References

- [`SYSTEM.md`](SYSTEM.md)
- [`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §3.1, §3.3 (Plot Designer split)
- [`../../docs/architecture/020_MEMORY_SYSTEM.md`](../../docs/architecture/020_MEMORY_SYSTEM.md) §9 — plot knowledge
- [`../../docs/architecture/030_BOOK_MODEL.md`](../../docs/architecture/030_BOOK_MODEL.md) §§17–18 — Chapter, Scene
- [`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.7–§4.8
- [`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §8.1
