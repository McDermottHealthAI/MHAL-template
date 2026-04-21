# Contributors Guide

Thank you for your interest in contributing to this project. The following sections outline the expectations
for contributors.

## Good Practice

- Discuss large changes with the maintainers before significant work begins.
- Keep commits focused and provide clear commit messages.
- Include tests for new functionality and bug fixes.
- Ensure documentation is updated for user-facing changes.

## Installing the Project for Development

This project uses [`uv`](https://docs.astral.sh/uv/) for environment management. To set up the project for
development, simply clone the repository, then sync the uv environment, activate it, and install the pre-commit
environment:

```bash
uv sync
source .venv/bin/activate # Not strictly necessary, but ensures if you don't use uv things still work.
uv run pre-commit install
```

## Documentation Standards

Documentation should be written using
[Google style docstrings](https://www.sphinx-doc.org/en/master/usage/extensions/example_google.html).
API validation can be tested directly inside these docstrings via doctests; this behavior is encouraged.

Helpers like `yaml_disk` and `print_directory` can be made available by
installing the helpers [`yaml_to_disk`](https://github.com/mmcdermott/yaml_to_disk) (which allows you to
populate temporary directories via a yaml snippet) and
[`pretty-print-directory`](https://github.com/mmcdermott/pretty-print-directory) (which allows you to print a
directory structure for doctest prettification) in the development environment. Once installed, these packages
_automatically register in the doctest namespace within pytest_, so you can use them directly in doctests
without needing to import them.

A global `conftest.py` further populates the namespace using `pytest`'s `doctest_namespace` fixture, meaning
you rarely need explicit imports in doctests.

## Running Checks

Pre-commit should run automatically upon commit (if you have installed the hooks above). You can also run it
directly via:

```bash
uv run pre-commit run --all-files
```

You can also run the tests (including doctests):

```bash
uv run pytest -v
```

Tests and pre-commit checks are run as part of the continuous integration process, so any failures will prevent
pull requests from being merged.

## AI-Assisted Development

This template is designed to work well with LLM coding agents. Agent-specific instructions live in
`AGENTS.md` (read by Claude Code, Cursor, Copilot, Codex CLI, Aider, and others). `CLAUDE.md` is a
symlink to `AGENTS.md` so Claude Code picks up the same instructions via its native path.

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
