# Skill Contract

This document defines the minimum contract every skill in this repository must follow.

## 1. Required Structure

Each skill must be stored in its own directory:

```text
skills/<skill-name>/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── scripts/
├── references/
└── assets/
```

Required:

* `SKILL.md`
* `agents/openai.yaml` when Codex support is enabled

Optional:

* `scripts/` for executable logic
* `references/` for detailed guidance and domain knowledge
* `assets/` for templates or output resources

Empty optional directories should not be committed.

## 2. Naming Rules

* Directory names must use lowercase kebab-case.
* The frontmatter `name` must match the directory name.
* Names should describe a capability or workflow.
* Avoid vague names such as `helper`, `assistant`, or `utils`.

Good:

```text
code-review
grill-with-docs
implementation-plan
```

Avoid:

```text
dev-helper
general-tool
my-skill
```

## 3. Required Frontmatter

Every `SKILL.md` must start with YAML frontmatter:

```yaml
---
name: code-review
description: Review code changes for correctness, maintainability, testing, and project constraints.
---
```

Requirements:

* `name` must match the skill directory.
* `description` must explain what the skill does.
* For model-invoked skills, the description must include clear trigger conditions.
* For user-invoked skills, the description should be concise and human-readable.
* Do not place the full workflow in the description.

## 4. Skill Responsibility

Each skill must have one primary responsibility.

A skill should be split when it:

* Handles unrelated stages of a workflow.
* Has multiple independent outputs.
* Contains instructions that are reusable elsewhere.
* Requires many conditional branches for unrelated scenarios.

Workflow skills may coordinate other capability skills, but should not duplicate their internal instructions.

## 5. Required Instruction Sections

Every `SKILL.md` must clearly define:

```markdown
# Skill Name

## Purpose

## When to use

## When not to use

## Required inputs

## Workflow

## Output

## Completion criteria

## Stop conditions
```

Sections may be renamed, but the same information must be present.

### Purpose

Describe the single problem the skill solves.

### When to use

Define clear invocation scenarios.

### When not to use

Define boundaries and common false-positive triggers.

### Required inputs

List the information, files, tools, or project context needed.

### Workflow

Provide ordered, operational steps.

### Output

Define the expected result and format.

### Completion criteria

State how the agent determines the task is complete.

### Stop conditions

State when the agent must pause, ask the user, or avoid proceeding.

## 6. Invocation Types

Every skill must be classified as one of the following.

### User-invoked workflow

Used when the user should explicitly choose to start the workflow.

Examples:

* project setup
* requirement grilling
* release execution
* destructive migration

These skills must not be triggered implicitly.

### Model-invoked capability

Reusable capabilities the agent may select when appropriate.

Examples:

* code review
* bug diagnosis
* domain modeling
* test design

Invocation behavior must be consistent across Claude Code and Codex metadata.

## 7. Skill Composition

Skills may compose other skills when:

* The dependency owns a distinct reusable capability.
* Reusing it avoids duplicated instructions.
* The dependency direction is clear.

Rules:

* Workflow skills may invoke capability skills.
* Do not copy another skill's instructions.
* Do not reference another skill's internal files directly.
* Avoid circular dependencies.
* User-controlled workflows must not silently invoke another user-controlled workflow.

## 8. Project Context Boundary

Reusable skills must not hard-code project-specific details such as:

* Programming language
* Framework
* Repository layout
* Test commands
* CI/CD provider
* Deployment environment
* Internal project names

Project-specific information should be read from the target repository, for example:

```text
AGENTS.md
CONTEXT.md
.sdlc/architecture.md
.sdlc/commands.md
.sdlc/quality-gates.md
```

A skill explicitly scoped to one technology may include technology-specific rules, but this scope must be stated in its name and description.

## 9. Progressive Loading

`SKILL.md` is the control plane.

It should contain only the instructions needed to begin and coordinate the task.

Move detailed content into `references/`, including:

* Checklists
* Policies
* Schemas
* Long examples
* Platform-specific instructions
* Domain-specific rules

Rules:

* Supporting files must be linked directly from `SKILL.md`.
* Avoid references that link to more references.
* Load supporting files only when relevant.
* Prefer keeping `SKILL.md` below 500 lines.

## 10. Scripts

Use scripts for deterministic, repetitive, or error-prone operations.

Scripts must:

* Be placed under `scripts/`.
* Document required arguments.
* Validate inputs.
* Return a non-zero exit code on failure.
* Avoid modifying files outside the requested scope.
* Produce concise and actionable errors.
* Be invoked explicitly from `SKILL.md`.

Do not add a script when a short, reliable instruction is sufficient.

## 11. References

Files under `references/` are intended for agent reasoning.

They should contain:

* Detailed procedures
* Engineering standards
* Domain rules
* Examples
* Checklists

Reference files longer than 100 lines should include a table of contents.

Do not place final-output templates or binary resources in `references/`.

## 12. Assets

Files under `assets/` are used to create or modify outputs.

Examples:

* Document templates
* Boilerplate files
* Images
* Configuration templates

Assets are not intended to be loaded as general instructions.

## 13. Output Contract

The skill must define the expected output precisely.

Where appropriate, specify:

* File names
* Markdown headings
* Required fields
* Ordering
* Severity levels
* Machine-readable formats
* Whether files may be modified

A skill should not claim completion if required output is missing.

## 14. Human Decision Boundaries

The skill must not silently decide:

* Product scope
* Destructive operations
* Architecture tradeoffs with material impact
* Security exceptions
* Data migration risk
* Release approval

The agent should investigate facts independently, present options and recommendations, and leave material decisions to the user.

## 15. Validation Requirements

A skill is valid when:

* Required files exist.
* Frontmatter is valid.
* Directory and skill names match.
* Referenced files exist.
* Referenced scripts are executable or runnable.
* Invocation policy is consistent across platforms.
* No circular dependency is introduced.
* At least one realistic usage scenario has been tested.
* The skill produces the documented output.
* No project-specific assumptions are unintentionally embedded.

## 16. Definition of Done

A skill change is complete only when:

* Its responsibility is clear.
* Trigger conditions are unambiguous.
* Inputs and outputs are documented.
* Workflow and completion criteria are defined.
* Stop conditions protect important human decisions.
* Supporting files are organized correctly.
* Platform metadata is aligned.
* Validation checks pass.
* A realistic example has been evaluated.
