# Contributing to pymatviz

Community contributions are very welcome! Whether it's reporting a bug, proposing a feature, or submitting code, your input is valuable.

## Reporting Issues & Requesting Features

- Use the [GitHub Issues](https://github.com/janosh/pymatviz/issues) to report bugs or suggest new features.
- For bug reports, please include:
  - A clear description of the issue and the package version you are using.
  - Steps to reproduce the bug (including a minimal code example).
  - What you expected vs. what actually happened.
  - Any relevant error messages or tracebacks.

## Contributing Code via Pull Requests

We strive for a quick turnaround on pull requests (PRs) for bug fixes and new features.

### Workflow

1. Fork the repository, clone your fork (`git clone https://github.com/YOUR-USERNAME/pymatviz.git`), and enter it (`cd pymatviz`).
1. Create a new branch for your changes (`git checkout -b your-feature-name`).
1. Follow the local setup instructions below before making changes.
1. Make code changes.
1. Add tests for any new functionality and fixes.
1. Update docs if necessary.
1. Push your branch to your fork (`git push origin your-feature-name`).
1. Open a Pull Request against the `main` branch of `janosh/pymatviz`.
1. Your PR must pass all automated checks (tests, linting, code coverage) that run in our GitHub Actions workflows before it can be merged.

### Local setup

Install [uv](https://docs.astral.sh/uv/getting-started/installation/) and [Node.js 24](https://nodejs.org/en/download), the Node version used by the lint workflow. `pymatviz` requires Python 3.12 or newer; the commands below use Python 3.12 to match the test workflow. `uv` will download it if needed.

From the root of your cloned repository, create and activate a virtual environment:

```sh
uv venv --python 3.12
source .venv/bin/activate
```

On Windows PowerShell, activate with `.venv\Scripts\Activate.ps1` instead. Reactivate the environment when opening a new terminal.

Install the package in editable mode, the test extras used in CI, and the development tools (including `prek`):

```sh
uv pip install -e '.[test,brillouin,export]' --group dev
```

Install the site dependencies even for Python-only changes: the shared hooks invoke `pnpm` and `vp staged`. Use [Corepack](https://github.com/nodejs/corepack#how-to-install) to select the pnpm version pinned in `site/package.json`. If your Node installation does not include Corepack, install it using the linked instructions first. Setting that version as Corepack's default also lets hooks invoke it from the repository root, which has no `package.json`.

```sh
corepack enable
cd site
corepack install --global "$(node -p 'require("./package.json").packageManager')"
pnpm install
cd ..
prek install
```

We use [prek](https://prek.j178.dev/quickstart/) to run the hooks in `.pre-commit-config.yaml`, including its built-in checks, rather than the `pre-commit` package. `prek install` installs both the `pre-commit` and `commit-msg` Git hooks. If you previously ran `pre-commit install`, use `prek install --overwrite` to replace those hooks.

### Running checks and tests

Hooks run automatically when you commit. To check all files manually:

```sh
prek run --all-files
```

Hooks may modify files; review and stage any fixes before committing again. The first run downloads the hook environments and can take longer.

Install Chromium for the browser-based tests, then run the Python tests from the repository root:

```sh
playwright install chromium
pytest
```

On Linux, use `playwright install --with-deps chromium` if browser system dependencies are missing; this may require administrator privileges. For a quicker targeted check, pass a test file, for example `pytest tests/test_pkg.py`.

By contributing, you agree that your contributions will be licensed under the same [MIT License](license) that covers the project. Thanks for contributing to `pymatviz`!
