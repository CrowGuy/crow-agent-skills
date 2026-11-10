---
id: uncle-bob/ub-mode/empty-implementation-check
kind: mechanism
cites:
  - SKILL.md:106
requires: []
topics: [testing]
---

# Discard a test that would pass if every function returned nothing

## Claim

Ask of each test whether it would still pass if every function returned nothing.
If it would, delete it.

## In its own words

> If the test would still pass when every function returned nothing, throw it out. (SKILL.md:106)

## Source terms

None.

## Context

Under "Then Make it Right" > Test Behavior, Not Implementation (SKILL.md:103-106),
after test-observable-behaviour.

## Extractor notes

- The file gives no tool or procedure: the check can be done by reasoning or by
  actually stubbing the functions.
- Mutation testing automates a similar check (mutants that replace return values
  with empty ones); this source's mutation card is mutation-testing-gate.
