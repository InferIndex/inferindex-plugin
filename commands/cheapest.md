---
description: Find the cheapest provider for an LLM model, with live prices from InferIndex
argument-hint: <model> [constraints, e.g. "eu, 128k context, tools"]
---

Find the cheapest API offer for this model: $ARGUMENTS

1. If the model isn't an exact id (`org/model`), call `search_models` first and pick the exact id. If the match
   is `ambiguous`, say which model you used and list the alternatives.
2. Call `cheapest` with that id. Translate any constraints from the request into tool parameters
   (`region`, `min_context`, `tools`, `json`, `vision`, `no_training`, `no_waitlist`; `strict: true` if the user
   needs them guaranteed).
3. Answer with the cheapest offer (provider, input/output price per 1M tokens, blended price) and the next two
   offers, then flag any promotion, `promo_expired`, `points_based`, `stale`, `below_official_list`, non-standard
   `tier` or `peak_pricing`. Give `api_url`.

Never state a price that isn't in the tool result.
