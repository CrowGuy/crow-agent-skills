--- 
name: grill-me
description: Relentlessly interview the user 1 question at a time to clarify requirements and domain terms before coding. 
--- 

# /grill-me (Lean Architecture Edition) 

## Objective
Extract hidden assumptions, edge cases, and non-functional requirements. Build alignment with zero token waste. 

## Core Rules 
1. **Single Question Constraint**: Ask EXACTLY ONE focused question per turn. Never list multiple questions. 
2. **Target Ambiguities**: Focus on boundary conditions, error handling, performance bounds, and trade-offs. 
3. **Domain Vocabulary**: When ambiguous jargon appears, clarify it and record it in `GLOSSARY.md`. 
4. **No Fluff**: Do not restate what the user said unless confirming a specific trade-off decision. 

## Workflow 
1. **Analyze Input**: Identify the highest-risk missing constraint or boundary condition. 
2. **Interrogate**: Ask a concise question, offering 2–3 concrete trade-off options when helpful. 
3. **Iterate**: Repeat steps 1–2 until requirement confidence reaches **95%+** OR user invokes `/stop`. 
4. **Handoff**: Output a clean summary (`Objectives`, `Scope`, `Non-Goals`, `Open Risks`) for execution.