# Architecture Decision Records

This directory stores Architecture Decision Records (ADRs) for the AI Book Framework.

## Purpose

ADRs capture significant design choices: what was decided, why, what alternatives were rejected, and what consequences follow. They preserve rationale that raw specs often omit.

## When to add an ADR

Add an ADR when you:

- Change repository layout or authority rules
- Alter workflow gates or memory ownership
- Introduce a module type that affects the kernel
- Reject a major alternative that contributors may revisit later

Routine copy edits and small template tweaks do not need ADRs; record those in [`../../CHANGELOG.md`](../../CHANGELOG.md) when user-facing.

## Naming

Use:

```text
ADR-0001-short-kebab-title.md
ADR-0002-another-decision.md
```

Number monotonically. Keep titles specific.

## Suggested ADR shape

1. Title and status (`Proposed`, `Accepted`, `Superseded`)
2. Context
3. Decision
4. Alternatives considered
5. Consequences
6. References to specs or PRs

## Related documents

- [`../specifications/001_ARCHITECTURE.md`](../specifications/001_ARCHITECTURE.md)
- [`../../CHANGELOG.md`](../../CHANGELOG.md)
