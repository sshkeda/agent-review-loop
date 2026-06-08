# Codex Review Loop

A small Codex skill for collecting Codex CLI review findings, then fixing actionable issues until `codex review` converges.

The skill is Codex-only. It must use the first-class Codex CLI review command directly, such as `codex review --uncommitted`, and must not spawn a subagent or ask another agent to perform the review.

## Use

Install or register this repository as a skill source, then invoke:

```text
Use $codex-review-loop to fix issues in my current changes.
```

## License

MIT
