# Contributing to OWL-Station

OWL-Station is a research-oriented project built around Optimal Wealth Laboratory (OWL).

## Development Setup

```bash
uv sync --extra dev
uv run pre-commit install
uv run pre-commit run --all-files
```


Here is a **clean, professional `CONTRIBUTING.md`** tailored exactly to the workflow you’ve built with **uv, extras, and pre-commit**. You can drop this in verbatim.

It explicitly explains:

* dev vs runtime installs
* why pre-commit behaves the way it does
* how contributors avoid the confusion you just worked through
* how to bypass hooks when appropriate

---

# Contributing to OWL-Station

Thank you for your interest in contributing to **OWL-Station**.
This document explains the development workflow, tooling expectations, and how to avoid common pitfalls.

OWL-Station is an **experimental design and orchestration framework** built on the **Optimal Wealth Laboratory (OWL)** solver. As such, we maintain a clear separation between **runtime dependencies** and **developer tooling**.

---

## Repository structure (high level)

* `src/` — OWL-Station source code
* `examples/` — mirrored examples from OWL (do not edit directly)
* `scripts/` — maintenance and sync utilities
* `pyproject.toml` — package metadata and dependency definitions
* `.pre-commit-config.yaml` — development-time safeguards

---

## Development vs Runtime Environments

OWL-Station distinguishes between:

### Runtime usage (end users)

This installs **only what is required to run OWL-Station**:

```bash
uv sync
```

This environment:

* does **not** include developer tools
* does **not** include `pre-commit`
* is appropriate for:

  * running the CLI
  * notebooks
  * Streamlit
  * Docker images
  * CI build stages

---

### Development usage (contributors)

This installs **developer tooling** such as `pre-commit` and `ruff`:

```bash
uv sync --extra dev
```

This environment is **required** if you intend to:

* modify code
* commit changes
* run formatting or linting hooks

> If you attempt to commit without dev dependencies installed, Git hooks will fail by design.

---

## Recommended developer setup

We recommend running this once per clone:

```bash
make dev
```

This is equivalent to:

```bash
uv sync --extra dev
```

After this, you should not need to think about extras again.

---

## Pre-commit hooks

OWL-Station uses **pre-commit** to enforce repository hygiene:

* formatting (ruff)
* whitespace cleanup
* merge conflict detection
* **blocking accidental commits of `.env` files and secrets**

Hooks run automatically on `git commit`.

---

### Manually running hooks (recommended)

Before committing, you can run all hooks explicitly:

```bash
uv run python -m pre_commit run --all-files
```

This:

* runs outside the commit process
* applies fixes where supported
* is the best way to test changes before committing

---

### When commits fail due to missing dev dependencies

If you see an error like:

```
No module named pre_commit
```

It means you are in a **runtime-only environment**.

Fix by running:

```bash
uv sync --extra dev
```

---

### Bypassing hooks (rare, advanced)

In exceptional cases (e.g., automated commits, emergency fixes), hooks can be bypassed:

```bash
git commit --no-verify
```

Use this sparingly.

---

## Secrets and credentials

### Never commit real secrets

The repository is configured to **block** commits of:

* `.env`
* `.env.local`
* `.env.production`
* similar files

Only the following is allowed:

```
.env.example
```

Use `.env.example` as a template and keep real credentials out of Git.

---

## Syncing OWL examples

Examples from the OWL project are mirrored into this repository.

To update them:

```bash
uv run python scripts/sync_owl_examples.py
```

These files are copied from the pinned OWL commit and **should not be edited directly**.

---

## Commit expectations

* Commits should be focused and descriptive
* Formatting and linting must pass
* Do not commit:

  * `.env`
  * credentials
  * large binary artifacts
* Do not modify mirrored OWL examples manually

---

## Questions or uncertainty

If something is unclear:

* open an issue
* ask in the PR
* describe what you expected vs what happened

Many of the tools used here (uv, pre-commit, Hydra) intentionally enforce good separation between **development** and **runtime** concerns. When something fails, it is usually a signal—not a bug.

---

## Summary

* Use `uv sync` for runtime usage
* Use `uv sync --extra dev` when contributing
* Pre-commit hooks are **developer safeguards**
* Production builds should never run `git commit`
* Secrets are blocked by policy

Thank you for contributing to OWL-Station.
