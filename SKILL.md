---
name: agent-review-loop
description: Collect Codex and Claude Code review findings, then fix actionable issues and rerun review until no issues remain.
---
# Agent Review Loop
From the repo root, infer review target from prompt and context.

Collect findings with available reviewers:
- Codex: `codex review --uncommitted`, `--commit <sha>`, `--base <branch>`, or repo-wide `codex review "Check repository for correctness, security, maintainability, and tests."`
- Claude: `/code-review` for current diff, path, or PR reference; no `--fix` by default.

Default to Codex as fixer: independently validate each flagged issue, fix confirmed actionable findings, then rerun the same review target until no actionable issues remain.
