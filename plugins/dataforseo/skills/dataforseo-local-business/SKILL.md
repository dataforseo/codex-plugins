---
name: dataforseo-local-business
description: Search Google Maps business listings and analyze local competitors with DataForSEO. Use for local SEO, lead generation, market sizing, ratings, reviews, categories, contact details, claimed profiles, and geographic business research.
---

# DataForSEO Local Business Research

Use the verified endpoint below directly. Do not use `docs_list_sections` or
`docs_index`.

## Business Listings Search

`POST /v3/business_data/business_listings/search/live`

All search fields are optional, but constrain every request by business intent
and geography. `location_coordinate` uses
`"latitude,longitude,radius_in_km"` with a radius from 1 to 100000.

```json
{
  "method": "POST",
  "path": "/v3/business_data/business_listings/search/live",
  "data": [{
    "categories": ["pizza_restaurant"],
    "location_coordinate": "40.7128,-74.0060,10",
    "filters": ["rating.votes_count", ">=", 20],
    "order_by": ["rating.value,desc", "rating.votes_count,desc"],
    "limit": 100
  }]
}
```

Search options:

- `categories`: up to 10 Google business categories.
- `title`: business name, up to 200 characters.
- `description`: description text, up to 200 characters.
- `is_claimed`: filter owner-verified profiles.
- `filters`: up to eight conditions.
- `order_by`: up to three rules.
- `limit`: default 100, max 1000.
- `offset`: default 0; use the returned `offset_token` for large pagination
  and keep other request parameters identical.

If the exact category identifier is unknown, use `title` or `description`
instead of inventing a category. Use `docs_search` directly for the categories
endpoint only when exact category coverage is essential.

## Useful filters

```json
["rating.value", ">=", 4]
```

```json
[
  ["rating.value", ">=", 4],
  "and",
  ["rating.votes_count", ">=", 50]
]
```

Use exact response field names when adding other filters.

## Output

Prioritize business name, primary/additional categories, address, distance or
coordinates, domain, phone, claimed state, rating, review count, hours, price
level, and last update time. Deduplicate by `place_id`, then `cid`, then
normalized title/address. Distinguish missing data from negative values, and
do not infer that unclaimed profiles are inactive businesses.
