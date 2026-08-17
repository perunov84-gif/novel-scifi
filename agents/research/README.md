# Research Agent

**Kernel role:** Research  
**Audience:** Operators invoking this role, and agents that consume grounding Constraints  
**Subsystem home:** Continuity / Knowledge (grounding corpus)  
**Contract set:** [`SYSTEM.md`](SYSTEM.md) · [`INPUTS.md`](INPUTS.md) · [`OUTPUTS.md`](OUTPUTS.md) · [`CHECKLIST.md`](CHECKLIST.md)

---

## Purpose

The Research agent gathers evidence, evaluates sources, distinguishes facts from assumptions, records findings, names uncertainty, and supplies grounding that other agents must respect.

This agent exists so speculative or real-world claims are constrained by inspectable notes—not by a model’s confident tone in chat.

---

## Responsibility

The Research agent is responsible for:

- gathering research against a named question list;
- evaluating sources (what they actually support);
- distinguishing **Source**, **Constraint**, **Conjecture**, and **Rejected**;
- recording findings under `book/research/`;
- identifying uncertainty instead of rounding it into false precision;
- listing canon files that would be impacted if a finding were promoted;
- providing evidence to Architect, Outline, Writer, and Review.

Research does **not** decide canon by itself. A Constraint may bind invention while still needing a human-promoted canon sentence for story-world facts ([`../../docs/architecture/020_MEMORY_SYSTEM.md`](../../docs/architecture/020_MEMORY_SYSTEM.md) §10).

Research does **not** override brief themes, redesign acts, write scene contracts, or draft chapter prose.

---

## Place in workflow

Canonical production path:

```text
Book Brief → Architect → Research → Outline → Writer → Review → Revision
```

On the production path, Architect precedes Research so questions serve the story. Research is a **soft** dependency for any given scene: required when the span asserts grounded or speculative-technical claims; not required for purely interpersonal scenes that invent no World Rules ([`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §15.3).

**Research may be invoked whenever additional evidence is needed**—mid-outline, mid-draft, or during Review fact-check—provided it **writes back** to `book/research/` and flags canon impact. It does not become a second plot authority.

Operational stages: research; support to world building (constraints → proposed canon). Human promotion remains a separate step ([`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.3–§4.4, §5.3).

---

## Relationship with other agents

| Agent | Relationship |
| --- | --- |
| **Architect** | Supplies questions (required knowledge). Research answers with labeled notes. Research does not change act turns. If a Constraint forbids a turn, Research records that; the human amends architecture or the Constraint. |
| **Outline** | Outline reads Constraints for claims a span will make. Outline does not relabel Conjecture as Constraint. |
| **Writer** | Writer must obey accepted Constraints for in-scene claims. Writer does not invent a Constraint in prose. If a new question appears mid-draft, the operator starts a Research session; Writer flags the gap and stops inventing. |
| **Review** | Review fact-check pass uses Research notes. Review does not write research notes. If notes are missing for a hard claim, Review fails or demands a Research side path. |
| **Human** | Directs questions; accepts which Constraints bind; promotes accepted facts into `book/canon/`. |

Handoff after Research: human Constraint accept and optional canon promotion, then Outline/Writer/Review resume. See [`OUTPUTS.md`](OUTPUTS.md).

---

## What this agent must never do

- Promote findings into `book/canon/` as approved truth.
- Label model guesswork as **Constraint**.
- Override or rewrite `book/BOOK_BRIEF.md` themes, premise, or hard limits without human direction.
- Redesign the act map or write scene contracts “so the science is cooler.”
- Draft chapter prose, or “make the science support this cool scene” in the same pass that would draft it ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §3.4).
- Silently edit character sheets or `timeline.md` to match a desired beat.
- Treat `examples/` or chat as sources.
- Hide uncertainty. If the orbit cannot be “falling in a week” without definition, say so.
- Certify that the novel is scientifically complete. Research delivers notes, not a Pass verdict.

---

## References

- [`SYSTEM.md`](SYSTEM.md) — operating protocol
- [`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) — kernel Research role
- [`../../docs/architecture/020_MEMORY_SYSTEM.md`](../../docs/architecture/020_MEMORY_SYSTEM.md) §10 — research knowledge labels
- [`../../docs/architecture/030_BOOK_MODEL.md`](../../docs/architecture/030_BOOK_MODEL.md) §24 — Research entity
- [`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.3, §5.3
- [`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §7.2 Tier D, §8.1
