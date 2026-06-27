# Pivotal XP — Claude Code Plugin

A Claude Code plugin for running solo or small-team coding sessions with Pivotal Labs–style Extreme Programming discipline. You and Claude pair on epics, slice them into stories, write EARS specifications, and TDD each spec one behavior at a time — all with explicit developer approval at every gate.

## What's in the box

Three skills, designed to ship together but usable independently:

- **`pivotal-xp`** — the workflow. Five phases from epic refinement to story completion, with developer-approval gates at every TDD transition.
- **`test-driven-development`** — a standalone RED-GREEN-REFACTOR discipline skill. Used as a fallback by `pivotal-xp` when no richer TDD skill is installed.
- **`ears-specifications`** — a reusable reference for EARS (Easy Approach to Requirements Syntax) patterns and the semantic-ID convention. Useful outside Pivotal-XP for any Spec-Driven-Development flow.

## Install

Add the community marketplace if you haven't already, then install:

```bash
claude plugin marketplace add anthropics/claude-plugins-community
claude plugin install pxp@claude-community
```

Or install directly from the repository:

```bash
claude plugin install github:infews/pxp_skill
```

Skills are namespaced under `pxp:` — see the table below for what's available.

| Skill | Invocation |
|---|---|
| Pivotal XP workflow | `/pxp:pivotal-xp` (or Claude picks it up automatically) |
| EARS Specifications | `/pxp:ears-specifications` |
| Test-Driven Development | `/pxp:test-driven-development` |

## Update

**Marketplace install:** auto-update is on by default. To pull updates manually:

```bash
/plugin marketplace update claude-community
```

**Direct git install:** reinstall when a new version is released:

```bash
claude plugin install github:infews/pxp_skill
```

## Uninstall

```bash
/plugin uninstall pxp@claude-community
```

Or if you installed directly from git, omit the marketplace suffix:

```bash
/plugin uninstall pxp
```

## First session

The skill triggers on phrasings like:

- "Let's pair on this epic"
- "I want to work this in Pivotal XP style"
- "Help me slice this into stories"
- "Let's TDD this spec"

A typical opener:

> **You:** I have a feature idea — let's work it as a Pivotal XP epic.
>
> **Claude:** Let's start with epic refinement. Share the rough idea and I'll help shape it into an epic doc under `/docs/epics/`. I'll ask clarifying questions in batches, then move us into story slicing.

## The workflow at a glance

Five phases, with developer approval at every gate:

1. **Epic Refinement** — Iterate on a markdown epic; clarify scope, users, acceptance criteria.
2. **Story Slicing** — Decompose the epic into small, independently testable stories.
3. **EARS Specifications + TDD Plan** — Convert Gherkin acceptance criteria into EARS specs; sequence them by dependency.
4. **Test-Driven Development** — One spec at a time. Strict RED → GREEN → REFACTOR per spec, with developer approval at every transition.
5. **Story Completion** — Run the full suite, update the parent epic doc, move on.

All artifacts live in `/docs/epics/` alongside your code — no Jira, no Linear, no external store.

## Works with

`pivotal-xp` Phase 4 prefers external TDD skills if available, falling back to the bundled one:

| TDD skill installed | What pivotal-xp uses |
| --- | --- |
| `superpowers-ruby:test-driven-development` | superpowers-ruby version (first choice) |
| `superpowers:test-driven-development` | superpowers version (second choice) |
| Neither | `pxp:test-driven-development` (bundled fallback) |

The package is fully self-contained — no other plugins required.

## Tips

- **Model choice.** Phases 1–3 reward Opus (epic refinement, story design, and EARS authoring are high-judgment). Phase 4 (mechanical RED-GREEN-REFACTOR) runs well on Sonnet, or use `/fast` to keep Opus quality with faster streaming. Switch with `/model` between phases or toggle `/fast` within a phase.
- **File ordering.** Epics and stories use 4-digit prefixes (`0001-`, `0010-`) with gaps, so you can insert later items without renumbering.
- **Resumable across sessions.** Because all spec state lives in the story doc with `@spec` annotations in the code, work picks back up cleanly across long sessions or after a context reset.
- **EARS standalone.** If you only want lightweight Spec-Driven-Dev (no Pivotal workflow), use the `ears-specifications` skill on its own — it's not coupled to `pivotal-xp`.

## Background

Pivotal Labs was a software consultancy known for rigorous Extreme Programming practice: 100% pair programming, test-first discipline, small vertical slices, and ruthless simplicity. This package adapts that discipline to a human-and-agent pair. The agent proposes, you refine and approve, you both stay focused on the spec in front of you.

For more on XP: Kent Beck's *Extreme Programming Explained* and Martin Fowler's writing on XP cover the underlying methodology.