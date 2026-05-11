# MHAL-template

[![Python 3.12+](https://img.shields.io/badge/-Python_3.12+-blue?logo=python&logoColor=white)](https://www.python.org/downloads/release/python-3100/)
[![PyPI - Version](https://img.shields.io/pypi/v/package_name)](https://pypi.org/project/package_name/)
[![Documentation Status](https://readthedocs.org/projects/package_name/badge/?version=latest)](https://package_name.readthedocs.io/en/latest/?badge=latest)
[![Tests](https://github.com/McDermottHealthAI/MHAL-template/actions/workflows/tests.yaml/badge.svg)](https://github.com/McDermottHealthAI/MHAL-template/actions/workflows/tests.yaml)
[![Test Coverage](https://codecov.io/github/McDermottHealthAI/MHAL-template/graph/badge.svg?token=BV119L5JQJ)](https://codecov.io/github/McDermottHealthAI/MHAL-template)
[![Code Quality](https://github.com/McDermottHealthAI/MHAL-template/actions/workflows/code-quality-main.yaml/badge.svg)](https://github.com/McDermottHealthAI/MHAL-template/actions/workflows/code-quality-main.yaml)
[![Contributors](https://img.shields.io/github/contributors/McDermottHealthAI/MHAL-template.svg)](https://github.com/McDermottHealthAI/package_name/graphs/contributors)
[![Pull Requests](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](https://github.com/McDermottHealthAI/package_name/pulls)
[![License](https://img.shields.io/badge/License-MIT-green.svg?labelColor=gray)](https://github.com/McDermottHealthAI/package_name#license)

A minimal python package/project template for McDermott Health AI Lab research projects.

## Quick Setup

This template contains the following files:

```python
>>> print_directory(
...     Path("."),
...     config=PrintConfig(ignore_regex=(
...         "^(\\.git|.*\\.gitkeep|\\.venv|\\.pytest_cache|.*__pycache__|.*\\.egg-info"
...         "|node_modules|\\.ruff_cache|\\.claude"
...         ")$"
...     ))
... )
├── .github
│   ├── actions
│   │   └── setup
│   │       └── action.yaml
│   ├── dependabot.yml
│   ├── workflows
│   │   ├── code-quality-main.yaml
│   │   ├── code-quality-pr.yaml
│   │   ├── python-build.yaml
│   │   └── tests.yaml
│   └── zizmor.yml
├── .gitignore
├── .pre-commit-config.yaml
├── .python-version
├── AGENTS.md
├── CLAUDE.md
├── CONTRIBUTORS.md
├── LICENSE
├── README.md
├── conftest.py
├── pyproject.toml
├── src
│   └── package_name
│       └── __init__.py
├── tests
└── uv.lock

```

Many of these files are standard, and others are less so. See below for some explanation of these files.

To use this template, simply click the "Use this template" button above to create a new repository initialized
from this repository; next, you will need to change the following aspects of the new repository:

- Rename the `package_name` directory in `src/` to your desired package name.
- Update the `pyproject.toml` file with your package name, author information, and other metadata.
- Update the `README.md` file to point to the correct badge links for your new repository, then update the
    rest of the file with information relevant to your project. You will want to find and replace both
    `package_name` and `MHAL-template` with your new package / repository name.
- Set-up trusted publishing on PyPI for your new package name pointing to the output repository.
- Set-up appropriate tokens for CodeCov or other services (if necessary) within your repository.
- Optionally, update the `LICENSE`, `CONTRIBUTING.md`, and `AGENTS.md` files with information relevant to
    your project.
- Update `AGENTS.md` with any project-specific conventions (e.g., domain-specific naming rules,
    special test fixtures, or additional build steps). Update the `known-first-party` value in the
    `[tool.ruff.lint.isort]` section of `pyproject.toml`.

> [!WARNING]
> Note there is no folder in this repository template for `data` -- this is because _you should not put data in your code repository_. Datasets (public or private) should be stored outside of the repository (even if your repository is private) to avoid risking leakage of sensitive data, unnecessary bloat in your code repository, and over specialization to a particular data resource. Similarly, API keys or other "Secrets" for your project should also not be committed to your `git` repository or pushed to GitHub. Note that this applies to the underlying `git` repository as well as the online `github` -- if something is in your `git` commit history, it can be found through the published repository even if it is not on the main branch; in the event that you accidentally commit data or a secret variable, you need to [purge](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository) that commit from your `git` history (and/or from your repository) in addition to any other steps you need to take depending on what was added to a repository and/or exposed.

## Documentation

### Python Build System: `uv`

This project uses [`uv`](https://docs.astral.sh/uv/) as its sole Python package manager. `uv` is a
fast Rust-based tool from [Astral](https://astral.sh) (the same team behind `ruff`) that replaces
`pip`, `pip-tools`, `virtualenv`, and `pyenv` with one tool and a single, consistent workflow.

Why we standardize on `uv` across the lab:

- **Reproducible environments.** `uv.lock` pins exact versions of every transitive dependency, so
    collaborators and CI install the identical environment.
- **Fast.** Dependency resolution and installs run in seconds, not minutes.
- **Modern defaults.** Lockfile-first, `pyproject.toml`-native; `uv` also installs the right Python
    interpreter for you based on `.python-version`, so you don't need a separate Python-version
    manager.
- **One tool, one workflow** across all of our research repos, which keeps onboarding fast.

The shortest path to install `uv` is `curl -LsSf https://astral.sh/uv/install.sh | sh` (Linux/macOS)
or `brew install uv`. See the [install guide](https://docs.astral.sh/uv/getting-started/installation/)
for other platforms, and see [`CONTRIBUTORS.md`](CONTRIBUTORS.md#build-system-uv) for the commands
you'll use day-to-day.

### Linting / Code Style

For linting and code style we use [`ruff`](https://docs.astral.sh/ruff/) following the
[Python Google Style Guide](https://google.github.io/styleguide/pyguide.html). Formatting and lint
fixes happen automatically on commit via [`pre-commit`](https://pre-commit.com/) hooks, configured
in [`.pre-commit-config.yaml`](.pre-commit-config.yaml). See
[`CONTRIBUTORS.md`](CONTRIBUTORS.md#code-style) for install steps and the full list of hooks
(which includes more than just ruff — secret scanning, dependency hygiene, workflow security, etc.).

### Testing

We use [`pytest`](https://docs.pytest.org/en/stable/) plus
[`doctest`](https://docs.python.org/3/library/doctest.html), with
[`pytest-cov`](https://github.com/pytest-dev/pytest-cov) reporting coverage to
[codecov.io](https://about.codecov.io/) for the README badge and PR-level coverage diffs. Run
`uv run pytest -v` to execute the full suite locally; see
[`CONTRIBUTORS.md`](CONTRIBUTORS.md#testing) for the full setup and conventions.

#### Testing Style and Doctests

While conventional wisdom in the software engineering community is to avoid doctests, I disagree. I feel that
doctests are an excellent way (provided appropriate APIs are written, tools are used, and the kinds of tests
included are appropriate) to ensure that code examples in docstrings and markdown documentation remains
accurate and reliable. This is especially important in research code, where the audience may be less
experienced programmers and more likely to copy-paste code examples from documentation. To this end, I
recommend, in general, writing conventional unit tests that validate a function or class's API as doctests
wherever possible. If such a test would be excessively long, complex, or unclear, then it should be written as
a standalone unit test in a `tests/**/test_*.py` file.

> [!NOTE]
> Note that when embedding doctests in markdown files, you must still use the `>>>` and `...` prompts, and you
> must ensure there is a new line separating the final output line from the `\`\`\`\` closing the code block.
> See above for an example.

Note that you can make doctests much easier to write and read (by omitting common setup or import code) by
using a [`conftest.py`](conftest.py) file to define common fixtures and add imports to the
[doctest namespace](https://docs.pytest.org/en/stable/how-to/doctest.html#doctest-namespace-fixture).
See the linked example for how to enable this functionality.

> [!NOTE]
> Note that the linked [`conftest.py`](conftest.py) file is located in the root directory, _not_ the root test
> directory (`tests/`). This is because we want the fixtures and imports to be available to doctests in
> non-test files (e.g., docstrings in the main package and markdown documentation files).

#### Additional Testing Packages

Beyond the default packages, you may also want to use:

- [`pytest-doctestplus`](https://github.com/scientific-python/pytest-doctestplus) for advanced doctest
    support.
- [`hypothesis`](https://hypothesis.readthedocs.io/en/latest/) for property-based testing.
- [`pretty-print-directory`](https://github.com/mmcdermott/pretty-print-directory) for easy
    visualization of directory structures in tests (especially doctests).
- [`yaml_to_disk`](https://github.com/mmcdermott/yaml_to_disk) to easily initialize a temporary directory
    structure from a YAML string in tests (especially doctests).
- [`pytest-codeblocks`](https://github.com/nschloe/pytest-codeblocks) to enable testing shell codeblocks as
    well as python codeblocks in markdown files; however, this would make it more challenging to have
    non-tested codeblocks in markdown files, so there are tradeoffs.

### Additional Files

#### `README.md`

This file contains the main documentation for your project, and should be kept up to date.

#### `LICENSE`

This file contains the license for your project. Often, [The MIT License](https://opensource.org/license/mit)
is a good choice for research projects.

#### `CONTRIBUTORS.md`

This is the **source of truth** for build, test, code-style, and PR-workflow conventions. Both
human contributors and AI agents (via `AGENTS.md`) are pointed at this file. The included guide in
the template is a good starting point — keep it up to date as your project's conventions evolve.

#### `AGENTS.md` and `CLAUDE.md`

`AGENTS.md` provides AI coding agents (Claude Code, Cursor, Copilot, Codex CLI, Gemini CLI, and
others) with a short pointer to `CONTRIBUTORS.md` plus a handful of agent-specific reminders
(use `gh` not the GitHub MCP, doctest namespace pre-population, TDD encouragement, what not to do).
`CLAUDE.md` is a symlink to `AGENTS.md` so Claude Code picks up the same instructions via its
native path. See the "AI-Assisted Development" section of [`CONTRIBUTORS.md`](CONTRIBUTORS.md) for
one-time setup instructions covering Claude Code, the `gh` CLI, and recommended MCP servers for
web search and library documentation.

### Repository management

This repository lives within the
[McDermott Health AI Lab GitHub Organization](https://github.com/McDermottHealthAI), which sets
default issue labels and (in time) issue templates that propagate to new repos. Use those labels
on new issues so filings stay searchable and consistent. All changes to `main` go through pull
requests; see the "Pull Request Workflow" section of [`CONTRIBUTORS.md`](CONTRIBUTORS.md) for the
expected flow (closing keywords, CI watching, comment replies, merge-vs-rebase policy). Versioning
follows semantic versioning, managed through `git` tags (e.g., `git tag 0.0.1`) — `setuptools-scm`
reads the tag and stamps the package version automatically.
