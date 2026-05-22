# Awesome SEO Skills

SEO workflow skills for autonomous agents. Powered by [AIsa](https://aisa.one).

One API key -> site crawling -> DataForSEO keyword data -> SERP validation -> LLM-assisted SEO strategy.

## What It Does

Give it a website, product, market, or SEO research brief, and it:

1. **Crawls the website first** to understand the product, audience, features, use cases, page structure, and existing content themes.
2. **Builds a site-informed business profile** instead of starting with noisy brand terms or generic keyword lists.
3. **Generates non-brand seed topics** from the actual website copy, docs, product positioning, integrations, competitors, and use cases.
4. **Queries AIsa DataForSEO endpoints** for keyword suggestions, search volume, difficulty, CPC, search intent, trends, competitor terms, and SERP data.
5. **Clusters keywords** into practical SEO themes such as product pages, feature pages, comparison pages, tutorials, API docs, and programmatic page sets.
6. **Identifies high-opportunity keywords** where difficulty is lower than 40 and search volume is greater than 1000.
7. **Recommends page types from SERP evidence** so the user knows whether to create a landing page, feature page, comparison page, pricing page, documentation page, programmatic page, or blog article.
8. **Synthesizes a concise SEO report** with representative keywords, opportunity rationale, SERP patterns, and a prioritized roadmap.

All through a single `AISA_API_KEY`. No separate DataForSEO account, scraping vendor, or LLM provider account required.

## Skills

| Skill | Purpose |
|---|---|
| `seo-keyword-research` | Crawl a site first, infer product and business direction, validate keywords with AIsa DataForSEO endpoints, and produce a SERP-aware keyword strategy. |

More SEO skills will be added as this repository grows.

## Install

Clone the repository:

```bash
git clone https://github.com/lijing-22/awesome-seo-skills.git
cd awesome-seo-skills
```

Install the skill into your agent runtime:

```bash
# For Claude Code
mkdir -p ~/.claude/skills
cp -r seo-keyword-research ~/.claude/skills/

# For Cursor
mkdir -p ~/.cursor/skills
cp -r seo-keyword-research ~/.cursor/skills/

# For Codex
mkdir -p ~/.codex/skills
cp -r seo-keyword-research ~/.codex/skills/

# For Gemini CLI
mkdir -p ~/.gemini/skills
cp -r seo-keyword-research ~/.gemini/skills/
```

Restart your agent after installing so it can load the new skill metadata.

## Setup

```bash
export AISA_API_KEY="your-key-here"
# Get one at https://aisa.one
```

Keep your key out of commits, shell history exports, screenshots, and shared logs. For persistent local use, store it in your normal shell profile or secret manager.

## Usage

### As an agent skill

Ask naturally:

- *"Use seo-keyword-research to find SEO opportunities for aisa.one in the US market."*
- *"Research non-brand keyword clusters for my SaaS homepage."*
- *"Crawl this website and tell me which keywords are worth targeting."*
- *"Find high-opportunity SEO keywords with difficulty below 40 and volume above 1000."*
- *"Based on the SERP, should we create landing pages, comparison pages, or blog posts?"*

The skill should start with the website content, then move into keyword data. It should not begin by researching brand terms unless you explicitly request brand SEO.

### As CLI helpers

The skill includes small helper scripts for agents and developers.

```bash
# Crawl a site and extract SEO research context
python3 seo-keyword-research/scripts/site_crawler.py \
  https://example.com \
  --max-pages 12 \
  --out site-profile.json

# Call an AIsa DataForSEO endpoint with a JSON payload
python3 seo-keyword-research/scripts/aisa_client.py data \
  /apis/v1/dataforseo/dataforseo_labs/google/keyword_suggestions/live \
  payload.json \
  --out keyword-suggestions.json

# Use the AIsa LLM gateway for clustering or report synthesis
python3 seo-keyword-research/scripts/aisa_client.py chat \
  --model gpt-5-mini \
  --system system-prompt.txt \
  --prompt cluster-prompt.txt \
  --out keyword-clusters.md
```

Example keyword suggestion payload:

```json
[
  {
    "keyword": "llm gateway",
    "language_name": "English",
    "location_name": "United States",
    "limit": 50
  }
]
```

## Example Output

```text
SEO KEYWORD RESEARCH: aisa.one, United States

Business Profile
- Category: AI infrastructure for autonomous agents
- Core offer: unified LLM gateway, 100+ data APIs, agent skills, payments
- Audience: developers, AI agent builders, growth teams, API-heavy startups

High-Opportunity Keywords

| Keyword | Volume | Difficulty | Intent | Recommended Page |
|---|---:|---:|---|---|
| github mcp server | 9900 | 32 | navigational | Developer guide / curated registry |
| what is an mcp server | 6600 | 24 | informational | Blog article / glossary guide |
| aws mcp server | 4400 | 27 | commercial | Integration comparison page |
| tavily api | 1300 | 16 | transactional | API tutorial / integration landing page |
| cloudflare ai gateway | 1300 | 19 | navigational | Competitor comparison page |

Keyword Clusters

1. Agent Skills & MCP
   Representative keywords: github mcp server, what is an mcp server,
   aws mcp server, claude mcp server, playwright mcp server

2. LLM Gateway & Model Routing
   Representative keywords: llm gateway, llm router, ai gateway,
   model gateway, llm observability

3. Data APIs for Agents
   Representative keywords: serp api, tavily api, perplexity api,
   perplexity api key, dataforseo api

SERP-Based Recommendations
- MCP terms: create developer guides and registry-style pages because SERPs
  are dominated by GitHub, official docs, and technical tutorials.
- AI gateway terms: create comparison pages where SERPs are vendor-heavy.
- API terms: create documentation and integration pages with code examples.

Next Actions
1. Publish "What is an MCP Server?"
2. Publish "Best GitHub MCP Servers for AI Agents"
3. Publish "AIsa vs Cloudflare AI Gateway"
4. Publish "Tavily API with AIsa"
5. Build internal links from docs, skills, and API reference pages
```

Numbers above are example report values. Always use fresh AIsa/DataForSEO data when running the skill for a real site.

## Project Structure

```text
awesome-seo-skills/
|-- README.md                                      # Repository overview
|-- LICENSE                                        # MIT license
|-- .gitignore                                     # Local outputs, env files, caches
`-- seo-keyword-research/
    |-- SKILL.md                                   # Skill definition and workflow
    |-- scripts/
    |   |-- aisa_client.py                         # Reusable AIsa API + LLM helper
    |   `-- site_crawler.py                        # Lightweight website crawler
    `-- references/
        |-- aisa-api-map.md                        # Relevant AIsa/DataForSEO endpoints
        `-- report-template.md                     # Keyword research report format
```

## AIsa APIs Used

| Capability | AIsa Endpoint | Purpose |
|---|---|---|
| LLM Gateway | `api.aisa.one/v1/chat/completions` | Site profile synthesis, seed topic generation, keyword clustering, SERP summary, report writing |
| DataForSEO Labs | `api.aisa.one/apis/v1/dataforseo/dataforseo_labs/google/*` | Keyword suggestions, related keywords, keyword overview, keyword difficulty, search intent, competitor and domain data |
| Keywords Data | `api.aisa.one/apis/v1/dataforseo/keywords_data/*` | Search volume, Google Ads keyword data, Bing keyword data, clickstream demand checks, trend checks |
| Google SERP | `api.aisa.one/apis/v1/dataforseo/serp/google/organic/live/advanced` | Live SERP validation, ranking page type analysis, competitor domains, page recommendation evidence |
| SERP Summary | `api.aisa.one/apis/v1/dataforseo/serp/ai_summary` | SERP-level synthesis when a compact summary is useful |
| OnPage Fallback | `api.aisa.one/apis/v1/dataforseo/on_page/*` | Crawl fallback, page parsing, raw HTML, links, microdata, and site-level evidence when local crawling is blocked |

See [`seo-keyword-research/references/aisa-api-map.md`](seo-keyword-research/references/aisa-api-map.md) for the full endpoint map and workflow notes.

## Contributing

1. Fork this repo
2. Create a feature branch
3. Keep skill content in English
4. Test helper scripts locally:

```bash
python3 seo-keyword-research/scripts/site_crawler.py https://aisa.one --max-pages 2
python3 seo-keyword-research/scripts/aisa_client.py --help
```

5. Validate that `SKILL.md` explains when the skill should trigger and what the agent should do
6. Submit a pull request to this repository, or to the future `AIsa-team/awesome-seo-skills` repository after publication

## License

MIT - see [LICENSE](LICENSE).

---

Built for [AIsa](https://aisa.one) - One key, every model, every data source.
