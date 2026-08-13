# 000 — Project Vision

**Document type:** Foundational specification  
**Status:** Canonical  
**Audience:** Framework maintainers, agent authors, novelists adopting the system, and future contributors  
**Scope:** Vision, mission, principles, architecture intent, and long-term direction for the AI Book Framework  

---

## 1. Project Vision

The AI Book Framework is a production-grade operating system for writing novels with artificial intelligence inside Cursor IDE.

It is not a chatbot prompt pack, not a one-shot chapter generator, and not a loose collection of markdown tips. It is a structured, agent-driven literary workspace: a repository-native system in which human authorship, machine assistance, editorial judgment, research discipline, and long-form continuity are coordinated through explicit roles, durable artifacts, and enforceable workflow gates.

The long-term vision is a professional creative environment where a novelist can take a story from first spark to publication-ready manuscript without abandoning craft, coherence, or ownership. In that environment:

- Intent is captured once and reused everywhere.
- Continuity is treated as infrastructure, not as an afterthought.
- Agents specialize the way a real writing room specializes.
- The human remains the executive producer, final editor, and moral author of the work.
- Quality is measurable, reviewable, and improvable across drafts.
- The repository itself becomes the memory, process, and product of the novel.

Cursor IDE is the primary runtime surface because it already combines version control, multi-file context, agent orchestration, rules, and human-in-the-loop editing. The framework turns that surface into a literary production system: briefs become contracts; outlines become maps; chapters become versioned deliverables; reviews become structured quality events; and project memory becomes the shared ground truth that prevents drift.

Success, in this vision, is not “AI wrote a book.” Success is: a human author shipped a better novel, faster, with fewer continuity failures, stronger thematic control, and a reproducible process that can be applied to the next book with even less friction.

---

## 2. Mission

The mission of the AI Book Framework is to give serious writers a reliable, extensible, Cursor-native operating system for novel production—one that multiplies human creative capacity without replacing human creative responsibility.

Concretely, the framework exists to:

1. **Encode professional writing process** into repository structure, agent roles, and review loops.
2. **Preserve narrative continuity** across hundreds of thousands of words through explicit memory, specs, and cross-checks.
3. **Separate concerns** so research, architecture, drafting, style, and critique do not collapse into a single undifferentiated prompt.
4. **Make AI assistance auditable** by leaving durable artifacts at every stage of the pipeline.
5. **Scale from one novel to many** without requiring reinvention of process for each title.
6. **Remain tool-honest**: designed for Cursor, compatible with git, and open to future agent runtimes without becoming hostage to any single model vendor.

The framework’s mission is craft amplification. It assumes that novels worth writing demand judgment, taste, thematic discipline, and emotional truth that no model can own alone. AI is treated as a capable junior staff: powerful, fast, inconsistent without supervision, and transformative when given clear briefs, constraints, and review.

---

## 3. Why the Framework Exists

### 3.1 The problem it addresses

Long-form fiction is uniquely hostile to naive AI usage. Short-form generation can hide inconsistency; a novel cannot. Across acts, chapters, and revisions, authors encounter recurring failure modes:

- **Continuity collapse:** character voice, timelines, facts, and foreshadowing drift.
- **Prompt amnesia:** each session restarts without institutional memory.
- **Style oscillation:** prose quality and tone vary by model, temperature, and mood of the request.
- **Process opacity:** drafts appear without a clear chain from brief → architecture → outline → prose → review.
- **Role confusion:** the same agent is asked to invent world rules, draft scenes, and critique itself—corrupting both creativity and judgment.
- **Tool fragmentation:** notes in one app, drafts in another, chat history elsewhere, and no single source of truth.
- **Scale fragility:** methods that work for a short story break at novel length.

Existing approaches tend to fail in one of two directions. Either they are too loose—chat threads that evaporate—or too rigid—templates that produce mechanical prose without a living editorial process. Professional writers need something in between: a system that is structured enough to protect continuity and flexible enough to serve art.

### 3.2 The opportunity

Cursor already provides the missing substrate for a literary OS:

