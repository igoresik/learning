<sub><a href="../README.md">Back to the notebook</a></sub>

# Turning a Python command-line tool into a Windows desktop app

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-python-dark.svg"><img src="../assets/tag-python.svg" alt="Python" height="22"></picture></a>&nbsp; **Date:** 2026-11-07

## What I built or changed

- Gave schema-engine a desktop app next to its command line, with the same engine underneath.
- A small local API (FastAPI) does the work, and a React interface in a native window (pywebview) talks to it.
- The app shows what the model will see about a file, runs the clean-up with each try appearing live, flags columns that lost values, and saves the cleaned file and the code.
- API keys entered in the app go into the system's password store, not a file, and the command line reads them from there too.
- Packaged it as an `.exe` with an installer, and CI builds the installer on every change and attaches it to each release.

## What I learned

- A packaged Python app has no `python.exe`, so anything that starts a separate Python process has to start the app itself in a special mode instead.
- A server that only listens on your own computer still needs to turn away requests that other websites could send from the browser.
- Moving shared logic out of the command line first meant the CLI and the app stayed one engine, not two copies.
- Testing the packaged build matters on its own: CI checks that the sandbox works inside the exe, not just in development.
- Trying the app with a real local model turned up a layout bug that the tests with a fake model never hit.

## Tools used

- Python, FastAPI, uvicorn, pywebview, keyring, PyInstaller, Inno Setup
- React, TypeScript, Vite, Tailwind CSS, Vitest
- pytest, Ruff, GitHub Actions, GitHub CLI, Ollama

## Link to the project repo

- [schema-engine](https://github.com/igoresik/schema-engine)
