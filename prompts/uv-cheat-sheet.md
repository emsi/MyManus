# uv Cheat Sheet – Modern Python Environment & Dependency Manager

## Installation and Upgrading **uv**

`curl -fsSL https://uv.dev/install.sh | bash`

or

`wget -qO- https://uv.dev/install.sh | bash`

* **Verify installation:** Run `uv version` to check the installed version (e.g., `uv 0.x.x`).
* **Upgrading uv:** If uv was installed with the standalone script, use the built-in self-updater:

  ```bash
  uv self update
  ```

  This re-runs the installer (set `INSTALLER_NO_MODIFY_PATH=1` to prevent shell profile changes). If installed via pip or another package manager, upgrade with that tool (e.g. `pip install --upgrade uv`).

## Initializing New Projects

* **Create a new project:**

  ```bash
  uv init <project-name>
  ```

  This command generates a new Python project in the current directory. By default this creates an *application* project with:

  * A `pyproject.toml` containing basic metadata (name, version, requires-python, etc.).
  * A sample Python file (e.g. `main.py`) and a `README.md`.
  * A `.python-version` file pinning the Python version for consistency.
* **Library projects:** Use:

  ```bash
  uv init --lib <name>
  ```

  for a library intended for distribution. This creates a `src/` layout with a package directory, an `__init__.py`, and a `py.typed` file for type hints. A build system is added to `pyproject.toml` so the project can be installed and published.
* **Packaged CLI apps:** Use:

  ```bash
  uv init --package <name>
  ```

  to create an application that will be packaged. This uses a `src/` layout and adds a `[project.scripts]` entry point in `pyproject.toml` (so it installs a console script). After initializing, you can run the app with:

  ```bash
  uv run <script-name>
  ```
* **Minimal project:** Use:

  ```bash
  uv init --bare <name>
  ```

  to create only a `pyproject.toml` (no code, README, or VCS setup). This skips creating the `.python-version` file, README, sample code, and doesn’t init a git repo. You can combine `--bare` with other flags (like `--lib` or `--build-backend`) if you want a minimal layout with just the configuration.

## Adding and Removing Dependencies

* **Add a dependency:**

  ```bash
  uv add <package>
  ```

  This command adds a package to your project’s dependencies (in `pyproject.toml`), similar to doing a `pip install` plus updating your configuration. For example:

  ```bash
  uv add httpx
  ```

  This will install the latest compatible version of **httpx** and add it to `[project.dependencies]` in `pyproject.toml`.

  * You can specify version constraints, e.g., `uv add "requests<3"`.
  * **Dev or optional dependencies:** Use `--dev` to add to development dependencies or `--group <name>` for a custom dependency group. For example, `uv add --dev pytest` adds **pytest** to the dev group. Use `--optional` to add to optional dependencies (extras).
  * You can also add direct URLs or VCS references. For example:

    ```bash
    uv add "mypkg @ git+https://github.com/user/mypkg.git"
    ```

    adds **mypkg** and records its Git source in `pyproject.toml`.
* **Remove a dependency:**

  ```bash
  uv remove <package>
  ```

  This command removes the package from `pyproject.toml` and uninstalls it from the environment. If the package is part of a specific group (like dev), use `--dev` or `--group <name>`.

## Installing & Syncing Dependencies

* **Sync project environment:**

  ```bash
  uv sync
  ```

  After adding or removing dependencies, this command installs the exact dependency set into your project’s virtual environment. It ensures that your environment matches the locked requirements, similar to using a requirements file.

* **Lockfile:** uv uses a lockfile (`uv.lock`) to record resolved dependency versions.

  * Running `uv lock` computes a fresh resolution of dependencies and updates the lockfile.
  * Use `uv lock --check` to verify that the lockfile is up-to-date with your project settings.
  * Use `uv lock --locked` to prevent automatic updates, and `uv lock --frozen` to use the lockfile without checking project metadata.

* **Install optional/groups:** By default, `uv sync` installs main project dependencies and the **dev** group. To install other groups or extras:

  * Use `uv sync --extra <extra_name>` to include a specific extra.
  * Use `--all-extras` to install all extras at once.
  * Use `--group <name>` to include a specific group, or `--no-group <name>` to exclude one. The special group `dev` is included by default but can be omitted with `--no-dev`.

