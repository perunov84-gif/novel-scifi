# Research — Outputs

**Agent:** Research  
**Audience:** Humans accepting Constraints; Outline, Writer, and Review consuming grounding  
**Companion files:** [`SYSTEM.md`](SYSTEM.md) · [`INPUTS.md`](INPUTS.md) · [`CHECKLIST.md`](CHECKLIST.md)

Outputs live under `book/research/` ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §6). Chat is not the archive.

---

## Expected outputs

| Output | Owner | Purpose |
| --- | --- | --- |
| `book/research/<topic>.md` (stable topic slug, e.g. `orbital-decay.md`) | Research writes; human accepts Constraint language | Grounding record for one question cluster |
| Labeled findings: **Source**, **Constraint**, **Conjecture**, **Rejected** | Research writes | Prevents other agents from mixing fact and guess |
| Impacted canon path list | Research writes | Promotion and cascade targets; prevents orphan notes |
| Uncertainty / open evidence questions | Research writes | What must not be treated as known |
| Lean `PROJECT_MEMORY.md` flags | Research may write | Constraint risks, promotion pending, next gate |
| Optional `*.alt.md` only for competing *interpretations* the human must choose | Research writes | Candidates, not a second live topic file |

**Does not write:** silent edits to character sheets or timeline; act maps; scene contracts; chapter prose; `Status: Approved` on canon; Review verdicts.

---

## Output format

One live markdown file per topic. Update in place. Do not create `orbital-decay-final.md`.

Recommended shape:

1. Header metadata.
2. **Question(s)** with IDs (`K-1`, `Q-4`, or `R-12` if Review-originated).
3. **Sources** — what was consulted; what each source actually supports.
4. **Constraints** — rules invention must not violate, if the human accepts them.
5. **Conjecture** — hypotheses, model estimates, unspecified parameters.
6. **Rejected** — considered and refused, with why, so agents do not revive them.
7. **Impacted canon files** — paths that would change on promotion (create-if-needed is allowed as a path name).
8. **Uncertainty** — remaining gaps.
9. **Promotion proposal (optional)** — suggested canon *sentences* clearly marked `Proposal:`, `Status` not approved.

Labels must appear as explicit headings or prefixed lines, not buried in prose.

---

## Required metadata

| Field | Research sets | Notes |
| --- | --- | --- |
| `Status:` | `Draft` | Human may later mark the note `Approved` for Constraint language, or leave Draft while only some lines are accepted. Research never self-approves. |
| `Owner:` | Author | Human. |
| `Stage:` | Research | |
| `Last-reviewed:` | Date or commit if known | Else omit. |
| `Questions:` | IDs this note answers | Traceability to Architect/Outline/Review |

Each **Constraint** line should be quotable in isolation (one rule, present tense, testable).

---

## Quality requirements

A note is fit to offer the human when:

- Every binding claim is labeled; unlabeled paragraphs are treated as defective.
- **Constraint** vs **Conjecture** is not blurred.
- Sources are not overclaimed (“this blog proves the station falls in seven days”).
- Impacted paths are named.
- Brief hard limits are respected (no Constraint that assumes FTL in a no-FTL book).
- Uncertainty is explicit where numbers are underdetermined.
- Ethical: no pasted copyrighted fiction presented as original research prose.

Anti-quality: a Wikipedia-tone essay with no labels; a Constraint invented to save a drafted spectacle; empty impact list.

---

## Validation

| Check | Pass |
| --- | --- |
| Path | Under `book/research/`, stable filename |
| Labels | Source / Constraint / Conjecture / Rejected used correctly |
| Canon | No silent canon edits |
| Orphans | Impact list non-empty whenever a Constraint or promotion proposal exists |
| Conflicts | Disagreement with brief, other Constraints, or canon is named |
| Role | No prose chapter, no Review verdict, no architecture rewrite |
| Memory | Project memory has pointers, not a duplicate of the note |

Review later treats Conjecture as **not** a pass for factual claims in prose ([`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §4.13).

---

## Downstream consumers

| Consumer | What they take | What they must not take |
| --- | --- | --- |
| **Human** | Accept Constraints; promote or strike proposals | Research does not skip this gate |
| **Architect** (re-entry only) | Evidence that a turn is illegal under a Constraint | A new act map written by Research |
| **Outline** | Accepted Constraints for the span’s claims section | Conjecture as scene clock or capability |
| **Writer** | Same Constraints for in-scene claims | License to invent missing numbers |
| **Review (fact-check pass)** | Notes listed for the scene | A substitute for filing defects |
| **Canon files** | Nothing until human copies accepted facts | Research is not World |

If Research runs mid-draft, consumers are Review and Writer on the **next** sessions after write-back—not a blended fix.

---

## Examples

### Example — sufficient note excerpt (pedagogical)

```markdown
Status: Draft
Owner: Author
Stage: Research
Questions: K-1

## Question
What still has power on the sealed deck, and what heat tell is honest?

## Sources
- Author stipulation in brief: no FTL (intent; not orbital mechanics).
- Author-provided note: battery packs dump heat when loaded.

## Constraints
- C-R1: The story must not claim Helios is “falling in a week” without a defined orbit and decay mechanism.
- C-R2: Sealed-deck power must be batteries, residual reactor, or a lie a character tells. Unexplained “still on” is forbidden.

## Conjecture
- Residual reactor vs batteries is not decided by sources in this note.

## Rejected
- Gravity plating as the heat source (conflicts with intended spin-gravity world; do not revive).

## Impacted canon files
- `book/canon/world/helios-station.md`
- `book/canon/timeline.md`
```

**Handoff:** Research finished. `book/research/orbital-decay.md` is `Draft`. Human must accept C-R1/C-R2 before Outline/Writer treat them as binding. Do not treat Conjecture as the power-source answer. Next gate: Constraint accept; optional promotion of “cold reactor / battery heat tell” into the world file.

### Example — invalid output

A paragraph in chat: “Stations fall fast, fusion is fine, I updated the character sheet so Aya knows the core is cold.” That violates path stability, labels, and canon authority.

---

## References

- [`CHECKLIST.md`](CHECKLIST.md)
- [`../outline/README.md`](../outline/README.md) — consumer of Constraints
- [`../writer/README.md`](../writer/README.md)
- [`../review/README.md`](../review/README.md) — fact-check consumer
- [`../../docs/architecture/020_MEMORY_SYSTEM.md`](../../docs/architecture/020_MEMORY_SYSTEM.md) §10, §28.1
- [`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §6
