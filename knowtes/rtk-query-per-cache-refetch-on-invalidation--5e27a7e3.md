---
id: "5e27a7e3-6be1-49b9-b5a8-f9da4e2a0624"
title: "RTK Query: Per-Cache Refetch on Invalidation"
tl_dr: "When a tag is invalidated, RTK Query refetches the affected cache entry — not shared state — and both subscribing components receive the updated result and re-render."
created_at: "2026-09-08T14:45:26.583119+00:00"
updated_at: "2026-09-08T14:45:26.583138+00:00"
source: "claude-sonnet-4-6"
---

# RTK Query: Per-Cache Refetch on Invalidation

When a product is invalidated via `invalidatesTags`, RTK Query does **not** update some shared global state. Instead, it refetches the specific **cache entry** that was marked stale.

- Each cache entry is keyed by endpoint + argument
- Both components subscribed to that cache entry receive the updated result
- Both re-render automatically with the fresh data

The distinction matters: it's cache-entry-level refetching, not a broadcast to shared state.

## Insight

Understanding that invalidation targets cache entries (not shared state) clarifies why two components showing the same data both update — they're both subscribers to the same cache entry, not reading from a separate store slice. This should inform how you structure cache keys and tag granularity.
