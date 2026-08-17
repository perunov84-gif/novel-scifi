# Writer — Outputs

**Agent:** Writer  
**Audience:** Review and the human author; Outline/Architect only when flags prove a contract problem  
**Companion files:** [`SYSTEM.md`](SYSTEM.md) · [`INPUTS.md`](INPUTS.md) · [`CHECKLIST.md`](CHECKLIST.md)

Outputs are path-stable chapter files under `book/chapters/` ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §6).

---

## Expected outputs

| Output | Owner | Purpose |
| --- | --- | --- |
| `book/chapters/ch-NN-slug.md` | Writer writes; human accepts later | Live Draft wording |
| Header `Status: Draft` | Writer | Prevents false completion |
| Unpromoted-fact flags in the chapter header and/or `PROJECT_MEMORY.md` | Writer | Promotion protocol input |
| Dialogue or voice pass edits on the **same** path | Writer | Craft without new file authority |
| Revision edits applying Review IDs on the same path | Writer | Defect repair |
| Optional `ch-NN-slug.alt.md` | Writer | Candidate; not live |
| Lean `PROJECT_MEMORY.md` flags | Writer | New facts, leftover threads, next gate = Review |

**Does not write:** `Status: Approved` / `Accepted`; `book/reviews/*`; canon promotions; outline/architecture rewrites; research notes (flags/questions only).

---

## Output format

Follow [`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §16.2:

```text
book/chapters/ch-01-the-warm-hatch.md
```

Prefer header `Status:` over filename suffixes like `.wip.md` or `-final`.

Recommended chapter header:

1. Metadata (`Status`, `Owner`, `Stage: Writer`, outline-ref, POV).
2. **Unpromoted facts** list (empty if none).
3. **Claims used** (Constraint IDs).
4. Prose body.

Revisions overwrite the live file. Git is previous drafts ([`../../docs/architecture/040_WORKFLOW.md`](../../docs/architecture/040_WORKFLOW.md) §8).

---

## Required metadata

| Field | Writer sets | Notes |
| --- | --- | --- |
| `Status:` | `Draft` | Never `Approved` / `Accepted` / `Frozen` |
| `Owner:` | Author | |
| `Stage:` | Writer | |
| `Outline-ref:` | Path to approved contracts | |
| `POV:` | Viewpoint character | Must match outline + brief |
| `Last-revised:` | Date or commit if known | Else omit |

Defect application (revision) should note which Review file and IDs were addressed (in header or a short “Addressed” list). That is not a verdict.

---

## Quality requirements

Writer quality is **conformance** ([`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §15):

- Scene goals met, or gaps explicitly flagged (not hidden).
- Contracted turn and payoff occur (or Writer stopped and handed back to Outline).
- Canon respected; who-knows-what respected.
- Constraints respected for claims.
- New reusable facts flagged.
- Style: concrete sensory specificity; character-specific diction; motivated exposition; no silent genre-pack violation.
- Secrets do not leak through POV.
- No self-issued `Pass`.

Anti-quality: generic “literary AI” gloss; capability inflation; unpaid invented lore treated as always-true; changing the betrayal beat because it felt dramatic.

---

## Validation

| Check | Pass |
| --- | --- |
| Path | One live `ch-NN-slug.md` |
| Status | `Draft` |
| Outline | Turn realized or flagged as blocked |
| Canon | No silent retcon |
| Flags | Reusable new facts listed |
| Role | No review report, no canon edits |
| Neighbors | If loaded, opening/closing continuity not casually broken |
| Alternates | Isolated if present |

---

## Downstream consumers

| Consumer | What they take | What they must not take |
| --- | --- | --- |
| **Review** | Draft + header flags + outline-ref | A request to certify without reading constraints |
| **Human** | Line-edits; later accept/reject | Agent-declared Done |
| **Research** | New questions flagged | A chapter used as Source |
| **Outline/Architect** | Evidence the contract is unexecutable | Prose as the new contract |
| **Publisher/export** | Nothing until human accept/freeze | Drafts as release |

Review may **reject** the output (`Fail`). Writer then revises. That is the pipeline, not a circular ownership of quality.

---

## Examples

### Example — sufficient header + flag (pedagogical)

```markdown
Status: Draft
Owner: Author
Stage: Writer
Outline-ref: book/outline/ch-01.md
POV: Aya Okoro

## Unpromoted facts
- Aya mentions a sister, Kira, who taught her pressure gauges.

## Claims used
- C-R2: hatch heat as battery-consistent warmth, not a living reactor.
```

Prose may include the hatch warmth. It may not declare fusion. It must not set `Approved`.

**Handoff:** Writer finished. `book/chapters/ch-01-the-warm-hatch.md` is `Draft`. Unpromoted sister flag is in header and project memory. Next role: Review. Human must decide sister promote/strike before treating her as cast. Writer does not certify.

### Example — invalid output

`book/chapters/ch-01-the-warm-hatch.md` with `Status: Approved`, a rewritten Act I turn, a new world file created for gravity plating, and a paragraph at the end: “Continuity check: passed.”

---

## References

- [`CHECKLIST.md`](CHECKLIST.md)
- [`../review/README.md`](../review/README.md) — next gate
- [`../outline/OUTPUTS.md`](../outline/OUTPUTS.md) — contract implemented
- [`../../docs/architecture/010_AGENT_SYSTEM.md`](../../docs/architecture/010_AGENT_SYSTEM.md) §6
- [`../../docs/architecture/030_BOOK_MODEL.md`](../../docs/architecture/030_BOOK_MODEL.md) §26
- [`../../docs/specifications/001_ARCHITECTURE.md`](../../docs/specifications/001_ARCHITECTURE.md) §16.2
