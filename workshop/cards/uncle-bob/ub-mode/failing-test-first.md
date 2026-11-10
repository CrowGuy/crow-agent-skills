---
id: uncle-bob/ub-mode/failing-test-first
kind: principle
cites:
  - SKILL.md:29-33
requires: []
topics: [testing, tdd]
---

# Write a failing test first and see it fail for the expected reason

## Claim

Before changing the code, write a test that fails, and confirm it fails for the
reason you expect. Then make the smallest change that makes it pass.

## In its own words

> Write the failing test before you change the code. (SKILL.md:31)

> See it fail for the reason you expect. (SKILL.md:32)

## Source terms

None.

## Context

Under "First Make it Work" (SKILL.md:16), in the section titled TDD. no-dishonest-test
is that section's exception. test-observable-behaviour says what the test checks.
The section has no refactor step; refactoring comes later, under "Then Make it Right"
(refactor-under-pinned-behaviour).

## Extractor notes

- These are the first two steps of test-driven development (Kent Beck): a failing
  test, then the least code that passes it.
