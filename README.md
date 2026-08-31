# MHAL-template

A [copier](https://copier.readthedocs.io/) template for new Python projects at the
[McDermott Health AI Lab](https://github.com/McDermottHealthAI).

> [!IMPORTANT]
> This used to be a flat GitHub template repo (used via the "Use this template" button + manual
> find-and-replace). It is now a copier template — see "Using this template" below. Existing
> projects that were scaffolded from the old flat template still work; they just don't get the
> `copier update` flow automatically.

## What the template gives you

A working Python project with:

- **`uv`-based environment management** (lockfile, fast installs, manages Python interpreter version).
- **`ruff`** for linting and formatting (Google Python style guide, line length 110).
- **`pytest`** + first-class doctests in docstrings and markdown files; `pytest-cov` reports
  coverage to [codecov.io](https://about.codecov.io/).
- **Pre-commit hooks** for ruff, mdformat, shellcheck, codespell, nbstripout, secret scanning
  (`gitleaks`), pyproject schema validation (`validate-pyproject`), dependency hygiene (`deptry`),
  GitHub Actions linting (`actionlint`), workflow security (`zizmor`), and uv-lock-in-sync (`uv-lock`).
- **GitHub Actions workflows** for test, code-quality, and a hardened PyPI publish-on-tag pipeline
  (trusted publishing, sigstore signing, GitHub Release with auto-generated notes).
- **Dependabot** with monthly cadence, 7-day cooldown, and per-ecosystem grouping for low-noise
  dependency updates.
- **Zizmor security hardening** of the workflows (explicit job permissions, `persist-credentials: false`
  on checkouts, env-var routing for all template-expansion-into-shell patterns).
- **`AGENTS.md` / `CLAUDE.md`** for AI coding agents — `AGENTS.md` is a short pointer at
  `CONTRIBUTORS.md` plus a few agent-specific notes; `CLAUDE.md` is a symlink to `AGENTS.md`.
- **`CONTRIBUTORS.md`** as the project's source of truth for build / test / style / PR-workflow
  conventions.

## Using this template

Install copier (one time, globally), then render this template into a new directory:

```bash
# Install copier (recommended via uv as a tool, or via pipx).
uv tool install copier
# or: pipx install copier

# Render into a new project directory. Copier will prompt for the variables
# defined in copier.yml (package name, author, license, etc.).
copier copy gh:McDermottHealthAI/MHAL-template my-new-project
```

You'll be prompted for: Python package name, PyPI distribution name, description, author name and
email, GitHub org and repo, license (MIT / Apache-2.0 / BSD-3-Clause), and Python version. Defaults
are sensible — most prompts can be accepted with Enter.

After rendering:

```bash
cd my-new-project
uv sync
uv run pre-commit install
git init && git add . && git commit -m "Initial commit from MHAL-template"
# Then create a GitHub repo and push.
```

### Pulling in template improvements over time

When this template ships a new pre-commit hook, workflow fix, or convention update, your
generated project can pull in the change:

```bash
copier update
```

Copier shows a merge-conflict-style diff for anything you've customized locally. This is the
biggest reason the lab moved to copier from a flat template — old projects can stay current.

## Repository layout

The template content lives in [`template/`](template/) and is rendered by copier. The configuration
(prompts, exclusions, post-render messaging) lives in [`copier.yml`](copier.yml). Files in
`template/` ending in `.jinja` are rendered as Jinja2 templates; everything else is copied verbatim.
Directory and file names can also contain `{{ ... }}` placeholders that copier substitutes —
notably `template/src/{{package_name}}/`.

## Contributing to the template

See [`template/CONTRIBUTORS.md`](template/CONTRIBUTORS.md) for the conventions the rendered
projects follow. The template itself follows the same conventions where applicable.

Open issues and PRs against this repository for: new pre-commit hooks, workflow improvements,
documentation fixes, copier-prompt additions, or new optional features.

## License

MIT — see [`template/LICENSE.jinja`](template/LICENSE.jinja) for the (templated) license that
ships with generated projects.
