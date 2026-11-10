# crow taste charter

The user's taste, used to rule every conflict between sources and to judge every
crow skill design. Decided in the wayfinder ticket *Draft the taste charter*
(`.scratch/crow-v1-blueprint/issues/02-draft-the-taste-charter.md`).

## How to apply it

1. **Hard constraints first.** A candidate that fails any of them is out, whatever it scores on values.
2. **Then the rankings.** When two values conflict, the higher-ranked one wins.
3. **Engineering before writing.** Engineering values decide *whether* something is
   in a skill; writing values decide only *how* it is written. A verification
   checklist that verifiability needs stays in, written concisely.
4. **No ruling found?** The charter does not cover the conflict. Bring it to the
   user; don't extend the charter by inference.

## Hard constraints

1. **Same behaviour on Claude Code and Codex.** Transcripts can't match across
   models, so "same" is defined on checkable outcomes:
   - **One content source.** Each skill has one `SKILL.md` and one set of referenced
     files, never per-platform variants. Platform-specific metadata may sit
     alongside, but only to make invocation match (e.g. user-only invocation:
     `disable-model-invocation` in frontmatter for Claude Code,
     `policy.allow_implicit_invocation` in `agents/openai.yaml` for Codex).
   - **Same results.** Every acceptance scenario meets the same pass bar on both platforms.
   - **Allowed to differ:** invocation syntax (`/skill` vs `$skill`), UI, and the underlying model.
   - **Forbidden:** an instruction that depends on a single-platform feature,
     unless the skill works fully without it. Known cases: "call the Skill tool",
     `$ARGUMENTS`, `` !`cmd` ``, `${CLAUDE_SKILL_DIR}`, `allowed-tools`,
     `context: fork`, hooks.
2. **Software engineering only.**
3. **At most five skills** in crow v1.
4. **Gates belong to the project.** Quality gates (complexity, duplication,
   mutation, coverage, and the like) are installed and configured in the project's
   own CI or git hooks, and exposed as one command that also runs locally. A crow
   skill never installs gate tools and never uses agent hooks. It finds and runs the
   project's gate command and treats the result as evidence. Each skill names the
   gate kinds it relies on; when the project lacks one, the skill reports it as
   absent and never skips it silently. Status and done follow the gate policy below.

### Gate policy

- **Gate status.** Report every gate the project has, and every gate kind the
  skill relies on, as exactly one of:
  - **verified**: ran on this change, in full, and passed;
  - **pending**: not yet run in full, including when the environment or missing
    tooling blocks the run. Report the blocker;
  - **failed**: ran and failed;
  - **flaky**: one rerun on unchanged code gave a different result;
  - **absent**: the skill relies on this gate kind, but the project has none.
- **Done** requires all three:
  1. every gate the project has is one of:
     - verified;
     - failed, with pre-existing evidence;
     - flaky, with pre-existing evidence *and* independent evidence that the
       change works (a test or run other than the flaky gate). The report names
       the uncertainty that remains.
  2. every absent gate is reported as absent, and the change has direct evidence
     that exercises it (a test or run, with its output in the report). That
     evidence does not stand in for the missing gate: never claim a passing test
     covers what an absent mutation, complexity, or other gate would have checked;
  3. no gate is pending.

  While any gate is pending, report the change as "verified locally, pending
  <gates>". Never call it done, and never say all gates pass.
- **Pre-existing failures** (failed or flaky) count only with before-and-after
  evidence: the same gate, run on the code before the change (e.g. the base
  commit), fails or flakes the same way. Without that evidence, report the origin
  as unknown; it then blocks done. Proven pre-existing failures are out of scope:
  don't fix them in passing.
- **Attempt cap:** after 3 failed fixes for the same failure of the same gate,
  stop and report the failure output, what was tried, and the suspected root cause.
- **Never weaken a gate.** Lowering a threshold, adding an exclusion, disabling or
  skipping a test, or adding a suppression comment is in the **irreversible** tier
  of the stop rule: always ask, even when autonomy is raised. Weakening a gate forges the evidence.
- **Flaky:** rerun a gate once on unchanged code only to check for flakiness, never
  in the hope of a pass. A flaky gate never counts as verified.
- **Slow gates** (e.g. mutation testing that runs for hours): locally, run the
  affected subset when the gate supports it, and leave the full run to CI. Until
  the full run passes, the gate is pending; the report says what scope actually ran.

## Engineering values (ranked)

1. **Intent alignment.** Build the thing the user actually wants. When unsure
   what is wanted, settle that before building, within the stop rule.
2. **Verifiability.** Back every claim of done with evidence (a test, a check
   run, an output) rather than "should work".
