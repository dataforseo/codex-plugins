---
name: dataforseo
description: Research SEO, search, keyword, backlink, content, and market data with the DataForSEO API.
---

# DataForSEO

Use the DataForSEO MCP tools to find the correct API endpoint and retrieve
current SEO and search marketing data.

## Workflow

1. Identify the user's research goal and the required output.
2. Use `docs_list_sections` when the relevant API area is unclear.
3. Use `docs_index` with the closest section to locate suitable endpoints.
4. Use `docs_search` to confirm the selected endpoint's request fields,
   supported locations and languages, and response structure.
5. Call `api_request` with the documented HTTP method, path, and task data.
6. Summarize the useful results and state the endpoint, location, language,
   date range, and other assumptions that materially affect the findings.

## Request guidance

- Prefer endpoint paths beginning with `/v3/` over manually constructed URLs.
- Pass request bodies through `data` in the exact shape required by the
  endpoint documentation.
- Keep the default AI-optimized response. Set `noAiMode` to `true` only when
  the user needs fields omitted from the optimized response.
- Treat live API requests as potentially billable. Avoid duplicate, speculative,
  or unnecessarily broad calls.
- Never ask the user to paste access tokens or API passwords into the chat.
- If an OAuth error occurs, ask the user to reconnect the DataForSEO plugin.

## Quality checks

- Do not invent endpoint names, request fields, location codes, or language
  codes; verify them in the documentation.
- Distinguish measured API data from estimates or interpretations.
- Preserve units and timestamps, and explain ambiguous metrics.
- For comparisons, use consistent endpoint settings across all targets.
