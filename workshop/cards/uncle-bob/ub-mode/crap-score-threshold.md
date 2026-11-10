---
id: uncle-bob/ub-mode/crap-score-threshold
kind: mechanism
cites:
  - SKILL.md:108-112
requires: [crapper tool]
topics: [code-quality, complexity, testing]
---

# Keep every function's CRAP score under 10

## Claim

Run a tool that computes each function's CRAP score, and bring every function
under 10 by splitting it, testing it, or both. For case statements, first consider
whether reducing them is worth it.

## In its own words

> Reduce each function under  CRAP 10 by splitting it, by testing it, or both. (SKILL.md:111)

> Consider whether case statements are worth reducing. (SKILL.md:112)

## Source terms

- **crapper** (coined): the tool's name.

## Context

Under "Then Make it Right" (SKILL.md:90), the first of three tool sections, with
duplication-threshold and mutation-testing-gate.

## Extractor notes

- CRAP (Change Risk Anti-Patterns) is Savoia and Evans' crap4j metric, combining
  a function's cyclomatic complexity with its test coverage.
- The file does not say where the crapper tool comes from or how to run it.
- The double space in "under  CRAP 10" is in the source.
