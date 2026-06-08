---
name: codex-review-loop
description: Use when an agent should run a Codex CLI code review on repository changes, including inferring whether the user wants uncommitted changes, a whole branch or repo diff, or a specific commit reviewed.
---

# Codex Review Loop

Use this skill to delegate a focused code review to the Codex CLI, then turn the review output into actionable findings.

## Workflow

1. Identify the repository root with `git rev-parse --show-toplevel` and run review commands from there.
2. Infer the review target from the user's prompt.
3. If needed, inspect lightweight git state before choosing a target.
4. Run `codex review` with the smallest command that matches the target.
5. Read the review output critically, discard low-confidence noise, and report the concrete issues first.

## Intent Routing

Prefer these targets:

- **Uncommitted changes**: If the prompt says "my changes", "current changes", "working tree", "before I commit", "unstaged", "staged", or is otherwise about work in progress, run:

```bash
codex review --uncommitted
```

- **Specific commit**: If the prompt names a SHA, says "this commit", "last commit", "HEAD", or asks to review one commit, resolve it with git if useful, then run:

```bash
codex review --commit <sha>
```

- **Branch or PR-style diff**: If the prompt says "this branch", "PR", "against main", "against trunk", or names a base branch, run:

```bash
codex review --base <base-branch>
```

- **Entire repository**: If the user explicitly asks to review the whole repo or codebase, tell Codex what kind of repo-wide review to perform. Use a prompt, because `codex review` is diff-oriented:

```bash
codex review "Review this repository broadly for correctness, security, maintainability, and missing tests. Focus on actionable issues with file and line references."
```

## Lightweight Git Checks

Use these only as needed to infer intent:

```bash
git status --short
git branch --show-current
git rev-parse --short HEAD
git log --oneline -5
```

If the user did not name a target and the working tree has changes, default to `--uncommitted`. If there are no uncommitted changes, default to the current branch against its likely base branch if discoverable; otherwise ask one short question.

## Custom Instructions

Pass concise review criteria as the optional prompt when the user asks for a specific lens:

```bash
codex review --uncommitted "Focus on regression risk and missing tests."
codex review --commit <sha> "Focus on security and data-loss risks."
codex review --base main "Focus on production-facing behavior changes."
```

Do not include secrets, customer data, private runtime data, or large proprietary context in the prompt. Let Codex inspect the local repository through its own allowed tools.

## Reporting

Report review findings in code-review style:

- Findings first, ordered by severity.
- Include file and line references when the review provides them or when you verify them locally.
- Separate confirmed bugs from speculative improvements.
- Mention when the review found no actionable issues.
- State the exact review command you ran.

Do not automatically edit code unless the user asked you to fix the findings. If the user asks for a fix, make the change after the review and verify it normally.
