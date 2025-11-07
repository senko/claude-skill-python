---
name: python-setup
description: Set up Python projects with uv, ruff, ty, pytest, pre-commit hooks, and GitHub Actions CI following best practices
---

# Python Project Setup Skill

This skill guides you through setting up Python projects with modern tooling and best practices.

## When to Use This Skill

Use this skill when the user asks to:
- Create a new Python project
- Set up a Python development environment
- Initialize a Python project with modern tooling
- Configure Python linting, formatting, type checking, and testing

## Platform and Tools

**Platform:** Assume Debian or Ubuntu Linux unless specified otherwise.

**Python Project Management:**
- Use `uv` for all Python project and dependency management
- Start new projects with `uv init`

**Code Quality Tools:**
- `ruff` - Code formatting and linting
- `ty` - Type checking
- `pytest` - Test runner
- `coverage` and `pytest-cov` - Test coverage reporting
- `pre-commit` - Git commit hooks

**Preferred Libraries:**
- For HTTP client: Use `httpx` (avoid `requests`)
- For secrets management: Use `python-dotenv` with `.env` files

**Available System Tools:**
- `rg` (ripgrep) - Fast file search
- `fd` (fdfind) - Fast file finder
- `jq` - JSON manipulation
- `curl` and `wget` - Web access and API testing

## Project Setup Workflow

### 1. Initial Setup

**IMPORTANT:** Before starting, ask the user for:
- **Project name** (e.g., "my-awesome-project")
- **Short description** (2-3 sentences about what the project does)

Then proceed with:

```bash
uv init <project-name>
cd <project-name>
```

### 2. Project Structure

Create the following directory structure:

```
<project-name>/
├── src/              # Source code goes here
├── tests/            # Test files go here
├── doc/              # Architecture docs and other documentation
├── .github/
│   └── workflows/
│       └── ci.yml    # GitHub Actions CI configuration
├── .pre-commit-config.yaml
├── .env.example      # Example environment variables
├── .gitignore
├── pyproject.toml
└── README.md
```

### 3. Dependencies

Add development dependencies:

```bash
uv add --dev ruff ty pytest coverage pytest-cov pre-commit
```

Add any project-specific dependencies. Common ones:
- `httpx` - For HTTP requests
- `python-dotenv` - For loading `.env` files

### 4. Configuration Files

#### pyproject.toml

Update `pyproject.toml` with:
- Derived one-line description from the user's short description
- pytest configuration
- Project metadata

Add these sections:

```toml
[tool.pytest.ini_options]
addopts = "-ra -q --cov=src --no-cov-on-fail"
pythonpath = ["src"]
testpaths = ["tests"]
asyncio_default_fixture_loop_scope = "function"
```

#### .pre-commit-config.yaml

Create with the following content (reference: `skill/resources/pre-commit-config.yaml`):

```yaml
fail_fast: true
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.12.8
    hooks:
      # Run the linter.
      - id: ruff-check
        args: [--fix]
      # Run the formatter.
      - id: ruff-format
  - repo: local
    hooks:
      - id: ty
        name: ty check
        stages: [pre-commit]
        types: [python]
        entry: uv run ty check
        language: python
        pass_filenames: false
  - repo: local
    hooks:
      # Run the tests
      - id: pytest
        name: pytest
        stages: [pre-commit]
        types: [python]
        entry: uv run pytest
        language: system
        pass_filenames: false
```

Install the hooks:
```bash
uv run pre-commit install
```

#### .github/workflows/ci.yml

Create GitHub Actions CI workflow (reference: `skill/resources/ci.yml`):

```yaml
name: CI

on:
  push:
    branches: ["main"]
  pull_request:
    branches: ["main"]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Set up Python 3.13
        uses: actions/setup-python@v5
        with:
          python-version: "3.13"

      - name: Set up uv
        run: curl -LsSf https://astral.sh/uv/install.sh | sh

      - name: Install dependencies
        run: uv sync --dev

      - name: Lint with ruff
        run: uv run ruff check --output-format github

      - name: Check code style with ruff
        run: uv run ruff format --check --diff

      - name: Check type hints with ty
        run: uv run ty check

      - name: Run tests
        run: uv run pytest --cov=src --cov-report=term-missing
```

#### .gitignore

Ensure `.gitignore` includes:
```
.venv
.env
__pycache__/
*.py[cod]
*$py.class
.pytest_cache/
.coverage
htmlcov/
.ruff_cache/
```

#### .env.example

Create an `.env.example` file with example/default values for any environment variables the project uses. Start with an empty file or basic structure:

```
# Example environment variables
# Copy this file to .env and fill in your values

# API_KEY=your_api_key_here
# DATABASE_URL=postgresql://localhost/dbname
```

### 5. README.md

Create a README following this structure:

```markdown
# <Project Name>

<User's description - 2-3 sentences>

## Quickstart

### Requirements

- Python 3.13 or later
- [uv](https://docs.astral.sh/uv/) (Install: `curl -LsSf https://astral.sh/uv/install.sh | sh`)

### Installation

\```bash
# Clone the repository
git clone <repo-url>
cd <project-name>

# Install dependencies
uv sync --dev

# Set up pre-commit hooks
uv run pre-commit install
\```

### Usage

\```bash
# Run the application
uv run python -m src.<module_name>
\```

## Development

### Running Tests

\```bash
# Run all tests
uv run pytest

# Run with coverage
uv run pytest --cov=src --cov-report=term-missing
\```

### Code Quality

\```bash
# Format code
uv run ruff format

# Lint code
uv run ruff check --fix

# Type check
uv run ty check
\```

## Project Structure

- `src/` - Source code
- `tests/` - Test files
- `doc/` - Documentation
- `.env.example` - Example environment variables

## License

[Specify license]
```

### 6. Initial Test

Create a basic test file `tests/test_basic.py` to verify the test harness works:

```python
def test_basic():
    """Verify that the test harness is working."""
    assert 1 + 1 == 2
```

### 7. Verification

Run these commands to verify everything is set up correctly:

```bash
# Format and lint
uv run ruff format .
uv run ruff check .

# Type check
uv run ty check

# Run tests
uv run pytest
```

All commands should pass successfully.

## Summary Checklist

When setting up a new Python project, ensure:

- [ ] Asked user for project name and description
- [ ] Initialized project with `uv init`
- [ ] Created directory structure (`src/`, `tests/`, `doc/`)
- [ ] Added dev dependencies (ruff, ty, pytest, coverage, pytest-cov, pre-commit)
- [ ] Configured `pyproject.toml` with pytest settings
- [ ] Created `.pre-commit-config.yaml` and installed hooks
- [ ] Set up GitHub Actions CI workflow
- [ ] Updated `.gitignore` to include `.venv` and `.env`
- [ ] Created `.env.example`
- [ ] Wrote comprehensive README.md
- [ ] Created initial test file
- [ ] Verified all tools run successfully

## Tips

- Always ask for specific project requirements before assuming dependencies
- Encourage the user to update `.env.example` as they add configuration needs
- Remind the user to fill in the license section of README.md
- If the project needs a CLI, suggest using `typer` or `click`
- If the project needs async support, mention `pytest-asyncio` is commonly used
- The `doc/` directory is for architecture decisions, API docs, and other project documentation
