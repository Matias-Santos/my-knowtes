---
id: "76effd22-1779-45a1-af50-9cb8e28808ab"
title: "useMemo vs useCallback: Key Distinction"
tl_dr: "useMemo returns a cached computed value, while useCallback returns a cached function reference — both recompute only when dependencies change."
created_at: "2026-08-23T23:39:36.103850+00:00"
updated_at: "2026-08-23T23:39:46.057519+00:00"
source: "web"
---

# useMemo vs useCallback: Key Distinction

## useMemo vs useCallback

### `useMemo`
- Returns the **result** of a calculation (a value)
- Use when you want to avoid expensive recomputations
- Example: `const total = useMemo(() => computeExpensiveValue(a, b), [a, b])`
- You get back: `total` → a number, object, array, etc.

### `useCallback`
- Returns the **function itself** (a memoized reference)
- Use when you want to avoid recreating a function on every render
- Example: `const handleClick = useCallback(() => doSomething(a), [a])`
- You get back: `handleClick` → a stable function reference

### What they share
- Both accept a dependency array
- Both **recompute / recreate** their memoized value or function when dependencies change
- Both are optimization tools to maintain referential stability across renders

### Mental model
> `useMemo(() => fn(), deps)` = memoized **value**
> `useCallback(fn, deps)` = memoized **function**
> In fact, `useCallback(fn, deps)` is equivalent to `useMemo(() => fn, deps)`

## Insight

The distinction matters most when passing things to child components or other hooks: pass a memoized value with useMemo when a child needs data, and a memoized function with useCallback when a child needs a handler — preventing unnecessary re-renders in both cases.
