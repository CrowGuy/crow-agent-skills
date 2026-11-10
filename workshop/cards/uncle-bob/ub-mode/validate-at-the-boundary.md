---
id: uncle-bob/ub-mode/validate-at-the-boundary
kind: principle
cites:
  - SKILL.md:71-72
  - SKILL.md:78
requires: []
topics: [validation, design, types]
---

# Check outside data once, where it enters

## Claim

Check and parse outside data once, where it enters the system. Inside, trust it
without checking it again.

## In its own words

> Check the data where it enters. / Inside, trust it. (SKILL.md:71-72)

> Parse outside data once, where it comes in. (SKILL.md:78)

## Source terms

- **Boundary Discipline** (coined): the section name; the section also holds pure-business-logic.

## Context

Under Architecture (SKILL.md:61), stated in Boundary Discipline and again in Type
System Discipline.

## Extractor notes

- Close to "parse, don't validate" (Alexis King, 2019). The source does not name it.
