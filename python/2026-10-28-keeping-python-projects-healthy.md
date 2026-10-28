<sub><a href="../README.md">Back to the notebook</a></sub>

# Keeping Python projects healthy with small, tested maintenance

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-python-dark.svg"><img src="../assets/tag-python.svg" alt="Python" height="22"></picture></a>&nbsp; **Date:** 2026-10-28

## What I built or changed

- Ran a health check over two of my apps (Paper Trader and Reseller Tracker): a linter, a spellchecker and a list of outdated packages.
- Fixed what was real: unused code and a few dependencies one patch version behind. Each fix went in as its own small pull request.
- Added CI to the project that had none, so its unit tests now run on every push and pull request.

## What I learned

- Linters and spellcheckers are a fast first pass, but many hits are false alarms (API names, tool keywords). Check each one before changing it.
- Even a patch bump needs proof: run the full test suite, and for screens with no tests, run the app once and check it starts without errors.
- Run that check on a copy of the data, so the real database is never touched.
- Package managers can quietly change how a version is written (npm adds `^` by default). Read the diff before committing.
- CI should run where the code really runs. Code that uses a Windows-only feature needs a Windows runner, or its tests prove nothing.

## Tools used

- pyflakes, codespell, pip, npm
- pytest, unittest, Streamlit AppTest, Vitest
- GitHub Actions, GitHub CLI

## Link to the project repo

- https://github.com/igoresik/paper-trader
- https://github.com/igoresik/reseller-tracker
