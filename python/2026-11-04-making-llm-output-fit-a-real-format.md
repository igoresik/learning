<sub><a href="../README.md">Back to the notebook</a></sub>

# Making an LLM's output fit a real format, and testing it on local models

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-python-dark.svg"><img src="../assets/tag-python.svg" alt="Python" height="22"></picture></a>&nbsp; **Date:** 2026-11-04

## What I built or changed

- Added export presets to schema-engine: the model is told a target format (a Shopify product import or a SQLite table), and its output is checked before anything gets written.
- When the output doesn't fit, the problems go back to the model as another try, the same way code errors already did.
- Added SQLite output: one script with a `CREATE TABLE` and the rows, ready for `sqlite3 my.db < file.sql`.
- Ran the tool on real models for the first time, locally through Ollama, and fixed what that turned up.

## What I learned

- A format check works best in two parts: fixes that need no model happen in code, and only real mistakes go back to the model.
- Error messages sent to a model need the same privacy care as the prompt. They name columns and rows, never cell values.
- For an import format, read the platform's own docs for the exact column names, because formats change and old names can stay accepted for a while.
- Output from a library's dump function can depend on the library version. CI caught an older SQLite writing a value that couldn't be loaded back.
- Tests with a fake model prove the engine works, but only a real run shows how a model behaves. Small local models (7-8B) ran the whole loop but rarely wrote working code for messy data.
- Prompt changes need evidence: try them over a few runs and drop the ones that don't help.

## Tools used

- Python, pandas, sqlite3, Typer
- Ollama with llama3.1 and qwen2.5-coder
- pytest, Ruff, GitHub Actions, GitHub CLI

## Link to the project repo

- [schema-engine](https://github.com/igoresik/schema-engine)
