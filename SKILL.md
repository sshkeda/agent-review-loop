---
name: codex-review-loop
description: Run the official Codex CLI `codex review` in a fix-rerun loop until no actionable issues remain.
---
# Codex Review Loop
Run `codex review` from the repo root. Infer target from prompt:

- `--uncommitted` for uncommitted changes
- `--commit <sha>` for a specific commit
- `--base <branch>` for a branch diff
- repo-wide: `codex review "Check repository for correctness, security, maintainability, and tests."`

Fix issues and rerun the same target until Codex reports no actionable issues.
