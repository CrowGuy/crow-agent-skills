# Git Agent Protocol (Atomic, Verifiable & Context-Optimized) 

## 1. Core Execution Loop (Save Point Pattern) 
Always execute code changes through this micro-cycle. Never skip steps: 
1. **Plan Micro-Slice:** Define 1 logical, ordered change (~100 lines target). 
2. **Implement & Test:** Modify code and run verification tests immediately. 
3. **Branching Logic:** 
   - [PASS] -> Commit local save point -> Move to next slice. 
   - [FAIL] -> Hard Revert (`git reset --hard HEAD`) -> Re-evaluate approach. 
4. **Final Sync:** Only push to `main` when the entire task is complete and verified. 

## 2. Commit Message & Proof Protocol 
Format every commit strictly using this template (No vague messages):

<type>: <short summary>

WHY: <Reason for this change>
WHAT: <High-level logical modifications>
PROOF: <Exact test command executed & proof of passage (e.g., test output/curl log)>

Types: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`.

## 3. Scope & Boundary Discipline
Before declaring task completion, explicitly declare change boundaries:
- **CHANGES MADE:** Summary of logical edits.
- **INTENTIONALLY UNTOUCHED:** Key files/modules deliberately ignored (prevents unsolicited renovation).

## 4. Hard Constraints & Red Flags (NON-NEGOTIABLE)
- NEVER mix formatting/linting edits with functional behavior changes.
- NEVER mix code refactoring with new feature additions in a single commit.
- NEVER commit without running tests and providing execution PROOF in the commit message.
- NEVER leave messy uncommitted states; revert to the last green save point upon failure.
- NEVER force-push (`git push -f`) or push unverified code to shared branches.