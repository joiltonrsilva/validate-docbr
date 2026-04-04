# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**validate-docbr** is a Python package for validating and generating Brazilian documents (CPF, CNPJ, CNH, CNS, PIS, Título Eleitoral, RENAVAM, Certidão). Published on PyPI as `validate-docbr`, currently at version 1.11.1.

## Language Conventions

- **Code** (class names, method names, variables, parameters): **English**
- **Comments, docstrings, documentation, commit messages, issues**: **Brazilian Portuguese (pt-BR)**
- **Claude files** (CLAUDE.md, agents, skills, commands): **English** (exception to the pt-BR rule)

## Commands

All commands run via Docker. Run `make build` first to set up the container.

```bash
make build            # Build Docker image (also installs git hooks)
make test             # Run all tests with pytest
make test-coverage    # Run tests with coverage (fail threshold: 97.50%)
make lint             # Run all linters (commit, markdown, dockerfile, yaml, shell, python)
make shell            # Open a bash shell in the container
```

To run a single test file or test inside the container:
```bash
docker compose run --rm -v $(pwd):/app app pytest tests/test_CPF.py
docker compose run --rm -v $(pwd):/app app pytest tests/test_CPF.py::TestCpf::test_mask
```

## Architecture

Every document type is a class inheriting from `BaseDoc` (abstract base class in `validate_docbr/BaseDoc.py`). `BaseDoc` defines the interface:

- `validate(doc)` — abstract, validates a document string
- `generate(mask)` — abstract, generates a valid document
- `mask(doc)` — abstract, applies formatting mask
- `validate_list`, `generate_list` — concrete, built on the abstract methods
- `_only_digits`, `_validate_input` — shared utility methods

Each document class (e.g., `CPF.py`, `CNPJ.py`) implements the three abstract methods with document-specific validation logic (check digits, length, masks).

`generic.py` provides `validate_docs()`, a standalone function that validates heterogeneous document lists.

## Adding a New Document

1. Create a class in `validate_docbr/` inheriting from `BaseDoc`, named by the document's acronym
2. Implement `validate`, `generate`, and `mask`
3. Export it in `validate_docbr/__init__.py`
4. Add tests in `tests/test_<Name>.py` (tests use `unittest`)

## CI/Pre-push

- **Pre-push hook** runs lint + test-coverage (installed via `make build` → `git config core.hooksPath .githooks`)
- **GitHub Actions** (`integration.yml`) runs lint then tests on PRs to `main`
- Coverage must stay at or above **97.50%**
