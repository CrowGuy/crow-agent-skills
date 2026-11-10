---
id: uncle-bob/ub-mode/data-shape-before-logic
kind: principle
cites:
  - SKILL.md:25
requires: []
topics: [design, data-modeling]
---

# Settle the data shape before the logic

## Claim

Get the shape of the data right before writing the logic that uses it.

## In its own words

> Get the data shape right before you write the logic. (SKILL.md:25)

## Source terms

- **Foundational Thinking** (coined): the section name; the section also holds
  enabling-work-first and the shared-state rule in separate-shared-mutable-state.

## Context

Under "First Make it Work" (SKILL.md:16). illegal-states-unrepresentable, under
Architecture, says one property the right shape has.

## Extractor notes

- The file gives no check for when the shape is "right".
