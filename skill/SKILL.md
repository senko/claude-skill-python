---
name: python-setup
description: Set up and maintain Python projects with uv, ruff, ty, pytest, pre-commit hooks, and GitHub Actions CI following best practices
---

# Python Development Skill

This skill guides you through setting up and maintaining Python projects with modern tooling and best practices. Use it both for initial project setup and throughout the development lifecycle.

## When to Use This Skill

Use this skill when the user asks to:
- **Initial Setup:** Create a new Python project, set up development environment, configure tooling
- **Ongoing Development:** Add dependencies, configure new features, update project structure
- **Maintenance:** Update tooling configuration, add tests, improve documentation

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
# Initialize the project in the current directory
uv init

# Remove the dummy hello.py file that uv creates
rm hello.py
```

**Note:** The user should already be in the directory where they want to create the project. Use `uv init` without parameters to initialize in the current directory, then update the project name in `pyproject.toml` if needed.

### 2. Project Structure

Create the following directory structure in the current directory:

```
.
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
- **Project name** - Update the `name` field with the user's project name
- **Derived one-line description** from the user's short description
- **pytest configuration** - Read `skill/resources/pyproject-pytest-config.toml` using the Read tool for the configuration to add
- Project metadata

#### .pre-commit-config.yaml

Create `.pre-commit-config.yaml` by reading the template from `skill/resources/pre-commit-config.yaml` using the Read tool.

Then install the hooks:
```bash
uv run pre-commit install
```

#### .github/workflows/ci.yml

Create the GitHub Actions CI workflow by reading the template from `skill/resources/ci.yml` using the Read tool. Place it at `.github/workflows/ci.yml`.

#### .gitignore

Update `.gitignore` by reading the template from `skill/resources/gitignore-template` using the Read tool. Ensure it includes `.venv` and `.env` at minimum.

#### .env.example

Create `.env.example` by reading the template from `skill/resources/env-example-template` using the Read tool. Update it with any project-specific environment variables as needed.

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

Create a basic test file `tests/test_basic.py` by reading the template from `skill/resources/test-basic-template.py` using the Read tool. This test verifies that the test harness is working correctly.

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
- [ ] Initialized project with `uv init` (in current directory)
- [ ] Removed the dummy `hello.py` file created by uv
- [ ] Updated project name in `pyproject.toml`
- [ ] Created directory structure (`src/`, `tests/`, `doc/`)
- [ ] Added dev dependencies (ruff, ty, pytest, coverage, pytest-cov, pre-commit)
- [ ] Configured `pyproject.toml` with pytest settings and description
- [ ] Created `.pre-commit-config.yaml` and installed hooks
- [ ] Set up GitHub Actions CI workflow
- [ ] Updated `.gitignore` to include `.venv` and `.env`
- [ ] Created `.env.example`
- [ ] Wrote comprehensive README.md
- [ ] Created initial test file
- [ ] Verified all tools run successfully

## Ongoing Development Tasks

Throughout the project lifecycle, proactively help with:

### Adding Dependencies

When adding new dependencies:
- Use `uv add <package>` for runtime dependencies
- Use `uv add --dev <package>` for development dependencies
- Update `.env.example` if the new dependency requires environment variables
- Update README.md if the dependency changes setup or usage instructions

### Environment Variables

When code references new environment variables:
- Add them to `.env.example` with example values and comments
- Update the README if they need to be documented
- Never commit actual values to `.env.example` - use placeholders

### Documentation

Proactively maintain documentation:
- Update README.md when features, usage, or setup instructions change
- Add architecture docs to `doc/` for significant design decisions
- Fill in the `[Specify license]` section in README.md if the user mentions a license
- Keep the "Project Structure" section current if directories are added

### Common Libraries

Recommend appropriate libraries when needed:
- CLI tools: Suggest `typer` or `click`
- Async support: Suggest `pytest-asyncio` for testing
- HTTP requests: Always use `httpx` (not `requests`)
- Configuration: Use `python-dotenv` for environment variables

### Testing

When writing new features:
- Add corresponding tests in `tests/`
- Follow the naming convention `test_*.py`
- Ensure tests can be imported from `src/` (already configured in pytest)
- Run `uv run pytest` to verify tests pass

### Code Quality

Before completing work:
- Format with `uv run ruff format`
- Check linting with `uv run ruff check --fix`
- Verify types with `uv run ty check`
- Run all tests with `uv run pytest`
