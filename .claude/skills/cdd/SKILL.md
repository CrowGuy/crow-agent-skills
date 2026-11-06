---
name: cdd 
description: Establishes a machine-checkable project quality contract (CONSTRAINTS.md) that prevents coding agents from quietly lowering expectations or bypassing quality gates. Use when asked to set up quality gates, CI/CD thresholds, or prevent agent shortcuts. 
--- 

# Constraint-Driven Development 

## Core Non-Negotiables (The Floor) 
Regardless of the project stack, the following rules are zero-tolerance non-negotiables written into `CONSTRAINTS.md`: 
1. **No Suppression Comments**: Strict ban on `@ts-ignore`, `eslint-disable`, `# noqa`, `# type: ignore`, `istanbul ignore`, and `gitleaks:allow`. 
2. **No Unimplemented Stubs**: Strict ban on `throw new Error("Not implemented")` or empty `catch {}` blocks.
3. **No Unjustified Test Cuts**: Strict ban on skipping (`.skip`) or deleting test files/assertions without explaining the reason in the commit message. 
4. **No Hardcoded Secrets**: Source code must not contain sensitive keys or tokens (scanned with `--redact`). 
5. **No Self-Weakening**: Agents must never modify or lower `CONSTRAINTS.md` standards to force a passing build. 

--- 

## Execution Workflow 
### Step 1: Static Auto-Detection (Detect First) 
Before asking the user, inspect the repository structure. Do not ask for information that can be automatically detected: 
- **Stack &amp; Package Manager**: `package.json`, `pyproject.toml`, `go.mod`, `Cargo.toml` 
- **Linters &amp; Testing**: `eslint.config.*`, `biome.json`, `vitest.config.*` 
- **Existing CI &amp; Agent Configs**: `.github/workflows/`, `CLAUDE.md`, `AGENTS.md` 
Output a short 1-line detection summary, then proceed directly to Step 2. 

--- 

### Step 2: 4 Key Questions (Max 4 Questions) 
Ask one question at a time. Every question must include a sensible default: 
1. **Dimension Selection**: "Which dimensions to lock down beyond the Floor? (a) Diff test coverage (b) Security scans (c) Performance budget (d) Accessibility (axe-core) [Default: a, b]" 
2. **Blocking Policy**: "Block on failure or warn during local dev? [Default: Block on Floor, Warn on others]" 
3. **Target Setting**: "Specify explicit targets, or record today's numbers as a ratchet? [Default: Record today's baseline; refuse to regress]" 
4. **Time Budget**: "Maximum allowed time for task completion verification? [Default: &lt; 90s]" 

--- 

### Step 3: Generate `CONSTRAINTS.md` Create `CONSTRAINTS.md` in the project root: 

```markdown 
# CONSTRAINTS 

## Floor (Always Enforced) 
- No new suppression comments (@ts-ignore, eslint-disable, # noqa) 
- No unimplemented stubs (throw new Error("Not implemented"), empty catch) 
- No skipped/deleted tests without commit reason 
- No hardcoded secrets 
- This file must not be weakened to pass a build 

## Enforced with Numbers 
| Dimension | Rule | Checked by | Runs at | 
|-----------|------|-----------|---------| 
| Types | Zero errors | `npm run check:fast` | edit | 
| Lint | Zero warnings/errors | `npm run check:fast` | edit | 
| Secrets | Zero leaked secrets | `gitleaks dir . --redact` | edit | 
| Coverage | Changed lines ≥ 80% | `npm run check:task` | task end | 
| Security | Dep vulnerabilities &lt; High | `osv-scanner scan source -r .` | CI | 


## Measured, Not Yet Enforced (Ratchets) 
| Metric | Today | Direction | 
|--------|-------|-----------| 
| Project Coverage | [Current Value]% | must not fall | 

## Exceptions 
| ID | Rule | Path | Reason | Owner | Expires | 
|----|------|------|--------|-------|---------|