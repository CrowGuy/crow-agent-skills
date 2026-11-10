---
id: uncle-bob/ub-mode/safe-worktree-cleanup
kind: mechanism
cites:
  - SKILL.md:143-146
requires: [git worktrees, user confirmation]
topics: [git, housekeeping]
---

# Remove worktrees only after confirming nothing uses them

## Claim

Find what is safe to remove, confirm nothing still in use depends on it, then
reclaim the disk space. Uncommitted work is not removed; it waits for a decision.

## In its own words

> Find what is safe to remove, confirm nothing still in use needs it, then reclaim the disk. (SKILL.md:145)

> Uncommitted work waits for a decision. (SKILL.md:146)

## Source terms

None.

## Context

Under Practice (SKILL.md:131).

## Extractor notes

- The section assumes git worktrees exist, but no other section of the file creates them.
- Whose decision uncommitted work waits for is not stated; `requires` lists user
  confirmation on the reading that it is the user's.
