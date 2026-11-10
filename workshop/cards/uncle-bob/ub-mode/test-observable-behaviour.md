---
id: uncle-bob/ub-mode/test-observable-behaviour
kind: principle
cites:
  - SKILL.md:103-105
requires: []
topics: [testing]
---

# Tests exercise code the way its user does

## Claim

A test calls the code the way its real user does and checks only the result that
user would see.

## In its own words

> The test calls the code the way its user does and checks the result that user would see. (SKILL.md:105)

## Source terms

- **Test Behavior, Not Implementation** (coined): the section name.

## Context

Under "Then Make it Right" (SKILL.md:90). failing-test-first says when to write the
test; this card says what it checks. empty-implementation-check is the same section's
test of a test.

## Extractor notes

- "User" may mean the calling code or the end user; the file does not say.
