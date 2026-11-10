---
id: uncle-bob/ub-mode/one-decision-one-place
kind: principle
cites:
  - SKILL.md:20
requires: []
topics: [design, duplication]
---

# Each decision lives in one place

## Claim

Each decision in the code is made in exactly one place.

## In its own words

> One decision lives in one place. (SKILL.md:20)

## Source terms

- **Laziness Protocol** (coined): the section name; the section also holds smallest-edit.

## Context

Under "First Make it Work" (SKILL.md:16), in the same section as smallest-edit.
duplication-threshold measures a related property after the code works.

## Extractor notes

- Close to the DRY principle (Hunt and Thomas, *The Pragmatic Programmer*: every
  piece of knowledge has a single, authoritative representation). The source says
  "decision", not "knowledge", and does not name DRY.
- "Decision" is not defined: a business rule, a design choice, a constant, or all of these.
