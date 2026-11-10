---
id: uncle-bob/ub-mode/pure-business-logic
kind: principle
cites:
  - SKILL.md:73
requires: []
topics: [design, architecture]
---

# Keep business logic in pure functions

## Claim

Keep the business logic in a pure function, one with no side effects.

## In its own words

> Keep the business logic in a pure function. (SKILL.md:73)

## Source terms

None.

## Context

Under Architecture > Boundary Discipline (SKILL.md:69-73), after validate-at-the-boundary.

## Extractor notes

- The singular "a pure function" is most likely general (pure functions); the file
  does not say.
- Close to "functional core, imperative shell" (Gary Bernhardt, 2012). The source
  does not name it.
