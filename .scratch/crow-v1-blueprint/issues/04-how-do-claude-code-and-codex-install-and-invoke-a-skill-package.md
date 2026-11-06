# How do Claude Code and Codex install and invoke a skill package?

Type: research
Status: resolved

## Question

From primary sources, how does each platform discover, install, and invoke a package of skills: directory layout, required manifests and metadata, user-invoked versus model-invoked control, and what is shared versus platform-specific in `SKILL.md`?

## Answer

Full findings: branch `research/skill-packaging` (local only, commit `df96cf3`), file `workshop/research/skill-packaging.md`. Sources accessed 2026-11-06: code.claude.com docs, OpenAI Codex docs, and `openai/codex` source at commit 7b54fd2 (rust-v0.167.0).

- **Shared format.** Both follow the agentskills.io `SKILL.md` standard. Only `name` and `description` are shared; each platform silently ignores the other's extra fields.
- **Standalone skill paths differ.** Claude Code: `~/.claude/skills`, `.claude/skills`. Codex: `~/.agents/skills`, `.agents/skills` (plus deprecated `~/.codex/skills`, `.codex/skills`, `/etc/codex/skills`).
- **One plugin tree can serve both.** Codex falls back to `.claude-plugin/plugin.json` and reads `.claude-plugin/marketplace.json`. Install: `claude plugin marketplace add` + `claude plugin install p@m`; `codex plugin marketplace add` + `codex plugin add p@m`. Plugin skills are namespaced `plugin:skill` on both.
- **Manifest `skills` key diverges.** Claude Code adds to the default `skills/`; Codex replaces it and rejects a bare `"./"`.
- **User-only skills need two switches.** Claude Code: `disable-model-invocation: true` in frontmatter. Codex: `policy.allow_implicit_invocation: false` in `agents/openai.yaml`. Codex has no equivalent of Claude's `user-invocable: false`.
- **Explicit invocation differs.** Claude Code `/skill`; Codex `$skill` or the `/skills` picker (`@` for plugins per its docs).
- **Codex has no Skill tool.** Its model reads the listed `SKILL.md` path itself and is told not to delegate that read. Shared wording such as "Call the Skill tool" relies on the Codex model's interpretation.
- **Claude-only features are inert on Codex** (inferred from source): `$ARGUMENTS`, `` !`cmd` ``, `${CLAUDE_SKILL_DIR}`, `allowed-tools`, `context: fork`, `hooks`.
- **Unconfirmed.** Whether Codex marketplace entries truly require `policy` and `category` (docs say yes; a working example omits them); symlink handling; exact Codex syntax for plugin skills; name-collision precedence; plugin auto-update.
