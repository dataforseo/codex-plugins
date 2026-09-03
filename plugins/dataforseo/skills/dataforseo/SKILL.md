---
name: dataforseo
description: Route SEO, search, keyword, competitor, backlink, on-page, and local-market research to request-ready DataForSEO API workflows.
---

# DataForSEO

Use the DataForSEO MCP server configured by this plugin. Prefer the focused
skills below; they contain verified endpoint paths and request parameters, so
routine requests do not need documentation discovery.

## Route the request

- Keyword discovery, metrics, difficulty, intent → `dataforseo-keywords`
- Organic competitors, rankings, keyword gaps → `dataforseo-competitors`
- Current Google results and SERP features → `dataforseo-serp`
- Backlinks, referring domains, link gaps → `dataforseo-backlinks`
- Page checks and technical site crawls → `dataforseo-onpage`
- Google Maps business and local-market research → `dataforseo-local-business`

## Fast workflow

1. Read the matching focused skill.
2. Collect only required inputs that cannot be inferred from the request.
3. Call `api_request` directly with its verified `/v3/` path and a task array
   in `data`.
4. Inspect both top-level and task-level status codes before using results.
5. Summarize findings and state the endpoint, target, location, language,
   device, and other assumptions that materially affect them.

Do not call `docs_list_sections` or `docs_index` for endpoints covered by the
focused skills. Use `docs_search` directly with a known path only when:

- no listed endpoint fits the request;
- a required field or response field is not covered locally; or
- the API rejects a locally documented parameter.

## Request rules

- POST bodies are arrays of task objects: `data: [{...}]`.
- Live endpoints normally accept one task per API call; make the smallest
  billable request that answers the question.
- Keep the default AI-optimized response. Set `noAiMode` to `true` only when
  the user needs fields omitted from the optimized response.
- Treat live API requests as potentially billable. Avoid duplicate, speculative,
  broad, or high-limit calls.
- Leave clickstream data, JavaScript rendering, resource loading, and other
  paid add-ons disabled unless they are required.
- Never ask the user to paste access tokens or API passwords into the chat.
- If an OAuth error occurs, ask the user to reconnect the DataForSEO plugin.

## Quality checks

- Do not invent endpoint names, request fields, location codes, or language
  codes. Prefer explicit location names when the user has not supplied a code.
- Distinguish measured API data from estimates or interpretations.
- Preserve units and timestamps, and explain ambiguous metrics.
- For comparisons, use consistent endpoint settings across all targets.
