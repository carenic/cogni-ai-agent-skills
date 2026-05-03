---
name: github-pr-review
description: >-
  Comprehensive PR review workflow for verifying code quality, metadata accuracy, and merge readiness.
  You must load this skill when reviewing, auditing, or verifying a GitHub Pull Request.
---

# GitHub PR Review

Elite autonomous PR review workflow for ensuring structural integrity, metadata accuracy, and zero-defect deployments.

## When to Activate

- Triggered when asked to review, audit, or verify a GitHub Pull Request.
- Loaded automatically during the final verification phase of a PR lifecycle.

## Core Review Checkpoints

Execute these checks systematically using `gh` and `git` tools:

1.  **Branch State**: Verify if the PR branch is up-to-date with the base branch.
2.  **Comments Resolution**: Ensure all conversation threads and review comments are marked as resolved or addressed.
3.  **Metadata Accuracy**:
    - Validate that the **PR Title** follows project conventions (e.g., Conventional Commits).
    - Ensure the **PR Description** accurately reflects the cumulative changes of all commits.
4.  **Code Hygiene**:
    - Scan for temporary/debug statements (e.g., `console.log`, `print`, `TODO`, `FIXME`, debugger breakpoints).
    - Identify unintended files (e.g., `.env`, temporary logs, IDE config, build artifacts).
5.  **Merge Readiness**:
    - Check mergeability status (conflicts).
    - Verify that all CI/CD checks (status checks) are passing.
6.  **Impact Assessment**: Evaluate if changes introduce regressions or break existing workflows/APIs.

## Workflow Execution

### 1. Context Gathering

```bash
# Get PR metadata and mergeability
gh pr view <pr-number> --json title,body,mergeable,state,baseRefName,headRefName

# List all files changed
gh pr view <pr-number> --json files --jq '.files[].path'

# Get status checks
gh pr checks <pr-number>
```

### 2. Deep Inspection

```bash
# Check for debug statements in diff
git diff $(gh pr view <pr-number> --json baseRefName --jq .baseRefName)...HEAD | grep -E "console\.log|debugger|print\(|TODO|FIXME"

# List comments to ensure resolution
gh api repos/:owner/:repo/pulls/<pr-number>/comments
```

### 3. Verification

- Run local tests if applicable.
- Perform a "Design-It-Twice" comparison if the PR implements a complex architectural change.

## What to Avoid
- **Blanket Approvals**: Never approve without verifying EVERY checkpoint.
- **Ignoring Checks**: Never skip checking the status of CI/CD pipelines.
- **Missing Context**: Always read the PR description and linked issues before reviewing code.
