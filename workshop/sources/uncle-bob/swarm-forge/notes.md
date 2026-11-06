# SwarmForge — notes

Citations are `<branch>:<path>:<line>` at the SHAs in `SOURCE.md`.

## What it is

- Coordinates AI agents "in isolated git worktrees and tmux sessions"; agents "exchange committed work through durable handoffs", and an operator drives a local dashboard (`main:README.md:7-10`).
- `main` is the shared runtime and "shared engineering law", not a runnable product. The products are packs (two-, four- and six-role pipelines installed into a project) and forges (multi-project hosts) (`main:README.md:14-33`).
- Backends: `grok`, `codex`, `claude`, `copilot`. The role-to-backend mapping is per-line config (`main:README.md:44-45`, `main:README.md:94-107`). The six-pack mixes codex and grok (`six-pack:swarmforge/swarmforge.conf:8-13`).

## Isolation mechanism

- Each role gets its own git worktree (`.worktrees/<name>`); exactly one role uses the project's main checkout (`main:README.md:97-99`).
- Each role runs in an isolated tmux session; a handoff daemon owns the tmux socket so agents never touch it (`main:README.md:195-198`, `main:swarmforge/handoff-protocol.md:5-9`).
- Agents may not look across the boundary: "Work only in your assigned branch or worktree. Do not inspect, diff, merge, or base work on another branch unless that branch is specifically named in a handoff" (`main:swarmforge/constitution/articles/workflow.prompt:6-7`).
- Fail loudly on a broken layout: "stop and report instead of silently working in the wrong place" (`main:swarmforge/constitution/articles/workflow.prompt:31`).
- Temporary files go in the worktree's `./tmp/`, never `/tmp` (`main:swarmforge/constitution/articles/workflow.prompt:26`).

## Handoff protocol

- Work moves only as commits. The sender commits, then writes a draft containing only headers (`type`, `to`, `priority`, `task`), and a helper fills in the SHA (`main:swarmforge/constitution/articles/handoffs.prompt:16-26`).
- There are two message types, `git_handoff` and `note`; notes need explicit authority (`main:swarmforge/constitution/articles/handoffs.prompt:7-11`).
- Bodies are kept short: "Do not write long handoff bodies. The helper generates the delivered payload." (`main:swarmforge/constitution/articles/handoffs.prompt:49`).
- The queue state is the file location (inbox, accepted, done), and audit timestamps live in the handoff file headers (`main:swarmforge/handoff-protocol.md:20-22`).
- Every forward task is forwarded, even non-functional churn (`main:swarmforge/constitution/articles/handoffs.prompt:28-31`). A reverse handoff is merge-only (`main:swarmforge/constitution/articles/handoffs.prompt:32-34`).
- Structure flows down the pipeline. On a forward merge the receiver's tree is the shape; on a reverse merge the inbound tree is the shape (`main:swarmforge/constitution/articles/handoffs.prompt:59-64`).
- Ambiguity goes to the operator through a dashboard clarify request, not into the pane (`main:swarmforge/constitution/articles/handoffs.prompt:12-15`).

## Instructions as composed data

- A constitution of shared articles (engineering, workflow, handoffs) is owned by `main`. Packs add `project.prompt` / `local-*.prompt`, and they cannot override the shared names: "a pack cannot silently replace common law" (`main:README.md:115-134`).
- Role prompts "divide ownership inside that law: what a role may change, what it must verify, what it must leave to another role, and where its next handoff goes" (`main:README.md:136-138`).
- The repo bans testing prompt text: "Prompt wording is not production behavior to pin" (`main:AGENTS.md:3-6`, `main:README.md:179-180`).

## Six-pack role split (specifier → coder → cleaner → architect → hardender → QA)

- **Specifier:** owns externally visible behaviour as Gherkin under the Acceptance Pipeline Specification, plus an end-to-end QA suite through the UI only (`six-pack:swarmforge/roles/specifier.prompt:3-20`).
- **Coder:** writes TDD per behaviour slice. Its unit tests "would fail for a plausible wrong implementation", followed by "only enough production code to pass", and it leaves broad cleanup to the cleaner (`six-pack:swarmforge/roles/coder.prompt:18-21`).
- **Cleaner:** does structure-preserving cleanup (names, cohesion, duplication, dead code) with no new behaviour and no architecture decisions (`six-pack:swarmforge/roles/cleaner.prompt:3-15`, `six-pack:swarmforge/roles/cleaner.prompt:32-35`).
- **Architect:** applies the dependency rule (dependencies point toward high-level modules far from IO), narrow interfaces owned by high-level modules, and UI/core separation (`six-pack:swarmforge/roles/architect.prompt:10-25`). It also owns property testing (`six-pack:swarmforge/roles/architect.prompt:33-37`).
- **Hardender:** runs differential mutation testing against a manifest, one file at a time (`six-pack:swarmforge/roles/hardender.prompt:14-24`).
- **QA:** does independent final verification, turns the specifier's QA procedures into scripts, and "Reproduce failures before changing code" (`six-pack:swarmforge/roles/QA.prompt:12-21`).
- Four-pack folds cleaner and property testing into a single **refactorer** (`four-pack:swarmforge/roles/refactorer.prompt:3-12`).

## Quality gates (tool-enforced)

- Agents install Martin's own CRAP, mutation and DRY tools fresh from GitHub at startup, per language (`main:swarmforge/constitution/articles/engineering.prompt:3-10`).
- The CRAP gate is ≤ 10. "A single `cond` or `case` that answers one question may stay above 10; do not split it into helpers that take booleans the caller already knew." (`six-pack:swarmforge/roles/cleaner.prompt:21-27`).
- Mutation counts are a hint only: "Split a source file when it has more than one job… Do not split a one-job module to chase a mutation-site count." (`main:swarmforge/constitution/articles/engineering.prompt:46`).
- There are no homegrown proxies for the gate tools (`main:swarmforge/constitution/articles/engineering.prompt:53`).
- Testable modules are kept apart from "environmentally unsuitable" ones, and the unsuitable boundary is minimised (`main:swarmforge/constitution/articles/engineering.prompt:23`).
- "IO-near modules must not reimplement a domain question." (`main:swarmforge/constitution/articles/engineering.prompt:24`).

## Relevance flags for extraction

- This is not a skill package. Its mechanisms (worktrees, tmux, a daemon, a dashboard) run outside any single agent session, and its principles (role ownership, gates, dependency rule) sit in prompts.
- Its stack is specific: Babashka/Clojure, Go and Java tool tables, and Gherkin via Martin's APS.