- Multi-file workspace context
- Agent rules and specialized agent definitions
- Version control as first-class history
- Diff-based human review
- Repeatable workflows encoded in the repository
- Local persistence of project memory and specifications

The opportunity is to stop treating novel writing with AI as improvisation and start treating it as **production engineering for narrative**. Film and games already use pipelines, bibles, and review gates. Novels deserve the same professionalization—adapted to prose, voice, and literary judgment rather than shot lists or level design.

### 3.3 The bet

The framework bets that:

- Durable markdown and structured specs beat ephemeral chat.
- Specialized agents beat a single “do everything” assistant.
- Explicit workflow stages beat freeform generation.
- Human approval gates beat fully autonomous book dumping.
- Repository-as-product beats app-as-silo.
- Genre-aware modules (starting with sci-fi) can later generalize without abandoning rigor.

If those bets hold, the result is not merely faster drafting. It is a new category of creative infrastructure: an AI-assisted novel operating system that professionals can trust.

---

## 4. Long-Term Goals

### 4.1 Near-term (foundation)

- Establish a complete, documented skeleton for novel projects inside the repository.
- Define agent roles for architecture, research, outlining, writing, and review.
- Codify a canonical workflow: Brief → Architect → Research → Outline → Writer → Review.
- Create durable project memory and book brief artifacts as the continuity backbone.
- Provide clear user guidance so a writer can start a first novel without reverse-engineering the system.

### 4.2 Mid-term (production maturity)

- Harden quality gates: continuity checks, voice checks, plot integrity, pacing audits.
- Introduce genre packs and style packs as modular extensions.
- Support multi-draft revision cycles with explicit change rationale.
- Build checklists and acceptance criteria for chapter and act completion.
- Improve agent handoffs so each stage consumes and produces typed artifacts.
- Enable reproducible runs: same inputs, comparable outputs, measurable deltas.

### 4.3 Long-term (ecosystem)

- Evolve from a sci-fi-first novel framework into a general professional book OS.
- Support series continuity across multiple volumes with shared canon registries.
- Offer optional tooling for export, manuscript formatting, and editorial packages.
- Allow teams (author + editor + researcher) to collaborate through the same repository conventions.
- Provide evaluation harnesses for agent quality over time (continuity scores, regression suites for canon).
- Remain portable across model generations while preserving process integrity.

### 4.4 North-star outcome

A working novelist can open a Cursor workspace, load the framework, and confidently produce a publishable novel with:

- coherent world and character continuity,
- intentional thematic architecture,
- professional revision discipline,
- transparent AI assistance,
- and a reusable process for the next book.

---

## 5. Core Principles

### 5.1 Human authorship is non-negotiable

The human author owns the creative thesis, final voice decisions, ethical boundaries, and publication responsibility. Agents propose; humans dispose. The framework must never obscure who is accountable for the work.

### 5.2 Continuity is infrastructure

Facts, timelines, character states, lore, and promises to the reader are treated as system state. They live in durable artifacts, not in unrecalled conversation. Drift is a defect, not a creative flourish—unless the author explicitly redesigns canon.

### 5.3 Separation of concerns

Architecture is not drafting. Research is not style. Critique is not invention. Agents and documents have distinct jobs so that creative generation and quality control do not contaminate each other.

### 5.4 Artifacts over chat

If a decision matters later, it must be written into the repository. Chat may accelerate thinking; only files create institutional memory.

### 5.5 Workflow before wizardry

Fancy prompts cannot compensate for a broken process. Stages, gates, and handoffs come first. Model cleverness is an accelerator, not a substitute for structure.

### 5.6 Review is a first-class stage

No chapter is “done” because it was generated. Completion requires review against brief, outline, continuity, voice, and craft criteria.

### 5.7 Explicit over implicit

Assumptions about genre conventions, POV, tone, taboo topics, and narrative promises must be stated. Ambiguity is expensive at novel scale.

### 5.8 Reproducibility and auditability

A future reader of the repository—human or agent—should be able to reconstruct why a chapter exists in its current form: which brief constraints applied, which outline beat it served, which review findings were addressed.

### 5.9 Minimal magic, maximal clarity

Prefer boring, inspectable markdown and conventions over opaque automation. Transparency compounds trust; hidden pipelines compound failure.

