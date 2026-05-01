---
name: github-issue
description: >-
  Skills for working with GitHub Issues.
  You must load this skill when working with issues
  or when runtime was triggered by an issue comment.
  Load this before any gh skills.
---

# github-issue Skill

This skill helps with work on issues.

## 1. Initialization & Context Routing

### Initialization Sequence

Upon receiving a new objective, you should focus on the user request first.
Identify the trigger source first to understand context and avoid ambiguity. A comment like 'fix it' could refer to a specific issue detail, a previous comment, or a bug report.

### Context & Response Routing

**Response Detection & Routing**:

Check `github.event_name` and payload to identify trigger source:
- **Issue comment** (`issue_comment`):
  - Condition: `if: ${{ !github.event.issue.pull_request }}`
  - Reply Method: `gh issue comment`

**Routing Invariants**:

- **Direct API Responses ONLY**: When asked to comment on an issue, you MUST use the `gh` CLI (`gh issue comment` etc.) to post the comment directly via API. NEVER write the comment text to a file in the workspace or commit such files. For long comments, use a HEREDOC:
  ```bash
  gh issue comment 123 --body "$(cat <<'INNER_EOF'
  [your comment text here]
  INNER_EOF
  )"
  ```
- **Workspace Cleanliness (No PR for Non-Code-Change Tasks)**: If your task is purely informational (e.g., analyzing an issue, posting a comment), you MUST ensure the workspace remains completely clean (no modified or untracked files). ANY modification to the workspace after a repo event triggers an automatic Pull Request. Delete temporary files or run `git clean -fd` before finishing.
- **Symmetric Routing**: ALWAYS reply via the exact originating channel. When asked to post or comment without providing a code fix, you MUST communicate back via the API without modifying any files.
- Parse `github.event.comment.id` to maintain thread continuity.

## 2. Fetching Issue Information

### Issue Comments

```bash
gh issue view <number> --json comments
```

## Related Skills

- `gh`: For low-level `gh` command operations.
