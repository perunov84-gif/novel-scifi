# Modules

Optional extension packs that plug into the framework kernel.

## Purpose

Modules add genre depth, style controls, export pipelines, series tooling, or evaluation helpers without forking the core OS.

## Module families (target)

- **Genre packs** — e.g. sci-fi plausibility rubrics and world schemas
- **Style packs** — voice profiles and anti-cliché constraints
- **Series** — shared canon roots and spoiler boundaries
- **Export** — manuscript assembly to publishing formats
- **Evaluation** — continuity regression checks and defect analytics

## Hard rules

A module may provide prompts, templates, rubrics, and optional tools.  
A module may **not**:

- Replace `BOOK_BRIEF` or memory authority
- Bypass review gates
- Require opaque binary state for core continuity

## Module manifest

Each module should include a `MODULE.md` describing name, version, kernel compatibility, provided artifacts, override policy, and enablement steps.

## Related documents

- [`../docs/specifications/000_PROJECT_VISION.md`](../docs/specifications/000_PROJECT_VISION.md)
- [`../docs/specifications/001_ARCHITECTURE.md`](../docs/specifications/001_ARCHITECTURE.md) (Future Modules section)
