# Define the concept card schema and extraction method

Type: prototype
Status: resolved
Blocked by: 01

## Question

What shape does an extracted concept take, and how is it extracted so sources stay unblended and every claim traces to its source? Prototype a schema plus a few sample cards drawn from the real sources, covering both mechanism-style material (skill repos) and principle-style material (books, articles), for the user to react to.

## Comments

- 2026-11-06: Prototype drafted at `workshop/PROTOTYPE-card-schema/` (schema + method in its README, 8 sample cards across all five sources, one full coverage ledger for ub-mode). Awaiting the user's reaction; it moves to a throwaway `prototype/card-schema` branch on resolution.
- 2026-11-10: [Draft the taste charter](02-draft-the-taste-charter.md) is resolved. Check the prototype's "plain-words claim" rule against the charter's writing value "established terms": the field's existing terms in their established meaning are allowed, coined or redefined ones are not (`workshop/charter.md`, Writing values 4).

## Answer

The user accepted the prototype on 2026-11-10 with two changes: the term rule now follows the charter's "established terms" value, and `requires` names single-platform features. The 8 sample cards were accepted as written. The prototype is kept as a primary source on the local throwaway branch `prototype/card-schema` (`workshop/PROTOTYPE-card-schema/`). Read that branch for worked examples, but follow the rules below where the two differ.

### Layout

```text
workshop/cards/
  <expert-slug>/<work-slug>/     # mirrors workshop/sources/
    INDEX.md                     # coverage ledger for that source
    <card-slug>.md               # one concept per file
```

A card's id is its path under `workshop/cards/` without `.md`, e.g. `uncle-bob/ub-mode/attack-the-premise`.

### Card schema

```markdown
---
id: <expert>/<work>/<slug>
kind: principle | mechanism
cites:                    # every claim in the card is backed by one of these
  - <citation in the source's own format (see SOURCE.md type)>
requires: []              # capabilities beyond a plain chat turn (see below)
topics: [<free tags>]     # loose; the controlled vocabulary is a synthesis decision
---

# <title: plain words or established terms>

## Claim
<1–3 sentences: what the source says, paraphrased in plain words or in established
terms used in their established meaning (see the term rule)>

## In its own words
> <short verbatim quote> (<cite>)

## Source terms
- **<term>** (coined | redefined): <what the source means by it>

## Context
<where it sits in the source: what triggers it, and which sibling cards *from the same
source* it is coupled to>

## Extractor notes
<not from the source: ambiguities, platform assumptions, coupling, the established
meaning of a redefined term. Never a value judgment.>
```

- **principle**: a claim about what good engineering work or output is.
- **mechanism**: a device that gets an agent to act on a principle (a checkpoint, gate, table, level switch, required output format, tool, or isolation scheme).
- When a passage carries both and crow could take one without the other, write two cards.
- **`requires`** lists every capability beyond a plain chat turn: external tools, subagents, git worktrees, user confirmation, and so on. A single-platform feature is named exactly as the charter's hard constraint 1 names it (`Skill tool`, `$ARGUMENTS`, `` !`cmd` ``, `${CLAUDE_SKILL_DIR}`, `allowed-tools`, `context: fork`, `hooks`), so synthesis can filter out cards that fail cross-platform parity.

### Extraction method

1. **One source per session, and only that source in context.** Don't open another source's files, or another source's cards, while extracting. Cards can't name, compare against, or link to another source; cross-source relations are synthesis work. An earlier session's cards *from the same source* may be read, to apply the duplicate-in-source skip.
2. **Scope first.** Write or extend the source's `INDEX.md` header: which files this session covers, which are out of scope and why. A source split across sessions gets one INDEX section per session.
3. **Read whole, then cut.** Read every in-scope file end to end before writing any card.
4. **Atomicity test:** if crow could adopt A without B, they are separate cards.
5. **Term rule** (charter, Writing values 4):
   - The Claim uses plain words, or established terms of the field in their established meaning.
   - A term the source coins never appears in the Claim. It goes in *Source terms*, labelled `coined`.
   - An established term the source uses with a changed meaning never appears in the Claim either. It goes in *Source terms*, labelled `redefined`, and *Extractor notes* gives the established meaning and its origin. Example: Matt Pocock's **seam** ("the public boundary you test at") redefines Michael Feathers' seam.
   - When unsure whether a term is established, treat it as coined; plain words are always allowed. Example of an established term the Claim may use: **CRAP score** (Savoia and Evans, crap4j).
6. **Every claim cited.** Each sentence in Claim traces to an entry in `cites`. Quotes are short (≤ 2 lines). Notes-only sources cite the upstream location as `notes.md` recorded it and quote only what `notes.md` quotes.
7. **No taste.** Cards never say good, weak, relevant, or "crow should". Ranking belongs to synthesis, under the taste charter.
8. **Close coverage.** Every in-scope section of every in-scope file appears in `INDEX.md` either as card ids or as `skip: <reason>`, with the reason from a fixed set: `non-engineering`, `stack-specific`, `duplicate-in-source (<card>)`, `no-content` (headings, boilerplate, examples restating a carded rule). A finished ledger has no `todo` rows.
