# grill-with-docs — Design

Status: **Design agreed, not yet implemented.**
Scope: **Project-agnostic.** Reads and writes the **target project's** files
(never this methodology repo's), per [skill-contract.md](skill-contract.md) §8.

**Authority:** before implementation, this document is the design source of
truth. After implementation, `SKILL.md` becomes the source of truth for
execution behavior, and this document retains only the design rationale.

This document captures decisions made during design so the plan does not live
only in a conversation log. Vocabulary follows [glossary.md](glossary.md);
structural rules follow [skill-contract.md](skill-contract.md).

## 1. Defining Constraint

**grill-with-docs clarifies and records Decisions through a doc-grounded
interrogation. It never writes code, never implements, and never does
open-ended investigation — investigation is a `research` task.**

It is the *conversation* that reaches a shared understanding; it is not where the
work gets built.

## 2. Problem

Turning a fuzzy requirement into resolved decisions needs sharp, targeted
questions — but ungrounded questions waste the user's time re-stating what is
already written down, and let the agent hallucinate. grill-with-docs grounds the
interrogation in existing documentation and code, so it asks only what is
genuinely undecided.

It is the **body**; [domain-modeling](domain-modeling.md) is the **brain**.
grill-with-docs runs the conversation and composes domain-modeling to structure
the answers into a domain model and ADRs.

## 3. Position in the SDLC

grill-with-docs executes the `grilling` exploration path: **deep
decision-clarification on one sub-problem**. It is the executor of a `grilling`
Exploration Issue (see [wayfinder.md](wayfinder.md) §6), and can also run
standalone. Its resolved decisions become the durable artifacts that feed
`/to-spec`.

## 4. Core Behavior (the interrogation method)

- **Ground in docs first.** Before asking anything, read the relevant project
  context (e.g. `AGENTS.md`, `CONTEXT.md`, `.sdlc/*`, existing ADRs, glossary)
  and code. What the docs already answer is a **Fact**; the agent must not ask
  the user about it.
- **Interrogate one focused thread at a time**, so the conversation stays in the
  **Smart Zone** rather than sprawling.
- **Separate Facts from Decisions** (per [domain-modeling](domain-modeling.md)
  §4): surface Facts with citations for confirmation; put every genuine
  **Decision** to the user explicitly.
- **Compose domain-modeling** to turn the resolved answers into Entities /
  Behaviors / Boundaries and ADRs. grill-with-docs owns the conversation;
  domain-modeling owns the reasoning and the artifacts.

## 5. Output Contract

grill-with-docs does not invent its own output format — it produces
[domain-modeling](domain-modeling.md)'s artifacts in the target project: **ADRs**
(with "why A over B" rationale) and **CONTEXT.md / glossary updates**. These
durable artifacts are the authoritative input to `/to-spec`; no decision lives
only in the chat log.

## 6. Confirmation Gate

grill-with-docs drives the conversation until domain-modeling's gate criterion
(§6 there) is met: it **may not end the session or hand off to `/to-spec` until
the user explicitly confirms shared understanding of the domain model.** Only
open **Decisions** block the gate; **Facts** do not.

## 7. Issue-consuming Adapter

Two modes; the core behavior (§4–§6) is identical, only input/output wiring
differs.

- **(a) Standalone** — invoked directly to grill a topic or sub-problem. Output
  artifacts land in the target project per §5.
- **(b) Consuming a `grilling` Exploration Issue** — reads the **Wayfinder-owned**
  sections (Problem / Goal, Known facts & context, Decisions to make) as the
  starting point and grounding. On completion it writes the **executor-owned**
  sections:
  - `## Agreed decisions` — summary of decisions **plus links to the ADRs**.
  - `## Result` — links to the artifacts.

  Durable artifacts stay authoritative; the issue sections are traceability
  links (consistent with [wayfinder.md](wayfinder.md) §5–§6 and
  [domain-modeling](domain-modeling.md) §7).

## 8. When to Use / When Not to Use

**Use** when a requirement or sub-problem needs deep decision-clarification
before a Spec or implementation is meaningful, or when executing a `grilling`
Exploration Issue.

**Do not use** (trigger-precision guardrails, since "grilling" is a broad word):

- Not for casual questions answerable directly, with no durable decision at
  stake.
- Not for implementation, coding, or refactoring (breaches §1).
- Not for comparing options / external investigation — that is a `research`
  task; hand off.

## 9. Completion Criteria & Stop Conditions

- **Complete** when: all Decisions for the sub-problem are resolved and recorded
  as ADRs (+ CONTEXT/glossary), and the user has confirmed shared understanding
  (§6). In issue-consuming mode, `## Agreed decisions` and `## Result` are filled.
- **Stop and ask** when: an item is a user Decision (never decide it silently),
  or the sub-problem needs investigation first (hand off to `research`).

## 10. Open Implementation Details

- How much project context to load before grilling (a grounding-depth budget).
- Interaction detail shared with `domain-modeling` §10 (ADR location/naming).
