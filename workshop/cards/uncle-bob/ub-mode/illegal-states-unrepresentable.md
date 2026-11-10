---
id: uncle-bob/ub-mode/illegal-states-unrepresentable
kind: principle
cites:
  - SKILL.md:65
  - SKILL.md:77
requires: []
topics: [design, types, domain-modeling]
---

# Make illegal states impossible to represent

## Claim

Structure the domain, and the types that hold its values, so that illegal states
and illegal values cannot be written at all.

## In its own words

> Put the domain in a structure that makes the illegal states impossible. (SKILL.md:65)

> Make the illegal value impossible to write. (SKILL.md:77)

## Source terms

- **Type System Discipline** (coined): the section name for SKILL.md:75-79.

## Context

Under Architecture (SKILL.md:61), stated in Model the Domain and again in Type
System Discipline. data-shape-before-logic says when to settle the shape;
leave-clear-local-code limits this rule.

## Extractor notes

- Established as "make illegal states unrepresentable" (Yaron Minsky, *Effective ML*, 2010).
  The source does not name it.