3. **Minimalism.** The least code and the least process that fully solve the task.
4. **Design quality.** Clean code and clean architecture. Yields to minimalism:
   no structure the task does not need.
5. **Autonomy.** Keep moving without interrupting the user, within the stop rule.

### Stop rule

The user is usually present but does not want frequent interruptions. Classify
a decision by asking, in order:

1. **Irreversible** if it publishes outward (a push, a release, a message or
   request others can see), changes state others share (a shared branch, database,
   environment, or cloud resource), or destroys data that can't be restored from
   git and the working tree.
2. **Directional** if undoing it later means touching things outside this change:
   others build on it (an interface, a data format, the dependency set, a module
   boundary), or it changes the scope the user asked for.
3. **Everything else** can be undone by editing this change's diff; the agent decides.

When the tier is unclear, take the higher one, because intent alignment outranks
autonomy. To stop and ask, batch the open questions of a tier into one message,
each with the agent's recommended answer, so the user can reply "yes" or rewrite it.
The table gives examples of each tier:

| Tier | Examples | Default | When the user raises autonomy |
|---|---|---|---|
| **Irreversible** | deleting or overwriting uncommitted work or data; push, publish, or sending anything outward; writing to a shared database or environment; rewriting git history; destructive migrations | stop and ask | stop and ask |
| **Directional** | adding a dependency; changing a public interface or data format; choosing an architecture or module boundary; shrinking or widening the scope the user asked for | stop and ask | decide, then report at the end |
| **Everything else** | naming, internal structure, how to test, implementation detail | decide | decide |

The user raises autonomy in plain words in the request (e.g. "fast iteration,
don't ask"). The charter defines no mode system; whether a skill needs modes is a
per-skill design decision.

## Writing values (ranked)

1. **Observable.** Every instruction can be checked from the agent's behaviour
   or output. This is what makes a skill evaluable.
2. **Concise.** Every token earns its place, since all skills share one context
   budget. Detail that is rarely needed goes in a file loaded on demand.
3. **Gives reasons.** A rule carries a one-line why, so the agent can extend it to cases the rule does not cover.
4. **Established terms.** Use the field's existing terms in their established
   meaning. Don't coin terms and don't redefine existing ones; when no term
   exists, describe it in plain words.

## Anti-goals

### Engineering

- **Ceremony regardless of size:** running a full spec → plan → test → review pipeline for a one-line change.
- **Unrequested additions:** abstractions, options, or "for later" code nobody asked for.
- **Done without evidence:** claiming success without having run anything.
- **Symptom patches:** a guard that hides the crash instead of fixing the root cause.

### Writing

- **Persona role-play** ("You are a lazy senior developer"): not observable; state the rules it stands for.
- **Long code examples in SKILL.md:** use one only when a rule can't be said in a
  sentence, and keep it in a file loaded on demand.
- **Unobservable adjectives** ("write clean code", "be careful").
- **Restating what the agent already knows** ("Improve naming / Remove duplication").

## Examples

Real excerpts from the v1 sources, judged by the user.

| # | Excerpt | Verdict | Why |
|---|---|---|---|
| E1 | "If the test would still pass when every function returned nothing, throw it out." (`uncle-bob/ub-mode/SKILL.md:106`) | good | Concise and observable, and it hands the agent a check it can actually run. |
| E2 | "You are a lazy senior developer. The best code is the code never written." (`dietrich-gebert/ponytail/skills/ponytail/SKILL.md:15`) | bad | A persona can't be observed or verified; a skill needs concrete rules. |
| E3 | "Before writing any test, write down the seams under test and confirm them with the user. No test is written at an unconfirmed seam." (`matt-pocock/skills/skills/engineering/tdd/SKILL.md:22`) | bad | It borrows an established term ("seam", from Michael Feathers) and changes its meaning, so the model applies the wrong one. It also stops for a decision the stop rule leaves to the agent. |
| E4 | The "Common Rationalizations" table, e.g. "This is too simple to test" → "Simple code gets complicated." (`addy-osmani/agent-skills/skills/test-driven-development/SKILL.md:365-375`) | good, with a limit | It heads off the excuses an agent uses to skip work that should be done, and those excuses are observable. Use it only on rules the agent actually makes excuses about, not in every skill. |
| E5 | "Reduce each function under CRAP 10 by splitting it, by testing it, or both." (`uncle-bob/ub-mode/SKILL.md:111`) | mixed | Good: an explicit number, observable and verifiable. Bad: it names a tool without saying where it comes from. Under hard constraint 4, the project provides the gate and the skill runs it. |

## Amendments

Only the user amends this charter. Each change adds one line below: date, what
changed, why. A ruling made under an earlier version is not reopened automatically;
it is re-ruled only when the user names it.

- 2026-11-09: First version.