### 5.10 Extensibility without chaos

New agents, genre modules, and tools may be added only if they respect the core workflow contracts and do not invent parallel sources of truth.

---

## 6. Design Philosophy

### 6.1 Literary production as a controlled system

The framework designs novel writing as a pipeline with feedback loops, not as a single generative act. Inspiration remains welcome; unmanaged inspiration is not allowed to overwrite canon silently.

### 6.2 Contracts between stages

Each stage has inputs, outputs, and acceptance criteria:

- **Book Brief** contracts premise, genre, audience, tone, themes, and constraints.
- **Architect** contracts structure, acts, major turns, and thematic spine.
- **Research** contracts factual or speculative grounding needed for credibility.
- **Outline** contracts scene-level intent, conflict, and payoff.
- **Writer** contracts prose delivery against outline and voice rules.
- **Review** contracts quality adjudication and required fixes.

These contracts make agent collaboration possible. Without them, every agent reinvents the book.

### 6.3 Dual audience design

Every major document is written for two readers: the human author and the agent runtime. Clarity, naming, and structure must serve both. Dense jargon that helps neither is rejected. Ambiguous prose that “sounds literary” in process docs is rejected.

### 6.4 Progressive disclosure

A new user should be able to start with a short path (brief → outline → draft → review) while advanced users unlock deeper architecture, research packs, series canon, and evaluation tools. Complexity is layered, not front-loaded.

### 6.5 Taste-preserving automation

Automation should remove drudgery—continuity lookups, checklist enforcement, structural reminders—not flatten voice into generic “AI prose.” Style guidance exists to protect distinctiveness, not to impose a house monotone unless the author requests one.

### 6.6 Fail loudly

When continuity conflicts, missing inputs, or incomplete briefs are detected, the system should surface blockers rather than quietly inventing patches. Silent invention is how novels rot.

### 6.7 Git as creative history

Commits, branches, and diffs are part of the craft loop. Alternative plot directions can live on branches. Reviews can reference diffs. Manuscript history is recoverable. The repository is not merely storage; it is the novel’s black box recorder.

### 6.8 Sci-fi first, principles general

The initial concrete instantiation targets science fiction—where worldbuilding, technobabble discipline, and speculative consistency are especially demanding. The design philosophy remains genre-agnostic at the OS layer so future packs (fantasy, thriller, literary, romance) can plug in without rewriting the kernel.

---

## 7. Guiding Principles

These guiding principles operationalize the vision for day-to-day decisions by maintainers and users.

1. **Prefer one source of truth.** If a fact exists in two places, define which document wins.
2. **Write for the next session.** Assume the next agent or human has no chat memory.
3. **Name roles precisely.** “Writer” writes; “Reviewer” reviews; do not blur titles.
4. **Keep stages sequential unless a loop is explicit.** Random jumps create invisible debt.
5. **Optimize for revision.** First drafts are expected to be wrong in useful ways.
6. **Measure coherence before eloquence.** Beautiful prose that breaks canon is still a defect.
7. **Protect the reader’s trust.** Foreshadowing, clues, and internal logic are promises.
8. **Keep secrets intentional.** Mystery is designed; confusion is accidental.
9. **Document constraints early.** Hard limits (POV, rating, themes to avoid) reduce thrash.
10. **Change canon deliberately.** Retcons require explicit updates to memory and specs.
11. **Treat models as replaceable.** Process must survive model upgrades and degradations.
12. **Leave the camp cleaner than you found it.** Every session should improve artifacts, not only generate text.

---

## 8. Target Users

### 8.1 Primary users

- **Serious novelists** writing long-form fiction who want AI leverage without losing control.
- **Sci-fi authors** who need disciplined worldbuilding and technical/speculative consistency.
- **Author-engineers** comfortable in git/IDE environments who prefer repository-native workflows.
- **Cursor power users** already orchestrating agents and rules for complex projects.

### 8.2 Secondary users

- **Editorial collaborators** who review structure, continuity, and prose quality inside the same workspace.
- **Series planners** managing multi-book canon and character arcs.
- **Writing teams** in small studios where roles map cleanly onto framework agents.
- **Framework contributors** extending agents, rules, genre packs, and quality tooling.

