# What eval and baseline-comparison capability does skill-creator provide?

Type: research
Status: resolved

## Question

What does the `skill-creator` skill offer for evaluating a skill against a no-skill baseline: how scenarios are written, how runs are executed and compared, what metrics and variance analysis it reports, and its limits?

## Answer

Full findings: branch `research/skill-creator-evals` (local only, commit `d821594`), file `workshop/research/skill-creator-evals.md`, every claim cited by file and line.

- **Two copies disagree.** The Skill-tool copy and the marketplace plugin copy differ in eight files; the findings report both.
- **Two eval systems.** Output quality (with-skill vs baseline on realistic prompts, graded against assertions) and triggering (does the description make Claude invoke the skill).
- **Case format.** Skill-tool copy: one folder per case under `evals/<case>/` (`prompt.md`, `case.yaml`, `graders/*.md`, `files/`), the layout `claude plugin eval` reads. Plugin copy: a single `evals/evals.json`.
- **Runs.** The orchestrating agent spawns a with-skill and a no-skill subagent per case; one run per configuration by default, repeats as `run-N/`.
- **Grading.** A grader subagent marks each assertion pass/fail with evidence (no partial credit) and critiques weak assertions. `aggregate_benchmark.py` reports mean, stddev, min, max and delta for pass rate, time, and tokens. No significance test. Human review via an HTML viewer.
- **Description optimizer.** ~20 queries, 60/40 train/test, 3 runs each via `claude -p`, up to 5 iterations (~300 calls).
- **Platform limit.** Every runner depends on Claude Code (subagents, `claude -p`, `.claude/` discovery, `claude plugin eval`). Codex is never mentioned; a Codex runner would have to be built. Case format, aggregator, viewer, and grader prompts may carry over (inferred, untested).
- **Unconfirmed.** Whether the no-skill baseline still sees installed skills; who writes the grader's `transcript.md`; what `claude plugin eval` does beyond one sentence; whether a temporary command triggers like a real skill.
