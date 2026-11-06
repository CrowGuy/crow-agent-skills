# Add v1 source material

Type: task
Status: open

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
type: skill-repo | book | article | talk
origin: <URL, or edition / ISBN>
version: <commit SHA, edition, or publish date>
retrieved: <YYYY-MM-DD>
license: <license, or "copyrighted – notes only">
---

Why: <one sentence from the user on why it's included>
```

By type:

- **skill-repo**: only skill files (`SKILL.md` and the files it references), original relative paths kept, pinned to a commit. Cite by path and line.
- **book**: `notes.md` with notes and short excerpts, each tagged with chapter or page. Never the full text: this repo is public. Cite by chapter or page.
- **article / talk**: `notes.md` with notes and short quotes; a full markdown copy only when the license allows. Cite by URL plus paragraph or timestamp.

Material is read-only once added. Ask the user for the `Why` line of any source that lacks one.
