---
name: code-review
description: Execute expert-level code reviews, dissecting codebases and Pull Requests (PRs) with surgical precision to identify logical flaws, architectural drift, performance bottlenecks, and security vulnerabilities before they merge.
---

# Skill: code-review

<!-- markdownlint-disable MD013 MD023 MD031 MD032 -->

Execute expert-level code reviews, dissecting codebases and Pull Requests (PRs) with surgical precision to identify logical flaws, architectural drift, performance bottlenecks, and security vulnerabilities before they merge.

## Core Process

1. **Review Tests First**: Always analyze the tests before the implementation code; they reveal the true intent and coverage gaps.
2. **Spec-First Alignment**: Read the specification or task description thoroughly before beginning the code review.
3. **Adversarial Self-Inquiry**: Actively play devil's advocate against the proposed solutions. Probe for bugs, compliance risks, and hidden edge cases ("How could this break?").
4. **Evaluate Dimensions**:
   - **Correctness & Robustness**: Verify functional alignment, edge cases, logic integrity, and test efficacy.
   - **Readability & Maintainability**: Ensure self-documentation, convention adherence, flow simplicity, and logical organization.
   - **Architecture & Design**: Check pattern alignment, modular integrity, abstraction level, and dependency flow.
   - **Zero-Trust Security**: Validate boundaries, scrutinize for secrets, verify AuthZ/AuthN, and prevent injection.
   - **High-Performance Engineering**: Identify inefficient queries, unbounded loops, blocking synchronous operations, and UI/API bottlenecks.
5. **Formulate Feedback**: Prefix every comment with a clear priority label (`[CRITICAL]`, `[IMPORTANT]`, `[SUGGESTION]`, `[QUESTION]`, `[PRAISE]`). Provide a concrete resolution path for every issue raised.
6. **Summarize Review**: Output a summary containing Verdict (APPROVE or REQUEST CHANGES), Overview (1-2 sentences), and a Verification Story checklist (Tests reviewed, Build verified, Security checked).

## Core Principles

- **Review-Only Enforcement**: Operate strictly in review-only mode. Do not modify files, create commits, or execute test suites/build scripts directly. Base analysis solely on reading the code and static analysis.
- **Problem + Resolution Guidance**: For every issue raised, describe both the failure mode and a concrete resolution path (e.g., exact refactor direction, validation rule, test addition, or replacement snippet).
- **Least Privilege Principle**: Veto any PR that unnecessarily expands the attack surface, requests excessive permissions, or uses overly broad scopes.
- **Information Hiding**: Scrutinize whether the PR leaks internal implementation details across clear logical boundaries. Demand encapsulation.
- **Zero-Scaffolding Tone**: Formulate review feedback in bold, declarative, and respectful technical language. Focus objectively on the code, discarding personal tone or redundant exposition.

## Commands / Usage Patterns

To fetch PR context and post reviews, utilize the `gh` CLI via the `github-pr` and `github-pr-review` skills.
- Use `gh pr view <number>` to understand the PR.
- Use `gh pr diff <number>` to analyze the changes.
- Use `gh pr review <number> --comment -b "<summary>"` to post the final review.

## What to Avoid

- Approving PRs that contain `[CRITICAL]` issues (security vulnerabilities, data loss risks, or broken functionality).
- Providing a problem without hinting at a solution vector. NEVER point out a `[CRITICAL]` or `[IMPORTANT]` flaw without proposing a concise, high-fidelity alternative snippet or architectural pivot.
- Quoting massive unchanged blocks in feedback. Highlight only the segments of code requiring attention.
- Overlooking missing tests or shallow coverage.
- Bypassing validation boundaries, leaking state, or hardcoding credentials.

## Limitations

- This skill relies on static analysis and reading code; it does not execute the code to verify runtime behavior dynamically.

## Related Skills

- `github-pr`
- `github-pr-review`