* **Export requirements:**

  ```bash
  uv export --format requirements-txt > requirements.txt
  ```

  This exports the lockfile to a pip-compatible `requirements.txt`.

## Running Code in the uv Environment

* **Running project code:**

  ```bash
  uv run <command>
  ```

  This command runs commands within the project’s environment, ensuring the correct Python interpreter and dependencies are used. For example:

  * `uv run python main.py` runs your **main.py** using the project’s Python.
  * `uv run pytest` runs **pytest** (if it’s a dev dependency) inside the environment.
    uv will auto-sync the environment before executing the command.
* **Running any command:** You can pass any command (not just Python) to `uv run`. For example:

  ```bash
  uv run bash scripts/migrate.sh
  ```

  runs the **bash** script with the project’s dependencies available. If your project defines a console script in `[project.scripts]`, you can run it directly with `uv run <script-name>`.
* **Inline additional dependencies:**

  ```bash
  uv run --with <pkg> example.py
  ```

  This temporarily installs a package (like **rich**) for that specific run without permanently adding it to your project. You can specify version constraints if needed. Use `--no-project` to run the script in a completely isolated environment.

## Running Single-File Scripts with Inline Dependencies

uv supports self-contained Python scripts that declare their own dependencies:

* **Initialize an inline script:**

  ```bash
  uv init --script <script.py> --python <version>
  ```

  This creates a new script file with a shebang and an inline dependency metadata header.
* **Declare script dependencies:**

  ```bash
  uv add --script <script.py> <dependency>
  ```

  For example:

  ```bash
  uv add --script myscript.py requests<3 rich
  ```

  This adds **requests** and **rich** as dependencies, inserting a special commented TOML block at the top of the script.
* **Run the script:**

  ```bash
  uv run myscript.py
  ```

  uv detects the inline metadata and automatically creates an isolated environment with the specified dependencies.

  * To run the script without using the project’s environment, use:

    ```bash
    uv run --no-project myscript.py
    ```
* **Have the script written:**
Use sheebang and the file header to both call uv and declare dependencies. Here’s an example of a script header:
```python
#!/usr/bin/env -S uv run --script
# /// script
# requires-python = ">=3.12"
# name = ""
# version = "0.0.1"
# dependencies = [
#   "bs4",
#   "joblib",
#   "pandas",
#   "typer",
#   "openai",
# ]
# ///
```

## Using CLI Tools (Package CLIs) with uv

uv can manage Python CLI tools similar to pipx:

* **Run a tool without installing (ephemeral environment):**

  ```bash
  uv tool run <package> [args...]
  ```

  Alternatively, you can use the alias `uvx`. For example:

  ```bash
  uvx black --version
  ```

  The tool is downloaded (if not cached) and run; subsequent calls reuse the cached environment.
* **Install a tool (persistent):**

  ```bash
  uv tool install <package>
  ```

  This installs a tool persistently, making the executable available on your PATH. For example:

  ```bash
  uv tool install httpie
  ```

  List installed tools with `uv tool list` and remove one with `uv tool uninstall <name>`. If a tool’s command isn’t found, run `uv tool update-shell` to update your PATH.
* **Choosing versions:** By default, `uv tool install` and `uvx` use the latest version available. To specify a version, include it in the command (e.g. `uvx package==1.2.0`). Use `--upgrade` to upgrade an installed tool.

## Managing Python Versions with uv

uv can manage multiple Python interpreters:

* **List Python versions:**

  ```bash
  uv python list
  ```

  This shows installed Python versions as well as available versions. Use `--only-installed` to filter only those already installed.
* **Install a Python version:**

  ```bash
  uv python install <version>
  ```

  For example:

  ```bash
  uv python install 3.10.12
  ```

  This downloads and installs the specified Python interpreter. You can install multiple versions in one command.
* **Create a venv with a specific Python:**

  ```bash
  uv venv --python <version>
  ```

  This creates a new virtual environment in the current directory (by default at `.venv/`) using the specified Python.
* **Run with a specific Python:**

  ```bash
  uv run --python <version> <command>
  ```

  For example:

  ```bash
  uv run --python 3.10.9 python --version
  ```

  This runs the command using the specified Python interpreter. You can also specify alternative implementations (e.g. `pypy@3.8`).
* **Pin Python version for project:**

  ```bash
  uv python pin <version>
  ```

  This writes the version to a local `.python-version` file and updates `requires-python` in `pyproject.toml`.

