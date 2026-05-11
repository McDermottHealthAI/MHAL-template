# Contributors Guide

Thank you for your interest in contributing to this project. This document is the **source of truth**
for build, test, code-style, and workflow conventions used across McDermott Health AI Lab
repositories. AI coding agents (Claude Code, Cursor, Copilot, Codex CLI, Aider, and others) are
also instructed via `AGENTS.md` to follow the guidelines here.

## Good Practice

- Discuss large changes with the maintainers before significant work begins.
- Keep commits focused and provide clear commit messages.
- Include tests for new functionality and bug fixes.
- Ensure documentation is updated for user-facing changes.

## Build System: `uv`

This project uses [`uv`](https://docs.astral.sh/uv/) as its sole Python package manager. **Do not
use `pip`, `conda`, `poetry`, or `pipenv` directly.** `uv` is written in Rust by [Astral](https://astral.sh)
(the same team that makes `ruff`).

Why uv:

- **Fast.** Resolves and installs dependencies in seconds, not minutes.
- **Reproducible.** `uv.lock` pins exact versions of every transitive dependency, so collaborators
    and CI install the identical environment.
- **One tool, one workflow.** Replaces the `pip` + `pip-tools` + `virtualenv` + `pyenv` stack. uv
    also manages Python interpreter versions itself — you don't need a separate `pyenv`.
- **Modern defaults.** Lockfile-first, `pyproject.toml`-native, sensible behavior out of the box.

### Install `uv`

See the [official install guide](https://docs.astral.sh/uv/getting-started/installation/). The
shortest path on Linux and macOS:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

On macOS you can also `brew install uv`. On Windows, see the official install guide.

### Common commands

```bash
uv sync                    # install/update project deps + create .venv
uv sync --group dev        # include the dev dependency group
uv run <cmd>               # run a command inside the project's environment
uv run pytest -v           # e.g. run the tests
uv add <pkg>               # add a runtime dependency to pyproject.toml + uv.lock
uv add --group dev <pkg>   # add a dev-only dependency
uv lock                    # regenerate uv.lock after editing pyproject.toml by hand
uv build                   # build a wheel + sdist into dist/
```

The project's **Python version** is pinned in `.python-version` (3.12+). uv reads this file and
installs the matching interpreter automatically — you don't need to manage Python yourself.

The project's **version number** is managed by `setuptools-scm` from git tags. Do not hardcode
versions in `pyproject.toml`. To release a new version, tag the commit (`git tag 0.1.0 && git push --tags`)
and the CI release workflow publishes to PyPI.

## Installing the Project for Development

Clone the repository, sync the environment, and install the pre-commit hooks:

```bash
git clone https://github.com/McDermottHealthAI/MHAL-template.git
cd MHAL-template
uv sync
uv run pre-commit install
```

The `uv run pre-commit install` step is important — it hooks `pre-commit` into your local git so
formatters and linters run automatically on every commit. Without it, you'll repeatedly fail CI on
issues that would have been auto-fixed locally.

## Testing

- **Run tests**: `uv run pytest -v`
- **Framework**: `pytest` with `pytest-cov` for coverage, integrated with [codecov.io](https://about.codecov.io/)
    for the coverage badge and PR-level coverage diffs.
- **Doctests are first-class tests.** Prefer writing API-validating tests as doctests in docstrings
    and markdown files. Use standalone `tests/**/test_*.py` files only for tests that would be
    excessively long, complex, or unclear as doctests.
- **TDD**: When fixing bugs or adding features, write a failing test first, confirm the failure
    captures the intended behavior, then implement the fix.

### Doctests in markdown

Use `>>>` and `...` prompts as you would in a docstring. **A blank line must separate the final
output line from the closing triple-backtick.**

### Doctest helpers

The project root `conftest.py` pre-populates the doctest namespace via `pytest`'s
[`doctest_namespace`](https://docs.pytest.org/en/stable/how-to/doctest.html#doctest-namespace-fixture)
fixture so you rarely need explicit imports in doctests. Useful helpers (auto-registered if installed):

- [`yaml_to_disk`](https://github.com/mmcdermott/yaml_to_disk) — create temp directory structures
    from a YAML string.
- [`pretty-print-directory`](https://github.com/mmcdermott/pretty-print-directory) — print
    directory trees cleanly in doctest output (provides `print_directory` and `PrintConfig`).

Both auto-register in the doctest namespace once installed.

> [!NOTE]
> The `conftest.py` lives in the project root, *not* `tests/`, so doctest fixtures and namespace
> entries are available to doctests inside the main package code and markdown files — not just
> the standalone test files.

## Code Style

- **Linter / formatter**: [`ruff`](https://docs.astral.sh/ruff/) (configured in `pyproject.toml`).
    Line length 110.
- **Style guide**: [Google Python Style Guide](https://google.github.io/styleguide/pyguide.html).
- **Docstrings**: [Google style](https://www.sphinx-doc.org/en/master/usage/extensions/example_google.html).
    All public functions, classes, and modules must have docstrings. Doctests inside those
    docstrings are encouraged and run as part of the test suite.
- **Imports**: Sorted by `isort` rules via ruff. No unused imports (except `__init__.py` re-exports).

## Running Checks

Pre-commit runs automatically on each commit once you've installed the hooks (see above). You can
also run all hooks against the full repo at any time:

```bash
uv run pre-commit run --all-files
```

This will **modify your code** as the hooks auto-fix many issues (formatting, import sorting,
trailing whitespace, end-of-file newlines, markdown formatting, etc.). Run it, review the diff, and
commit the result.

Run the test suite:

```bash
uv run pytest -v
```

Both pre-commit and pytest run in CI on every PR. Failures block merge.

## Issue Management

- **Open an issue before starting non-trivial work** so maintainers can flag duplication, scope
    concerns, or design issues early.
- **Search existing issues first** — even minor things may already be tracked.
- For bug reports, include: minimal reproduction, expected vs. actual behavior, environment
    details (Python version, uv version, OS).
- For feature proposals, include: motivation (concrete use case), proposed API/UX sketch, and
    whether you're volunteering to implement it.
- Labels are managed at the [organization
    level](https://github.com/McDermottHealthAI). Use existing labels rather than creating new
    per-repo labels unless a repo-specific need clearly justifies it.

## Pull Request Workflow

- **Link PRs to issues with closing keywords.** If the PR resolves an issue, include `Closes #<n>`
    (or `Fixes #<n>` / `Resolves #<n>`) in the PR body so GitHub auto-closes the issue on merge.
    For PRs that relate to an issue but do not fully resolve it, use `Refs #<n>` instead so the
    issue stays open.
- **Watch CI after pushing.** Every time you push a commit to a PR branch, wait for the CI checks
    to complete and assess the results. Use `gh pr checks <pr-number> --watch` to block on
    completion. If any check fails, pull the logs (`gh run view --log-failed`), diagnose, fix, and
    push again. Do not declare a PR ready for review while CI is red.
- **Respond individually to every PR review comment.** Scan *both* locations: in-line review
    comments (attached to diff lines) *and* top-level PR conversation comments. It is easy to miss
    one set if you only look at the diff. For each comment, reply on that specific thread — not as a
    consolidated summary comment on the PR — and either (a) describe how the comment was addressed
    (with a commit SHA if applicable), (b) answer the question posed, or (c) push back with
    reasoning if you disagree.
- **Resolve AI review threads when fully addressed.** For straightforward Copilot / AI-reviewer
    line comments where the fix is unambiguous and the reply is a simple acknowledgment, mark the
    review thread resolved via the GraphQL API:
    `gh api graphql -f query='mutation{resolveReviewThread(input:{threadId:"<id>"}){thread{isResolved}}}'`.
    This only applies to review threads (line comments); top-level PR comments have no resolved
    state. Do **not** auto-resolve threads from human reviewers — leave that to the reviewer.
- **Sync with main via merge, not rebase.** To pick up new changes from `main` into a PR branch,
    run `git merge origin/main` and push. Do not rebase + force-push unless the branch history is
    obviously broken or the maintainer explicitly asks. Merge preserves review context — in-flight
    review comments stay anchored to the lines they were written on.
- **Scan for upstream branch updates at the start of every work stream and periodically.** Before
    starting new work, and on long-running branches, run `git fetch --all --prune` and check
    whether the base branch has moved: `git log --oneline HEAD..origin/<base>`. If so, merge the
    updates in before continuing. Keeps stacked PRs coherent and surfaces conflicts early.
- **Consider squash vs. regular merge on every PR.** Default to a regular merge; use squash only
    when the PR is a single logical change whose per-commit history adds no value. **Never squash
    roll-up PRs or release-candidate PRs going into `main`** — component commits are load-bearing
    for changelogs, bisecting, and attribution.

## Repository Conventions

- **No data in the repo.** Datasets (public or private) belong outside the repository. Never
    commit data files, even to private repos. Data in `git` history is recoverable after deletion;
    if you accidentally commit data or a secret, you need to
    [purge it from history](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository),
    not just delete it from the current tree.
- **No secrets in the repo.** API keys, tokens, and credentials must not appear in commits.
    `gitleaks` runs as a pre-commit hook to catch this, but treat it as a backstop, not a substitute
    for care.
- **Commit messages**: Clear and focused. One logical change per commit.
- **Branching**: Use feature branches and pull requests for all changes to `main`.
- **Semantic versioning**: Managed through git tags (`git tag 0.1.0`); `setuptools-scm` derives the
    package version from the latest tag.
- **Notebooks**: Cell outputs are stripped on commit via `nbstripout`. Notebooks are linted via
    `nbQA` + `ruff`.
- **GitHub Actions versions** are kept current by Dependabot (configured in
    `.github/dependabot.yml`).

## AI-Assisted Development

This template is designed to work well with LLM coding agents. Agent-specific instructions live in
`AGENTS.md` (read by Claude Code, Cursor, Copilot, Codex CLI, Aider, and others). `CLAUDE.md` is a
symlink to `AGENTS.md` so Claude Code picks up the same instructions via its native path. The
`AGENTS.md` file is intentionally short — it instructs agents to follow the conventions in *this*
file, plus a few agent-specific notes.

The rest of this section covers one-time setup for agent-assisted work on a fresh machine. It is
optional — you can contribute without any of this — but following it lets agents be productive on
the repo without repeated prompting.

### Claude Code

[Claude Code](https://docs.claude.com/en/docs/claude-code) is Anthropic's terminal-based coding
agent. Install and authenticate:

```bash
# Requires Node.js >= 18
npm install -g @anthropic-ai/claude-code
claude --version
claude auth login
```

Start it inside the project directory (`claude`) and it will read `AGENTS.md` / `CLAUDE.md`
automatically on session start.

### GitHub CLI (`gh`)

Agents should use the `gh` CLI for all GitHub operations (PRs, issues, releases, actions logs).
It is faster, more reliable, and cheaper in tokens than the GitHub MCP server.

```bash
# Ubuntu/Debian
sudo apt install gh

# macOS
brew install gh

gh auth login
```

### Web search MCP (recommended)

Claude Code has no built-in internet access. Adding a web-search MCP lets agents look up current
docs, verify library behavior, and check recent releases.

[Brave Search](https://brave.com/search/api/) (free tier, general-purpose search):

```bash
claude mcp add brave-search --scope user \
	-e BRAVE_API_KEY=your-key \
	-- npx -y @modelcontextprotocol/server-brave-search
```

### Library docs MCP (recommended)

[Context7](https://context7.com/) fetches current, version-specific documentation for libraries,
frameworks, and SDKs. No API key required. This is more reliable than web search for library-
specific questions because it pulls from the actual docs.

```bash
claude mcp add context7 --scope user -- npx -y @upstash/context7-mcp
```

Install these with `--scope user` so they are available across all your projects, not just this
one.

### Other agents

Most modern LLM coding agents read `AGENTS.md` natively (Cursor, Codex CLI, Aider, Windsurf,
GitHub Copilot). For agents that expect a different filename (e.g., Gemini CLI expects
`GEMINI.md`), symlink it:

```bash
ln -s AGENTS.md GEMINI.md
```

This keeps one source of truth for agent instructions.
