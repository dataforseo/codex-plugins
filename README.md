# DataForSEO Codex Plugins

Official DataForSEO plugins for Codex.

## DataForSEO

Turn live search and web data into actionable SEO and marketing analysis
directly in Codex. The DataForSEO plugin can help you:

- Research keywords, search volume, intent, difficulty, CPC, and trends.
- Analyze localized desktop and mobile SERPs, rankings, SERP features, and
  Google AI Overviews.
- Compare competitors, ranking keywords, landing pages, content gaps, and
  market visibility.
- Audit technical and on-page SEO issues across websites.
- Examine backlinks, referring domains, anchors, and link opportunities.
- Research local businesses, listings, ratings, and reviews.
- Analyze products, sellers, and prices across shopping platforms.
- Work across countries, cities, languages, devices, and search engines.

DataForSEO supplies structured data without requiring you to maintain search
scrapers, proxy networks, crawlers, keyword databases, or backlink indexes.

## Installation

Add this repository as a Codex plugin marketplace:

```bash
codex plugin marketplace add dataforseo/codex-plugins
```

Open the Codex plugin browser and install **DataForSEO**.

## Authentication

The plugin connects to the hosted DataForSEO MCP server:

```text
https://mcp.dataforseo.com/v3/mcp
```

Codex discovers the DataForSEO OAuth service automatically and opens a browser
for authorization during installation. API credentials and access tokens do
not need to be added to this repository.

## Example prompts

- Research high-intent keywords for my website in the United States.
- Compare my organic search visibility with three competitors.
- Find backlink opportunities that my competitors have but I do not.
- Audit my website for technical SEO issues.
- Analyze which SERP features appear for my target keywords.
- Compare product prices and sellers across selected markets.

## Usage and pricing

Some DataForSEO requests are billable. Cost depends on the selected API,
endpoint, parameters, and workload. The plugin avoids unnecessary requests,
but you should review current pricing and monitor usage in your DataForSEO
account.

## Resources

- [DataForSEO](https://dataforseo.com/)
- [API documentation](https://docs.dataforseo.com/v3/)
- [API Playground](https://dataforseo.com/help-center/dataforseo-api-explorer)
- [API access](https://app.dataforseo.com/api-access)

## License

Apache-2.0
