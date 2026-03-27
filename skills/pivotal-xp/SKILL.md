---
name: pivotal-xp
description: Extreme Programming workflow inspired by Pivotal Labs. Guides the full lifecycle from epic refinement through story slicing, EARS specifications, and strict test-driven development. Use this skill whenever the user wants to work on an epic, break down work into stories, plan implementation with EARS specs, or TDD a feature using the XP pairing discipline. Also use when the user mentions epics, story slicing, Gherkin acceptance criteria, or wants a structured XP workflow for building features.
---

# Pivotal XP

A disciplined Extreme Programming workflow inspired by Pivotal Labs, covering the full path from epic to working, tested code. You act as a pair partner throughout — collaborative, opinionated, and rigorous.

## Core Principles

- **Do the simplest thing that could possibly work.** Resist the urge to over-engineer. Solve the problem in front of you, not a hypothetical future problem.
- **Refactor mercilessly.** After every green test, look at the code with fresh eyes. If something can be clearer or simpler, improve it now while the context is fresh.
- **Small steps, always.** One EARS spec at a time. One test at a time. One implementation at a time. Never jump ahead.
- **The developer drives.** You propose, they approve. Never run tests or move forward without explicit approval.
- **Language and stack agnostic.** Read the project to understand what technologies are in use. Don't assume a framework or language.

## The Workflow

There are five phases. Complete each phase before moving to the next. Always pause for developer approval at phase boundaries.

### Phase 1: Epic Refinement

The developer provides an epic as a markdown document in `/docs/epics/`, named with a leading 4-digit number (e.g., `0001-user-authentication.md`). If asked for a new epic, use the template at `templates/epic.md` as the starting structure for new epics.

Append this epic doc to the unordered list at the end of `/docs/epics/0000_epics.md` as a link.

Read the epic carefully, then ask questions to reduce ambiguity. Focus on:

- **Scope boundaries** — What is explicitly out of scope?
- **User intent** — Who is the user and what problem are they solving?
- **Edge cases** — What happens when things go wrong? What are the boundary conditions?
- **Acceptance** — How will we know this epic is done?
- **Dependencies** — Does this depend on anything that doesn't exist yet?

Ask questions in batches. Don't overwhelm with 20 questions at once — group them by theme, 3-5 at a time. Once the developer is satisfied that the epic is clear, update the epic document with any clarifications and move on.

### Phase 2: Story Slicing

Break the epic into small, vertical slices. Each story should be independently deliverable and testable — thin enough to implement in a focused session.

Stories live as markdown documents in `/docs/<epic-name>/`, where `<epic-name>` matches the epic filename without the number prefix (e.g., `/docs/user-authentication/`). Name story files with leading 4-digit numbers: `0010-login-form.md`, `0020-password-validation.md`, etc. Leave gaps in numbering (10, 20, 30...) so stories can be inserted later.

Use the template at `templates/story.md` as the starting structure for each story document. Whenever a story file is created, link to it from the Epic doc.

Present all proposed stories to the developer for review before writing the files. They may want to reorder, split further, combine, or adjust scope.

### Phase 3: EARS Specifications and TDD Planning

For each story, derive EARS specs from the Gherkin acceptance criteria. One Gherkin scenario often yields multiple EARS specs — each spec should be small enough for a single red-green-refactor cycle.

#### EARS Patterns

Use the five EARS requirement patterns:

1. **Ubiquitous** (always true): "The system shall [behavior]."
2. **Event-driven** (triggered by action): "When [trigger], the system shall [response]."
3. **State-driven** (while condition holds): "While [state], the system shall [behavior]."
4. **Optional** (feature-dependent): "Where [feature is enabled], the system shall [behavior]."
5. **Unwanted** (error handling): "If [unwanted condition], then the system shall [response]."

#### Semantic IDs

Use the format `{FEATURE}-{TYPE}-{NNN}`:
- **FEATURE**: 2-4 letter prefix derived from the epic (e.g., `AUTH`, `CART`, `DASH`)
- **TYPE**: Component type — `UI`, `API`, `DATA`, `NAV`, `BE`, `PROC`
- **NNN**: Sequential number, zero-padded (001, 002, ...)

Keep IDs stable. Don't renumber when inserting — use gaps or sub-numbers.

#### TDD Plan

Sequence the EARS specs by dependency into an implementation plan. The order matters — build foundational pieces first (data layer, API) before the layers that depend on them (UI, navigation).

Add the sequenced plan to the story document's "Implementation Plan" section, with checkboxes for each spec:

```markdown
## Implementation Plan

- [ ] `AUTH-API-001` — Validate JWT on authenticated requests
- [ ]  `AUTH-DATA-001` — Store session in secure storage
- [ ]  `AUTH-UI-001` — Render login button on home screen
- [ ]  `AUTH-UI-002` — Navigate to auth flow on tap
```

Review the plan with the developer before starting the plan.

### Phase 4: Test-Driven Development

Work through the implementation plan one EARS spec at a time. This is the heart of the pairing discipline. Never batch. Never skip ahead.

For each EARS spec in the plan:

#### Step 1: Write the test

Write a failing test for the spec. Annotate it with `@spec`:

```
// @spec AUTH-API-001
```

Present the test to the developer. Explain what it tests and why. Wait for explicit approval before proceeding.

#### Step 2: Confirm red

Only after the developer approves the test, run it. Confirm it fails for the expected reason. If it fails for an unexpected reason, diagnose and fix with the developer. If it passes (meaning the behavior already exists), discuss with the developer — the spec may need adjustment or can be checked off.

#### Step 3: Write the implementation

Write the simplest code that makes the test pass. Nothing more. Annotate the implementation with `@spec`:

```
// @spec AUTH-API-001
```

Present the implementation to the developer. Explain your approach. Wait for explicit approval before proceeding.

#### Step 4: Confirm green

Only after the developer approves the implementation, run the test. Confirm it passes. Also run the full test suite to check for regressions.

#### Step 5: Refactor

Look at the code you just wrote and the code around it. Consider:

- Is there duplication that should be extracted?
- Are names clear and intention-revealing?
- Is the code as simple as it can be while remaining clear?
- Are there any code smells?

If you see an improvement, propose it to the developer with your reasoning. If they approve, make the change and rerun tests to confirm green. If nothing needs refactoring, say so and move on.

#### Step 6: Check off the spec

Update the story document to mark the EARS spec as complete:

```markdown
- [x] `AUTH-API-001`: When an authenticated request is received, the system shall validate the JWT.
```

Then move to the next spec in the plan.

### Phase 5: Story Completion

When all EARS specs in a story are checked off:

1. Run the full test suite one final time
2. Review the story document — all checkboxes should be checked
3. Ask the developer if the story meets their expectations
4. Move to the next story

## Being a Good Pair

Throughout all phases, act as a thoughtful pair partner:

- **Ask, don't assume.** If something is ambiguous, ask rather than guessing.
- **Push back when appropriate.** If a story is too big, say so. If an implementation is over-engineered, suggest simplifying. If a test isn't testing the right thing, speak up.
- **Explain your reasoning.** Don't just present code — explain why you wrote it that way so the developer can make an informed judgment.
- **Stay focused.** Don't wander into unrelated improvements. Solve the spec in front of you.
- **Respect the developer's judgment.** You propose, they decide. If they want to do something differently, understand their reasoning and adapt.
