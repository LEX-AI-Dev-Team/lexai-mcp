# LEX AI for Claude

LEX AI regulatory content (legal updates, summaries, regulations, acts) inside Claude. Sign in with your existing LEX AI account, or start a 14-day trial — sign up at https://web.lexai.co/mcp.

## Claude Desktop or Claude Cowork (Mac)

The primary path. Works for any Claude, Copilot and other AI agents.

1. Open **Claude Desktop** or **Claude Cowork** on your Mac.
2. Click the gear icon (top right) → **Connectors** → **Add custom connector**.
3. Name it *LEX AI*. Paste this URL:

   ```
   https://api.lexai.co/mcp
   ```
4. Save. Claude opens a sign-in page in your browser; enter your LEX AI email + password (or the trial credentials emailed to you).

Once connected, ask Claude things like *"Search LEX for GDPR transfer obligations and pull up adjacent posts."*

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

- `search_fts(query, page_size?, page_number?, post_type?, start_date?, end_date?)`:Full Text keyword search.
- `graph_neighbors(post_ids, top_k?, min_similarity?)`: semantic graph search, Chain after `search_fts`.
- `get_post_by_url(url)`: full document (title, content) by source URL.

## Limits

| Window | Trial | Enterprise |
|---|---|---|
| Burst (60s) | 30 calls | 120 calls |
| Sustained (24h) | 300 calls | 5,000 calls |
| Cost budget (24h) | 600 | 12,000 |

Tool weights: `search_fts` = 1, `graph_neighbors` = 2, `get_post_by_url` = 5. On exceed, the server returns JSON-RPC error `-32000` with `data: { reason, retry_after_s }`. Need higher limits, email `support@lexai.co`.

## License

Proprietary, LEX AI GmbH. Plugin metadata published for transparency; the MCP server itself is hosted by LEX AI.
