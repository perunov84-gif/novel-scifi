# Quick Start

Five-minute onboarding for the AI Book Framework in Cursor.

For the full operating manual, see [`USER_GUIDE.md`](USER_GUIDE.md). For mission and system design, see [`docs/specifications/000_PROJECT_VISION.md`](docs/specifications/000_PROJECT_VISION.md) and [`docs/specifications/001_ARCHITECTURE.md`](docs/specifications/001_ARCHITECTURE.md).

---

## Prerequisites

- [Cursor](https://cursor.com) installed
- This repository cloned and opened as a Cursor workspace
- Basic familiarity with markdown files and git commits

---

## Step 1 — Open the workspace

Open the repository root in Cursor (the folder that contains `README.md` and `docs/`).

Confirm you can see:

- `docs/specifications/000_PROJECT_VISION.md`
- `docs/specifications/001_ARCHITECTURE.md`
- `USER_GUIDE.md`
- `WORKFLOW.md`

---

## Step 2 — Read the kernel (in this order)

1. This file (you are here)
2. [`README.md`](README.md) — what the framework is
3. [`WORKFLOW.md`](WORKFLOW.md) — Brief → Architect → Research → Outline → Writer → Review
4. [`docs/specifications/000_PROJECT_VISION.md`](docs/specifications/000_PROJECT_VISION.md) — why the system exists
5. [`docs/specifications/001_ARCHITECTURE.md`](docs/specifications/001_ARCHITECTURE.md) — where truth lives and how parts connect

You do not need to memorize every section. You need the mental model: **durable artifacts, staged roles, human approval gates**.

---

## Step 3 — Know what Sprint 1 gives you

After Sprint 1 you have:

- Professional operator documentation
- Canonical vision and architecture specifications
- Repository directories for prompts, templates, examples, modules, tools, and docs
- MIT license and changelog

Sprint 1 does **not** yet include:

- Finished agent role files under `agents/`
- Cursor rule packs under `.cursor/rules/`
- Book templates or novel manuscript content under `book/`

Those arrive in later sprints. Do not invent a parallel process in chat while waiting—use the documented workflow shape so later artifacts plug in cleanly.

---

## Step 4 — Learn the production path (even before book files land)

When book artifacts are enabled, the first production actions will be:

1. Create or fill `book/BOOK_BRIEF.md` from the brief template (templates sprint).
2. Keep `book/PROJECT_MEMORY.md` short and current.
3. Run stages in order; do not jump to prose without approved intent and outline.
4. After each drafting session, update memory/canon and commit logical units in git.
5. Treat review as a real gate, not optional praise.

Until those files ship, practice the discipline in documentation form: write decisions into markdown, not only into chat.

---

## Step 5 — Set a useful Cursor habit now

In every future writing session:

1. State the **role** you want (architect, research, outline, writer, or review).
2. `@`-reference the **files that role must read** (brief, memory, outline slice, chapter under review).
3. Require **file outputs**, not chat-only answers.
4. Stop when inputs are missing—**fail loudly** instead of inventing canon.

This matches the architecture’s “artifacts over chat” rule.

---

## Step 6 — Optional: skim the folder map

| Path | You use it for |
| --- | --- |
| `docs/specifications/` | Normative rules of the OS |
| `docs/guides/` | Practical how-tos |
| `docs/decisions/` | Recorded design decisions |
| `prompts/` | Reusable task playbooks (populated later) |
| `templates/` | Blank schemas for book artifacts (populated later) |
| `examples/` | Format samples, never live canon |
| `book/` | The active novel’s state (content later) |
| `agents/` | Role contracts (later) |
| `.cursor/rules/` | Persistent IDE constraints (later) |

---

## Done when

You can answer yes to all of the following:

- I can explain the framework in one paragraph without calling it a “prompt pack.”
- I can list the canonical workflow stages in order.
- I know that chat is ephemeral and repository files are authoritative.
- I know Sprint 1 is documentation/kernel shape, not full novel tooling yet.
- I know where to read next: [`USER_GUIDE.md`](USER_GUIDE.md).

---

## Next reading

- [`USER_GUIDE.md`](USER_GUIDE.md) — day-to-day operation, quality habits, git practice
- [`CHANGELOG.md`](CHANGELOG.md) — what shipped in this version
