# AIsa API Map for SEO Keyword Research

This reference lists the AIsa API groups most useful for keyword research. Endpoint availability is based on AIsa's DataForSEO integration and should be called with `AISA_API_KEY`.

## Authentication

```bash
export AISA_API_KEY="your-aisa-api-key"
```

Use bearer authentication:

```text
Authorization: Bearer $AISA_API_KEY
Content-Type: application/json
```

## Base URLs

```text
Data APIs: https://api.aisa.one/apis/v1
LLM gateway: https://api.aisa.one/v1/chat/completions
```

## Keyword Discovery

Use these endpoints to generate or expand keyword candidates:

```text
/apis/v1/dataforseo/dataforseo_labs/google/keywords_for_site/live
/apis/v1/dataforseo/dataforseo_labs/google/keyword_suggestions/live
/apis/v1/dataforseo/dataforseo_labs/google/keyword_ideas/live
/apis/v1/dataforseo/dataforseo_labs/google/related_keywords/live
/apis/v1/dataforseo/keywords_data/google_ads/keywords_for_keywords/live
/apis/v1/dataforseo/keywords_data/google_ads/keywords_for_site/live
/apis/v1/dataforseo/keywords_data/bing/keywords_for_keywords/live
/apis/v1/dataforseo/keywords_data/bing/keywords_for_site/live
```

## Metrics and Demand

Use these endpoints to validate demand:

```text
/apis/v1/dataforseo/keywords_data/google_ads/search_volume/live
/apis/v1/dataforseo/keywords_data/bing/search_volume/live
/apis/v1/dataforseo/keywords_data/clickstream_data/bulk_search_volume/live
/apis/v1/dataforseo/keywords_data/clickstream_data/global_search_volume/live
/apis/v1/dataforseo/keywords_data/dataforseo_trends/explore/live
/apis/v1/dataforseo/keywords_data/dataforseo_trends/merged_data/live
/apis/v1/dataforseo/keywords_data/dataforseo_trends/subregion_interests/live
```

## Difficulty, Intent, and Overview

Use these endpoints to qualify keywords:

```text
/apis/v1/dataforseo/dataforseo_labs/google/keyword_overview/live
/apis/v1/dataforseo/dataforseo_labs/google/bulk_keyword_difficulty/live
/apis/v1/dataforseo/dataforseo_labs/google/search_intent/live
/apis/v1/dataforseo/dataforseo_labs/google/categories_for_keywords/live
/apis/v1/dataforseo/dataforseo_labs/google/top_searches/live
```

## Competitor Expansion

Use these endpoints to find competitor gaps and page opportunities:

```text
/apis/v1/dataforseo/dataforseo_labs/google/competitors_domain/live
/apis/v1/dataforseo/dataforseo_labs/google/domain_intersection/live
/apis/v1/dataforseo/dataforseo_labs/google/page_intersection/live
/apis/v1/dataforseo/dataforseo_labs/google/ranked_keywords/live
/apis/v1/dataforseo/dataforseo_labs/google/relevant_pages/live
/apis/v1/dataforseo/dataforseo_labs/google/serp_competitors/live
/apis/v1/dataforseo/dataforseo_labs/google/bulk_traffic_estimation/live
```

## SERP Validation

Use these endpoints to inspect search-result reality before recommending a page:

```text
/apis/v1/dataforseo/serp/google/organic/live/advanced
/apis/v1/dataforseo/serp/google/organic/live/regular
/apis/v1/dataforseo/serp/google/organic/live/html
/apis/v1/dataforseo/serp/bing/organic/live/advanced
/apis/v1/dataforseo/serp/ai_summary
/apis/v1/dataforseo/serp/screenshot
```

## LLM Analysis

Use the AIsa LLM gateway after collecting data:

```text
POST https://api.aisa.one/v1/chat/completions
```

Recommended uses:

- Cluster keywords by topic and intent.
- Summarize SERP patterns.
- Translate metrics into content priorities.
- Create a keyword map.
- Generate a concise stakeholder report.

Guardrail: never ask the LLM to invent SEO metrics. Send it the metrics you collected and require it to mark uncertain recommendations.

