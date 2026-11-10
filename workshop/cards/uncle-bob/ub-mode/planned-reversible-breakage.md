---
id: uncle-bob/ub-mode/planned-reversible-breakage
kind: principle
cites:
  - SKILL.md:54
requires: []
topics: [planning, change-management]
---

# A break partway through is allowed only if planned and reversible

## Claim

Something may be broken partway through the work, but only when the break was
planned and can be undone.

## In its own words

> A break in the middle is allowed when you planned it and can undo it. (SKILL.md:54)

## Source terms

- **Outcome-Oriented Execution** (coined): the section name; the section also holds
  aim-at-checkable-end-state.

## Context

Under "First Make it Work" (SKILL.md:16). one-verified-unit-at-a-time asks for each
unit to be checked before the next starts; the file does not say how a planned
break fits with that.

## Extractor notes

- "Break" is not defined: failing tests, a broken build, or broken behaviour for users.
