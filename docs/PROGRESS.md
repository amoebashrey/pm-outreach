# Progress

## Done
- M0 — checked the original app, live API, Algolia record and email helpers; created this continuity log — 24 Oct 2026. Commit: `eb56954` (`M0: progress docs`).

## In progress
- None.

## Next
- M1 — rename the product and add YC batch, region and industry filters. Algolia supports server-side filters on `regions` and `industries`; use an allowlist for region values.

## Known issues
- The app works today. The current API response omits Algolia `regions`; it maps `subindustry` to `industry`.

## How to run
- `python3 serve.py` then open http://localhost:8765
- Python 3.9+; standard library only.
