# Python Setup Skill for Claude Code

A Claude Code skill that teaches Claude how to set up Python projects following modern best practices with `uv`, `ruff`, `ty`, `pytest`, and automated CI/CD.

## What This Skill Does

This skill enables Claude to automatically:

- Set up Python projects with modern tooling (`uv`, `ruff`, `ty`, `pytest`)
- Configure pre-commit hooks for automated code quality checks
- Create GitHub Actions CI workflows
- Structure projects with proper directory layout (`src/`, `tests/`, `doc/`)
- Set up test coverage reporting
- Configure environment variable management
- Follow security best practices (no hardcoded secrets)

## Features

**Project Management:**
- Uses `uv` for fast, reliable dependency management
- Initializes projects with proper structure

**Code Quality:**
- `ruff` for lightning-fast linting and formatting
- `ty` for type checking
- `pytest` with coverage reporting
- Pre-commit hooks that run all checks before commits

**CI/CD:**
- GitHub Actions workflow for automated testing
- Runs linting, formatting checks, type checks, and tests on every push

**Best Practices:**
- Source code in `src/`, tests in `tests/`, docs in `doc/`
- Environment variables in `.env` (gitignored) with `.env.example` template
- Comprehensive `.gitignore` for Python projects

## Installation

### Option 1: Personal Installation (Available in All Projects)

Install this skill globally for your user account:

```bash
# Create the skills directory if it doesn't exist
mkdir -p ~/.claude/skills

# Copy the skill directory
cp -r skill ~/.claude/skills/python-setup
```

### Option 2: Project-Specific Installation

Install this skill for a specific project (shareable with your team):

```bash
# In your project directory
mkdir -p .claude/skills

# Copy the skill directory
cp -r skill .claude/skills/python-setup

# Commit to version control to share with your team
git add .claude/skills/python-setup
git commit -m "Add Python setup skill"
```

### Verify Installation

Once installed, Claude Code will automatically discover and use this skill when you ask it to set up Python projects. You can verify by asking Claude:

```
Can you set up a new Python project for me?
```

Claude should recognize the skill and follow the workflow defined in it.

## Usage

After installation, simply ask Claude Code to help with Python projects:

**Creating a new project:**
```
Create a new Python project called "my-api" that will be a REST API service
```

**Setting up an existing project:**
```
Set up this project with uv, pytest, and pre-commit hooks
```

**Adding specific features:**
```
Add GitHub Actions CI to this Python project
```

Claude will:
1. Ask for the project name and description (if not provided)
2. Initialize the project with `uv init`
3. Create the proper directory structure
4. Set up all development dependencies
5. Configure pre-commit hooks
6. Create GitHub Actions workflow
7. Add a basic test to verify everything works
8. Run all checks to ensure the setup is correct

## What Gets Created

When you use this skill to set up a project, you'll get:

```
your-project/
├── src/                    # Your source code
├── tests/                  # Test files
│   └── test_basic.py      # Initial test
├── doc/                    # Documentation
├── .github/
│   └── workflows/
│       └── ci.yml         # GitHub Actions CI
├── .pre-commit-config.yaml # Pre-commit hooks
├── .env.example           # Environment variable template
├── .gitignore             # Python-specific gitignore
├── pyproject.toml         # Project configuration
└── README.md              # Project readme
```

## Requirements

This skill assumes you have:
- Python 3.13 or later
- `uv` installed ([install instructions](https://docs.astral.sh/uv/))
- Git (for pre-commit hooks)

Optional but recommended:
- `ripgrep` (`rg`) for fast file searching
- `fd` (`fdfind`) for fast file finding

## Customization

You can customize the skill by editing `skill/SKILL.md`:

- Change default Python version
- Modify pre-commit hooks
- Update CI/CD workflow
- Add or remove development dependencies
- Adjust project structure

After making changes, the skill will automatically use your updated configuration.

## Resources

The skill includes templates in `skill/resources/`:

- `pre-commit-config.yaml` - Pre-commit hooks configuration
- `ci.yml` - GitHub Actions workflow
- `gitignore-template` - Python .gitignore template
- `env-example-template` - Environment variables template
- `pyproject-pytest-config.toml` - Pytest configuration
- `test-basic-template.py` - Basic test template

## Example Session

```
You: I need to create a new Python project called "weather-cli" that fetches weather data

Claude: I'll help you set up a new Python project called "weather-cli".
        Could you provide a short description (2-3 sentences) about what
        this project does?

You: It's a command-line tool that fetches current weather data from
     OpenWeatherMap API and displays it in a nice formatted output.

Claude: Perfect! I'll set up the project now...
        [Creates full project structure with all tooling]

        Your project is ready! Here's what I set up:
        - Project structure with src/, tests/, and doc/
        - uv for dependency management
        - ruff for formatting and linting
        - ty for type checking
        - pytest with coverage
        - Pre-commit hooks (will run before each commit)
        - GitHub Actions CI
        - httpx for HTTP requests
        - python-dotenv for environment variables

        I've also added a basic test. You can verify everything works:

        cd weather-cli
        uv run pytest

        All checks passed! You're ready to start coding.
```

## Contributing

This skill is open source. Feel free to:
- Submit issues for bugs or feature requests
- Create pull requests with improvements
- Fork and customize for your own workflow

## License

See [LICENSE](LICENSE) file for details.

## Changelog

### Version 1.0.0
- Initial release
- Support for uv, ruff, ty, pytest
- Pre-commit hooks configuration
- GitHub Actions CI workflow
- Project structure templates