## Updating Dependency Versions (Upgrading Packages)

* **Upgrade all dependencies:**

  ```bash
  uv lock --upgrade
  ```

  This updates all dependencies to their latest allowed versions and updates the lockfile. After running it, execute `uv sync` to install the new versions.
* **Upgrade a specific package:**

  ```bash
  uv lock --upgrade-package <name>
  ```

  For example:

  ```bash
  uv lock --upgrade-package django
  ```

  This updates only the specified package while respecting your version constraints.
* **Apply updated constraints:** If you edit version constraints manually, run:

  ```bash
  uv lock && uv sync
  ```

  to update and install changes.
* **Inspect dependency tree:**

  ```bash
  uv tree
  ```

  This displays the dependency graph and versions in a tree format.

## Advanced Usage Scenarios

* **Dependency Groups:**
  uv supports organizing dependencies into groups (e.g., dev, test, docs). For example:

  * `uv add --group test pytest` adds **pytest** under a custom group named "test".
  * The `dev` group is added by default with `uv add --dev <pkg>`. Use `uv sync --no-dev` to exclude the dev group in production.
* **Optional Dependencies (Extras):**
  Add an extra using `uv add --optional <pkg>` (or `-o`) to place it under `[project.optional-dependencies]`. These extras are installed only when requested with `uv sync --extra <name>` or `--all-extras`.
* **Workspaces (Monorepos):**
  uv can handle multi-project repositories via workspaces. Declare multiple projects in one top-level configuration. Dependencies between workspace members are resolved together and share a lockfile. Each workspace member typically gets its own virtual environment, but uv coordinates them.
* **Continuous Integration (CI):**
  In CI pipelines, use:

  ```bash
  uv sync --frozen
  ```

  to install dependencies exactly as locked. This ensures reproducible builds. You might also pre-install Python with `uv python install <version>` to ensure consistency.
* **Explicit Virtual Envs:**
  uv manages environments automatically (often creating a `.venv` for projects). To explicitly create one, run:

  ```bash
  uv venv
  ```

  If you need to remove a cached environment, use `uv cache clean`.
* **Using pip interface:**
  uv provides a `uv pip` command that mimics pip. For example, `uv pip install <pkg>` lets you use familiar pip commands while leveraging uv’s resolver. This helps integrate uv into existing workflows.

## Best Practices for Efficient **uv** Usage

* **Install uv in isolation:** Use pipx or another isolated method to prevent conflicts.
* **Commit your lockfile:** Always check in the `uv.lock` file to version control to ensure reproducibility.
* **Pin Python for projects:** Use `uv python pin` to lock the Python version and avoid “works on my machine” issues.
* **Use `uv run` and `uvx`:** Rely on these commands to execute scripts and tools within the correct environment without manual virtualenv activation.
* **Separate dev dependencies:** Keep development and testing dependencies in their own groups to streamline production environments.
* **Leverage uv’s speed:** uv is designed to add, remove, and upgrade dependencies quickly. Run `uv lock --upgrade` periodically to refresh versions.
* **Stay updated:** Keep uv updated and enable shell tab-completion to improve efficiency.

## Troubleshooting Common Issues

* **Dependency resolution errors:** If you see an error like “No solution found when resolving dependencies,” check your version constraints in `pyproject.toml` and update them as needed. Rerun `uv lock` after adjustments.
* **Build failures (C extensions):** For build errors (e.g., missing compiler or headers), ensure you have a C compiler installed (like build-essential on Debian/Ubuntu or Xcode Command Line Tools on macOS) and install any necessary development libraries.
* **Command not found / PATH issues:** If `uv` or an installed tool isn’t recognized, verify that your PATH is set correctly. For uv tools, run `uv tool update-shell` and restart your shell if needed.
* **Python version not available:** If uv cannot find a specified Python version, verify your `.python-version` file and ensure the interpreter is installed using `uv python list --only-installed` or install it with `uv python install <version>`.
* **Cache or environment issues:** If a tool or script isn’t picking up new versions, try running `uv cache clean` to clear uv’s caches and recreate a fresh environment.
* **“Module not found” errors:** Ensure all required dependencies are added via `uv add` and that you are running commands with `uv run` so that the project’s packages are available.
* **Getting help:** Use `uv --help` to view available commands and options. If you encounter bugs or persistent issues, check uv’s official documentation or community forums for assistance.