### 8.3 User attributes that fit

The framework is optimized for users who:

- accept that process discipline is part of craft,
- are willing to maintain briefs and memory documents,
- want transparency over “magic book button,”
- and value revision as central to quality.

### 8.4 Users this is not optimized for

- Writers seeking fully autonomous one-click novels with no review.
- Users unwilling to work in a repository/IDE paradigm.
- Projects that reject any structure in favor of pure improvisation with no continuity needs.
- Short-form social content pipelines (different problem class).

These exclusions are respectful, not dismissive: different tools serve different jobs.

---

## 9. Non-Goals

The framework explicitly does **not** aim to:

1. **Replace the author.** It will not claim autonomous creative authorship or moral ownership of the novel.
2. **Be a general chatbot UI.** Conversation is a means; durable artifacts are the product.
3. **Guarantee bestseller outcomes.** Process improves odds and quality control; markets remain uncertain.
4. **Support every genre on day one.** Sci-fi rigor comes first; other genres follow via packs.
5. **Hide model limitations.** Hallucinations, blandness, and inconsistency are expected failure modes to be managed, not denied.
6. **Become a closed SaaS prison.** The core remains repository-native and inspectable.
7. **Auto-publish without human gates.** Export helpers may exist later; publication decisions stay human.
8. **Optimize for infinite free-form roleplay.** Narrative RP and novel production share DNA but are different products.
9. **Enforce a single aesthetic.** The OS provides control surfaces for style; it does not impose one house voice as dogma.
10. **Boil the ocean with premature platformization.** Prefer a sharp novel OS over a vague “AI creativity suite.”
11. **Substitute legal/editorial counsel.** Rights, defamation, and publishing law remain outside scope.
12. **Provide covert plagiarism workflows.** Research and inspiration must remain ethical and attributable where required.

Clear non-goals protect the roadmap from dilution and keep contributors aligned.

---

## 10. Success Criteria

The framework is succeeding when the following are true.

### 10.1 Process success

- A new project can be initialized from framework conventions without ad-hoc reinvention.
- The canonical workflow is understandable in under thirty minutes for a technical writer.
- Each stage produces durable inputs for the next stage.
- Reviews catch continuity and structural issues before they compound across acts.

### 10.2 Continuity success

- Character facts, timelines, and world rules remain consistent across chapters unless intentionally changed.
- Project memory and brief documents are actively maintained and consulted.
- Contradictions are detected and resolved through explicit updates, not silent overwrites.

### 10.3 Craft success

- Prose quality improves across revision cycles under human direction.
- Voice remains recognizably intentional rather than generically machine-smoothed.
- Pacing, stakes, and thematic through-lines are traceable from architecture to scenes.

### 10.4 Operational success

- Authors can resume after weeks away without reconstructing context from chat logs.
- Git history meaningfully explains major narrative decisions.
- Agent roles remain stable as models change.
- Contributors can extend the system without breaking core contracts.

### 10.5 Product success (longer horizon)

- Multiple complete novels are produced with the framework.
- At least some users reuse the system for a second book with less setup cost.
- External contributors can add genre/style modules cleanly.
- The framework’s reputation is “professional and trustworthy,” not “gimmicky generator.”

### 10.6 Negative success signals (watchouts)

- Users skip briefs and wonder why continuity fails.
- Agents overwrite canon without updating memory.
- Review stages are treated as optional decoration.
- The repository becomes prompt spaghetti with no ownership of truth.
- “Faster chapters” is celebrated while “worse book” accumulates.

Success is a better finished novel and a healthier process—not merely higher word count per hour.

---

## 11. Technology Stack

The framework is intentionally lean and IDE-native.

### 11.1 Primary runtime

- **Cursor IDE** as the operator console for agents, rules, edits, and human review.
- **Git** for versioning, branching alternate plotlines, and audit history.
- **Markdown** as the universal artifact format for briefs, memory, outlines, chapters, and specs.
- **Repository structure** as the system schema (folders and naming conventions as API).

