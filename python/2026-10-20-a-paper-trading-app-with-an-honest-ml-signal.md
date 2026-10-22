<sub><a href="../README.md">Back to the notebook</a></sub>

# A paper trading desktop app with an honestly tested machine-learning signal

<a href="./"><picture><source media="(prefers-color-scheme: dark)" srcset="../assets/tag-python-dark.svg"><img src="../assets/tag-python.svg" alt="Python" height="22"></picture></a>&nbsp; **Date:** 2026-10-20 (versions 1.0.0, 1.1.0 and 1.2.0 released 2026-10-21, 1.3.0 and 1.4.0 on 2026-10-22)

## What I built or changed

- A Windows desktop app for practising trades with fake money against live US market prices: a Python backend (FastAPI) with a React interface shown in its own window, shipped as an installer.
- A money ledger in SQLite: portfolios, market, limit and stop orders, positions and an audit log of every fill.
- A market data pipeline: price history from Yahoo Finance cached in the database, plus a live price stream that drives the charts and fills waiting orders.
- A model that guesses whether the next day closes up, down or flat, tested the way trading models should be: walk-forward, against simple rules, and as a strategy against buying and holding.
- A trading interface: charts from 1-minute to weekly bars with indicators, an order ticket, portfolio views and a "Model lab" where the model's settings can be tested.
- Packaging and delivery: a one-folder build turned into a per-user installer, a Docker image of the same app, and GitHub Actions running the tests, a Docker smoke test and the release build.
- A review pass before 1.0.0: code and security reviews of the whole app, each fix in its own pull request with a test, and stock-split handling across the ledger, the charts' history and the model's record.
- Version 1.1: a signal that admits when it has no edge, a separate reading for how big the next move will be, a report card that scores the trader the way the model is scored, a history replay mode for practising on past days, and what a public release needs (a license, a changelog, an update check, a clear message when the data source is down).
- Version 1.2: suggestions from an outside review of the model, each tested on decades of real data before it went in: inputs measured in units of volatility, dropping inputs that don't help, sizing positions by the model's confidence, and a multi-day "which barrier first" target.
- Version 1.3, after the repo went public: short selling with margin and an automatic buy-in, component tests for the React interface, dependency and secret scanning on GitHub, a copy of the database before each upgrade, and a fix for a test that failed now and then.
- Version 1.4: a Model lab backtest that holds less money before the days the model expects big moves, tested on several funds with its rule fixed in advance.

## What I learned

