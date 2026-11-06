# domain-modeling — Design

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

**domain-modeling defines only Entities, Behaviors, and Boundaries. It never
touches implementation details and never writes code.**

This keeps the agent in the logical layer of the **Smart Zone** instead of
sinking into implementation chores. When a choice is ambiguous, resolve it away
from implementation.

## 2. Problem

A fuzzy requirement like "I want a shopping cart" is not something a Spec can be
built from. It has to be turned into a professional technical structure — e.g.
"an `Item` entity, a totalling behavior, and a discount decision rule" — before
any Spec or code is meaningful. domain-modeling is the "brain" that performs that
transformation. It is composed by [grill-with-docs](grill-with-docs.md), which
supplies the interrogation conversation; domain-modeling supplies the reasoning
and the artifacts.

## 3. Position in the SDLC

domain-modeling is a **Chain Step**:

```
input:     a fuzzy requirement, or a Wayfinder exploration result
execution: an interrogation conversation (via grill-with-docs)
output:    updated glossary / CONTEXT + ADRs in the target project
downstream: those structured artifacts are the authoritative input to /to-spec,
            so /to-spec can synthesize a Spec without re-interviewing the user
```

It sits before `/to-spec` and produces the durable structure `/to-spec` reads.

## 4. Core Behavior (the modeling method)

domain-modeling turns the requirement into a domain model expressed as Entities,
Behaviors, and Boundaries. Two disciplines make the output stable:

### Facts vs Decisions (stability)

Every element of the model is tagged as one of:

- **Fact** — existing information discovered by reading the codebase (cite the
  source).
- **Decision** — an architectural choice that must be made explicitly by the
  user.

This prevents the agent from "self-interrogating" or hallucinating: it never
invents an answer to something that is actually a user Decision, and it never
re-litigates an established Fact. Only **Decisions** require user sign-off at the
gate (§6).

### Leading Words (prior-knowledge trigger)

The model is expressed using classic engineering Leading Words so the agent
applies high-quality design patterns from its prior knowledge — e.g.
**Deep Module**, **Seam**, **Bounded Context**. Using these words in the model
is deliberate: it raises modeling precision. See [glossary.md](glossary.md).

## 5. Output Contract

The value of modeling is **recording the reasoning behind decisions**, not
drawing diagrams. The output is therefore a set of **non-obvious artifacts** in
the target project:

- **ADRs (Architectural Decision Records)** — one per material Decision. Each ADR
  must record **why A was chosen over B** (the alternatives and the rationale),
  because that rationale is what `/to-spec` needs to synthesize the Spec. Minimum
  ADR schema:

  ```markdown
  # ADR NNNN: <decision title>
  Status: proposed | accepted | superseded
  ## Context      (the Facts and forces at play)
  ## Decision     (what was chosen)
  ## Alternatives (A vs B — what else was considered)
  ## Rationale    (why this over the alternatives)
  ## Consequences (what this commits us to)
  ```

- **CONTEXT.md / glossary updates** — the entities, behaviors, boundaries, and
  domain terms are recorded in the target project's `CONTEXT.md` and glossary.

**Locations are read from the target project's context, never hard-coded** (per
[skill-contract.md](skill-contract.md) §8). If the project has no configured ADR
path, the location is a Decision to confirm with the user, not a silent default.

These durable artifacts — not any chat log or issue comment — are the
authoritative input to `/to-spec`.

## 6. Confirmation Gate

domain-modeling must not push the SDLC forward on an unstable foundation.

- It **may not end the session or hand off to `/to-spec` until the user
  explicitly confirms: "we share an understanding of the domain model."**
- Only open **Decisions** block the gate; **Facts** do not need sign-off.
- Reaching that shared understanding is what the interrogation conversation
  (grill-with-docs) is for; domain-modeling owns the *criterion*, grill-with-docs
  owns the *conversation*.

This avoids advancing on a shaky base and the downstream refactoring cost that
follows.

## 7. Issue-consuming Adapter

domain-modeling runs in two modes; the core behavior (§4–§6) is identical, only
the input source and output wiring differ.

- **(a) Standalone** — invoked directly on a fuzzy requirement. Output artifacts
  land in the target project per §5.
- **(b) Consuming a `grilling` Exploration Issue** — reads the **Wayfinder-owned**
  sections of the issue (Problem / Goal, Known facts & context, Decisions to
  make) as input. It writes the ADR/CONTEXT artifacts per §5, then records in the
  issue's **executor-owned** sections:
  - `## Agreed decisions` — a summary of the decisions **plus links to the ADRs**.
  - `## Result` — links to the artifacts (ADRs, CONTEXT/glossary changes).

  The durable artifacts remain authoritative; the issue sections are the
  traceability link (consistent with [wayfinder.md](wayfinder.md) §5–§6).

## 8. When to Use / When Not to Use

**Use** when a requirement must be turned into domain structure (entities,
behaviors, boundaries) before a Spec or implementation is meaningful, or when
executing a `grilling` Exploration Issue.

**Do not use** (trigger-precision guardrails, since "modeling" is a broad word):

- Not for casual questions or explanations that need no durable decision.
- Not for implementation, coding, or refactoring (that breaches §1).
- Not for open-ended investigation of options — that is a `research` task.

## 9. Completion Criteria & Stop Conditions

- **Complete** when: the model's Decisions are resolved and recorded as ADRs, the
  CONTEXT/glossary updates are written, and the user has confirmed shared
  understanding (§6).
- **Stop and ask** when: an item is a user Decision (never decide it silently),
  or when the requirement needs investigation before it can be modeled (hand off
  to `research`).

## 10. Open Implementation Details

- Exact ADR file naming/numbering scheme within the target project.
- How CONTEXT.md and glossary sections are located/created when absent.
