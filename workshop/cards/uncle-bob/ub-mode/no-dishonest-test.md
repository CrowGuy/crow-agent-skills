---
id: uncle-bob/ub-mode/no-dishonest-test
kind: principle
cites:
  - SKILL.md:34
requires: []
topics: [testing]
---

# Write no test rather than a dishonest one

## Claim

When an honest test is impractical, do not write a bad one instead.

## In its own words

> If an honest test is impractical, do not write a bad one. (SKILL.md:34)

## Source terms

None.

## Context

The last line of the TDD section (SKILL.md:29-34), and the exception to
failing-test-first. empty-implementation-check gives one test of whether a test is bad.

## Extractor notes

- The file defines neither "honest" nor "impractical", and does not say what checks
  the change when no test is written.
