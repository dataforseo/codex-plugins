---
name: dataforseo-backlinks
description: Analyze backlink profiles, referring domains, individual links, and competitor link gaps with DataForSEO. Use for authority comparisons, link prospecting, lost-link research, spam checks, and backlink audits.
---

# DataForSEO Backlink Research

Call `api_request` directly with the templates below. Do not use
`docs_list_sections` or `docs_index` for these endpoints.

For domains and subdomains, remove protocol and `www.`. For a specific page,
use its absolute URL.

## Choose an endpoint

- Fast profile totals and health → Summary
- Best individual linking pages and anchors → Backlinks
- Best linking root domains → Referring Domains
- Domains linking to competitors but not the user's site → Domain Intersection

## Summary

`POST /v3/backlinks/summary/live`

```json
{
  "method": "POST",
  "path": "/v3/backlinks/summary/live",
  "data": [{
    "target": "example.com",
    "include_subdomains": true,
    "exclude_internal_backlinks": true,
    "backlinks_status_type": "live",
    "rank_scale": "one_hundred"
  }]
}
```

Only `target` is required. `backlinks_status_type` is `live` (default), `lost`,
or `all`. Use the same `rank_scale` (`one_hundred` or default `one_thousand`)
across comparisons.

## Backlinks

`POST /v3/backlinks/backlinks/live`

```json
{
  "method": "POST",
  "path": "/v3/backlinks/backlinks/live",
  "data": [{
    "target": "example.com",
    "mode": "one_per_domain",
    "include_subdomains": true,
    "exclude_internal_backlinks": true,
    "backlinks_status_type": "live",
    "filters": ["dofollow", "=", true],
    "order_by": ["domain_from_rank,desc"],
    "limit": 100,
    "rank_scale": "one_hundred"
  }]
}
```

`mode` is `as_is` (default), `one_per_domain`, or `one_per_anchor`. Limit
defaults to 100 and maxes at 1000. Use the returned `search_after_token` beyond
20,000 results and keep all other parameters identical.

## Referring Domains

`POST /v3/backlinks/referring_domains/live`

```json
{
  "method": "POST",
  "path": "/v3/backlinks/referring_domains/live",
  "data": [{
    "target": "example.com",
    "include_subdomains": true,
    "exclude_internal_backlinks": true,
    "backlinks_filters": ["dofollow", "=", true],
    "order_by": ["rank,desc"],
    "limit": 100,
    "rank_scale": "one_hundred"
  }]
}
```

Use `filters` for domain aggregates, such as `["backlinks", ">", 10]`.
Use `backlinks_filters` to constrain the source links used to calculate those
aggregates. Limit defaults to 100 and maxes at 1000.

## Domain Intersection (link gap)

`POST /v3/backlinks/domain_intersection/live`

Put competitors in `targets`; put the user's domain in `exclude_targets`.
`targets` is an object with string keys and supports up to 20 entries.

```json
{
  "method": "POST",
  "path": "/v3/backlinks/domain_intersection/live",
  "data": [{
    "targets": {
      "1": "competitor-one.com",
      "2": "competitor-two.com"
    },
    "exclude_targets": ["example.com"],
    "intersection_mode": "partial",
    "include_subdomains": true,
    "exclude_internal_backlinks": true,
    "backlinks_status_type": "live",
    "order_by": ["1.backlinks,desc"],
    "limit": 100,
    "rank_scale": "one_hundred"
  }]
}
```

`intersection_mode: partial` returns intersecting opportunities; `all`
merges results and is the default. `exclude_targets` accepts up to 10 entries.

## Output

For profile comparisons, report rank, backlinks, referring main domains,
dofollow/nofollow distribution, broken links, spam scores, and new/lost state.
For prospects, prioritize relevant domains with strong rank, live dofollow
links, reasonable spam score, and editorial anchors. Do not equate backlink
count with quality.
