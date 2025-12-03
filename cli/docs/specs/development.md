# Development

## Setting Up uv

This project is set up to use [uv](https://docs.astral.sh/uv/) to manage Python and
dependencies. First, be sure you
[have uv installed](https://docs.astral.sh/uv/getting-started/installation/).

Then [fork the jlevy/speculate repo](https://github.com/jlevy/speculate/fork) (having
your own fork will make it easier to contribute) and
[clone it](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository).

## Basic Developer Workflows

The `Makefile` simply offers shortcuts to `uv` commands for developer convenience.
(For clarity, GitHub Actions don’t use the Makefile and just call `uv` directly.)

```shell
# First, install all dependencies and set up your virtual environment.
# This simply runs `uv sync --all-extras` to install all packages,
# including dev dependencies and optional dependencies.
make install

# Run uv sync, lint, and test (and also generate agent rules):
make

# Build wheel:
make build

# Linting:
make lint

# Run tests:
make test

# Delete all the build artifacts:
make clean

# Upgrade dependencies to compatible versions:
make upgrade

# To run tests by hand:
uv run pytest   # all tests
uv run pytest -s src/module/some_file.py  # one test, showing outputs

# Build and install current dev executables, to let you use your dev copies
# as local tools:
uv tool install --editable .

# Dependency management directly with uv:
# Add a new dependency:
uv add package_name
# Add a development dependency:
uv add --dev package_name
# Update to latest compatible versions (including dependencies on git repos):
uv sync --upgrade
# Update a specific package:
uv lock --upgrade-package package_name
# Update dependencies on a package:
uv add package_name@latest

# Run a shell within the Python environment:
uv venv
source .venv/bin/activate
```

See [uv docs](https://docs.astral.sh/uv/) for details.

## Agent Rules

See [.cursor/rules](.cursor/rules) for agent rules.
These are written for [Cursor](https://www.cursor.com/) but are also used by other
agents because the Makefile will generate `CLAUDE.md` and `AGENTS.md` from the same
rules.

```shell
make agent-rules
```

## IDE setup

If you use VSCode or a fork like Cursor or Windsurf, you can install the following
extensions:

- [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)

- [Based Pyright](https://marketplace.visualstudio.com/items?itemName=detachhead.basedpyright)
  for type checking. Note that this extension works with non-Microsoft VSCode forks like
  Cursor.

## Releasing to PyPI

The CLI is published to PyPI as `speculate-cli` (users install with `pip install
speculate-cli` or `uv add speculate-cli`). The executable is named `speculate`.

**Versioning:** This project uses
[uv-dynamic-versioning](https://github.com/ninoseki/uv-dynamic-versioning/) to
automatically derive versions from git tags.
Since this is a monorepo, CLI tags use the `cli-v` prefix.

**Release process:**

```shell
# 1. Ensure all tests pass
make test

# 2. Create a git tag with the cli-v prefix
git tag cli-v0.1.0

# 3. Push the tag to GitHub
git push origin cli-v0.1.0

# 4. Create a GitHub release from the tag
#    Go to: https://github.com/jlevy/speculate/releases/new
#    Select the cli-v0.1.0 tag and publish the release

# 5. The publish.yml workflow will automatically:
#    - Run tests
#    - Build the package
#    - Publish to PyPI using trusted publishing (OIDC)
```

**Version format:**

- Tagged releases: `cli-v0.1.0` → version `0.1.0` on PyPI

- Development versions (between tags): `0.1.1.dev3+g1234567`

## Documentation

- [uv docs](https://docs.astral.sh/uv/)

- [basedpyright docs](https://docs.basedpyright.com/latest/)

* * *

*This file was built with
[simple-modern-uv](https://github.com/jlevy/simple-modern-uv).*
