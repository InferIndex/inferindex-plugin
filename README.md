# InferIndex plugin for Claude Code and Codex

Live LLM API prices inside Claude Code, Cowork and Codex. Ask which provider is cheapest for a model, compare every
provider with its usage conditions, estimate what a workload will cost, or check how a price moved — Claude
answers from [InferIndex](https://inferindex.dev) data instead of from memory, and gives you the API call behind
each number.

## What's included

- **MCP server** `inferindex` — the remote InferIndex server at `https://mcp.inferindex.dev/mcp` (read-only, no
  API key needed). Tools: `search_models`, `cheapest`, `compare_providers`, `price_history`, `estimate_cost`.
- **Skill** `llm-api-pricing` — used automatically when you choose an LLM provider or model, compare inference
  prices or estimate a cost. It resolves exact model ids first, never invents a price, cites the source
  (`api_url`) and flags promotions, stale prices, prices converted from points and ambiguous model names.
- **Commands**
  - `/inferindex:cheapest <model> [constraints]` — cheapest offer for a model, e.g.
    `/inferindex:cheapest deepseek-v3.2 eu, tools`
  - `/inferindex:llm-cost <model> <input tokens> <output tokens> [requests/day] [cached share]` — estimated cost
    per request and per month, e.g. `/inferindex:llm-cost gpt-oss-120b 2000 500 1000`

## Install in Claude Code

From the official plugin directory, once listed:

```
/plugin install inferindex@claude-plugins-official
```

Or directly from this repository:

```
/plugin marketplace add InferIndex/inferindex-plugin
/plugin install inferindex@inferindex
```

Then try: *"Which provider is cheapest for DeepSeek V3.2 right now?"*

## Install in Codex

```bash
codex plugin marketplace add InferIndex/inferindex-plugin
codex plugin add inferindex@inferindex
```

The plugin brings the same MCP server and skill (the slash commands are Claude Code only): just ask a pricing
question.

## Good to know

- Prices are in USD per million tokens, collected from providers' public pricing. InferIndex is a comparison
  tool: confirm the price on the provider's own page before committing to real spend.
- The MCP server is free to use and rate-limited (120 requests per minute per IP). More about the data and its
  limits: [InferIndex API documentation](https://github.com/InferIndex/inferindex-docs).

## Privacy

The plugin sends your tool calls (model names, filters, token counts) to the InferIndex API to answer them. It
doesn't send your conversation or code. See the [InferIndex privacy policy](https://api.inferindex.dev/privacy).
Use of the service is subject to the [InferIndex terms of use](https://api.inferindex.dev/terms).

## Support

Questions and issues: open an issue on this repository, or write to contact@inferindex.dev. Security reports:
security@inferindex.dev.

## License

MIT — see [LICENSE](LICENSE). This license covers this plugin only, not the InferIndex service or its data.
