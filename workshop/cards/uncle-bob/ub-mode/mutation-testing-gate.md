---
id: uncle-bob/ub-mode/mutation-testing-gate
kind: mechanism
cites:
  - SKILL.md:118-123
requires: [mutator tool]
topics: [testing, code-quality]
---

# Run mutation testing: cover uncovered code, kill surviving mutants, delete dead code

## Claim

Run a mutation-testing tool. Add tests for the code it shows as uncovered, add or
strengthen tests until no mutant survives, and delete dead code.

## In its own words

> Cover the uncovered. / Kill the survivors. / Delete dead code. (SKILL.md:121-123)

## Source terms

- **mutator** (coined): the tool's name.

## Context

Under "Then Make it Right" (SKILL.md:90), the last of three tool sections, after
crap-score-threshold and duplication-threshold.

## Extractor notes

- "Survivors" is the established mutation-testing term for mutants the tests fail to detect.
- Unlike the other two tool sections, this one gives no threshold; read literally,
  every survivor is killed.
- The file does not say how dead code is identified, or where the mutator tool comes from.
