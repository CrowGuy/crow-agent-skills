---
id: uncle-bob/ub-mode/refactor-under-pinned-behaviour
kind: principle
cites:
  - SKILL.md:125-129
requires: []
topics: [refactoring, testing]
---

# Pin behaviour before restructuring, then prove it unchanged

## Claim

When refactoring, first pin down what the code currently does, then change the
structure, then prove the behaviour did not change.

## In its own words

> Pin what the code does. / Change the structure. / Prove the behavior did not change. (SKILL.md:127-129)

## Source terms

None.

## Context

The last section under "Then Make it Right" (SKILL.md:90). failing-test-first has
no refactor step; this is where refactoring sits.

## Extractor notes

- Refactoring is used in its established meaning (Martin Fowler: changing structure
  without changing observable behaviour).
- "Pin" likely means characterization tests (Michael Feathers); the file does not
  say how to pin or prove.
