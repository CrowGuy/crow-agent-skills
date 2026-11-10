---
id: uncle-bob/ub-mode/attack-the-premise
kind: principle
cites:
  - SKILL.md:41-45
requires: []
topics: [debugging, design]
---

# Stop fixing when two fixes fail on the same assumption

## Claim

When two fixes that rest on the same assumption both fail the same check, stop
fixing. State that assumption in one sentence, count which parts of the system
hold the imbalance it causes, and remove the assignment it rests on before
trying another fix.

## In its own words

> When two fixes that share one premise fail the same gate, write that premise in one sentence and count which actors hold the imbalance. (SKILL.md:43)

> Do not start another fix until you have done that. (SKILL.md:45)

## Source terms

- **Attack the Premise** (coined): the section name.
- **actors** and **imbalance** (coined): used without definition; see Extractor notes.

## Context

Under "First Make it Work" (SKILL.md:16), next to redesign-for-new-requirement
(SKILL.md:36-39), which makes the same kind of move for new requirements.
fix-root-cause covers a single failure.

## Extractor notes

- "Remove the assignment." (SKILL.md:44) is ambiguous: it might mean deleting the
  variable assignment that encodes the assumption, or dropping a responsibility
  assigned to some part of the system. The Claim keeps it literal.
- "Count which actors hold the imbalance" is not explained anywhere in the file.
- "Gate" is used without definition; in its established sense (a quality gate) it
  is a check the change must pass.