### 11.2 Intelligence layer

- **LLM agents** invoked through Cursor’s agent surfaces.
- **Specialized agent definitions** under `agents/` for role separation.
- **Cursor rules** under `.cursor/rules/` for persistent constraints and style/process enforcement.
- **Project memory documents** as long-context anchors that outlive individual chats.

### 11.3 Content layer

- Book brief and premise documents
- Architecture and outline artifacts
- Research notes and canon registries
- Chapter drafts and revision notes
- Review reports and fix lists
- Optional style guides and genre packs

### 11.4 Supporting tooling (evolutionary)

Not all of the following are required on day one; they are compatible directions:

- Lightweight scripts for manuscript assembly and checks
- Lint-like validators for continuity fields and required metadata
- Export pipelines to DOCX/EPUB/print-ready formats
- Evaluation harnesses comparing drafts against outline/canon checklists
- CI-style checks for repository invariants (required files present, broken references, etc.)

### 11.5 Stack principles

- **File-first:** databases are optional; durable text is mandatory.
- **Vendor-agnostic process:** model APIs may change; workflow contracts should not.
- **Human-readable by default:** binary or opaque state is avoided unless there is a clear win.
- **Local-friendly:** a writer should be able to work primarily in-repo without mandatory cloud lock-in for core process.

---

## 12. High-Level Architecture

### 12.1 Architectural metaphor

Think of the framework as a **literary operating system**:

- **Kernel:** repository conventions, source-of-truth rules, workflow gates
- **Drivers:** Cursor agents and rules
- **Applications:** genre packs, style packs, research modules
- **Filesystem:** briefs, memory, outlines, chapters, reviews
- **Userland:** the human author directing priorities and accepting/rejecting output

### 12.2 Core pipeline

```text
Book Brief
    → Architect (structure / thematic spine / act design)
        → Research (canon grounding / technical or speculative support)
            → Outline (scene intentions / conflict / payoff)
                → Writer (prose generation under constraints)
                    → Review (quality, continuity, craft adjudication)
                        ↺ Revision loop back to Outline or Writer as needed
```

This pipeline is the backbone. Side paths are allowed (e.g., targeted research mid-draft) but must return updates to memory and specs.

### 12.3 Major subsystems

1. **Intent Subsystem**  
   Captures what the book is trying to be: genre, premise, audience, themes, tone, boundaries.

2. **Continuity Subsystem**  
   Stores and protects canon: characters, timeline, world rules, open threads, promises.

3. **Structure Subsystem**  
   Holds architecture and outlines: acts, beats, scene purposes, escalation logic.

4. **Production Subsystem**  
   Produces chapters and intermediate drafts under constraints from the above.

5. **Quality Subsystem**  
   Reviews, checklists, defect reports, and acceptance criteria.

6. **Operations Subsystem**  
   Guides, changelogs, agent specs, rules, and contributor conventions that keep the OS coherent.

### 12.4 Data flow and authority

Authority flows downward unless a formal revision elevates a change:

- Brief constrains Architecture.
- Architecture constrains Outline.
- Outline constrains Writer.
- Memory/Canon constrains all generative stages.
- Review can demand changes but does not silently rewrite canon; it files defects.
- Human approval can amend any layer, with mandatory artifact updates.

### 12.5 Agent topology (logical)

- **Architect Agent:** narrative structure, stakes progression, thematic alignment.
- **Research Agent:** domain grounding, speculative consistency, reference synthesis.
- **Outline Agent:** scene planning and beat-level clarity.
- **Writer Agent:** prose generation in voice.
- **Review Agent:** adversarial quality and continuity critique.

Agents may be implemented as distinct Cursor agent specs, rule bundles, or both. The architecture cares about role purity more than file count.

### 12.6 Human-in-the-loop control points

Mandatory human checkpoints include at least:

- approving or revising the Book Brief,
- accepting architectural direction for the novel,
- green-lighting act/outline plans before mass drafting,
- reviewing chapter accept/reject/revise decisions,
- authorizing canon changes.

Automation may prepare options; humans commit direction.

### 12.7 Repository as runtime

