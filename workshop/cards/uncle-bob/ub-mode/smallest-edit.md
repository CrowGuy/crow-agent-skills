---
id: uncle-bob/ub-mode/smallest-edit
kind: principle
cites:
  - SKILL.md:21
requires: []
topics: [scope, change-size]
---

# Make the smallest edit that solves the problem

## Claim

Deliver the smallest edit that solves the problem.

## In its own words

> Ship the smallest edit that solves the problem. (SKILL.md:21)

## Source terms

- **Laziness Protocol** (coined): the section name; the section also holds one-decision-one-place.

## Context

Under "First Make it Work" (SKILL.md:16). failing-test-first applies the same size
rule inside the test-first cycle (SKILL.md:33).

## Extractor notes

- "Smallest" has no stated measure (lines, files, concepts).
- The file does not say how this fits with redesign-for-new-requirement, which can
  call for a larger change than the smallest edit.
