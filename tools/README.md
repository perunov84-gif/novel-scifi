# Tools

Optional automation that serves repository contracts.

## Purpose

Tools validate invariants, assemble manuscripts, and check continuity metadata. They are servants of the file-based API defined in the architecture—not a second source of truth.

## Appropriate tools

- Manuscript assemblers that concatenate approved chapter files
- Continuity linters that verify required headers or broken references
- Checklist runners for stage entrance/exit criteria
- Export helpers used by an export module

## Design constraints

- Prefer readable inputs/outputs (markdown, JSON, plain text logs)
- Fail with actionable messages when contracts are violated
- Do not hide canon inside undiffable binary databases for core workflow
- Keep secrets out of tool configs committed to git

## Related documents

- [`../docs/specifications/001_ARCHITECTURE.md`](../docs/specifications/001_ARCHITECTURE.md)
- [`../modules/README.md`](../modules/README.md)
- [`../.gitignore`](../.gitignore)
