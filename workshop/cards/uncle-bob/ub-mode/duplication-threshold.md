---
id: uncle-bob/ub-mode/duplication-threshold
kind: mechanism
cites:
  - SKILL.md:114-116
requires: [dryer tool]
topics: [code-quality, duplication]
---

# Remove duplication that a tool ranks 0.8 or higher

## Claim

Run a duplication-detection tool and remove every duplication it ranks 0.8 or above.

## In its own words

> Use the dryer tool to eliminate duplication that rank 0.8 or above. (SKILL.md:116)

## Source terms

- **dryer** (coined): the tool's name.
- **rank** (coined): the tool's score for a duplication; the file does not give its scale.

## Context

Under "Then Make it Right" (SKILL.md:90), between crap-score-threshold and
mutation-testing-gate. one-decision-one-place states a related rule before the code works.

## Extractor notes

- The file does not say where the dryer tool comes from, how to run it, or what the
  score measures (similarity on a 0-1 scale is a likely reading, not a stated one).
