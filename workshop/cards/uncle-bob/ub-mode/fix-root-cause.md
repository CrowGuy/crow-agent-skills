---
id: uncle-bob/ub-mode/fix-root-cause
kind: principle
cites:
  - SKILL.md:92-96
requires: []
topics: [debugging]
---

# Reproduce the failure and fix its cause

## Claim

Reproduce the failure, then fix its cause. A guard that only hides the crash does
not count as a fix.

## In its own words

> A guard that only hides the crash is not a fix. (SKILL.md:96)

## Source terms

None.

## Context

The first section under "Then Make it Right" (SKILL.md:90). attack-the-premise
covers the case where repeated fixes fail.

## Extractor notes

- The file does not say whether the reproduction must become a test.
