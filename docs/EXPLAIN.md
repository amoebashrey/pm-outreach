# Code tour (starter notes)

This is the start of the plain-English code tour. Milestone 6 will finish the full function-by-function guide and request flow.

## Files

- `index.html` — the complete browser interface, with inline CSS and JavaScript. It loads YC batches, company details and founder data; builds email drafts; stores settings and batch data in browser localStorage; and optionally calls Apify directly from the browser.
- `api/yc.py` — the server-side YC API used by Vercel and the local server. It looks up the Algolia key from YC, searches batches and companies, reads YC company pages, finds company-site emails, and guesses founder email patterns.
- `serve.py` — local standard-library web server. It serves the page and routes `/api/yc` requests to the same handler used on Vercel.
- `yc_scraper.py` — command-line batch scraper. It reuses the basic YC and email logic but retries requests and checks more company contact pages.
- `apify_enrich.py` — optional command-line email enrichment using the user's own Apify token. It writes local JSON and CSV outputs.
- `vercel.json` — gives the serverless API function a 60-second maximum runtime.
- `.gitignore` — excludes secrets, Python cache files, Vercel metadata and scraped CLI output.
- `.vercelignore` — excludes local output files and secrets from Vercel deployments.
- `README.md` — describes the app, how to run it, the API, deployment and data sources.
- `LICENSE` — the original MIT license; keep it unchanged.

## Main functions and what they do

### `api/yc.py`

- `get()` fetches a URL with a short timeout and returns its text, or `None` on failure.
- `algolia()` reads and caches YC's Algolia credentials, then sends a search request.
- `batch_key()` converts a batch label into a sortable year and season.
- `batches()` gets the YC batch list and sorts it newest first.
- `companies()` gets all companies in one validated batch and selects fields for the API response.
- `domain_of()` extracts a lowercase company domain from a website.
- `ascii_name()` turns a name into lowercase ASCII words for email patterns.
- `site_emails()` checks the company home page and contact page for same-domain email addresses.
- `guesses()` makes common email-pattern guesses from a founder name and domain.
- `resolves()` checks whether the company domain has DNS records.
- `open_jobs()` selects complete job postings from a YC company page.
- `founders()` fetches a company's YC page and returns founder details, emails, and open jobs.
- `route()` validates the requested action and parameters, then calls the matching search function.
- `handler.do_GET()` turns the route result into an HTTP JSON response.

### `serve.py`

- `Handler.do_GET()` sends `/api/yc` requests to `api/yc.py`, returns 404 for other API paths, and serves normal files otherwise.
- The main block changes to the project folder and starts the local HTTP server on port 8765 by default.

### `index.html`

- `store.get()` and `store.set()` read and write JSON values in localStorage.
- `$()`, `esc()`, `url()`, `sleep()`, and `say()` are small helpers for finding page elements, escaping text, allowing safe links, delaying, and showing status.
- `api()` calls the local or deployed YC endpoint and turns HTTP errors into readable JavaScript errors.
- `best()` selects the strongest available email for a founder; `contacts()` sorts founders by email quality and CEO title.
- `fill()` substitutes a person's and company's values into the draft template.
- `rolesLine()` and `jobsRow()` format open-role information.
- `render()` builds the company cards; `draft()` fills a card's subject/body and mail link; `update()` refreshes open drafts; `filter()` applies search, email, sent and hiring filters.
- `loadBatch()`, `loadMore()`, `needsFetch()`, `mergeCompany()`, `save()`, `busy()`, and `progress()` manage batch data and staged founder loading.
- `apify()`, `enrich()` run optional Apify lookups from the browser and save results with the batch.
- The remaining event handlers connect buttons, filters, template editing, exports and initial batch loading to those functions.

### `yc_scraper.py` and `apify_enrich.py`

These CLI scripts have their own small functions for HTTP requests, Algolia access, company processing, site email checks, email guesses and CSV/JSON output. The full line-by-line guide will be completed in M6.
