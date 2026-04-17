# Agent Guidelines for McDermott Health AI Lab Projects

This file provides instructions for AI coding agents working on this repository. Human contributors
should also refer to `CONTRIBUTORS.md`.

## Build System

- **Package manager**: `uv` (not pip, not conda, not poetry).
- **Install**: `uv sync` to set up the environment. Use `uv run <cmd>` to run commands.
- **Pre-commit**: `uv run pre-commit install` (first time), then `uv run pre-commit run --all-files`.
- **Python version**: 3.12+ (see `.python-version`).
- **Versioning**: Managed by `setuptools-scm` via git tags (e.g., `git tag 0.1.0`). Do not hardcode versions.

## Testing

- **Run tests**: `uv run pytest -v`
- **Framework**: `pytest` with `pytest-cov` for coverage.
- **Doctests are first-class tests.** Prefer writing API-validating tests as doctests in docstrings and
  markdown files. Use standalone `tests/**/test_*.py` files only for tests that would be excessively long,
  complex, or unclear as doctests. The `conftest.py` in the project root pre-populates the doctest namespace
  (e.g., `Path`, `datetime`, `tempfile`) so you rarely need imports in doctests.
- **Doctest format in markdown**: Use `>>>` / `...` prompts. A blank line must separate the final output
  line from the closing triple-backtick.
- **Useful test helpers** (available in doctest namespace if installed): `yaml_to_disk` for creating temp
  directory structures from YAML, `print_directory` for directory tree display.
- **TDD**: When fixing bugs or adding features, write a failing test first, confirm the failure captures the
  intended behavior, then implement the fix.

## Code Style

- **Linter/formatter**: `ruff` (configured in `pyproject.toml`). Line length 110.
- **Style guide**: Google Python Style Guide.
- **Docstrings**: Google style. All public functions, classes, and modules must have docstrings.
- **Imports**: Sorted by `isort` rules via ruff. No unused imports (except `__init__.py` re-exports).
- **Pre-commit hooks auto-fix** formatting, so run `uv run pre-commit run --all-files` before committing
  to catch and apply fixes.

## GitHub

- Use `gh` CLI for all GitHub operations (PRs, issues, code search, actions). Do not use the GitHub MCP
  server — `gh` is more reliable, uses far fewer tokens, and Claude already knows it well.

### Pull Requests

- **Link PRs to issues with closing keywords.** If the PR resolves an issue, include `Closes #<n>`
  (or `Fixes #<n>` / `Resolves #<n>`) in the PR body so GitHub auto-closes the issue on merge.
  For PRs that relate to an issue but do not fully resolve it, use `Refs #<n>` instead so the
  issue stays open.
- **Watch CI after pushing.** Every time you push a commit to a PR branch, wait for the CI checks
  to complete, then assess the results. Use `gh pr checks <pr-number> --watch` (or
  `gh run watch <run-id>`) to block on completion. If any check fails, pull the logs via
  `gh run view --log-failed`, diagnose, fix, and push again. Do not declare a PR ready for review
  while CI is red.
- **Respond individually to every PR review comment.** For each line comment and each top-level
  review comment, post a reply on that specific comment (via `gh api` to
  `/repos/:owner/:repo/pulls/:pr/comments/:id/replies` for line comments, or the issue-comment
  endpoint for top-level comments). Each reply should either (a) describe how the comment was
  addressed (with a commit SHA if applicable), (b) answer the question posed, or (c) push back
  with reasoning if you disagree. Do **not** consolidate responses into a single summary comment
  on the PR — reviewers need to track resolution per thread.
- **Sync with main via merge, not rebase.** To pick up new changes from `main` into a PR branch,
  run `git merge origin/main` and push. Do not rebase + force-push unless there is a specific
  reason (e.g., the branch history is obviously broken, or the maintainer asks). Merge preserves
  review context: in-flight review comments stay anchored to the lines they were written on, and
  reviewers can see exactly what you added since their last pass. Force-pushes invalidate that
  context.

## CI

- **Tests**: Run on push to `main` and on PRs (`uv run pytest` with coverage uploaded to Codecov).
- **Code quality**: Pre-commit hooks run on changed files (PRs) or all files (main pushes).
- **Build**: `uv build` on every push; PyPI publish on git tags via trusted publishing.
- All CI checks must pass before merging a PR.

## Repository Conventions

- **No data in the repo.** Datasets (public or private) belong outside the repository. Never commit data
  files, even to private repos.
- **No secrets in the repo.** API keys, tokens, and credentials must not appear in commits. If accidentally
  committed, purge from git history immediately.
- **Commit messages**: Clear and focused. One logical change per commit.
- **Branching**: Use feature branches and pull requests for all changes to `main`.
- **Semantic versioning**: Managed through git tags.
- **Notebooks**: Cell outputs are stripped on commit via `nbstripout`. Notebooks are linted via `nbQA`.

## What Not to Do

- Do not use `pip install` directly; use `uv sync` or `uv add`.
- Do not skip or disable pre-commit hooks.
- Do not add broad `# noqa` or `# type: ignore` comments without a specific code and justification.
- Do not modify CI workflows without discussing with maintainers first.
