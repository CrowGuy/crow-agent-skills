# What eval method and pass bar do crow's acceptance scenarios use?

Type: grilling
Status: open
Blocked by: 05

## Question

Every crow skill's design carries acceptance scenarios. What method runs them against baseline, and what result counts as a pass?

## Comments

- 2026-11-06: Two filed sources carry prior art for this question upstream (not filed): `addyosmani/agent-skills` `evals/` and `dietrichgebert/ponytail` `benchmarks/`. See the Answer of [Add v1 source material](01-add-v1-source-material.md).
- 2026-11-10: [Draft the taste charter](02-draft-the-taste-charter.md) sets a hard constraint for this ticket: every acceptance scenario meets the same pass bar on Claude Code and Codex (`workshop/charter.md`, Hard constraints 1). With skill-creator's runners Claude Code-only, this needs a Codex runner or an equivalent.
