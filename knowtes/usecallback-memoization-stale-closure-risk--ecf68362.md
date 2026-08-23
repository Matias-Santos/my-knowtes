---
id: "ecf68362-d4de-428e-884f-11a8f0f981ae"
title: "useCallback: Memoization & Stale Closure Risk"
tl_dr: "useCallback memoizes a function across renders, but incorrect dependencies can trap stale values inside a closure."
created_at: "2026-08-23T23:52:49.056082+00:00"
updated_at: "2026-08-23T23:52:57.331602+00:00"
source: "web"
---

# useCallback: Memoization & Stale Closure Risk

## How useCallback Works

`useCallback` returns a memoized version of a function that only changes when one of its declared dependencies changes. This keeps the function reference stable across renders.

## The Stale Closure Problem

Because the memoized function is created at a point in time, it **closes over** the values of any variables it references at that moment. If a dependency is missing from the dependency array:

- The function is not recreated when that value changes
- The function continues to use the **old, stale value** from when it was first created
- This is called a **stale closure**

## Example

```js
const handleClick = useCallback(() => {
  console.log(count); // may log stale value if count is missing from deps
}, []); // ⚠️ count is not listed as a dependency
```

## How to Avoid It

- Always include every reactive value the function reads in the dependency array
- Use the `eslint-plugin-react-hooks` exhaustive-deps rule to catch missing dependencies automatically

## Insight

The power of useCallback comes with a subtle danger — omitting dependencies doesn't just cause bugs, it causes *silent* bugs where the function appears to work but operates on outdated state. Treat the dependency array as a contract: everything the function touches must be declared.
