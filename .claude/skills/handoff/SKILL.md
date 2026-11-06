---
name: handoff 
description: Serialize session state into a high-density HANDOFF.md to enable clean context resets without information loss. 
---

# /handoff (Production Lean Edition) 

## Objective
Serialize current session progress, architectural decisions, and unresolved edge cases into `HANDOFF.md` before resetting the context window. 

## Core Rules 
1. **High-Density Serialization**: Omit conversational fluff. Focus strictly on state, decisions, and immediate next steps. 
2. **State Isolation**: Write to `HANDOFF.md` at the project root or working directory. Never dump raw conversation transcripts. 
3. **Single Next Action**: Explicitly define the EXACT first command or prompt to run in the fresh session. 

## Anti-Rationalization Guardrails 
| Agent Excuse | Mandatory Rebuttal | 
| :--- | :--- | 
| *"I can just summarize the progress in chat."* | **Chat disappears on reset.** You MUST write a persistent `HANDOFF.md` file. | 
| *"I'll omit unresolved edge cases to keep it clean."* | **Suppressing risks causes regressions.** Document all open risks and failing seams. | 
| *"The next session can re-read git history."* | **Git logs lack intent.** You must record *why* decisions were made (ADRs). | 

## HANDOFF.md Structure When invoked, generate or overwrite `HANDOFF.md` with the following template:
 ```markdown 
# Session Handoff State 
 
## 1. Executive Summary 
- **Goal**: [Core objective of the ended session] 
- **Status**: [Completed / In-Progress / Blocked] 
 
## 2. Completed Work &amp; Seams 
- [x] [Completed task/feature 1] (Files modified: `path/to/file`) 
- [x] [Completed task/feature 2] 
 
## 3. Key Architectural Decisions (ADRs) 
- **Decision**: [Why approach A was chosen over B] 
 
## 4. Pending Tasks &amp; Unresolved Risks 
- [ ] [Next task 1] 
- **Open Risk/Edge Case**: [Known issue or failing edge case] 
 
## 5. Fresh Session Kickoff Command 
- **First Action**: [Exact command or prompt for the next session]

```

## Execution Workflow

1. **Audit Current State**: Inspect modified files, failing tests, and decisions made in current session.
2. **Generate** **HANDOFF.md**: Fill the template with concise, high-density facts.
3. **Confirm &amp; Prompt User**: Inform user that `HANDOFF.md` is updated and safe to reset session.