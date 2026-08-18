# LEX AI for Claude &amp; Copilot

LEX AI regulatory content (legal updates, summaries, regulations, acts) inside Claude or Microsoft Copilot. Sign in with your existing LEX AI account, or start a 14-day trial — sign up at https://web.lexai.co/mcp.

## Add the connector

Works in **Claude Desktop**, **Claude Cowork**, and **Microsoft Copilot** (any MCP-capable client).

1. Open your client's connector settings (Claude Desktop / Cowork: gear icon → **Connectors** → **Add custom connector**).
2. Name it *LEX AI* and paste this URL:

   ```
   https://api.lexai.co/mcp
   ```
3. Save. A sign-in page opens in your browser; enter your LEX AI email + password (or the trial credentials emailed to you).

Once connected, ask things like *"Search LEX for GDPR transfer obligations and pull up adjacent posts."*

## For developers: Claude Code (CLI)

The two `/plugin ...` lines are slash commands typed inside a running Claude Code session, not shell commands.

1. Open Claude Code: run `claude` in a terminal.
2. Inside the session, type:

   ```
   /plugin marketplace add LEX-AI-Dev-Team/lexai-mcp
   ```
3. Then:

   ```
   /plugin install lexai@lexai
   ```

On first tool call, Claude opens the same sign-in page.

## Tools

- `search_fts(query, order_by?, page_size?, page_number?, post_type?, start_date?, end_date?, org?, region?)` — keyword search across LEX AI content, ranked by tsvector match by default; `order_by=date_desc` / `date_asc` orders by publish date instead. A bare term also matches posts tagged with it (e.g. `aml` → `#aml`); those carry no tsvector match, score 0, and sort below every keyword match. `org` filters by organization name; `region` by country/region name or ISO code (e.g. `EU`, `US`, `DE`).
- `graph_neighbors(urls, top_k?, min_similarity?)` — related posts a keyword search misses; chain after `search_fts` using the `url` values it returns.
- `get_post_by_url(url)` — full document (title, content, sources, hashtags, connections) by URL.

## Limits

| Window | Trial | Enterprise |
|---|---|---|
| Burst (60s) | 30 calls | 120 calls |
| Sustained (24h) | 300 calls | 5,000 calls |
| Cost budget (24h) | 600 | 12,000 |

Tool weights: `search_fts` = 1, `graph_neighbors` = 2, `get_post_by_url` = 5. On exceed, the server returns JSON-RPC error `-32000` with `data: { reason, retry_after_s }`. Need higher limits, email `support@lexai.co`.

## License

Proprietary, LEX AI GmbH. Plugin metadata published for transparency; the MCP server itself is hosted by LEX AI.
