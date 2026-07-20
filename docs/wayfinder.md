# Wayfinder — Design

Status: **Design agreed, not yet implemented.**
Scope: **GitHub-only (v1).**

**Authority:** before implementation, this document is the design source of
truth. After implementation, `SKILL.md` becomes the source of truth for
execution behavior, and this document retains only the design rationale. Keep
them from drifting into two competing contracts.

This document captures decisions made during design so the plan does not live
only in a conversation log. Vocabulary follows [glossary.md](glossary.md);
structural rules follow [skill-contract.md](skill-contract.md).

**Naming & scope:** the skill keeps the Leading-Word name `wayfinder` (not
`gh-wayfinder`); GitHub is a pluggable backend, not the skill's identity, so the
GitHub-only scope is carried in its `description`, not its name. This relies on
the pluggable-backend carve-out in [skill-contract.md](skill-contract.md) §8.

## 1. Defining Constraint

**Wayfinder navigates and decomposes. It never performs the implementation, and
it never executes the exploration it plans.**

Everything below is downstream of this one constraint. When a design choice is
ambiguous, resolve it in the direction that keeps Wayfinder out of execution.
The only thing Wayfinder writes to the outside world is Issues.

## 2. Problem

In the planning and wayfinding stage, work is still **wrapped in Technical Fog**:
the problem, goal, and detailed features are large and unformed. Clarifying the
path is high-complexity and must be broken into several steps.

Historically that planning lived only in an AI conversation log — private,
unshareable, and lost when the session closed. Wayfinder externalizes it as a
**Shared Map** on GitHub Issues.

## 3. Position in the SDLC

```
requirement (fog)
  → [ Wayfinder: build a map of Exploration Issues ]
  → the user executes those issues (research / prototype / grilling / human-action)
        → facts + decisions
  → /to-spec: synthesize facts + decisions into a Spec
  → the Spec is decomposed into Implementation Tickets
```

- Wayfinder owns **planning the exploration before the Spec**. Its output is a
  map of **Exploration Issues** — not facts, not decisions, not a Spec.
- Facts and decisions are produced later, when the user executes each issue.
- The handoff point is `/to-spec`, which owns **converging into a Spec**.
- **Naming boundary** (so a fresh agent can judge its execution boundary from
  the noun alone): everything Wayfinder creates is an **Exploration Issue**.
  Only after `/to-spec` do we have **Implementation Tickets**. See
  [glossary.md](glossary.md).

## 4. The Shared Map (Output Contract)

The map is a concrete structure on GitHub Issues, not prose in a chat.

- **Parent Tracking Issue** — the map itself. Contains a Task List as the
  progress overview. This Task List is a **derived view only** (see below).
