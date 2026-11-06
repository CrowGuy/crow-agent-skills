# Add v1 source material

Type: task
Status: resolved

## Question

Bring the v1 source material into `workshop/sources/` so extraction can start. HITL then AFK: the user supplies the URLs of the material they choose; the agent fetches and files each one by the convention below.

### Convention

```text
workshop/sources/
  <expert-slug>/            # e.g. uncle-bob, matt-pocock, addy-osmani
    <work-slug>/            # e.g. clean-code, skills
      SOURCE.md             # required
      ...                   # the material itself
```

`SOURCE.md`:

```markdown
---
title: <title>
author: <author>
type: skill-repo | agent-framework | book | article | talk
origin: <URL, or edition / ISBN>
version: <commit SHA, edition, or publish date>
retrieved: <YYYY-MM-DD>
license: <license, or "copyrighted – notes only">
---

Why: <one sentence from the user on why it's included>
```

By type:

- **skill-repo**: only skill files (`SKILL.md` and the files it references), original relative paths kept, pinned to a commit. Cite by path and line.
- **agent-framework**: an agent orchestration repo with no skill files (prompts, protocols, runtime). Files are copied only when the license allows; otherwise `notes.md` only. Pin every branch read to a commit. Cite by `<branch>:<path>:<line>`.
- **book**: `notes.md` with notes and short excerpts, each tagged with chapter or page. Never the full text: this repo is public. Cite by chapter or page.
- **article / talk**: `notes.md` with notes and short quotes; a full markdown copy only when the license allows. Cite by URL plus paragraph or timestamp.

Material is read-only once added. Ask the user for the `Why` line of any source that lacks one.

## Answer

v1 uses five sources, all filed under `workshop/sources/` on 2026-11-06 with `SOURCE.md` (Why lines confirmed by the user):

| Source | Type | Filed |
|---|---|---|
| `matt-pocock/skills` | skill-repo | all of `skills/` @ `4ae2317` |
| `addy-osmani/agent-skills` | skill-repo | `skills/` + repo-level `references/` @ `301b02b` |
| `uncle-bob/ub-mode` | skill-repo | `SKILL.md` @ `3da62bf` |
| `dietrich-gebert/ponytail` | skill-repo | canonical `skills/` @ `96608e0` |
| `uncle-bob/swarm-forge` | agent-framework | `notes.md` only (no license); main + two/four/six-pack branches pinned |

Resulting facts for later tickets:

- The convention gained an `agent-framework` type (see above) for orchestration repos with no skill files.
- The user confirmed the source list is closed for v1: no books or articles.
- Not filed, but relevant to eval design: `addyosmani/agent-skills` `evals/` (per-skill cases, fixtures, graders) and `dietrichgebert/ponytail` `benchmarks/` (baseline-vs-skill results). Both are upstream at the pinned SHAs.

## Comments

- 2026-11-06: Filed four skill repos under `workshop/sources/` (matt-pocock/skills, addy-osmani/agent-skills, uncle-bob/ub-mode, dietrich-gebert/ponytail), each pinned to a commit with `SOURCE.md` and the upstream MIT `LICENSE`. Why lines were given in Chinese and translated; awaiting user confirmation. 
- 2026-11-06: User chose to keep `unclebob/swarm-forge` as a new `agent-framework` type, notes only (no license). Convention above extended. Filed `uncle-bob/swarm-forge/` (SOURCE.md + notes.md, main and pack branches pinned).
