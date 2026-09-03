---
name: dataforseo-serp
description: Retrieve current Google organic results, rankings, domains, snippets, and SERP features with DataForSEO. Use for live rank checks, SERP analysis, AI Overview detection, People Also Ask research, and localized desktop or mobile search results.
---

# DataForSEO Live Google SERP

Use the verified endpoint below directly. Do not use `docs_list_sections` or
`docs_index`.

## Live Organic Advanced

`POST /v3/serp/google/organic/live/advanced`

Required: `keyword` and one of `location_name`, `location_code`, or
`location_coordinate`. Use `language_code` explicitly for reproducibility.

```json
{
  "method": "POST",
  "path": "/v3/serp/google/organic/live/advanced",
  "data": [{
    "keyword": "best crm software",
    "location_name": "United States",
    "language_code": "en",
    "device": "desktop",
    "os": "windows",
    "depth": 10
  }]
}
```

Important options:

- `depth`: default 10, max 200; billing scales by each block of up to 10
  results.
- `device`: `desktop` (default) or `mobile`.
- `os`: desktop supports `windows` (default) or `macos`; mobile supports
  `android` (default) or `ios`.
- `target`: return only matching result URLs. Use `example.com*` for all pages
  on a domain or `*example.com*` to include subdomains.
- `load_async_ai_overview`: loads asynchronous AI Overviews and adds cost.
- `people_also_ask_click_depth`: 1–4 and adds cost per click.
- `calculate_rectangles`: enables pixel positions and adds cost.
- `remove_from_url`: remove up to 10 tracking parameters, for example
  `["srsltid"]`.

For a rank check that may be beyond page one, avoid a large fixed `depth` when
a stopping target is known:

```json
{
  "method": "POST",
  "path": "/v3/serp/google/organic/live/advanced",
  "data": [{
    "keyword": "seo api",
    "location_name": "United States",
    "language_code": "en",
    "device": "mobile",
    "stop_crawl_on_match": [{
      "match_type": "with_subdomains",
      "match_value": "example.com"
    }],
    "max_crawl_pages": 5
  }]
}
```

`stop_crawl_on_match` accepts up to 10 targets. `match_type` is `domain`,
`with_subdomains`, or `wildcard`. The request is billed for pages crawled
through the target.

## Output

Inspect `tasks[0].result[0]`. Report `datetime`, location, language, device,
`check_url`, `se_results_count`, and notable `item_types`. For organic items,
use `rank_group` for rank among organic results and `rank_absolute` for visual
order among all SERP elements. Preserve featured snippets, local packs,
shopping, People Also Ask, and AI Overview items instead of flattening
everything into organic links.
