---
id: "ecf68362-d4de-428e-884f-11a8f0f981ae"
title: "useCallback: Memoization & Stale Closure Risk"
tl_dr: "useCallback memoizes a function across renders, but missing dependencies can trap stale values inside a closure."
created_at: "2026-08-23T23:52:49.056082+00:00"
updated_at: "2026-09-02T13:31:57.172144+00:00"
source: "web"
---

# useCallback: Memoization & Stale Closure Risk

## How useCallback Works

`useCallback` returns a memoized version of a function that only recreates when one of its declared dependencies changes, keeping the function reference stable across renders.

## The Stale Closure Problem

The memoized function closes over the values of any variables it references **at the time it was created**. If a dependency is omitted from the dependency array:

- The function is never recreated when that value changes
- It continues reading the **old, stale value** from its original creation
- This bug is called a **stale closure**

## Example

```js
const handleClick = useCallback(() => {
  console.log(count); // ⚠️ logs stale value — count missing from deps
}, []); // count should be listed here
```

## How to Avoid It

- Include **every reactive value** the function reads in the dependency array
- Enable the `exhaustive-deps` rule from `eslint-plugin-react-hooks` to catch omissions automatically at lint time

## Insight

Stale closures are one of the most common silent bugs in React — the function appears to work but operates on outdated state. Treat the `exhaustive-deps` ESLint rule as non-negotiable on any project using hooks; suppressing it should require an explicit, documented justification.
