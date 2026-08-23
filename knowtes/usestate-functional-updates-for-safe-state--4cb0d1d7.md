---
id: "4cb0d1d7-8821-4e2d-840f-e1427463c9ef"
title: "useState: Functional Updates for Safe State"
tl_dr: "When next state depends only on previous state, use a functional update (e.g., setCount(c => c + 1)) to avoid stale state and reduce dependencies."
created_at: "2026-08-23T23:58:54.481716+00:00"
updated_at: "2026-08-23T23:58:54.481728+00:00"
source: "claude-sonnet-4-6"
---

# useState: Functional Updates for Safe State

## Functional Updates in useState

When your next state value is derived purely from the previous state, pass a **function** to the state setter instead of a direct value:

```js
// ❌ Direct update — can read stale state
setCount(count + 1);

// ✅ Functional update — always reads latest state
setCount(c => c + 1);
```

### Why it matters

- **Stale state risk**: If `count` is captured in a closure that hasn't re-rendered, `count + 1` may use an outdated value.
- **Fewer dependencies**: When using `useCallback` or `useEffect`, a functional update means you no longer need `count` in the dependency array — the setter itself is stable.

```js
// Without functional update — count must be a dependency
const increment = useCallback(() => setCount(count + 1), [count]);

// With functional update — no dependency needed
const increment = useCallback(() => setCount(c => c + 1), []);
```

This keeps the callback reference stable and avoids unnecessary re-renders in memoized children.

## Insight

Functional updates are a small syntactic shift with a large architectural payoff: they decouple state logic from closure scope, making memoized callbacks truly stable and eliminating an entire class of stale-state bugs.
