---
name: dataforseo
description: Research keywords, SERPs, competitors, backlinks, websites, content, local businesses, products, apps, and AI-search visibility with the DataForSEO API. Use for SEO, search, and digital-market analysis in Codex.
---

# DataForSEO

Use the DataForSEO MCP tools to find the correct API endpoint and retrieve
current SEO and search marketing data.

## Workflow

1. Define the research question, target, market, and desired output.
2. Select the narrowest suitable endpoint:
   - If the endpoint path is known, call `docs_search` for it directly.
   - If only the API area is known, use `docs_index` for that section.
   - Use `docs_list_sections` only when the API area is unclear.
3. Use `docs_search` to verify required fields, limits, defaults, and response
   fields. Request code examples only when they help construct a complex body.
4. Collect only missing inputs that materially affect the result, such as
   target, location, language, device, search engine, or date range.
5. Call `api_request` with the documented method, `/v3/` path, and task data.
6. Check top-level and task-level status codes before analyzing results.
7. Paginate only when the requested answer requires more results.
8. Summarize the findings, methodology, material assumptions, and limitations.

## Request guidance

- Prefer endpoint paths beginning with `/v3/` over manually constructed URLs.
- Most POST endpoints expect `data` to be an array of task objects:
  `data: [{...}]`. Follow the endpoint documentation exactly.
- Keep the default AI-optimized response. Set `noAiMode` to `true` only when
  the user needs fields omitted from the optimized response.
- Treat live API requests as potentially billable. Avoid duplicate, speculative,
  unnecessarily broad, high-limit, or repeated polling calls.
- Prefer one focused request over many exploratory requests. Reuse compatible
  results instead of calling another endpoint for the same information.
- Keep optional paid features disabled unless needed, including clickstream
  data, JavaScript rendering, resource loading, and deeper SERP collection.
- Preserve identical location, language, device, filters, and rank scales when
  comparing targets.
- Use returned pagination tokens when documented; do not guess offsets beyond
  endpoint limits.
- Never ask the user to paste access tokens or API passwords into the chat.
- If an OAuth error occurs, ask the user to reconnect the DataForSEO plugin.

## Target and market rules

- Domains normally omit protocol and `www.`; exact page targets usually require
  an absolute URL. Follow the selected endpoint's rule.
- Prefer the user's explicit location and language. Do not assume the United
  States or English when market choice could change the answer.
- Do not invent location, language, category, or search-engine codes. Resolve
  them through the relevant documented endpoint when necessary.
- Distinguish live SERP data from periodically updated database metrics.

## Response handling

- Treat a successful transport response as insufficient: inspect
  `status_code`, `status_message`, `tasks_error`, and each task status.
- Use `tasks[].result` as the analysis source and note missing or partial data.
- Preserve units, currencies, timestamps, ranking scopes, and estimated versus
  measured metrics.
- For long-running tasks, retain the task ID and use the documented result
  endpoint. Stop polling when complete, failed, or clearly unavailable.

## Quality checks

- Do not invent endpoint names, parameters, filters, or response fields.
- Distinguish measured API data from estimates or interpretations.
- Explain ambiguous metrics and avoid presenting correlation as causation.
- Report the endpoint, target, market settings, device, date range, limits, and
  filters when they materially affect reproducibility.
