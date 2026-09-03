---
name: dataforseo-keywords
description: Discover, expand, and evaluate Google keywords with search volume, CPC, competition, difficulty, intent, trends, and SERP context. Use for keyword research, content planning, seed expansion, or website keyword discovery with DataForSEO.
---

# DataForSEO Keyword Research

Call `api_request` directly with the templates below. Do not use
`docs_list_sections` or `docs_index` for these endpoints.

## Choose an endpoint

- Metrics for a known keyword list → Keyword Overview
- Keywords relevant to a domain → Keywords For Site
- Broad ideas from seed topics → Keyword Ideas

Use `location_name` plus `language_code` unless the user provides codes.
Ask for the target market if it materially changes the answer. Keep
`include_clickstream_data: false`; enabling it doubles the request price.

## Keyword Overview

`POST /v3/dataforseo_labs/google/keyword_overview/live`

Required: `keywords` (1–700), one location, and one language. Each keyword is
limited to 80 characters and 10 words.

```json
{
  "method": "POST",
  "path": "/v3/dataforseo_labs/google/keyword_overview/live",
  "data": [{
    "keywords": ["crm software", "sales crm"],
    "location_name": "United States",
    "language_code": "en",
    "include_serp_info": true,
    "include_clickstream_data": false
  }]
}
```

Set `include_serp_info` only when SERP features, current relevant URLs, or
top-result backlink averages are useful. Results also include intent without a
separate intent request.

## Keywords For Site

`POST /v3/dataforseo_labs/google/keywords_for_site/live`

Required: `target` without protocol and one location. Language is optional but
should be explicit for comparable research.

```json
{
  "method": "POST",
  "path": "/v3/dataforseo_labs/google/keywords_for_site/live",
  "data": [{
    "target": "example.com",
    "location_name": "United States",
    "language_code": "en",
    "include_subdomains": true,
    "limit": 100,
    "filters": ["keyword_info.search_volume", ">", 0],
    "order_by": ["relevance,desc"]
  }]
}
```

Options: `include_subdomains` defaults to `true`; `limit` defaults to 100 and
maxes at 1000; `offset` defaults to 0. Use the returned `offset_token` for
large pagination, passing only that token and `limit` on the next request.

## Keyword Ideas

`POST /v3/dataforseo_labs/google/keyword_ideas/live`

Required: `keywords` (up to 200) and one location.

```json
{
  "method": "POST",
  "path": "/v3/dataforseo_labs/google/keyword_ideas/live",
  "data": [{
    "keywords": ["crm software", "sales automation"],
    "location_name": "United States",
    "language_code": "en",
    "closely_variants": false,
    "ignore_synonyms": true,
    "limit": 100,
    "filters": ["keyword_info.search_volume", ">=", 100],
    "order_by": ["keyword_info.search_volume,desc"]
  }]
}
```

`closely_variants: true` uses phrase-match behavior; `false` uses broad match.
`ignore_synonyms: true` removes highly similar terms. Limit defaults to 700 and
maxes at 1000.

## Filters and output

Filters support up to eight conditions. Combine conditions as:

```json
[["keyword_info.search_volume", ">=", 100], "and",
 ["keyword_properties.keyword_difficulty", "<=", 40]]
```

Prioritize `keyword`, `keyword_info.search_volume`, `keyword_info.cpc`,
`keyword_info.competition_level`, `keyword_properties.keyword_difficulty`,
`search_intent_info.main_intent`, and `keyword_info.monthly_searches`.
Separate paid competition from organic keyword difficulty in the explanation.
