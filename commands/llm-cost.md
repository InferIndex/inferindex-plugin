---
description: Estimate what an LLM workload will cost per request and per month, across providers
argument-hint: <model> <input tokens> <output tokens> [requests per day] [cached share]
---

Estimate the cost of this LLM workload: $ARGUMENTS

1. Identify the model, input tokens per request, output tokens per request, and if given the number of requests
   per day and the share of input read from cache (0–1). Ask only for what's missing and needed: the model and at
   least one token count.
2. If the model isn't an exact id (`org/model`), call `search_models` first; mention it if the match is
   `ambiguous`.
3. Call `estimate_cost` with `prompt_tokens`, `output_tokens`, and `cached_ratio` / `requests_per_day` when known.
4. Show the three cheapest offers: provider, estimated cost per request, and per month (30 days) when
   `requests_per_day` was given. Flag `estimate_approx` with its `estimate_notes` (e.g. pricing brackets not
   fully published, peak pricing, context exceeded), plus `points_based`, `stale` and promotions. Give `api_url`.

Estimates use listed token prices only (no taxes, minimum charges or free quotas). Never state a number that isn't
in the tool result.
