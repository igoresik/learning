<sub><a href="../README.md">Back to the notebook</a></sub>

# Checking a bug report before trusting it, and testing the path users really take

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-python-dark.svg"><img src="../assets/tag-python.svg" alt="Python" height="22"></picture></a>&nbsp; **Date:** 2026-11-23

## What I built or changed

- Ran two read-only bug hunts, one on a paper-trading app and one on a reselling tracker, then checked every finding myself before changing anything.
- Fixed the ones that held up, each as a small pull request with a test:
  - the paper-trading app: live fills that never reached the window, a stock split paying a leftover fraction at the wrong price, a failed price download wiping stored history, and a download that was retried on every click
  - the reselling tracker: a slow tracking check overwriting a newer edit made in a second window
  - the ETL tool: numbers with many decimals changing on their way back from the sandbox
- Added a note to the order ticket, a test for the app's own live events, and a changelog for about forty fixes that had never been written down.
- Started the app on a throwaway database and drove it in the browser to see the real screens instead of only reading code.

## What I learned

- A bug report is only as current as the copy of the code it read. The reselling tracker report was written against a checkout 14 commits behind, and three of its four findings were already fixed. Fetch first, then read.
- Reading the provider's documentation can settle a "bug". A timestamp that looked like it lost its timezone is local time by design, so converting it would have made it wrong.
- A test can pass for the wrong reason. My first fix for the decimals checked the JSON file, but the app reads it back with pandas, whose default parser changed the last digits again. The test only counted once it went through the same read-back the app uses.
- The tool I use to run tests can import a different copy of the code than the one I am editing. An editable install pointed at another folder made a test pass without my fix, which a "does it fail without the change?" check exposed.
- Some behaviour is on purpose. An empty answer being retried was covered by an existing test, so the real fix was a short pause, not a different rule.
- Driving the real app is slow and has its own traps, like a confirm step that times out between my clicks. That was my test setup, not a bug, so I dropped that suggestion.

## Tools used

- Python, pytest, ruff, vitest and TypeScript
- FastAPI, SQLite, pandas, React, Streamlit
- Git worktrees, GitHub CLI and pull requests, a browser driven by script

## Link to the project repo

- [igoresik/schema-engine](https://github.com/igoresik/schema-engine) and [igoresik/reseller-tracker](https://github.com/igoresik/reseller-tracker)
- The paper-trading app is a private repository, not linked.
