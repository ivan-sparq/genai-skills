---
name: test-and-commit
description: Run uv-based test and lint loops until clean, then auto-commit and push changes. Use when the user wants a full test-lint-commit cycle automated.
---

# Test and Commit

## Instructions

Follow this workflow whenever this skill is invoked:

1. **Prepare**
   - Ensure the working directory is the project root.
   - Inspect the repository for the configured test and lint commands (default: `uv run pytest` for tests, `uv run ruff check` for linting). If another linter is configured (for example in `pyproject.toml` or CI configs), prefer that instead of `ruff`.

2. **Run tests until they pass**
   - Run: `uv run pytest`.
   - If tests pass, proceed to step 3.
   - If tests fail:
     - Analyze failures and error messages.
     - Make focused edits to fix the root causes.
     - Repeat `uv run pytest` after each fix.
     - Continue this loop until all tests pass.

3. **Run linters until they pass**
   - Determine the primary Python linter for this repo:
     - If `ruff` is configured, run `uv run ruff check`.
     - Otherwise, run the linter configured in the repo (for example: `uv run pylint`, `uv run flake8`, or a command defined in `pyproject.toml` or pre-commit).
   - If linting passes, proceed to step 4.
   - If linting fails:
     - Read linter output and fix issues directly in the codebase.
     - Prefer minimal, standards-compliant changes over broad rewrites.
     - Re-run the same linter command.
     - Continue this loop until linting is completely clean.

4. **Final verification test run**
   - Run `uv run pytest` one more time.
   - If any test fails at this stage:
     - Treat it as in step 2: fix the issues and repeat test + lint loops until both are green again.

5. **Commit and push**
   - Confirm there are no remaining test or lint failures.
   - Review `git status` to see changed and untracked files.
   - Stage only relevant project files (exclude artifacts, cache, and secrets).
   - Generate a concise, descriptive commit message that focuses on the *why* and groups related changes logically.
   - Create the commit.
   - Push the current branch to its upstream remote (for example: `git push` or `git push -u origin <branch>` if no upstream is set).

## Notes

- Respect existing project conventions (formatters, linters, commit message style, and branching model).
- Never commit secrets or machine-specific configuration.
- Do not create an empty commit if there are no changes after running tests and linters.
