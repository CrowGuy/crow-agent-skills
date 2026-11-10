---
id: uncle-bob/ub-mode/separate-shared-mutable-state
kind: principle
cites:
  - SKILL.md:27
  - SKILL.md:85-88
requires: []
topics: [concurrency, design]
---

# Give each writer its own state, or allow only one writer

## Claim

When concurrent writers would matter, do not share state. If the parties do not
need the same mutable data, give each its own; if they do, allow only one writer
and make the structure of the code enforce that.

## In its own words

> If a concurrent writer would matter, do not share state. (SKILL.md:27)

> If they do, allow one writer, and enforce that in the structure. (SKILL.md:88)

## Source terms

- **Separate Before Serializing Shared State** (coined): the section name.

## Context

Stated twice: briefly under Foundational Thinking (SKILL.md:27), and in full under
Architecture (SKILL.md:85-88).

## Extractor notes

- "They" (SKILL.md:87-88) has no stated antecedent: threads, processes, services, or agents.
- The file does not say what structure enforces a single writer (ownership in the
  type system, a single owning process, a lock).
