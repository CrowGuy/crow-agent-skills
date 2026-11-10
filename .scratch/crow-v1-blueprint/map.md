# Map: crow v1 Blueprint

## Destination

The crow v1 **Blueprint**: a personal, software-engineering-only core of at most five skills that behave the same on Claude Code and Codex, with every design decision made (including each skill's acceptance scenarios and pass bar) so each skill can be written in its own session.

## Notes

- **Plan, don't do.** Tickets produce decisions, not skills. Writing skills and running evals happen after the Blueprint.
- **Language:** every repo artifact (skills, glossary, ADRs, map, tickets, cards) is in English; quotes from sources stay in their original language.
- **Ordering:** the taste charter gates synthesis and conflict rulings; it does not gate source intake or extraction.
- **Layout:** follow `AGENTS.md` (product in `plugins/crow/`, everything else in `workshop/`). Sources follow the convention in [Add v1 source material](issues/01-add-v1-source-material.md).
- **Skills to consult:** grilling tickets call `grilling` + `domain-modeling`; research tickets call `research`; prototype tickets call `prototype`; any question about skill content consults `writing-for-agents`; eval questions consult `skill-creator`.
- **Bias watch:** Matt Pocock's workflow is both a tool here and a source. Keep extraction neutral and flag when a decision leans on his vocabulary by default.
- **Research branches** stay local (`research/<name>`); never push without asking the user.

## Decisions so far

<!-- the index: one line per closed ticket, enough to judge relevance, then zoom the link for the detail the ticket holds -->

- [How do Claude Code and Codex install and invoke a skill package?](issues/04-how-do-claude-code-and-codex-install-and-invoke-a-skill-package.md): one `.claude-plugin` tree can serve both; only `name`/`description` are shared, user-only invocation needs a switch on each platform, and Codex has no Skill tool.
- [What eval and baseline-comparison capability does skill-creator provide?](issues/05-what-eval-capability-does-skill-creator-provide.md): two eval systems (output vs baseline, triggering); all runners are Claude Code-only, Codex would need its own runner.
- [Add v1 source material](issues/01-add-v1-source-material.md): five sources filed (Matt Pocock skills, Addy Osmani agent-skills, Uncle Bob ub-mode, Dietrich Gebert ponytail, Uncle Bob swarm-forge as notes-only `agent-framework`); list closed for v1.
- [Draft the taste charter](issues/02-draft-the-taste-charter.md): [`workshop/charter.md`](../../workshop/charter.md): hard constraints (cross-platform parity on outcomes, gates live in the project), engineering values intent > verifiability > minimalism > design > autonomy with a three-tier stop rule, writing values observable > concise > reasons > established terms.
- [Define the concept card schema and extraction method](issues/03-define-the-concept-card-schema-and-extraction-method.md): one Markdown card per concept (`principle` or `mechanism`), every Claim sentence cited, coined or redefined terms kept out of the Claim, single-platform features named in `requires`; one source per session, closed coverage ledger per source, cards in `workshop/cards/`.

## Not yet specified

- **Synthesis runs.** Building the full concept matrix and ruling every conflict, once [Define the concept matrix and conflict-ruling method](issues/19-define-the-concept-matrix-and-conflict-ruling-method.md) settles how, and extraction is complete.
- **Core skill selection.** Which at most five skills make up crow v1, and why those, judged against the charter. Includes whether crow needs a skill that sets up a project's gates (the charter leaves gates to the project).
- **Per-skill design.** For each chosen skill: problem, triggers, mechanisms, traceability to cards or rulings, acceptance scenarios and pass bar.

## Out of scope

- Sharing or publishing crow to anyone else (v1 is personal).
- Productivity and other non-engineering skills.
- Writing the skills and running their evals (follows the Blueprint).
- Multi-agent isolation and orchestration (swarm-forge's core): beyond a single skill's reach; ruled out by [Draft the taste charter](issues/02-draft-the-taste-charter.md), left for a future framework or script effort.
