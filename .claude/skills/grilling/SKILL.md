---
name: grilling 
description: Relentlessly interview the user 1 question at a time to clarify requirements and domain terms before coding. 
---

Extract hidden assumptions, edge cases, and non-functional requirements. Build alignment with zero token waste. 

## Core Directives & Hard Constraints

1. **Phase 0: Detect Before Asking (Environment Fact Scanning)**

  * Quietly scan project files (`package.json`, `pyproject.toml`, `Cargo.toml`, `.claude/`, etc.) using Claude Code tools.
  * Output **ONLY a 2-line summary** to main context (e.g., `STACK: Node 20, React 19, Vitest`). NEVER ask the user for readable codebase facts.
2. **Phase 1: Surface Assumptions & Scope Bounds**

  * Explicitly list 3-5 background assumptions in Round 1 before deep diving.
  * Reframe vague goals into quantifiable criteria (e.g., "LCP &lt; 2.5s" instead of "make it faster").
  * Output a high-level Capability Map for multi-module or complex feature requests.
3. **Phase 2: Frontier Budget & Sane Defaults (Max 1-3 Questions/Turn)**

  * Ask max 1-3 questions per turn, strictly targeting unresolved Frontier nodes.
  * Every question MUST include a concrete recommended answer (`➡️ 建議解答: ...`).
  * Frame phrasing so the user can authorize entirely by replying "Yes" or "採納".
4. **Phase 3: Consolidate 3-Tier Boundaries**

  * Before implementation, output a 3-tier boundary matrix:  
    * **Always Do**: Non-negotiable quality checks and standards.
    * **Ask First**: Schema/DB changes, new dependencies, or CI edits.
    * **Never Do**: Commit secrets, add suppression comments (`@ts-ignore`), or delete failing tests.
5. **Strip Reasoning & Minimal SRU (Context Defense)**

  * Omit lengthy chain-of-thought and debate histories from prompt outputs; pass only `Artifact + Contract`.
  * Prevent "Lost in the Middle" and instruction decay.

## Negative Constraints (Strict Prohibitions)

* **NO Open-Ended Queries**: Never ask questions without providing a recommended default (`➡️ 建議解答`).
* **NO Heavy Doc Dumping**: Do NOT fetch or paste entire API reference manuals during exploration.
* **NO Premature Execution**: Do NOT write heavy installation scripts or trigger multi-model debate CLI loops during exploration.