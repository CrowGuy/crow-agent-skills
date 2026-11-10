# Draft the taste charter

Type: grilling
Status: resolved

## Question

What does the user value in an agent skill, strongly enough to rule conflicts between sources? Draft the charter: a ranking of values, anti-goals, and a few concrete examples of skill writing the user finds good or bad, each with the reason.

## Answer

The charter is [`workshop/charter.md`](../../../workshop/charter.md) (first version 2026-11-09, finalised 2026-11-10). Gist:

- **Application order:** hard constraints first, then the rankings (higher value wins), engineering values before writing values. A conflict the charter does not cover goes back to the user.
- **Hard constraints:** same behaviour on Claude Code and Codex, defined on outcomes: one content source per skill, the same acceptance pass bar on both platforms, no single-platform features. Software engineering only. At most five skills. Gates belong to the project's CI or git hooks, and crow skills never install gate tools.
- **Gate policy:** five statuses (verified / pending / failed / flaky / absent), a three-part definition of done, pre-existing failures only with before-and-after evidence, an attempt cap of 3, and weakening a gate always stops for the user.
- **Engineering values:** intent alignment > verifiability > minimalism > design quality > autonomy. A three-tier stop rule (irreversible / directional / everything else) with classification questions. The user can raise autonomy in plain words, and there is no mode system.
- **Writing values:** observable > concise > gives reasons > established terms (in their established meaning; never coin or redefine).
- **Anti-goals:** four engineering, four writing. **Examples:** five real source excerpts judged by the user.
- **Amendments:** only the user amends it, each change logged with date and reason. Old rulings are not reopened automatically.

Consequences for the rest of the map:

- Multi-agent isolation and orchestration (swarm-forge's core) is ruled out of scope. A separate future framework or script effort may pick it up.
- Core skill selection must also consider whether crow needs a skill that sets up a project's gates (hard constraint 4 leaves gate setup to the project).
- The eval method must give every acceptance scenario the same pass bar on Codex, so it needs a Codex runner (see the skill-creator research).
