---
name: gh-aw-compile
description: Regenerate and post-process all agentic workflows. Use when gh-aw is updated, workflow .md files change, or when asked to recompile/regenerate workflows.
allowed-tools: Bash(gh:*), Bash(npx:*), Read, Glob, Edit
---

# Recompile Agentic Workflows

Use this skill when you need to regenerate all agentic workflow lock files and apply post-processing.

## IMPORTANT: Post-processing is required after EVERY lock file change

Any time `.lock.yml` files are regenerated — whether via `gh aw compile`, `gh aw upgrade`, or any other gh-aw
command — you MUST run the post-processing script afterward. This is not optional.

## Steps

### 1. Compile or upgrade workflows

Use whichever command is appropriate:

```bash
# Full upgrade (updates agents, actions, codemods, then compiles)
gh aw upgrade

# Just recompile (when only .md workflow files changed)
gh aw compile
```

If any workflow fails to compile (e.g., strict mode violations like `contents: write`), fix the `.md` source file and re-run.

## Common Issues

### Strict mode violations

Newer gh-aw versions enforce strict mode which disallows write permissions like `contents: write`,
`issues: write`, etc. Workflows should use `safe-outputs` for write operations and only request
`read` permissions.

### Discussion category warnings

Warnings about "General" vs "general" discussion category casing are non-blocking.

## Verification

After both steps, run `git diff --stat` to review all changed files. Expect changes in:

- `.github/agents/` - Updated agent files
- `.github/aw/actions-lock.json` - Updated action pins
- `.github/workflows/*.lock.yml` - Regenerated lock files
- `.github/workflows/*.md` - If codemods applied fixes

## References

- <https://github.com/github/gh-aw-firewall/blob/main/.claude/skills/recompile-workflows/SKILL.md>
