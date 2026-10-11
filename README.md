# InferIndex plugin for Claude Code and Codex

Live LLM API prices inside Claude Code, Cowork and Codex. Ask which provider is cheapest for a model, compare every
provider with its usage conditions, estimate what a workload will cost, check how a price moved, or find out
whether renting GPUs to run an open model beats paying an API — your assistant answers from
[InferIndex](https://inferindex.dev) data instead of from memory, and gives you the API call behind each number.

## What's included

- **MCP server** `inferindex` — the remote InferIndex server at `https://mcp.inferindex.dev/mcp` (read-only, no
  API key needed). Eight tools:
  - `search_models` — Search models: find the exact id of a model.
  - `cheapest` — Cheapest offers for a model: cheapest current offers, with filters and signals.
  - `compare_providers` — Compare providers for a model: one line per provider and reseller, with region and conditions.
  - `price_history` — Price history of a model: how prices moved over time, plus the lab's official price.
  - `estimate_cost` — Estimate the cost of a workload: cost per request and per month.
  - `list_gpus` — GPU types tracked for rental prices: lowest hourly price per GPU, by tier.
  - `gpu_rentals` — Rental offers for one GPU, by tier: hourly rental prices, tiers never compared with each other.
  - `self_host_or_api` — Self-host an open model or use the API?: break-even volume and the throughput a rented
    setup must deliver; declines closed models, which cannot be priced for self-hosting.
- **Skill** `llm-api-pricing` — used automatically when you choose an LLM provider or model, compare inference
  prices, estimate a cost or ask whether self-hosting is worth it. It resolves exact model ids first, never
  invents a price, cites the source (`api_url`) and flags promotions, stale prices, prices converted from points
  and ambiguous model names.
- **Commands**
  - `/inferindex:cheapest <model> [constraints]` — cheapest offer for a model, e.g.
    `/inferindex:cheapest deepseek-v3.2 eu, tools`
  - `/inferindex:llm-cost <model> <input tokens> <output tokens> [requests/day] [cached share]` — estimated cost
    per request and per month, e.g. `/inferindex:llm-cost gpt-oss-120b 2000 500 1000`

## Install in Claude Code

When the plugin is listed in Anthropic's directory, you will be able to add it from there. Until then, install it
from this repository:

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

The plugin includes the same MCP server and skill. The slash commands work only in Claude Code; in Codex, just ask
a pricing question.

## Good to know

- Prices are in USD per million tokens, collected from providers' public pricing. InferIndex is a comparison
  tool: confirm the price on the provider's own page before committing to real spend.
- The "self-host or API?" answer is an estimate built from published benchmarks and GPU rental prices. It covers
  hardware rental only, not engineering, monitoring, redundancy, start-up time, or storage and network billed
  separately.
- The MCP server is free to use and rate-limited to 120 requests per minute per IP address. Tool calls that read
  price data also count toward the API limit of 60 requests per minute per IP address. More on the data and its
  limitations: [InferIndex API documentation](https://github.com/InferIndex/inferindex-docs).
- This plugin is MIT-licensed. The InferIndex service itself is free to use, but it is not open source.

## Privacy

The plugin sends your tool calls (model names, filters, token counts) to the InferIndex API to answer them. It
doesn't send your conversation or code. See the [InferIndex privacy policy](https://api.inferindex.dev/privacy).
Use of the service is subject to the [InferIndex terms of use](https://api.inferindex.dev/terms).

## Support

Questions and issues: open an issue on this repository, or write to contact@inferindex.dev. Security reports:
security@inferindex.dev.

## License

MIT — see [LICENSE](LICENSE). This license covers this plugin only, not the InferIndex service or its data.
