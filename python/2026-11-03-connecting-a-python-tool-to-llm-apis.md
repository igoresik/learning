<sub><a href="../README.md">Back to the notebook</a></sub>

# Connecting a Python tool to LLM APIs with the user's own key

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-python-dark.svg"><img src="../assets/tag-python.svg" alt="Python" height="22"></picture></a>&nbsp; **Date:** 2026-11-03

## What I built or changed

- Built the provider layer for schema-engine, my local-first tool that has an LLM write Pandas cleaning code from a dataset's metadata.
- One interface with two clients: a local one for Ollama, and one that works with any OpenAI-compatible API (OpenAI and Groq). Users bring their own API key, and requests go straight to the provider.
- Added a `test-connection` command that sends a one-token request and shows latency, tokens used and the rate limit left.
- Wrote tests for all of it that run without keys or internet, so CI never needs secrets.

## What I learned

- If the rest of the code only talks to one interface, switching between a local model and a cloud one is a config change, not a code change.
- Settings can come from several places (flags, environment, `.env`, a config file, defaults). Pick one clear order and document it.
- Keep API keys out of anything that gets printed: hide them from object reprs, and mask them in command output.
- Only retry errors that can fix themselves (rate limits, server errors, network drops). Wait longer each time, respect the server's Retry-After, and cap the wait. Never retry a bad key.
- LLM APIs change fast: models get retired, parameters get renamed, and newer reasoning models can reject settings older ones accepted. Check the provider's docs and deprecation pages before picking defaults.
- HTTP code can be tested without a network: a mock transport returns scripted responses, and swapping in a fake sleep makes retry tests run instantly.

## Tools used

- Python, httpx, Typer
- pytest, pytest-cov, Ruff
- GitHub Actions, GitHub CLI

## Link to the project repo

- [schema-engine](https://github.com/igoresik/schema-engine)
