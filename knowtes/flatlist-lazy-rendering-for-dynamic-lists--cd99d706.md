---
id: "cd99d706-7b20-45fb-a421-8f1783e93a64"
title: "FlatList: Lazy Rendering for Dynamic Lists"
tl_dr: "FlatList renders items lazily as you scroll, making it preferable to ScrollView for dynamic or long lists."
created_at: "2026-08-24T00:13:35.974768+00:00"
updated_at: "2026-08-29T14:50:41.812557+00:00"
source: "web"
---

# FlatList: Lazy Rendering for Dynamic Lists

In React Native, **FlatList** is the preferred component for rendering dynamic or large lists. Unlike `ScrollView`, which renders all items at once on mount, `FlatList` uses **lazy (windowed) rendering** — it only renders the items currently visible on screen (plus a small buffer), and discards off-screen items from memory as you scroll.

**Key advantages of FlatList:**
- **Performance**: avoids rendering hundreds of items upfront
- **Memory efficiency**: off-screen items are unmounted
- **Built-in features**: `keyExtractor`, `onEndReached`, `refreshing`, `ListHeaderComponent`, etc.

**When ScrollView is fine:**
- Small, fixed-length lists (e.g. fewer than ~20 static items)
- Content that is not a uniform list (mixed layouts)

**Rule of thumb:** if the list is dynamic, data-driven, or potentially long — always reach for `FlatList`.

## Insight

The core trade-off is render-time cost vs. scroll-time cost. ScrollView pays the full cost upfront; FlatList spreads it over time. For any data-driven list of unknown or large length, FlatList is the safe default — using ScrollView risks janky load times and excessive memory usage as the dataset grows.
