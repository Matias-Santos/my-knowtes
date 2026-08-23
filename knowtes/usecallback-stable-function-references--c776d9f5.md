---
id: "c776d9f5-d831-4d9c-b9cb-1c7472dcbfc3"
title: "useCallback: Stable Function References"
tl_dr: "useCallback prevents memoized child components from re-rendering by keeping the parent's function reference stable across renders."
created_at: "2026-08-23T23:49:50.724027+00:00"
updated_at: "2026-08-23T23:49:50.724041+00:00"
source: "claude-sonnet-4-6"
---

# useCallback: Stable Function References

Every time a parent component renders, any function defined inside it is recreated as a new object in memory. Even if the function's logic hasn't changed, this new reference looks different to React — and to memoized children.

**useCallback** solves this by caching the function reference and only producing a new one when its declared dependencies change.

**Why this matters for memoized children:**
- A child wrapped in `React.memo` skips re-rendering if its props haven't changed
- But if a parent passes a callback prop, and that callback is a new object every render, `React.memo` sees a changed prop and re-renders anyway
- Wrapping the callback in `useCallback` keeps the reference stable → `React.memo` correctly bails out

**Example:**
```js
const handleClick = useCallback(() => {
  doSomething(id);
}, [id]); // only changes when `id` changes
```

Without `useCallback`, `handleClick` is a new function on every parent render, defeating the purpose of `React.memo` on the child.

## Insight

useCallback and React.memo are complementary — React.memo alone is insufficient if the parent passes inline functions as props. Always pair them: wrap child components in React.memo AND wrap the passed callbacks in useCallback, otherwise you're paying the memoization cost with none of the benefit.
