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
...         "|node_modules|\\.ruff_cache"
...         ")$"
...     ))
... )
├── .github
│   ├── actions
│   │   └── setup
│   │       └── action.yaml
│   └── workflows
│       ├── code-quality-main.yaml
│       ├── code-quality-pr.yaml
│       ├── python-build.yaml
│       └── tests.yaml
├── .gitignore
├── .pre-commit-config.yaml
├── .python-version
├── AGENTS.md
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

> [!WARNING]
> Note there is no folder in this repository template for `data` -- this is because _you should not put data in your code repository_. Datasets (public or private) should be stored outside of the repository (even if your repository is private) to avoid risking leakage of sensitive data, unnecessary bloat in your code repository, and over specialization to a particular data resource. Similarly, API keys or other "Secrets" for your project should also not be committed to your `git` repository or pushed to GitHub. Note that this applies to the underlying `git` repository as well as the online `github` -- if something is in your `git` commit history, it can be found through the published repository even if it is not on the main branch; in the event that you accidentally commit data or a secret variable, you need to [purge](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository) that commit from your `git` history (and/or from your repository) in addition to any other steps you need to take depending on what was added to a repository and/or exposed.

## Documentation

### Python Build System

This project is built with the [`uv`](https://docs.astral.sh/uv/) tool for managing python builds and environments. `uv` allows you to update package dependencies, install and build your package, and maintain a "lockfile" of specific package and python versions for developmental reproducibility, all through a seamless, `pip` like interface. To use `uv` to install a virtual environment with package dependencies, you can run `uv sync` when in the repository directory. Learn more about it in the documentation linked above.

### Linting / Code Style

For linting and code style, we use [`ruff`](https://docs.astral.sh/ruff/) and follow the
[Python Google Style Guide](https://google.github.io/styleguide/pyguide.html). Linting and formatting happen
automatically upon commit via [`pre-commit`](https://pre-commit.com/) hooks. **You must install the pre-commit
hooks locally via `uv run pre-commit install` after installing the package locally via `uv sync` to enable this
functionality!** Regardless of pre-commit installation status, you can always run `uv run pre-commit run --all-files` to check all files at any time. _This process will modify your code_ as it will attempt to fix
many errors automatically.

Pre-commit tests are configured via the [`.pre-commit-config.yaml`](.pre-commit-config.yaml) file.

### Testing

We use [`pytest`](https://docs.pytest.org/en/stable/) and
[`doctest`](https://docs.python.org/3/library/doctest.html) for testing. To run the tests, after installing the
package locally via `uv sync`, simply run `uv run pytest` in the root directory. If you install the package via
`pip install -e .`, you can also run `pytest` directly. In addition, we use `pytest-cov` for code coverage
reporting, integrated with [codecov.io](https://about.codecov.io/) for tracking coverage over time, PR
integration, and a README badge.

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

#### `CONTRIBUTING.md`

This file helps guide contributors to contribute in the manner and style you prefer. It is also often used to
guide large language model (LLM) agents that may be contributing to the project. The included guide in this
template is a good starting point.

#### `AGENTS.md`

For contribution instructions that are specifically targeted at LLM agents, you can include an `AGENTS.md`
file. This template includes such a file, but it only directs agents to the `CONTRIBUTING.md` file. It may be
updated in the future with custom instructions for LLM agents that are found to be helpful.

### Repository management

This repository (by virtue of being within the [McDermott Health AI Lab GitHub
Organization](https://github.com/McDermottHealthAI) comes pre-built with template GitHub issues. These have
clear names and descriptions, and should be used on newly filed issues to ensure issues are easily searchable
and clear to newcomers. Pull requests should be used for any new features to the main branch, and semantic
versioning should be used, managed through `git` tags (e.g., `git tag 0.0.1`); these will automatically update
the project's version due to the configuration in `pyproject.toml`.