- **Sub-issue per Exploration Issue** — each is an independent Issue so it can
  be executed in a **single agent session** ("close one session, open the next
  issue"). Its body must follow the schema in §5.
- **Labels** — required, one type label per issue, so a fresh agent session can
  immediately identify its Leading Word and execution boundary:
  - `research`, `prototype`, `grilling`, `human-action`
- **Blocking Relationships** — the dependency chain of key decisions:
  - **GitHub-native `blocked-by` / `blocking` relationships are the single
    source of truth.**
  - The Parent Issue's Task List presents a **derived summary** of those
    relationships for human reading (e.g. `Blocked by #12`). It is never
    authoritative; if the two ever disagree, the native relationship wins.

## 5. Exploration Issue Body Schema

A type label states *what kind* of issue it is; the body carries *everything
needed to execute it in a fresh session*. Every Exploration Issue body must
contain:

```markdown
<!-- map-id: E-01 -->

## Problem / Goal
What this issue must resolve.

## Known facts & context
What Wayfinder already established via read-only exploration, with links.

## Decisions to make
The questions that must be answered (primarily for `grilling`).

## Blockers
`None`, or one or more `Blocked by E-02`-style map-local references to *other*
issues in the draft (never to itself). These are mechanically replaced by the
real `#NN` after creation (see §7). The native relationship is authoritative;
this section is a summary.

## Expected artifact
What is produced and where it lands (see §6 per type).

## Completion criteria
When the issue is done and may be closed.

## Agreed decisions
(Executor-owned. For `grilling`.) The final decisions, written here on
completion. This section is the single source of truth for decisions —
`/to-spec` reads only this, never scattered comments or discussion threads.

## Result
(Executor-owned.) Filled on completion: link the research report, the prototype
branch/path, or the human-action completion evidence here. This is the source of
truth for handoff to `/to-spec` or a fresh agent — so a downstream reader never
has to search comments or discussion history to find the actual output.
```

The first line is a hidden `map-id` marker carrying the immutable map-local ID
(see §7). Without this schema the "single session" guarantee does not hold, so
an issue missing required sections is not a valid map entry.

**Section ownership** (governs reconcile — see §7):

- **Wayfinder-owned:** the `map-id` marker, Problem / Goal, Known facts &
  context, Decisions to make, Blockers, Expected artifact, Completion criteria.
- **Executor-owned:** `## Agreed decisions` and `## Result`, written by the
  capability or human that executes the issue.

## 6. Exploration Issue Types

Wayfinder decomposes the fog into issues, each with an explicit purpose and a
defined artifact destination.

| Type           | Purpose                                                                 | Artifact & destination                                                                 | Backed by |
| -------------- | ----------------------------------------------------------------------- | -------------------------------------------------------------------------------------- | --------- |
| `research`     | Investigate technical options. An **AFK task** (runs as an independent process, no interactive input needed). | Markdown report committed as a **repo file** (e.g. `docs/research/<topic>.md`), linked from **`## Result`**. | capability skill |
| `prototype`    | Build a cheap, rough, concrete artifact to raise the fidelity of discussion (esp. frontend). | Code on a **throwaway branch or a clearly marked directory**, **never merged to production as-is** (state **"prototype only"**); branch/path recorded in **`## Result`**. | capability skill |
| `grilling`     | Deep decision-clarification on one sub-problem.                         | Final decisions written to the issue's fixed **`## Agreed decisions`** section (the single source of truth; see §5). | capability skill |
| `human-action` | Non-automatable chores driven by a human or the environment (e.g. provisioning access, configuring permissions). | Completion evidence in **`## Result`**, issue closed by a human.                       | **label only — no skill** |

`human-action` has no operating logic to automate, so it is only a placeholder /
to-do on the map, completed and closed by a human.

## 7. Skill Blueprint

To be built: **1 workflow skill + 3 capability skills**. `human-action` needs no
skill.

| Component   | Type              | Responsibility                                                      |
| ----------- | ----------------- | ------------------------------------------------------------------- |
| `wayfinder` | workflow skill    | Build the map, decompose into Exploration Issues, maintain blocking relationships |
| `research`  | capability skill  | Investigate technical options → Markdown report                     |
| `prototype` | capability skill  | Build a concrete artifact to raise discussion fidelity              |
| `grilling`  | capability skill  | Deep decision-clarification                                         |

Per [skill-contract.md](skill-contract.md) §7, Wayfinder **creates the entry-point
Issue for each capability but does not invoke, dispatch, or auto-start any of
them.** It does not duplicate their internal instructions. Wayfinder is
user-invoked and must not silently trigger another user-invoked workflow.

**Capability scope:** `research`, `prototype`, and `grilling` are **general,
model-invoked capabilities**, not Wayfinder-only helpers. Because their names are
broad, each must ship a complete standalone contract with **precise trigger
conditions** so it is not mis-triggered in unrelated contexts. They consume an
Exploration Issue when one exists but do not depend on Wayfinder to run.

### Stable identity & two-phase references

The map needs an identity that survives across re-runs and does not depend on
issue titles (which can be renamed or duplicated).

- **Immutable map-local ID.** Every Exploration Issue is assigned an ID such as
  `E-01`, stored in a `<!-- map-id: E-01 -->` marker at the top of the issue
  body (see §5). This ID never changes and is the only key used for matching.
- **Two-phase references.** Blocking is drafted with map-local IDs
  (`Blocked by E-01`) because real issue numbers do not exist yet at the
  Confirmation Gate. After issues are created, Wayfinder mechanically replaces
  the `E-01`-style references **inside the `## Blockers` section only** with the
  real `#NN`, and generates the Parent Task List. The `<!-- map-id: E-01 -->`
  marker is **never** rewritten — it is the permanent reconcile key, so the
  substitution must not touch it. This mechanical substitution needs **no
  re-confirmation**; only a semantic change to the map does.

### Reconcile (re-run)

- A re-run **must be given the existing Parent Issue URL/number**; Wayfinder
  matches only within that parent's native sub-issues, by `map-id`.
- It **never** guesses by title or searches the whole repository.
- **Matched** issues (draft ↔ existing, same `map-id`) are updated in place,
  but reconcile **only rewrites Wayfinder-owned sections** (see §5).
  Executor-owned sections (`## Agreed decisions`, `## Result`) are left
  untouched, so a re-run never clobbers exploration output. Resetting an
  executor-owned section is a separate **destructive change** that must be
  approved explicitly at the gate — not something the user has to catch by
  reading a diff line-by-line.
- **Unmatched drafted** issues (in the new draft, no existing match) are created
  with a new `map-id`.
- **Unmatched existing** issues (exist under the parent, but the new draft no
  longer includes them) are shown at the gate as **retained existing issues**.
  Wayfinder does **not** close, detach, modify, or mark them superseded; they
  stay in the Parent's derived summary. Retiring such an issue is a separate
  action the user must explicitly approve — otherwise the native sub-issues and
  the Parent Task List would diverge again. Retirement may **close or mark the
  issue superseded, but must never detach it** from the Parent: a retired issue
  stays in the Parent hierarchy permanently as identity history, so its `map-id`
  remains visible and its sequence number can never be reused. No issue is
  deleted (see §10).

**`map-id` integrity and allocation:**

- A **duplicate, missing, or malformed** `map-id` marker among the parent's
  sub-issues **stops the run and is reported** — Wayfinder never guesses which
  issue was meant.
- A **new** ID is the highest sequence ever used under that Parent, incremented
  by one.
- IDs are **never recycled**, even after an issue is retired — so a retired
  `E-03` is never reassigned.

## 8. Workflow

```
requirement input
  → shallow, READ-ONLY repo exploration
      (read code / git log / existing docs to separate known facts from
       unknowns; only enough to decompose correctly. Any unknown that needs
       investigation or a decision becomes a research/grilling issue — Wayfinder
       does not investigate it itself.)
  → draft the map (Exploration Issues + types + blocking relationships)
  → validate the draft (blocking-graph integrity — see below)
  → [ Confirmation Gate ]  (see §9)
  → write to GitHub Issues (Parent + Sub-issues + Labels + native blocked-by)
  → STOP. The user chooses which issue to start.
```

The shallow clarification may itself be a short back-and-forth with the user;
asking questions to draft the map does not violate the execution boundary.
Wayfinder reads to draft the map but **writes nothing except Issues**.

**Where shallow exploration stops** (a heuristic, so read-only access does not
quietly turn into doing the research — which would breach §1):

- **Allowed (shallow):** directly reading known files, configuration, and git
  history to separate knowns from unknowns.
- **Stop → create a `research` issue** when the question needs comparing several
  options, external research, an experiment, or a long investigation.
- **Stop → create a `grilling` issue** when it needs a user preference or a
  material trade-off decision.

### Draft validation

Structural errors in the blocking graph are caught deterministically by
Wayfinder **before** the Confirmation Gate — not left for the user to spot by
eye. The draft must satisfy all of:

- Every blocker ID resolves to an Exploration Issue **under the same Parent**
  (no dangling references).
- No **self-reference** and no **dependency cycle** (e.g. E-01 → E-02 → E-01).
- No blocker points to a **retired / superseded** issue.
- A non-empty map has **at least one unblocked issue** that can be started.

If validation fails, Wayfinder **does not enter the Confirmation Gate and writes
nothing to GitHub**; it reports the specific structural error so the draft can be
fixed.

## 9. Confirmation Gate

Position: **after the draft map, before any write to GitHub.**

At the gate the user (Senior Engineer) reviews and approves the **whole mutation
set** — on both first run and re-run, so no existing issue is changed without
approval:

- the **target repository**,
- **Issues to create** (titles, types, bodies, map-local IDs),
- **Existing issues to update**, each with its **diff**,
- **Existing issues retained** without change (see §7 reconcile),
- **Native blocking relationships** to add or remove, and
- the derived **Parent Task List** update.

Label creation is **not** part of the gate: the four type labels must already
exist (see §11). If any is missing, Wayfinder **reports the missing labels and
stops** — this is a precondition blocker resolved in chat, before any issue can
be written. It is not itself an Exploration Issue (the gate has not passed,
nothing may be written, and the missing label could be `human-action` itself).

Nothing is written to GitHub until this is approved.

Rationale:
- **Reduce handoff anxiety** — the value of Wayfinder is letting the user close
  one session and open the next issue. Auto-dispatching would cram multiple
  issues into one session and risk blowing out of the **Smart Zone**.
- **Consensus before implementation** — the map is an *exploration plan*; the
  user must approve it before any issue starts.
- **Async / AFK work** — tasks like `research` are best run as independent
  processes, not as an automatic extension of the main flow.

## 10. External Writes & Failure Recovery

Writing the map to GitHub is a core part of the contract, not an implementation
detail. Safe defaults:

- **Re-run = reconcile / update**, never automatic rebuild. Running Wayfinder
  against an existing map updates it in place; it does not duplicate issues.
- **Partial-failure = stop.** If issue creation fails midway, stop and list the
  issue URLs already created.
- **No automatic rollback or deletion** of issues, ever.
- **Re-confirm before repair or retry** — return to the Confirmation Gate.

## 11. Preconditions (GitHub-only, v1)

- An authenticated `gh` CLI whose version supports `--parent` and
  `--blocked-by`.
- A repository with Issues enabled.
- **The four type labels (`research`, `prototype`, `grilling`, `human-action`)
  already exist.** Wayfinder does not create labels; creating a label needs
  `write` permission, whereas Wayfinder deliberately stays at least-privilege.
- The user has at least **triage** permission — enough to apply existing labels
  and to create sub-issues and dependencies, without granting label/repo
  mutation. This is why §1 can state that Wayfinder's only external write is
  Issues.
- **GitHub Enterprise Server** capability differs and is **not supported in v1**.

## 12. Open Implementation Details

Deferred to when `SKILL.md` is written; listed here so nothing is lost.

- Exact `gh` command sequence and the minimum version that satisfies §11.
