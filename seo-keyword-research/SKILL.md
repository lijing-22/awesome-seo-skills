---
name: seo-keyword-research
description: Use this skill when a user asks for SEO keyword research, keyword discovery, search volume analysis, keyword difficulty, search intent mapping, topic clusters, content opportunities, competitor keyword gaps, or a keyword strategy for a domain, URL, product, market, or seed topic. This skill uses AIsa API access to DataForSEO keyword, SERP, trend, and Labs endpoints plus AIsa LLM reasoning.
license: MIT
compatibility: "Works with any agentskills.io-compatible harness, including Claude Code, Claude, OpenCode, Cursor, Codex, Gemini CLI, OpenClaw, Hermes, Goose, and others. Requires Python 3, curl, and AISA_API_KEY."
metadata:
  short-description: AIsa-powered SEO keyword research
  aisa:
    homepage: https://aisa.one
    requires:
      bins:
        - python3
        - curl
      env:
        - AISA_API_KEY
    primaryEnv: AISA_API_KEY
    harnesses:
      - claude-code
      - claude
      - opencode
      - cursor
      - codex
      - gemini-cli
      - openclaw
      - hermes
      - goose
---

# SEO Keyword Research

This skill builds a practical SEO keyword strategy from a domain, URL, seed topic, product, market, or competitor set. It combines structured DataForSEO data through AIsa with AIsa LLM reasoning for clustering, search-intent interpretation, opportunity scoring, and content planning.

## Requirements

This skill requires an AIsa API key.

```bash
export AISA_API_KEY="your-aisa-api-key"
```

Use these AIsa endpoints:

- Data APIs: `https://api.aisa.one/apis/v1/...`
- LLM gateway: `https://api.aisa.one/v1/chat/completions`

Never print or commit API keys. If the key is missing, ask the user to set `AISA_API_KEY`.

## Compatibility

Works with any agentskills.io-compatible harness, including Claude Code, Claude, OpenAI Codex, Cursor, Gemini CLI, OpenCode, Goose, OpenClaw, Hermes, and other agent runtimes that support skill folders.

Requires Python 3, curl, and `AISA_API_KEY`. Get an API key at `https://aisa.one`.

## Quick Start

```bash
export AISA_API_KEY="your-aisa-api-key"

python3 seo-keyword-research/scripts/aisa_client.py data \
  /apis/v1/dataforseo/dataforseo_labs/google/keyword_suggestions/live \
  payload.json \
  --out keyword-suggestions.json
```

Then use the AIsa LLM gateway to cluster, score, and summarize the collected keyword data.

## When to Use

Use this skill for requests like:

- "Find SEO keywords for this site."
- "Build a keyword strategy for my SaaS."
- "Research keywords for this product category."
- "Find keyword gaps between us and competitors."
- "Cluster these keywords by search intent."
- "Pick the best SEO content topics for next month."
- "Create a keyword map for these landing pages."

Do not use this skill for full technical audits, backlink audits, schema implementation, or content writing unless the user specifically asks for keyword research as part of that workflow.

## Core Workflow

### 1. Define the research scope

Collect or infer:

- Target domain or URL
- Seed topics, products, services, or categories
- Target country, language, and search engine
- Competitors, if provided
- Business goal: traffic, leads, sales, awareness, local visibility, or content planning
- Constraints: brand terms only, non-brand terms only, blog topics, landing pages, commercial pages, or programmatic pages

If country and language are missing, default to the user's market when obvious. Otherwise use United States and English, and note the assumption.

### 2. Build the initial keyword universe

Use AIsa DataForSEO endpoints in this order when inputs are available:

1. Site-derived keywords:
   - `/apis/v1/dataforseo/dataforseo_labs/google/keywords_for_site/live`
   - `/apis/v1/dataforseo/keywords_data/google_ads/keywords_for_site/live`
2. Seed expansion:
   - `/apis/v1/dataforseo/dataforseo_labs/google/keyword_suggestions/live`
   - `/apis/v1/dataforseo/dataforseo_labs/google/keyword_ideas/live`
   - `/apis/v1/dataforseo/dataforseo_labs/google/related_keywords/live`
   - `/apis/v1/dataforseo/keywords_data/google_ads/keywords_for_keywords/live`
3. Demand and trend checks:
   - `/apis/v1/dataforseo/keywords_data/google_ads/search_volume/live`
   - `/apis/v1/dataforseo/keywords_data/clickstream_data/global_search_volume/live`
   - `/apis/v1/dataforseo/keywords_data/dataforseo_trends/explore/live`
4. Difficulty and intent:
   - `/apis/v1/dataforseo/dataforseo_labs/google/bulk_keyword_difficulty/live`
   - `/apis/v1/dataforseo/dataforseo_labs/google/search_intent/live`
   - `/apis/v1/dataforseo/dataforseo_labs/google/keyword_overview/live`

