# A small data app with Python, SQLite and Streamlit (Logistics Tracker)

**Date:** 2026-10-13

## What I built or changed

- Started a tracker for reselling clothes: one Streamlit app with three tabs, backed by a local SQLite file.
- Inventory: a form to log a clothing listing (title, brand, size, platform, cost and list price) and a table of the active ones.
- Profit: pick a listing, enter the buyer's counteroffer and the shipping I pay, and see the platform fee, net profit and margin update straight away. The fee rates are stored per platform and can be edited in the app.
- Shipping: marking a listing as sold opens a shipment, and its status, carrier and tracking number can be updated afterwards.
- Split the code by job: `schema.sql` for the tables, `db.py` for the queries, `finance.py` for the calculation and `app.py` for the screen.
- Unit tests for the profit calculation, and a scripted run through the whole flow (add, offer, sold, shipped) without opening a browser.
- Looked at which marketplaces let an app connect to a seller account. Only eBay is open to anyone; Vinted and Depop have official seller APIs but only for approved businesses or partners.
- Added tracking lookup through the Ship24 API: one button asks for the parcel's events and updates the shipment status from them.
- Added an eBay connection: sign in through eBay's consent page, then a read-only tab with active listings, Best Offers and recent orders.
- A Settings tab where each person who runs the app enters their own keys, saved only in their local database.
- Gave the app its own look: a colour and font theme, a headline that sums up the stock in one sentence, the profit breakdown drawn as a till receipt, and a progress bar for each parcel.
- Packaged it as one Windows exe: double-click, and the app opens in its own window instead of a browser tab. Closing the window stops it.

## What I learned

- Streamlit runs the whole script again on every click. A form holds its inputs until the submit button, and after writing to the database the script has to be rerun so the tables show the new data.
- A widget remembers its value by its key. Giving the offer input a key per listing makes it start again from that listing's price.
- SQLite does not enforce foreign keys unless `PRAGMA foreign_keys = ON` is set on each connection.
- The database can guard the data itself: `CHECK` keeps a status inside a fixed list, and `UNIQUE` on the listing id means one shipment per listing.
- Two writes that belong together (mark as sold, create the shipment) go in one transaction, so there is never a sold item with no shipment.
- `with connection:` in Python's sqlite3 commits or rolls back but does not close the connection. `contextlib.closing` does that.
- A percentage fee is taken from the price the buyer actually pays, so a counteroffer lowers the fee as well as the profit. Margin is the net profit as a share of that sale price.
- Keeping the calculation as a plain function, away from the screen code, makes it easy to test with `unittest`.
- Streamlit's `AppTest` can fill in inputs and click buttons from a script, which is enough to check a whole flow.
- An API key in a public repo is a leaked key. For an app other people download, each user brings their own key and it stays in a file that git ignores.
- How the OAuth authorization code flow works: the user agrees on the provider's page, the app gets a short-lived code, swaps it for a refresh token, and uses that to get a fresh access token when needed. The app never sees the password.
- An "official API" is not always a public one. Check who is allowed to use it before planning around it, and do not build on scrapers that break a site's terms.
- Changing the schema of a database that already has data needs a small migration (`ALTER TABLE ... ADD COLUMN`), because `CREATE TABLE IF NOT EXISTS` leaves an old table as it is.
- Streamlit is themed from `.streamlit/config.toml` (colours, radius, fonts loaded from a URL), with no CSS needed for the standard widgets. Custom pieces go in with `st.html` and a small stylesheet.
- Streamlit's own CSS can win over mine; a more specific selector fixes it. Text put into custom HTML has to be escaped first.
- Look at the result, not only the code: the screenshots showed a "-0.00", raw "None" cells and two different euro formats that the tests did not catch.
- A Streamlit app is a web server, so a "desktop app" version still runs that server inside. PyInstaller bundles Python and the app into one exe, a small launcher starts the server, and Edge or Chrome in `--app` mode shows it as a window with no browser bars.
- A one-file exe unpacks to a temporary folder on every start, so user data must live somewhere else (`%APPDATA%`), or it is lost.
- By default the Streamlit server listens on the whole network. An app that shows saved keys should be bound to `127.0.0.1` so only the same computer can open it.
- Code that calls an outside API can be tested with fake responses (`unittest.mock`), but that only proves my code handles the shape I expected, not that the real API answers that way.

## Tools used

- Python 3.14, venv
- Streamlit
- SQLite (sqlite3)
- requests, Ship24 Tracking API, eBay APIs (OAuth, Trading, Fulfillment)
- PyInstaller
- unittest, unittest.mock, Streamlit AppTest

## Link to the project repo

- https://github.com/igoresik/logisticstracker

## Screenshot (optional)

<!-- ![Description](screenshot.png) -->
