---
name: dataforseo-onpage
description: Audit a webpage or crawl a website for technical and on-page SEO issues with DataForSEO. Use for metadata checks, indexability, broken links, redirects, duplicate content, performance, Core Web Vitals, and prioritized site-audit findings.
---

# DataForSEO OnPage Audits

Call `api_request` directly with the verified workflows below. Do not use
`docs_list_sections` or `docs_index`.

## Choose a workflow

- One URL, immediate result → Instant Pages
- Multiple pages or full-site issues → create crawl, poll summary, fetch pages

JavaScript, browser rendering, resource loading, and keyword density can add
cost. Leave them disabled unless the requested audit needs them.

## One-page audit

`POST /v3/on_page/instant_pages`

Required: absolute `url`.

```json
{
  "method": "POST",
  "path": "/v3/on_page/instant_pages",
  "data": [{
    "url": "https://example.com/page",
    "accept_language": "en-US",
    "enable_browser_rendering": false,
    "load_resources": false,
    "store_raw_html": false,
    "check_spell": false
  }]
}
```

To collect Core Web Vitals, set `enable_browser_rendering: true`; Instant Pages
then enables JavaScript and resources automatically. `browser_preset` can be
`desktop`, `mobile`, or `tablet` when rendering is enabled.

## Site crawl

### 1. Create task

`POST /v3/on_page/task_post`

Required: protocol-free `target` and `max_crawl_pages`.

```json
{
  "method": "POST",
  "path": "/v3/on_page/task_post",
  "data": [{
    "target": "example.com",
    "max_crawl_pages": 100,
    "max_crawl_depth": 3,
    "accept_language": "en-US",
    "allow_subdomains": false,
    "respect_sitemap": true,
    "crawl_sitemap_only": false,
    "load_resources": false,
    "enable_javascript": false,
    "enable_browser_rendering": false
  }]
}
```

Save `tasks[0].id`. `crawl_delay` defaults to 2000 ms. To crawl one specific
page asynchronously, set `start_url` to its absolute URL and
`max_crawl_pages: 1`. Browser rendering requires JavaScript and resources.

### 2. Poll summary

`GET /v3/on_page/summary/{id}` with no request body:

```json
{
  "method": "GET",
  "path": "/v3/on_page/summary/TASK_ID"
}
```

Use a bounded wait between checks. Continue when
`tasks[0].result[0].crawl_progress` is `finished`; report
`extended_crawl_status` if it is not `no_errors`.

### 3. Fetch affected pages

`POST /v3/on_page/pages`

```json
{
  "method": "POST",
  "path": "/v3/on_page/pages",
  "data": [{
    "id": "TASK_ID",
    "limit": 100,
    "filters": [
      ["resource_type", "=", "html"],
      "and",
      [
        ["checks.is_broken", "=", true],
        "or",
        ["checks.no_title", "=", true]
      ]
    ],
    "order_by": ["page_timing.duration_time,desc"]
  }]
}
```

`id` is required. Limit defaults to 100 and maxes at 1000. Filters support up
to eight conditions. Use the returned `search_after_token` for deep pagination
and keep all other fields identical.

Common check fields include `is_4xx_code`, `is_5xx_code`, `is_redirect`,
`redirect_chain`, `no_title`, `title_too_short`, `title_too_long`,
`no_description`, `duplicate_title_tag`, `no_h1_tag`, `no_image_alt`,
`canonical_to_broken`, `canonical_to_redirect`, `is_orphan_page`,
`high_waiting_time`, `high_loading_time`, and
`has_render_blocking_resources`.

## Output

Lead with crawl coverage, completion status, and `onpage_score`; then group
issues by severity and affected-page count. Include representative URLs and
concrete fixes. Treat crawl failures, indexability, 4xx/5xx errors, broken
canonicals, and redirect loops as higher priority than cosmetic metadata
warnings.
