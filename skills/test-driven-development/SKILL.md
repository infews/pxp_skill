---
name: test-driven-development
description: "Use when implementing any feature or bugfix in code — guides the strict RED-GREEN-REFACTOR cycle: write one failing test, confirm it fails for the right reason, write the minimal code to pass, refactor while the context is fresh."
---

# Test-Driven Development

A disciplined cycle for writing code one behavior at a time: red (failing test) → green (minimal code) → refactor.

## The Iron Law

**No production code without a failing test first.** Not even one line. Not even "I'll just sketch this out." Not even "the test is obvious — I'll add it after."

## The Cycle

### 1. Write the failing test

Write a single test for one behavior. The test should:

- Describe *what* you want, not *how* it's done.
- Fail in a meaningful way (assertion error — not syntax error, not missing import).
- Be small — favor one method's behavior over a whole class.

### 2. Confirm red

Run the test. Confirm it fails for the expected reason.

- Unexpected failure reason: diagnose first; don't paper over with implementation.
- Passes without any implementation: the behavior already exists, or the test isn't testing what you think. Investigate.

### 3. Write the minimal implementation

Write the simplest code that makes the test pass. Resist the urge to add more.

- No "while I'm here" additions.
- No defensive code for cases not yet tested.
- No abstractions for hypothetical future tests.

### 4. Confirm green

Run the test. Confirm it passes. Run the full suite to check for regressions.

### 5. Refactor

Look at the code you just wrote and the code around it:

- Is there duplication to extract?
- Are names clear and intention-revealing?
- Is the code as simple as it can be while remaining clear?
- Any code smells?

If you see an improvement, make it. Re-run tests. If nothing needs refactoring, say so out loud and move on.

## Common Rationalizations

| Excuse | Reality |
|---|---|
| "Test is trivial, I'll skip it" | Trivial tests catch real regressions later. Write it. |
| "I already manually tested it" | Manual tests don't run in CI. Write the automated test. |
| "I'll write the test after" | Tests-after asks "what does this do?" Tests-first asks "what should this do?" Different exercises. |
| "Refactor isn't needed here" | Say so explicitly. Don't silently skip Step 5. |
| "I'll batch the refactor at the end" | Refactor with context fresh, not after you've moved on. |

## Red Flags — STOP

- Code before test.
- "I'll just sketch this out and add the test later."
- Test added after the code already passed manually.
- Implementing more than one behavior per cycle.
- Skipping Step 5 without saying so.

All of these mean: stop, delete the unprotected code, start the cycle properly.