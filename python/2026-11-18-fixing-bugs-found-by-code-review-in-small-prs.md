<sub><a href="../README.md">Back to the notebook</a></sub>

# Finding bugs with code reviews and fixing them one small, tested PR at a time

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-python-dark.svg"><img src="../assets/tag-python.svg" alt="Python" height="22"></picture></a>&nbsp; **Date:** 2026-11-18

## What I built or changed

- Ran a read-only code review of three of my own Python projects (a paper-trading app, an ETL tool and a reselling tracker). Each review returned a ranked list of findings, marked as confirmed or only plausible.
- Turned the findings into small pull requests, one fix per branch, each with a test:
  - the paper-trading app: a removed watchlist symbol stopped streaming, a fake crash in the equity curve on days with no price, order fills that stalled after one error, price history wiped by an empty answer from the data provider, a lock around the first model training, and an honest label on the backtest
  - the ETL tool: refuse to overwrite the input file, a plain message for files that can't be opened, a Windows jail for generated code, a byte-order mark so Excel reads accents in CSV output, and leading zeros in Excel input
  - the reselling tracker: confirm before removing a listing, a delivered parcel no longer undone by a late update, returned parcels counted as a loss with a Relist button, and a crash log that is no longer overwritten
- Left some findings alone on purpose, because on a closer look they were decisions about how the app should work, not bugs.

## What I learned

- A review is a list of claims, not facts. Two of them didn't reproduce (a damaged workbook already had a clear message, and text cells with leading zeros were already kept), so I fixed the documentation or skipped them instead of changing working code.
- Some "bugs" are design choices, such as whether a flat commission may exceed a tiny sale. I note those and leave the decision to the owner.
- A fix needs a test that fails without it. Checking by switching the fix off showed that two of my first security tests passed for the wrong reason.
- Reproduce first, then fix. For the Excel zeros I built a real workbook with openpyxl, and only the numeric cells with a zero-padded format turned out to be affected.
- Run exactly what CI runs before merging. I ran the tests but not the linter, so the ETL tool's main branch went red on line length until a follow-up PR fixed it.
- Use a separate Git worktree per fix, so unfinished work in the main folder is never touched, and run tests against that worktree's code, not an editable install of another checkout.
- Keep each PR small and say in it what it does not cover, so reviewing and merging stays easy.
- A review run by a tool can print a secret from a local file into its own output. Tell the tool what never to read or print, and rotate a key if it shows up anyway.

## Tools used

- Python, pytest, ruff
- FastAPI, SQLite, pandas, openpyxl, Streamlit (the three projects)
- Git worktrees, GitHub CLI and pull requests

## Link to the project repo

- [igoresik/schema-engine](https://github.com/igoresik/schema-engine) and [igoresik/reseller-tracker](https://github.com/igoresik/reseller-tracker)
- The paper-trading app is a private repository, not linked.
