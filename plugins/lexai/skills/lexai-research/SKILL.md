---
name: lexai-research
description: Use the LEX AI tools to research regulatory and compliance topics. Chain keyword search with related-post lookup to surface adjacent material a keyword search alone would miss.
---

# Researching with LEX AI

You have three LEX AI tools (provided by the `lexai` MCP server). Use them in this order for any regulatory/compliance research question.

## 1. `search_fts(query, ...)`: start here

Keyword search across LEX AI content (legal updates, summaries, regulations, acts). Use the user's terms as-is, or quoted phrases for multi-word concepts (e.g. `"data protection"`). A bare term also matches posts carrying it as a hashtag (e.g. `aml` surfaces `#aml`-tagged posts); a hashtag-only match carries no tsvector match, scores 0, and sorts below every keyword match. Returns up to 50 posts with `title`, `url`, `type`, `publish_date`, `snippet`.

Optional filters:
- `order_by`: `rank` (default) orders by tsvector match strength; `date_desc` / `date_asc` order by publish date. A "what changed this week" question wants `date_desc`, not relevance.
- `org`: organization name (e.g. `"BaFin"`) — returns only posts from matching organizations.
- `region`: country/region name or ISO code — e.g. `"Germany"`/`"DE"`, `"United States"`/`"US"`, `"European Union"`/`"EU"`. United Kingdom is `"United Kingdom"`/`"GB"`/`"UK"`; China is `"Mainland China"`. Unknown values return nothing.
- `post_type`, `start_date`, `end_date`, `page_size` (≤50), `page_number`.

If results are sparse or feel narrow, *immediately* run step 2; don't ask the user.

## 2. `graph_neighbors(urls, top_k?, min_similarity?)`: expand

Pass the top 3–5 `url` values from step 1. Returns related posts the keyword search missed. Default `top_k=10`, `min_similarity=0.7`. Lower `min_similarity` to ~0.55 for exploratory research.

## 3. `get_post_by_url(url)`: fetch full content

When the user asks "summarize that post" or "what does <url> say", fetch the full post by URL and answer from the body.

## Output

When citing a LEX post in your reply, always include the `url` from the tool result so the user can open it in LEX. Don't summarize lists of titles without urls; the citation is the value.

## Don'ts

- Don't call `graph_neighbors` with an empty `urls` array; run `search_fts` first.
- Don't ask the user to refine their query before chaining search to related-posts; that's free signal you can gather yourself.
- Don't paginate beyond `page_number=2` for `search_fts` unless the user explicitly asks. Keep responses tight.
