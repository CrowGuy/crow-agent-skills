# Skill Development Guidelines

These rules apply to every skill change in this repository, including instructions, references, scripts, assets, metadata, and installation tooling.

## Operating Principles

### 1. Understand the skill before editing

* Identify the user problem the skill is intended to solve.
* Define the expected input, output, and completion criteria.
* State assumptions before implementation.
* Ask when a decision materially affects the skill contract.
* Prefer concrete usage examples over abstract descriptions.

### 2. Keep skills small and composable

* Each skill should have one clear responsibility.
* Do not create a large skill when the workflow can be composed from smaller capabilities.
* Avoid duplicating instructions already owned by another skill.
* Prefer invoking or referencing an existing skill over copying its behavior.
* Do not introduce abstractions for hypothetical future use.

### 3. Separate workflow from capability

* Workflow skills coordinate stages and decide what happens next.
* Capability skills perform reusable work such as grilling, planning, testing, reviewing, or diagnosing.
* User-controlled workflow entry points must not silently trigger other user-controlled entry points.
* Reusable capability skills may be composed by workflow skills.

### 4. Keep the skill contract explicit

Every skill must clearly define:

* When it should be used.
* When it should not be used.
* Required inputs.
* Expected outputs.
* Execution workflow.
* Completion criteria.
* Stop conditions or decisions requiring user confirmation.

The skill description must accurately reflect its trigger conditions.

### 5. Keep context usage efficient

* Treat `SKILL.md` as the control plane, not a knowledge dump.
* Keep core instructions concise and operational.
* Move detailed policies, examples, schemas, and domain knowledge into `references/`.
* Load supporting files only when relevant.
* Avoid deep chains of references.

### 6. Prefer deterministic tooling where appropriate

* Use scripts for fragile, repetitive, or deterministic operations.
* Do not encode complex parsing, validation, or transformation logic only as prose when a script would be more reliable.
* Place executable files under `scripts/`.
* Scripts must include clear inputs, outputs, error handling, and usage instructions.

### 7. Make surgical changes

* Touch only files required by the requested change.
* Do not rewrite unrelated skills.
* Preserve existing naming, layout, and writing conventions.
* Remove only artifacts made obsolete by the current change.
* Mention unrelated issues instead of fixing them silently.

### 8. Keep platform behavior aligned

* Maintain one shared behavioral source of truth in `SKILL.md`.
* Keep Claude Code and Codex metadata aligned with the same invocation intent.
* Do not duplicate full skill instructions in platform-specific metadata.
* When platform capabilities differ, document the difference explicitly.
* Avoid changes that make one platform silently behave differently from the other.

## Skill Structure

Use the following structure when applicable:

```text
skill-name/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── scripts/
├── references/
└── assets/
```

Required:

* `SKILL.md`

Use only when needed:

* `agents/` for platform metadata.
* `scripts/` for executable and deterministic operations.
* `references/` for detailed instructions and domain knowledge.
* `assets/` for templates and files used in generated output.

## Required Workflow

### Before editing

* Summarize the intended skill change.
* Identify the target user and usage scenario.
* Define expected input and output.
* List assumptions, ambiguities, and invocation behavior.
* Identify affected files.
* Define how the change will be verified.

### During editing

* Keep the change minimal.
* Keep `SKILL.md` concise.
* Do not mix project-specific details into reusable skills.
* Do not copy policies between skills without a clear ownership reason.
* Keep references directly discoverable from `SKILL.md`.
* Update platform metadata only when invocation or presentation behavior changes.

### After editing

* Validate required files and frontmatter.
* Verify referenced files and scripts exist.
* Check that invocation descriptions match actual behavior.
* Run relevant scripts or validators.
* Test at least one realistic usage scenario.
* Summarize changed files.
* Report risks, skipped checks, assumptions, and known limitations.

## Validation Checklist

A skill change is complete only when:

* The skill has one clear responsibility.
* Trigger conditions are unambiguous.
* Inputs and outputs are explicit.
* Workflow and completion criteria are defined.
* User decisions are not silently made by the agent.
* Detailed content is separated from core instructions where appropriate.
* Referenced files and commands are valid.
* Claude Code and Codex metadata express consistent behavior.
* At least one realistic example has been evaluated.
* No unrelated files were changed.

## Repository-Level Rules

* Shared skills define reusable methods.
* Project repositories define project-specific context, architecture, commands, and policies.
* Do not hard-code a specific project, language, framework, or CI system unless the skill is explicitly scoped to it.
* New skills should begin as small, testable capabilities.
* Promote a skill into a broader workflow only after it has been exercised in real projects.
* Deprecated skills must remain clearly separated from active skills.