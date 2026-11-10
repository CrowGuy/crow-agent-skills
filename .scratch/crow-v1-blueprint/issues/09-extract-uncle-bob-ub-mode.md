# Extract uncle-bob/ub-mode

Type: task
Status: resolved
Blocked by: 03

## Question

Extract concept cards from `workshop/sources/uncle-bob/ub-mode/`, following the card schema and extraction method in the Answer of [Define the concept card schema and extraction method](03-define-the-concept-card-schema-and-extraction-method.md). Cards and the coverage ledger go in `workshop/cards/uncle-bob/ub-mode/`. Done when every in-scope section in this ticket's scope is carded or skipped in `INDEX.md`, with no `todo` rows.

Scope for this session: the whole source (`SKILL.md`, ~5 KB).

## Answer

Done 2026-11-10. 35 cards (25 principle, 10 mechanism) in
[`workshop/cards/uncle-bob/ub-mode/`](../../../workshop/cards/uncle-bob/ub-mode/), with the
coverage ledger in [`INDEX.md`](../../../workshop/cards/uncle-bob/ub-mode/INDEX.md): all 30
sections of `SKILL.md` are carded, the four bare headings are `skip: no-content`, and
there are no `todo` rows. `LICENSE` and `SOURCE.md` are out of scope.

Facts later tickets depend on:

- **Where the cards differ from the prototype samples.** The atomicity test split two
  accepted samples: `tool-gated-quality` became `crap-score-threshold`,
  `duplication-threshold`, and `mutation-testing-gate` (one per tool, so each gate can
  be taken alone); `test-observable-behaviour` lost its last line to a separate
  mechanism, `empty-implementation-check`. `attack-the-premise` now labels its source
  terms `coined`, per the term rule.
- **`requires`.** No card needs a single-platform feature from the charter's hard
  constraint 1. Three cards need external tools the source never locates (`crapper`,
  `dryer`, `mutator`); four git cards need git (`safe-worktree-cleanup` also needs user
  confirmation, `push-to-mainline-at-end` needs push access to the main branch).
- **One redefined term:** "idempotent", which the source stretches to cover re-running
  after a crash partway (`idempotent-operations`).
- **Repeated in-source rules carded once, citing every location:** shared mutable state
  (lines 27 and 85-88), illegal states (65 and 77), parsing at the boundary (71-72 and 78).
- **In-source tensions noted, not ruled:** smallest edit against redesign for a new
  requirement; a planned break partway through against checking each unit before the next.
