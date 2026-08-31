# Agent Guidelines for McDermott Health AI Lab Projects

This file is read by AI coding agents (Claude Code, Cursor, Copilot, Codex CLI, Aider, Gemini CLI,
and others). `CLAUDE.md` is a symlink to this file.

## Read `CONTRIBUTORS.md` first

**The project's conventions live in [`CONTRIBUTORS.md`](CONTRIBUTORS.md): build system (uv),
testing, code style, issue/PR workflow, repository conventions.** Follow them. The notes below are
agent-specific additions and reminders that build on those conventions — they are not a substitute.

## Agent-specific tooling

- **Use `gh` CLI for all GitHub operations** (PRs, issues, code search, actions logs). Do **not**
  use the GitHub MCP server — `gh` is faster, more reliable, and uses far fewer tokens for the
  same operations.
- **Doctest namespace is pre-populated.** The project root `conftest.py` registers `Path`,
  `datetime`, `tempfile`, plus `yaml_to_disk` and `print_directory` / `PrintConfig` (if installed)
  into the doctest namespace. You rarely need explicit imports inside doctests.

## Working style

- **TDD.** When fixing bugs or adding features, write a failing test first, confirm the failure
  captures the intended behavior, then implement the fix.
- **Pre-commit hooks auto-fix many issues** (formatting, import sorting, EOF newlines, markdown
  format). Run `uv run pre-commit run --all-files` before committing — it will modify files. Stage
  the result.
- **Doctests are first-class.** Prefer adding API-validating tests as doctests in docstrings or
  markdown files; standalone `tests/**/test_*.py` files are for cases where a doctest would be
  excessively long or unclear.

## What not to do

- Do not use `pip install` directly — use `uv sync` or `uv add` (see CONTRIBUTORS.md "Build
  System: uv").
- Do not skip or disable pre-commit hooks (no `--no-verify`).
- Do not add broad `# noqa` or `# type: ignore` comments without a specific rule code and a
  one-line justification.
- Do not modify CI workflows (`.github/workflows/*.yaml`) without discussing with maintainers
  first. Workflow security is enforced by `zizmor` (see `.github/zizmor.yml`); changes there often
  surface real security findings.
- Do not commit data files or secrets. `gitleaks` runs as a pre-commit hook but treat it as a
  backstop, not a substitute for care.
