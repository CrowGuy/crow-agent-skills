---
id: uncle-bob/ub-mode/idempotent-operations
kind: principle
cites:
  - SKILL.md:81-83
requires: []
topics: [reliability, design]
---

# Operations end in the same state when re-run or resumed after a crash

## Claim

Running an operation twice, or running it again after it crashed partway through,
ends in the same state as one complete run.

## In its own words

> Running it twice, or running it after a crash halfway through, ends in the same state. (SKILL.md:83)

## Source terms

- **idempotent** (redefined): the section title "Make Operations Idempotent" uses
  it to cover re-running after a crash partway, not only repeated runs.

## Context

Under Architecture (SKILL.md:61).

## Extractor notes

- Established meaning: an operation is idempotent when applying it more than once
  has the same effect as applying it once (from mathematics; for HTTP methods,
  RFC 9110 §9.2.2). Finishing correctly after a crash partway is a further property
  (crash safety, resumability) that the source folds in.
- "The same state" has no stated reference point. The Claim reads it as the state
  after one complete run.