- Money should be whole cents in integer columns, and every rounding should go against the trader, so the ledger can never create money. Keeping a position's total cost instead of a rounded average stops cents going missing over many trades.
- In SQLite, `BEGIN IMMEDIATE` takes the write lock before anything is read, so two orders can't both spend the same cash. `CHECK` constraints are a last safety net, and triggers can make a table append-only. Firing many orders from parallel threads is how to prove it holds.
- How limit and stop orders fill: at the limit or better, and at a worse price when the market gaps past a stop. Replaying the minutes the app missed lets orders fill correctly even when it was closed.
- Testing a prediction model on time series: a random train/test split leaks the future, so it has to be walk-forward (train on the past, test on the next block, roll on). It must be compared with naive baselines on the same days, and judged as a strategy after costs.
- An honest result is still a result: my model was only slightly better than simple rules at calling direction and earned far less than buying and holding. Indicators only help if they use past data alone, and a big gap between training and test scores means the model is memorising.
- Live data needs care: a WebSocket stream has to be re-subscribed and reconnected, and gaps filled from history. Reading a library's source showed me a reconnect bug I had to work around with my own small client.
- A local web server still needs protecting. Other websites can reach `127.0.0.1`, so the app checks the Host header (against DNS rebinding), takes only JSON bodies, and checks where requests come from: another site's page can still send a plain request with no body, so anything that changes data has to check its origin too.
- Market data and a ledger have to agree on corporate actions. Price history arrives already adjusted for stock splits, so on a split the held shares, open orders and stored prices must move to the new share count too. A ledger of whole shares pays out a leftover fraction in cash, the way brokers do.
- An app people have installed needs database upgrades, not a fresh schema: numbered upgrade steps run once each, so existing data survives, and testing them on a copy of a real database shows they do.
- Background work needs priorities: time-critical work like filling orders gets its own thread instead of queueing behind slow downloads and model training, and a burst of price ticks is matched in one go so the queue can't grow.
- A prediction only means something next to how often that outcome happens anyway. A 37%-sure "buy" that was right 33% of the time, when buy days came 30% of the time, is no edge, and a confidence interval (Wilson) shows that honestly even for small samples.
- An edge needs a range, not a single number: resampling the tested days in blocks (because neighbouring days move together) gives a likely range, and every extra test run on the same data is another chance to get lucky, so the range must widen with each one (Bonferroni).
- Splitting a prediction into its parts shows where the skill really is. The three-way model beat simple rules, but up against down alone it was right 52% of the time: its edge was telling quiet days from big ones. Big moves cluster, so move size is far more predictable than direction, and more history (30 years instead of 5) and market context like the VIX helped more than tuning the model.
- Labels that look several days ahead leak in new places: the last training rows before each test block see into it (so they're left out, "purging"), a "same as yesterday" baseline must use a label that is already known, and calls a day apart share most of their path, so they count as far fewer independent tries. My own first prototype got the baseline wrong, and the leak showed up as an impossible 35-point gap.
- A strategy has to be compared with the right "do nothing clever" version: sizing positions by confidence looked much better than going all in or all out, but almost all of that came from simply being in a rising market, which holding the same average amount every day also gets.
- Practising on history only teaches something if the future stays hidden: a replay needs its own clock, a chart cut off at that day, orders that fill on later days only, and model calls that were made out of sample.
- Review findings need checking before fixing: a quick test showed one reported risk was already blocked by the framework, while the real gap was smaller and different. Writing the failing test first proved each bug was real.
- Building a trading UI in React: server state in TanStack Query refreshed by live events, a small store for streaming quotes, and charts with TradingView Lightweight Charts. Colours are checked with a palette validator so up and down still read for colour-blind users.
- Shipping a Python desktop app: a pywebview window, opening the window before the heavy imports so the first start doesn't look frozen, a per-user Inno Setup installer that keeps the user's data on uninstall, and a Docker image for running it without the window.
- CI catches what local runs hide: the bare `pytest` command doesn't put the project on the import path, while `python -m pytest` does.
- How short selling and margin work, and how to fit them into a ledger: the sale's money is held as collateral with half as much again (the US 150% rule), and when the price rises until the collateral is only 130% of the cost of buying back, the position is bought back automatically. A short is a position below zero shares with a negative cost basis, and treating the buy-in as a buy stop let the existing live, catch-up and replay matching handle it with no new code paths.
- Risky actions should need clear intent: selling short takes its own flag, so a double-click or an out-of-date screen can't open a short by accident, and one order never flips a position from long to short.
- Testing a React interface with React Testing Library and jsdom, finding elements the way a screen reader does. Deliberately breaking the code shows whether tests are worth anything: my first ticket tests passed with the rounding broken, because round prices never round. Prices like $100.03 made every rounding show.
- Dependency alerts earn their keep: the first Dependabot scan of the now-public repo found a long list of known holes in one package. Checking how the package is used mattered as much as the fix: it was a dev-only icon tool that never shipped in the installer.
- A test that fails "sometimes" has a cause: this one called a real web service, but only during market hours. Forcing those conditions (market open, a slow reply) made it fail every time, which proved the fix.
- Tuning settings on one dataset mostly picks luck. Fixing the settings first and running them unchanged on funds the model was never tuned on separated the two: its skill at calling big moves held on every fund, while its up-or-down calls only worked where they had been chosen.
- Stepping back before predicted big moves isn't free: it lost to holding a steady amount on all four funds, because big moves go both ways and the days rated likeliest to move big were also the days the market rose most on average. Flipping the rule after seeing that would just fit it to this result.

## Tools used

- Python 3.14, FastAPI, uvicorn, SQLite (sqlite3), pandas, NumPy, scikit-learn, yfinance, websockets
- React 19, TypeScript, Vite, Tailwind CSS, Radix UI, TanStack Query, TradingView Lightweight Charts, Sonner
- pytest, Vitest, React Testing Library, jsdom
- pywebview (WebView2), PyInstaller, Inno Setup, Docker, GitHub Actions, Dependabot, GitHub secret scanning

## Link to the project repo

- https://github.com/igoresik/paper-trader
