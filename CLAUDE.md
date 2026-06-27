# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

`pxp` is a Claude Code plugin. The plugin manifest lives at `.claude-plugin/plugin.json`. It ships three skills under `skills/` that work together to support a Pivotal Labs–style XP pairing workflow: epic refinement → story slicing → EARS specs → TDD.

This is a pure-markdown repo. There are no build steps, no test suite, and no package manager.

## Skill architecture

```
skills/
  pivotal-xp/          — main workflow skill (5-phase XP cycle)
  ears-specifications/ — sub-skill for EARS pattern reference
  test-driven-development/ — bundled fallback TDD sub-skill
```

Each skill is a directory containing `SKILL.md` (with YAML frontmatter) and optional subdirectories. The `pivotal-xp` skill also has a `templates/` directory with `epic.md` and `story.md` starters.

### How the skills relate

`pivotal-xp` is the orchestrator. In Phase 3 it requires `pxp:ears-specifications` as a sub-skill. In Phase 4 it requires a TDD skill via a fallback chain:

1. `superpowers-ruby:test-driven-development` — preferred if the user has it
2. `superpowers:test-driven-development` — second choice
3. `pxp:test-driven-development` — the bundled copy in this package (last resort)

This package must work standalone for users without any superpowers plugin installed. The bundled `test-driven-development` skill is the safety net — it should never be the primary recommendation in external contexts.

**Rule for all future cross-skill references:** external/upstream first, local bundled copy last.

## Skill frontmatter

Every `SKILL.md` begins with a YAML frontmatter block:

```yaml
---
name: skill-name
description: When to trigger this skill (used by Claude Code for automatic matching)
---
```

The `description` field is what Claude Code reads to decide when to invoke the skill. It should describe the trigger conditions precisely — too broad causes false positives, too narrow misses valid uses.

## Artifact conventions (for the pivotal-xp workflow)

When the `pivotal-xp` skill creates project artifacts, they follow these conventions:

- **Epic index:** `/docs/epics/0000-epics.md` — unordered list of links to all epics
- **Epic docs:** `/docs/epics/NNNN-<slug>.md` (e.g., `0010-user-auth.md`)
- **Story docs:** `/docs/epics/<epic-slug>/NNNN-<slug>.md` (e.g., `/docs/epics/0010-user-auth/0010-login-form.md`)
- **Numbering:** 4-digit prefixes with gaps (`0010`, `0020`, `0030`...) so items can be inserted without renumbering

EARS spec IDs follow `{FEATURE}-{TYPE}-{NNN}` (e.g., `AUTH-API-001`). IDs are permanent once issued — never renumber, use gaps or sub-numbers like `001a` for insertions.

## README

The README is the primary user-facing document for this package. It covers install methods (`claude plugin install`), the skill namespace (`pxp:`), first-session trigger phrases, the five workflow phases, and the TDD sub-skill fallback table. Keep the README in sync with any structural changes to the skills.
