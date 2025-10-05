# uv

**uv** is a high-performance, modern tool for Python project and dependency management, written in Rust.

At its core, uv unifies many of the roles played by tools like pip, virtualenv, pipx, Poetry, pyenv, and twine — so instead of mixing and matching different tools, you can use **uv** as a single, fast, all-in-one solution.

## npm → uv Command Cheatsheet

| **npm command**                   | **uv equivalent**              | **Notes**                                                    |
| --------------------------------- | ------------------------------ | ------------------------------------------------------------ |
| `npm init`                        | `uv init`                      | Create a new Python project (`pyproject.toml`).              |
| `npm install`                     | `uv sync`                      | Install all dependencies from `pyproject.toml` and lockfile. |
| `npm install <pkg>`               | `uv add <pkg>`                 | Add a package to project dependencies.                       |
| `npm install -D <pkg>`            | `uv add --dev <pkg>`           | Add a development-only dependency.                           |
| `npm uninstall <pkg>`             | `uv remove <pkg>`              | Remove a dependency from the project.                        |
| `npm update`                      | `uv sync --upgrade`            | Upgrade all dependencies to their latest allowed versions.   |
| `npm run <script>`                | `uv run <cmd>`                 | Run a command inside the project’s virtual environment.      |
| `npx <tool>`                      | `uvx <tool>`                   | Run a tool temporarily (no permanent install).               |
| `npm exec <tool>`                 | `uv tool run <tool>`           | Run a globally installed tool.                               |
| `npm install -g <tool>`           | `uv tool install <tool>`       | Install a command-line tool globally for the user.           |
| `npm list -g`                     | `uv tool list`                 | List all globally installed tools.                           |
| `npm update -g`                   | `uv tool upgrade --all`        | Upgrade all globally installed tools.                        |
| `npm uninstall -g <tool>`         | `uv tool uninstall <tool>`     | Remove a globally installed tool.                            |
| `npm ci`                          | `uv sync`                      | Sync environment exactly to lockfile (removes extras).       |
| `npm list`                        | `uv pip list`                  | List installed packages in the current environment.          |
| `npm view <pkg>`                  | `uv pip show <pkg>`            | Show detailed info about a specific package.                 |
| `npm outdated`                    | `uv pip list --outdated`       | Show which packages have updates available.                  |
| `npm audit`                       | `uvx pip-audit`                | Run a security audit of dependencies.                        |
| `npm cache clean`                 | `uv cache clean`               | Clear the local package cache.                               |
| `npm config set/get`              | `uv config`                    | Manage configuration values (limited scope).                 |
| `npm link`                        | `uv tool install --editable .` | Install current project as editable for CLI use.             |
| `npm pack`                        | `uv build`                     | Build source distributions and wheels in `dist/`.            |
| `npm publish`                     | `uv publish`                   | Publish a package to PyPI (supports tokens).                 |
| `npm login` / `logout` / `whoami` | _(no direct equivalent)_       | Use environment variables or flags for credentials.          |
| `npm version <bump>`              | _(manual)_                     | Update version manually in `pyproject.toml`.                 |
| `npm prune`                       | `uv sync`                      | Removes unlisted dependencies (exact sync).                  |

- **One-off tools:** `uvx black`, `uvx ruff`, `uvx pytest` — similar to `npx` usage.
- **Global installs:** Use `uv tool install <pkg>` and ensure your `~/.local/bin` is in `PATH`.
- **Lockfiles:** `uv.lock` ensures reproducible installs (like `package-lock.json`).

## `pyproject.toml` Schema Overview

### `[project]`

Defines the project’s core metadata — similar to `package.json` in npm.

| Key                     | Type           | Description                                                |
| ----------------------- | -------------- | ---------------------------------------------------------- |
| `name`                  | string         | Package name (must be unique on PyPI).                     |
| `version`               | string         | Current version of the package.                            |
| `description`           | string         | Short description of the project.                          |
| `readme`                | string / table | Path to README file or inline content.                     |
| `authors`               | list of tables | Each with `name` and optional `email`.                     |
| `license`               | string / table | License identifier or path to file.                        |
| `requires-python`       | string         | Minimum Python version (e.g., `>=3.10`).                   |
| `dependencies`          | list           | Runtime dependencies (e.g., `["requests>=2.0"]`).          |
| `optional-dependencies` | table          | Named groups of optional deps (e.g., `test = ["pytest"]`). |
| `keywords`              | list           | Search keywords for packaging.                             |
| `classifiers`           | list           | Trove classifiers describing compatibility.                |
| `urls`                  | table          | Links like homepage, repository, documentation.            |
| `scripts`               | table          | CLI entry points (`"cmd" = "module:function"`).            |

---

### `[build-system]`

Specifies how the project should be built and which backend to use.

| Key             | Type   | Description                                                                        |
| --------------- | ------ | ---------------------------------------------------------------------------------- |
| `requires`      | list   | Build-time dependencies (e.g., `["setuptools", "wheel"]`).                         |
| `build-backend` | string | The build backend module (e.g., `"setuptools.build_meta"` or `"hatchling.build"`). |

---

### `[tool.<toolname>]`

Tool-specific configuration — flexible and extensible.

| Example Tool | Section                     | Purpose                                                                   |
| ------------ | --------------------------- | ------------------------------------------------------------------------- |
| `uv`         | `[tool.uv]`                 | Project-specific settings for uv (e.g., indexes, groups).                 |
| `black`      | `[tool.black]`              | Code formatting preferences.                                              |
| `ruff`       | `[tool.ruff]`               | Linter configuration.                                                     |
| `pytest`     | `[tool.pytest.ini_options]` | Pytest runtime options.                                                   |
| `poetry`     | `[tool.poetry]`             | Poetry’s equivalent of `[project]` for dependency and package management. |

---

### `[tool.<toolname>.scripts]`

Defines command-line entry points managed by a tool.
Example:

```toml
[tool.uv.scripts]
start = "myapp.cli:main"
```

---

### `[tool.<toolname>.group.<name>]`

Used to declare dependency groups or environments.

```toml
[tool.uv.group.dev]
dependencies = ["pytest", "black"]
```

---

### `[project.scripts]` vs `[tool.<toolname>.scripts]`

- `[project.scripts]`: standard Python entry points (portable).
- `[tool...]`: tool-specific and may not apply outside that tool.
