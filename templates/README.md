# Templates

Blank schemas for framework and book artifacts.

## Purpose

Templates define the minimum viable structure so humans and agents produce interoperable documents: briefs, memory files, outlines, chapters, reviews, character sheets, and research notes.

## Rules

- Copy a template to its destination path; do not turn this folder into the live manuscript.
- Prefer additive fields over renames when evolving a template.
- Mark required sections clearly.
- Keep examples out of templates when possible; put teaching samples in [`../examples/`](../examples/).

## Expected template set (as production sprints land)

| Template | Typical destination |
| --- | --- |
| `BOOK_BRIEF.template.md` | `book/BOOK_BRIEF.md` |
| `PROJECT_MEMORY.template.md` | `book/PROJECT_MEMORY.md` |
| `SCENE_OUTLINE.template.md` | `book/outline/` |
| `CHAPTER.template.md` | `book/chapters/` |
| `REVIEW_REPORT.template.md` | `book/reviews/` |
| `CHARACTER_SHEET.template.md` | `book/canon/characters/` |
| `RESEARCH_NOTE.template.md` | `book/research/` |

## Related documents

- [`../docs/specifications/001_ARCHITECTURE.md`](../docs/specifications/001_ARCHITECTURE.md) (Templates section)
- [`../USER_GUIDE.md`](../USER_GUIDE.md)
