---
name: codex-review-loop
description: Collect Codex CLI review findings, then fix actionable issues and rerun the Codex review until no issues remain.
---
# Codex Review Loop
From the repo root, infer review target from prompt and context.

Collect findings only with the first-class Codex CLI review command:
- Current working tree: `codex review --uncommitted`
- Specific commit: `codex review --commit <sha>`
- Branch comparison: `codex review --base <branch>`
- Repo-wide review: `codex review "Check repository for correctness, security, maintainability, and tests."`

Do not run any non-Codex reviewer. Do not spawn a subagent or ask another agent to review; run the Codex CLI review command directly.

Independently validate each flagged issue, fix confirmed actionable findings, then rerun the same Codex review target until no actionable issues remain.
