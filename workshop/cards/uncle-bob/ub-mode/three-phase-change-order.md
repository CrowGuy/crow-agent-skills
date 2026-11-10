---
id: uncle-bob/ub-mode/three-phase-change-order
kind: mechanism
cites:
  - SKILL.md:1-7
  - SKILL.md:9-14
requires: []
topics: [workflow]
---

# Every change goes through three ordered phases

## Claim

Every requested change goes through three phases in order: first make it work,
then make it right, then follow a set of working practices. The skill applies
when the user invokes it by name or asks for its rules.

## In its own words

> Every time you are asked to make a change follow: (SKILL.md:11)

> * First Make it Work. / * Then Make it Right. / * Follow Practice (SKILL.md:12-14)

## Source terms

- **reduced principles** (coined): the description's name for the rules the skill lists (SKILL.md:4, 6).
- **Practice** (coined): the third phase; it holds working habits (logging, context, worktrees, commits) rather than properties of the code.

## Context

The three phases are the file's top-level headings (SKILL.md:16, 90, 131). Every
other card from this source sits under one of them.

## Extractor notes

- "Make it work, make it right" is Kent Beck's phrase, usually continued with
  "make it fast". The source drops the third step and adds Practice.
- The file does not say whether a later phase can send work back to an earlier one.
- Practice is listed third, but some of its items (action-log, keep-main-context-small)
  can only be followed throughout the change, not after it.
- User invocation by name differs in syntax between platforms (`/skill` vs `$skill`),
  which the charter allows.
