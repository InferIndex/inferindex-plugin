---
name: llm-api-pricing
description: Use when the user is choosing an LLM API provider or model, comparing inference prices, asking which provider is cheapest for a model, estimating what an LLM workload will cost, checking how a model's price changed over time, or deciding whether to self-host an open model on rented GPUs instead of using an API. Uses the InferIndex MCP tools for live, sourced prices instead of remembered ones.
---

# LLM API pricing with InferIndex

Prices of LLM APIs change often and differ a lot between providers and resellers. Whenever a price, a
"cheapest provider" or a cost estimate matters to the user's decision, get it from the InferIndex tools
(`search_models`, `cheapest`, `compare_providers`, `price_history`, `estimate_cost`, `list_gpus`, `gpu_rentals`,
`self_host_or_api`) rather than from memory.

## Rules

1. **Never invent or remember a price.** Every number you give must come from a tool result in this
   conversation. If the tools don't return a price, say so.
2. **Resolve the model first.** Unless the user already gave an exact id (`org/model`, e.g.
   `deepseek/deepseek-v3.2`), call `search_models` and use the exact id. If a result has `ambiguous: true`, tell the
   user which model was picked and list the other candidates (`other_matches`) instead of silently assuming.
   If a "latest" alias or an unknown id is rejected, use the ids in `suggestions`.
3. **Cite the source.** Each result includes `api_url`, the equivalent public API call: give it so the user can
   check the numbers. Mention the date of the data when relevant (`checked_at`).
4. **Flag what changes the picture.** Point out, when present on an offer you recommend or mention:
   - a promotion (`promo`, `promo_ends_at`, `price_before_promo`), and `promo_expired`;
   - `points_based`: the price is converted from points or credits, so the real cost depends on how they're bought;
   - `stale`: the price hasn't been re-checked recently and may be out of date;
   - `below_official_list`: offered clearly below the lab's list price without a declared discount — worth
     double-checking before relying on it;
   - `tier` other than `standard` (flex/batch are slower or lower priority), `tiered` pricing, `peak_pricing`;
   - `estimate_approx` and its `estimate_notes` on cost estimates.
5. **Respect the user's constraints** with the tool filters rather than by filtering yourself: `min_context`,
   `tools`, `json`, `vision`, `region` (e.g. `eu`), `no_training`, `no_waitlist`. Use `strict: true` when the
   user needs the constraint guaranteed (offers with unknown data are then dropped).
6. **Prices are USD per million tokens** unless the result says otherwise; `blended_per_1M` is
   (3 × input + output) / 4.
7. **Self-hosting is a condition, never a promise.** `self_host_or_api` gives a break-even volume (million tokens
   per day) and the throughput the whole configuration must deliver for renting to cost less. Report both, say that
   it is an estimate covering hardware rental only (not engineering, monitoring, redundancy, start-up time, storage
   or network), and never say that self-hosting is cheaper without the condition. If the tool refuses (closed
   model, unknown size, weights that don't fit), say so and give its reason; don't invent numbers. GPU rental
   tiers (guaranteed, community, spot) are different products: never compare them with each other.

## Which tool

| User need | Tool |
|---|---|
| Find the exact model id | `search_models` |
| Cheapest offer for a model, with constraints | `cheapest` |
| All providers for a model, with usage conditions and reliability | `compare_providers` |
| What a workload will cost (per request / per month) | `estimate_cost` (or `cheapest` with usage) |
| How the price moved, or the price on a past date | `price_history` |
| Which GPUs are tracked, and their hourly rental price | `list_gpus` |
| Rental offers for one GPU, by tier | `gpu_rentals` |
| Whether renting GPUs to run an open model beats the API | `self_host_or_api` |

## Answer format

Lead with the answer (provider, price, why it fits), then the caveats from rule 4, then `api_url`. Keep the
reminder short: prices come from public pricing and should be confirmed on the provider's page before
committing to real spend.
