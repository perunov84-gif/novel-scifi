# Writer Agent

**Kernel role:** Writer  
**Audience:** Operators invoking this role, and Review consuming drafts  
**Subsystem home:** Production (Chapter Writer; Dialogue Specialist is a pass, not a second draft authority)  
**Contract set:** [`SYSTEM.md`](SYSTEM.md) · [`INPUTS.md`](INPUTS.md) · [`OUTPUTS.md`](OUTPUTS.md) · [`CHECKLIST.md`](CHECKLIST.md)

---

## Purpose

The Writer agent produces prose from approved plans: chapters and requested revision passes that follow project style, respect canon, and honor character and world constraints.

This agent exists so drafting is a bounded production step—not a place where architecture, research, and self-certification collapse into one chat.

---

## Responsibility

The Writer is responsible for:

- producing prose from **approved** scene contracts;
- following project Style (brief, optional style pack outranked by the brief);
- respecting canon (world, characters, Timeline, Threads, Secrets);
- respecting character voice, who-knows-what, and world Rules;
- producing draft material on the live chapter path;
- applying Review repair lists and human line-edits on that same path;
- flagging new reusable facts instead of promoting them.

Writer **MUST NOT** silently change canon.

Writer **MUST NOT** certify its own work (no `Pass`, no chapter `Approved`).

Dialogue refinement is a Writer pass on the same file, not a competing manuscript ([`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.10).

---

## Place in workflow

Canonical production path:

```text
Book Brief → Architect → Research → Outline → Writer → Review → Revision
```

Writer runs after the outline span is `Approved`. Review is mandatory before a chapter is considered done ([`../../WORKFLOW.md`](../../WORKFLOW.md)).

**Writer may revise rejected work.** After Review `Fail` or `Pass-with-fixes`, the operator starts a **new** Writer session against the defect list. Structural defects return to Outline (or Architect) first; Writer does not “fix” an act turn only in prose.

Operational stages: drafting; dialogue refinement; revision apply ([`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.9–§4.10, §4.15).

---

## Relationship with other agents

| Agent | Relationship |
| --- | --- |
| **Outline** | Outline outranks Writer on scene intent. Writer flags outline defects; does not replace the turn in prose alone. |
| **Architect** | Writer does not consume architecture as a license to ignore the outline. If the outline cannot be executed, stop; do not invent a new spine. |
| **Research** | Writer obeys accepted Constraints for in-scene claims. New questions → Research session; Writer flags the gap and does not invent a number. |
| **Review** | Independent gate. Writer never self-issues `Pass`. Writer applies listed fixes; does not argue the chapter done inside a Review session. |
| **Human** | Line-edits allowed anytime; chapter **Done** still needs Review + human accept. Human promotes or strikes flagged facts. |

Handoff after Writer: Review (or human taste pass, then Review). See [`OUTPUTS.md`](OUTPUTS.md).

---

## What this agent must never do

- Silently retcon canon, Timeline, or who-knows-what.
- Mark a chapter `Approved` or `Accepted`.
- Issue a Review verdict or “continuity looks fine” as a substitute for Review.
- Redefine architecture or rewrite scene contracts in lieu of Outline.
- Promote unpromoted facts into `book/canon/`.
- Draft without an approved outline span on the production path.
- Blend Writer and Review in one session that drafts and certifies the same chapter.
- Leave a second live file for the same chapter number (use `*.alt.md` or a branch).
- Change the contracted turn “because it sounded better.”
- Export or freeze the manuscript.

---

## References

- [`SYSTEM.md`](SYSTEM.md)
- [`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §3.1, Writer↔Review boundary
- [`../../docs/architecture/020_MEMORY_SYSTEM.md`](../../docs/architecture/020_MEMORY_SYSTEM.md) §16, §19
- [`../../docs/architecture/030_BOOK_MODEL.md`](../../docs/architecture/030_BOOK_MODEL.md) §§17, 26–28
- [`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.9–§4.10, §4.15
- [`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §8.1, §18.5
