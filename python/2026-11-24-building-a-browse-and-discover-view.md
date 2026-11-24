<sub><a href="../README.md">Back to the notebook</a></sub>

# Building a browse-and-discover view: one cached data call, easy navigation and tests with a fake data source

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-python-dark.svg"><img src="../assets/tag-python.svg" alt="Python" height="22"></picture></a>&nbsp; **Date:** 2026-11-24

## What I built or changed

- Added a Discover view to the paper-trading app, so a stock can be found without knowing its ticker.
  - About 150 US stocks and ETFs in twelve categories, each card showing the price, the change today, over a month and over a year, and a small month chart.
  - A category sidebar, a filter box, a stocks/ETFs switch and sort buttons (popular, gainers, losers, 1 month, 1 year).
  - One click to add a stock to the watchlist or to open its chart.
  - Stocks I already watch show the model's call when it is clear.
- Behind it, one new endpoint. It makes a single batched price download for the whole list and reuses it for ten minutes. If Yahoo fails, it serves the last good copy instead of an empty page.
- Tests: seven backend tests and eight screen tests, all using a fake data source. Then I ran the app on a throwaway database with live prices and clicked through it.
- Fixed a merge conflict with another pull request that had added tests to the same file, keeping both sides.

## What I learned

- One batched call for a whole list is far cheaper than a call per card. Cache it, and sort and filter in the browser, because a few hundred rows is tiny.
- A feature that can fail needs three screens, not one: loading, "can't reach the data" with a retry button, and "nothing matches". They took as much care as the happy path.
- Easy navigation is a design job: categories you can scan, a keyboard shortcut for the filter box, a button that says what it does, and a visible "Watching" state so nothing is added twice.
- Falling back to the last good data is better than failing, but only if the screen still shows when the data was last updated.
- Putting the download behind one small function lets tests swap in a fake, so none of them touch the network. A test that proves the cache works asserts how many times the fake was called.
- A hand-made list goes stale: a ticker can change or disappear, and a company can be bought. Leaving out symbols the data source no longer knows keeps the view working without a fix.
- Working in a separate git worktree let me build the feature while other branches in the same repository were still open, and I removed it afterwards.

## Tools used

- Python, FastAPI, pandas, yfinance, pytest
- React, TypeScript, TanStack Query, Tailwind CSS, vitest, React Testing Library
- Git worktrees, GitHub CLI and pull requests, a browser driven by script

## Link to the project repo

- [igoresik/paper-trader](https://github.com/igoresik/paper-trader)
