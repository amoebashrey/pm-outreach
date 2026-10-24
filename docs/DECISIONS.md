# Decisions

- 24 Oct 2026 — Use Algolia's `regions` and `industries` facet fields for M1 filtering. Live queries accepted `facetFilters` for `regions:India` and `industries:Consumer`, so filtering can happen on YC's server. Keep region values validated against a short allowlist as required by the project rules. Browser filtering was unnecessary for these fields.
- 24 Oct 2026 — Keep M0 limited to inspection and continuity docs. The app and original source remain unchanged while the baseline is recorded.

## M0 Algolia company record

One raw `Indemni` company hit contained these fields:

`id`, `name`, `slug`, `former_names`, `small_logo_thumb_url`, `website`, `all_locations`, `long_description`, `one_liner`, `team_size`, `industry`, `subindustry`, `launched_at`, `tags`, `tags_highlighted`, `top_company`, `isHiring`, `nonprofit`, `batch`, `status`, `industries`, `regions`, `stage`, `app_video_public`, `demo_day_video_public`, `app_answers`, `question_answers`, `objectID`, `_highlightResult`.

`regions` and `industries` are arrays. `_highlightResult` was present on the raw hit but not requested explicitly; its nested structure mirrors highlighted searchable values. The record's `regions` value was `United States of America`, `America / Canada`, `Remote`, `Partly Remote`; `industries` was `B2B`, `Supply Chain and Logistics`.

The live `Winter 2024` API call returned 248 company records. The raw record above was fetched directly through the same YC Algolia query path. Server-side facet checks: `batch:Winter 2024` + `regions:India` returned 4 hits; adding `industries:Consumer` returned 0; `batch:Winter 2024` + `industries:Consumer` returned 29.
