---
name: dataforseo-competitors
description: Find organic search competitors, inspect a domain's ranking keywords, and calculate competitor keyword gaps with DataForSEO. Use for SEO competitor discovery, visibility comparisons, ranking research, and content-gap analysis.
---

# DataForSEO Competitor Research

Call `api_request` directly with the templates below. Do not use
`docs_list_sections` or `docs_index` for these endpoints.

## Choose an endpoint

- Discover competitors from shared SERPs → Competitors Domain
- List keywords and positions for one site/page → Ranked Keywords
- Compare two domains or find missing keywords → Domain Intersection

Normalize domains by removing protocol and `www.`. Use the same location,
language, and item types across comparisons.

## Competitors Domain

`POST /v3/dataforseo_labs/google/competitors_domain/live`

Required: `target`, one location, and one language.

```json
{
  "method": "POST",
  "path": "/v3/dataforseo_labs/google/competitors_domain/live",
  "data": [{
    "target": "example.com",
    "location_name": "United States",
    "language_code": "en",
    "item_types": ["organic"],
    "exclude_top_domains": true,
    "max_rank_group": 100,
    "limit": 50,
    "order_by": ["metrics.organic.count,desc"]
  }]
}
```

`limit` defaults to 100 and maxes at 1000. `exclude_domains` accepts up to
1000 domains. Results include shared keyword intersections, average position,
and full organic/paid metrics such as keyword count and estimated traffic.

## Ranked Keywords

`POST /v3/dataforseo_labs/google/ranked_keywords/live`

Only `target` is required. A domain/subdomain omits protocol; an exact webpage
must be an absolute URL. Specify location and language for focused comparisons.

```json
{
  "method": "POST",
  "path": "/v3/dataforseo_labs/google/ranked_keywords/live",
  "data": [{
    "target": "example.com",
    "location_name": "United States",
    "language_code": "en",
    "item_types": ["organic"],
    "historical_serp_mode": "live",
    "limit": 100,
    "filters": [
      ["ranked_serp_element.serp_item.rank_group", "<=", 20],
      "and",
      ["keyword_data.keyword_info.search_volume", ">", 0]
    ],
    "order_by": ["keyword_data.keyword_info.search_volume,desc"]
  }]
}
```

`item_types` supports `organic`, `paid`, `featured_snippet`, `local_pack`, and
`ai_overview_reference`; default is organic plus paid. `historical_serp_mode`
is `live`, `lost`, or `all`. Limit defaults to 100 and maxes at 1000.

## Domain Intersection

`POST /v3/dataforseo_labs/google/domain_intersection/live`

Required: `target1`, `target2`, one location, and one language.

For keywords a competitor has but the user's domain lacks, put the competitor
in `target1`, the user's domain in `target2`, and set `intersections: false`.

```json
{
  "method": "POST",
  "path": "/v3/dataforseo_labs/google/domain_intersection/live",
  "data": [{
    "target1": "competitor.com",
    "target2": "example.com",
    "location_name": "United States",
    "language_code": "en",
    "intersections": false,
    "item_types": ["organic"],
    "limit": 100,
    "filters": ["keyword_data.keyword_info.search_volume", ">=", 100],
    "order_by": ["keyword_data.keyword_info.search_volume,desc"]
  }]
}
```

Set `intersections: true` for shared keywords. Limit defaults to 100 and maxes
at 1000. Leave `include_clickstream_data` off unless explicitly needed because
it doubles the request price.

## Output

Report the market settings first. For competitors, prioritize relevance
(intersections), organic keyword count, estimated traffic (`etv`), traffic
cost, and average position. For gaps, group keywords by intent/topic and
separate high-volume opportunities from terms where the competitor only ranks
weakly.