Keep source labels for each keyword: `site`, `seed`, `suggestion`, `related`, `competitor`, `trend`, `serp`, or `llm-generated`. Treat `llm-generated` keywords as hypotheses until validated by search volume or SERP data.

### 3. Expand through competitors and SERPs

When competitors are provided, or when DataForSEO returns SERP competitors:

- Use `/apis/v1/dataforseo/dataforseo_labs/google/competitors_domain/live`
- Use `/apis/v1/dataforseo/dataforseo_labs/google/domain_intersection/live`
- Use `/apis/v1/dataforseo/dataforseo_labs/google/ranked_keywords/live`
- Use `/apis/v1/dataforseo/dataforseo_labs/google/serp_competitors/live`
- Use `/apis/v1/dataforseo/dataforseo_labs/google/relevant_pages/live`

For the strongest candidate keywords, inspect live search results:

- `/apis/v1/dataforseo/serp/google/organic/live/advanced`
- `/apis/v1/dataforseo/serp/ai_summary`
- `/apis/v1/dataforseo/serp/screenshot`

Use SERP data to identify ranking page types, dominant content formats, user intent, SERP features, freshness patterns, weak results, and content gaps.

### 4. Normalize and clean the data

Before scoring:

- Lowercase only for deduplication; preserve original keyword casing in output.
- Merge close duplicates, singular/plural variants, and obvious spelling variants.
- Remove irrelevant brand, adult, navigational, and off-market terms unless requested.
- Mark keywords with missing volume, difficulty, or intent as incomplete rather than guessing numbers.
- Keep localized variants separate when intent differs by geography.

### 5. Cluster by intent and topic

Use AIsa LLM reasoning to cluster validated keywords. Prefer compact structured output.

Suggested cluster dimensions:

- Parent topic
- Subtopic
- Search intent: informational, commercial, transactional, navigational, local, comparison, or troubleshooting
- Funnel stage: awareness, consideration, conversion, retention
- Best page type: blog post, comparison page, landing page, product page, category page, tool page, glossary page, local page, or programmatic template

Do not let the LLM invent metrics. It may classify, summarize, and prioritize, but metrics must come from AIsa/DataForSEO data or be marked as qualitative.

### 6. Score opportunities

Score each keyword or cluster from 0 to 25:

- Demand: search volume, trend, and market size
- Relevance: fit with the domain, product, ICP, or page
- Intent value: likelihood to drive qualified traffic
- Ranking feasibility: inverse of difficulty plus SERP weakness
- Strategic value: supports product positioning, topical authority, or conversion

Use a simple label:

- `High priority`: strong demand, clear fit, feasible SERP, valuable intent
- `Medium priority`: useful but constrained by difficulty, ambiguity, or lower demand
- `Low priority`: weak fit, weak demand, or poor feasibility
- `Validate first`: interesting idea with incomplete data

### 7. Produce the final deliverable

Return a concise keyword research report with:

- Executive summary
- Research assumptions
- Top keyword clusters
- Priority keyword shortlist
- Best content opportunities
- Competitor or SERP insights
- Recommended next pages or updates
- Data gaps and validation notes

Use `references/report-template.md` when a full report is requested.

## AIsa LLM Usage

Use the AIsa LLM gateway for:

- Classifying search intent
- Grouping keywords into clusters
- Summarizing SERP patterns
- Translating raw metrics into SEO decisions
- Drafting a keyword strategy report
- Turning keywords into page recommendations

Recommended request pattern:

```bash
curl -sS "https://api.aisa.one/v1/chat/completions" \
  -H "Authorization: Bearer $AISA_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-5-mini",
    "messages": [
      {
        "role": "system",
        "content": "You are an SEO strategist. Use only provided metrics as facts. Mark unverified ideas clearly."
      },
      {
        "role": "user",
        "content": "Cluster these keyword rows by topic, search intent, and best page type."
      }
    ]
  }'
```

## Helper Script

Use `scripts/aisa_client.py` for quick API calls:

```bash
python3 seo-keyword-research/scripts/aisa_client.py data \
  /apis/v1/dataforseo/dataforseo_labs/google/keyword_suggestions/live \
  payload.json \
  --out keyword-suggestions.json
```

```bash
python3 seo-keyword-research/scripts/aisa_client.py chat \
  --model gpt-5-mini \
  --system system-prompt.txt \
  --prompt cluster-prompt.txt \
  --out clusters.md
```

## Quality Rules

- Prefer live AIsa/DataForSEO data over browser scraping.
- Cite which endpoint groups were used.
- Separate facts from recommendations.
- Do not invent search volume, CPC, keyword difficulty, rank, or trend values.
- Use LLM output for interpretation, not as a substitute for keyword data.
- Keep raw exports private if they contain customer domains, competitors, or internal strategy.
- Make the final answer actionable: the user should know which keywords to target, which page type to create, and why.

## References

- `references/aisa-api-map.md` for endpoint groups and usage notes.
- `references/report-template.md` for the final keyword research report structure.
