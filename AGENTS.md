# AGENTS.md

This repo is a workshop that produces one installable skill package, **crow**
(provisional name; the final name is decided on the wayfinder map).

## Layout

- `plugins/crow/` is the **product**. Only finished crow skills live here.
- `workshop/` is the **workshop**: source material, extracted concepts, decisions,
  and evals. Nothing in it is ever installed.
- crow skills never go in this repo's `.claude/skills/` (or any auto-loaded skill
  directory). Auto-loading them would contaminate baseline evals and collide with
  the tooling skills used to build them. Install crow explicitly to dogfood it.

## Working rules

- Material under `workshop/sources/` is read-only once added.
- Every claim extracted from a source cites where it came from.
- Taste conflicts between sources are the user's decisions: surface them with a
  recommendation, never resolve them silently.
