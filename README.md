# AI Book Framework

A production-grade operating system for writing novels with AI inside [Cursor](https://cursor.com) IDE.

This repository is not a prompt pack and not a one-click book generator. It is a **repository-native literary workspace**: durable markdown artifacts, specialized agent roles, explicit workflow gates, and git-backed history—so long-form fiction stays coherent while AI accelerates research, structure, drafting, and review.

The human author remains the executive producer, final editor, and moral owner of the work.

---

## Why this exists

Naive AI chat collapses at novel length. Continuity drifts, sessions forget decisions, and the same assistant is asked to invent, draft, and critique itself. The AI Book Framework treats novel production as **process infrastructure**:

- Intent is captured once in a book brief and reused everywhere.
- Continuity lives in project memory and canon files, not in unrecalled chat.
- Roles stay separated: architect, research, outline, writer, review.
- Every meaningful stage leaves inspectable artifacts in the repository.
- Cursor provides the runtime; git provides the audit trail.

For the full rationale, see [`docs/specifications/000_PROJECT_VISION.md`](docs/specifications/000_PROJECT_VISION.md).

---

## Canonical workflow

```text
Book Brief → Architect → Research → Outline → Writer → Review → Revision
```

Each stage consumes upstream contracts and produces durable outputs. Review is mandatory before a chapter is considered done. A short operator view lives in [`WORKFLOW.md`](WORKFLOW.md); the architectural model is in [`docs/specifications/001_ARCHITECTURE.md`](docs/specifications/001_ARCHITECTURE.md).

---

## Current status (Sprint 1)

Sprint 1 establishes the **framework kernel documentation and repository shape**:

| Area | Status |
| --- | --- |
| Vision & architecture specs | Present |
| Operator docs (README, Quick Start, User Guide) | Present |
| Changelog & MIT license | Present |
| Directory scaffolding (docs, prompts, templates, examples, modules, tools) | Present |
| Specialized agent contracts | Not yet (later sprint) |
| Cursor rule packs | Not yet (later sprint) |
| Book production templates & novel content | Not yet (later sprint) |

You can read the system, learn the workflow, and prepare to write. Agent specs and book artifact packs arrive in subsequent sprints.

---

## Quick start

1. Open this repository in Cursor.
2. Read [`QUICK_START.md`](QUICK_START.md) (five-minute path).
3. Read [`USER_GUIDE.md`](USER_GUIDE.md) before running a full novel pipeline.
4. Study [`docs/specifications/000_PROJECT_VISION.md`](docs/specifications/000_PROJECT_VISION.md) and [`docs/specifications/001_ARCHITECTURE.md`](docs/specifications/001_ARCHITECTURE.md).

When book templates and agents are added in later sprints, production begins by filling the book brief and advancing stage by stage—never by asking a single chat to “write the novel.”

---

## Repository map

```text
.
├── README.md                 ← you are here
├── QUICK_START.md            ← shortest onboarding path
├── USER_GUIDE.md             ← full operator manual
├── WORKFLOW.md               ← stage summary
├── PROJECT.md                ← project identity metadata
├── CHANGELOG.md              ← release history
├── LICENSE                   ← MIT
├── AGENTS.md                 ← index for agent specs (populated later)
├── docs/
│   ├── specifications/       ← normative framework law
│   ├── guides/               ← practical authoring guides
│   └── decisions/            ← architecture decision records
├── agents/                   ← role contracts (later sprint)
├── prompts/                  ← reusable task playbooks
├── templates/                ← blank artifact schemas
├── examples/                 ← format samples (non-canon)
├── modules/                  ← optional genre/style/export packs
├── tools/                    ← optional validators and assemblers
├── book/                     ← active novel state (content in later sprints)
└── .cursor/rules/            ← Cursor runtime rules (later sprint)
```

Authority and conflict rules for these trees are defined in the [architecture specification](docs/specifications/001_ARCHITECTURE.md).

---

## Core principles (summary)

1. **Artifacts over chat** — if a decision matters later, write it into the repository.
2. **Continuity is infrastructure** — facts, timelines, and promises have durable homes.
3. **Separation of concerns** — architecture ≠ drafting ≠ critique.
4. **Human gates** — approve brief, structure, and chapter acceptance yourself.
5. **Fail loudly** — do not silently invent away missing inputs or canon conflicts.
6. **Git as history** — commits and branches record narrative and process evolution.

---

## Who this is for

- Novelists who want AI leverage without surrendering control
- Sci-fi authors who need disciplined worldbuilding (sci-fi-first instantiation)
- Writers comfortable in an IDE + git workflow
- Cursor users who already orchestrate multi-file agent work

If you want a fully autonomous “generate my book” button with no review, this framework is the wrong tool. See non-goals in the [project vision](docs/specifications/000_PROJECT_VISION.md).

---

## Documentation index

| Document | Purpose |
| --- | --- |
| [QUICK_START.md](QUICK_START.md) | Fast path to understanding and preparing the workspace |
| [USER_GUIDE.md](USER_GUIDE.md) | Day-to-day operating manual |
| [WORKFLOW.md](WORKFLOW.md) | Stage pipeline summary |
| [CHANGELOG.md](CHANGELOG.md) | What changed between versions |
| [docs/specifications/000_PROJECT_VISION.md](docs/specifications/000_PROJECT_VISION.md) | Mission, principles, success criteria |
| [docs/specifications/001_ARCHITECTURE.md](docs/specifications/001_ARCHITECTURE.md) | System structure, memory, agents, conventions |
| [docs/guides/](docs/guides/) | Practical guides as they are added |
| [docs/decisions/](docs/decisions/) | Architecture Decision Records |

---

## License

This project is released under the [MIT License](LICENSE).

---

## Contributing posture

Framework changes should preserve the kernel contracts in `docs/specifications/`. Prefer additive modules over rewriting authority rules. Significant architectural pivots belong in `docs/decisions/` as ADRs and must be reflected in [`CHANGELOG.md`](CHANGELOG.md).