Unlike app architectures that hide state in databases, this system treats the repository as the live state machine. Folder layout, filenames, and document sections are part of the API. That makes the architecture inspectable, diffable, and resilient to tool churn.

---

## 13. Repository Philosophy

### 13.1 The repository is the product

The novel project is not “whatever is in the chat.” The repository—specifications, memory, chapters, reviews—is the product and the process record. If it is not in the repo, it is not yet real for the system.

### 13.2 Convention over configuration

Stable naming, predictable folders, and canonical document types beat elaborate config languages. Authors should spend energy on story, not on inventing structure each time.

### 13.3 Small durable cores, expandable packs

The core OS stays thin: vision, workflow, agent contracts, memory model, quality gates. Genre-specific depth lives in packs and project-level documents so the kernel does not become a monolith of special cases.

### 13.4 Docs as executable intent

Specifications are not decorative. They instruct humans and agents. Ambiguous docs are bugs. Outdated docs are defects. Documentation debt equals continuity risk.

### 13.5 Changelog honesty

Significant process or canon shifts should be visible in project history. Surprises are for readers of the novel, not for operators of the framework.

### 13.6 Safety and ethics in-repo

Boundaries (graphic content limits, research ethics, copyright hygiene) belong in explicit project constraints. The repository should make the author’s rules discoverable to every agent session.

### 13.7 Contribution posture

Extensions should arrive as coherent modules with clear ownership: new agent specs, new rules, new checklists, new genre packs. Drive-by prompt dumping without contracts is rejected as anti-architecture.

### 13.8 Stage-appropriate completeness

Early repository stages may be skeletal. That is acceptable if the skeleton is intentional and the vision remains clear. Empty files without purpose are not “minimalism”; they are holes. Placeholder documents should declare what they will become.

---

## 14. Future Scalability

### 14.1 Scaling story complexity

As plots, POVs, and timelines grow, the continuity subsystem must scale through structured registries, cross-reference discipline, and review checklists—not through longer unstructured chats.

### 14.2 Scaling to series

Multi-book franchises require:

- shared canon separated from book-specific arcs,
- volume-level briefs that inherit series constraints,
- character life-state tracking across titles,
- spoiler boundary controls for agents working on later books.

### 14.3 Scaling teams

Future collaboration may include author, co-author, editor, and researcher roles mapped onto branch permissions and document ownership. The architecture should not assume a single loner forever, even if v1 optimizes for one primary author.

### 14.4 Scaling quality systems

Expect progression from manual review prompts → standardized rubrics → semi-automated continuity linters → regression tests for canon invariants. Scalability means defects become cheaper to catch as the manuscript grows, not more expensive.

### 14.5 Scaling genres and styles

Genre packs should plug into the same kernel:

- shared workflow and memory model,
- specialized research and tropes modules,
- distinct review rubrics,
- optional style guides.

### 14.6 Scaling models and tooling

Model vendors will change. Context windows will change. Cursor features will change. Scalability here means insulating narrative contracts from vendor specifics: agents consume artifacts and produce artifacts; transport mechanisms remain replaceable.

### 14.7 Scaling ambition carefully

The roadmap should expand by proving each layer:

1. one coherent novel process,
2. strong continuity discipline,
3. repeatable revision loops,
4. modular extensions,
5. multi-book canon,
6. optional automation and export.

Skipping to “platform” before “working novel OS” is a failure mode.

---

## 15. Quality Philosophy and Editorial Stance

Although quality appears throughout this vision, it deserves a direct statement.

The framework treats editorial quality as a system property emerging from:

- clear intent,
- sound structure,
- disciplined continuity,
- voice control,
- iterative review,
- and human taste.

It rejects the false dichotomy between “art” and “process.” Professional art domains use process precisely to protect artistic risk where it matters. By stabilizing continuity and structure, the author can take bolder swings in theme, character, and prose.

Review agents should be constructively adversarial: loyal to the book’s ambitions, not to the writer agent’s ego. Their job is to find breaks in logic, pacing sag, thematic drift, exposition dumps, and continuity faults. They do not “win” by being cruel; they win by being specific and actionable.

---

## 16. Ethical and Creative Responsibility

