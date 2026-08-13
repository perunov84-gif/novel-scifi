# Prompt Library

Reusable task playbooks for framework stages.

## Purpose

Prompts encode known-good procedures for narrow jobs (for example: act design, continuity audit, voice pass). They improve reproducibility across sessions and models.

Prompts are **not** a substitute for agent contracts or specifications. They are procedures invoked inside a role.

## Layout

```text
prompts/
  architect/   ← structure and thematic spine tasks
  research/    ← grounding and consistency tasks
  outline/     ← scene and act planning tasks
  writer/      ← drafting and rewrite tasks
  review/      ← quality and continuity adjudication tasks
```

## Prompt document standard

Each prompt file should include:

- Intent
- Required context files
- Preconditions (gates that must already be green)
- Instruction body
- Output schema and destination path
- Stop conditions / human escalation
- Anti-patterns

## Relationship to other surfaces

| Surface | Job |
| --- | --- |
| `docs/specifications/` | Law |
| `agents/` | Role identity and boundaries |
| `.cursor/rules/` | Always-on runtime constraints |
| `prompts/` | Single-job playbooks |

## Related documents

- [`../docs/specifications/001_ARCHITECTURE.md`](../docs/specifications/001_ARCHITECTURE.md) (Prompt Library section)
- [`../USER_GUIDE.md`](../USER_GUIDE.md)