### 16.1 Authorship transparency

Authors using AI assistance remain responsible for disclosure norms in their publishing contexts. The framework does not launder authorship; it clarifies process.

### 16.2 Respect for sources

Research modules must encourage lawful and ethical use of references. The system should help authors track inspirations and avoid regurgitating copyrighted text.

### 16.3 Harm and boundaries

Projects may explore dark themes; they should do so under explicit author constraints. Agents must honor project boundaries regarding content, representation, and safety limits defined by the author.

### 16.4 No deceptive automation theater

The framework should not pretend that unsupervised generation equals finished literature. Honesty about review needs is part of professional ethics.

---

## 17. Relationship to Cursor IDE

Cursor is the chosen cockpit because it uniquely compresses:

- agent orchestration,
- rules and persistent instructions,
- multi-file reasoning,
- inline human edits,
- and git-native iteration.

The framework is therefore **Cursor-first**, not Cursor-exclusive in philosophy. If future environments provide similar affordances, the same artifact contracts should transfer. What must not transfer away is the discipline: briefs, memory, staged workflow, and review gates.

Cursor-specific assets (agents, rules) are adapters. Narrative architecture is the system of record.

---

## 18. Roadmap Posture (Vision-Level)

Without prescribing implementation tickets, the vision implies a sequenced posture:

1. **Define the OS kernel** — vision, workflow, roles, memory model, repository law.
2. **Make one path work end-to-end** — brief to reviewed chapters for a real novel.
3. **Harden continuity and review** — checklists, defect language, acceptance criteria.
4. **Modularize** — genre/style packs and cleaner agent contracts.
5. **Extend** — series support, export, evaluation harnesses, team workflows.

Each phase should leave the repository more operable than before, not merely more annotated.

---

## 19. Risks and Mitigations (Strategic)

| Risk | Why it matters | Mitigation direction |
| --- | --- | --- |
| Process theater | Docs exist but are unused | Make stages produce mandatory artifacts; reviews check them |
| Canon drift | Novel collapses at scale | Single sources of truth + continuity reviews |
| Generic prose | Books sound like “AI” | Style constraints, human voice passes, anti-slop review criteria |
| Over-automation | Author disengagement | Human gates at intent, architecture, and acceptance |
| Model churn | Breaks prompts and habits | Artifact-centered contracts; avoid vendor lock-in in kernel |
| Scope creep | OS becomes vague suite | Enforce non-goals; sci-fi-first depth before breadth |
| Empty skeleton syndrome | Structure without practice | Drive development through real book production |

---

## 20. Definition of Done for This Vision Document

This document is doing its job when:

- a new contributor can explain what the framework is and is not,
- roadmap debates can appeal to principles rather than taste alone,
- agent authors know which roles and contracts they must respect,
- users understand why briefs, memory, and reviews are mandatory,
- and future specifications can inherit terminology without redefining the mission.

Subsequent specification documents should refine interfaces, schemas, and workflows. They should not reinvent the reason the system exists.

---

## 21. Closing Statement

The AI Book Framework exists because novels deserve professional creative infrastructure.

Writers already carry the hardest parts: vision, taste, emotional truth, and the courage to revise. What they should not have to carry alone is the operational burden of continuity, process amnesia, role confusion, and unmanaged AI chaos.

By turning Cursor into a literary operating system—complete with specialized agents, durable artifacts, explicit workflow gates, and human authority—the framework aims to make ambitious novels more finishable without making them less human.

The destination is not a machine that writes books.  
The destination is a disciplined partnership in which machines help build worlds, hold continuity, draft options, and stress-test quality—while the author remains the mind that chooses what the book becomes.

This vision is the north star for every subsequent specification, agent, rule, and manuscript produced under the framework.

---

## Document Control

| Field | Value |
| --- | --- |
| ID | `000_PROJECT_VISION` |
| Title | Project Vision — AI Book Framework |
| Layer | Foundational |
| Depends on | None (root vision) |
| Enables | Workflow specs, agent specs, memory model, quality gates, genre packs |
| Maintenance rule | Update when mission, non-goals, or architectural philosophy materially change |

---

*End of document.*
