---
id: "9b2cd8d6-a019-4af9-96ef-9e6281498f8e"
title: "React.memo: Referential Stability Requirement"
tl_dr: "React.memo only prevents re-renders when props are referentially stable — inline objects, arrays, or functions created on each render will defeat it."
created_at: "2026-08-24T00:27:01.724747+00:00"
updated_at: "2026-08-24T00:39:06.054630+00:00"
source: "web"
---

# React.memo: Referential Stability Requirement

## React.memo and Referential Equality

`React.memo` wraps a component and bails out of re-rendering if its props haven't changed. However, React uses **referential equality** (`===`) to compare props, not deep equality.

### What defeats React.memo
- **Inline objects**: `style={{ color: 'red' }}` creates a new object reference on every parent render
- **Inline arrays**: `items={[1, 2, 3]}` is a fresh array each time
- **Inline functions**: `onPress={() => doSomething()}` is a new function reference every render

In all three cases, the prop technically *changed* from React's perspective — even if the value looks identical — so `React.memo` will not bail out and the child re-renders anyway.

### How to fix it
- Hoist static objects/arrays outside the component
- Use `useMemo` for computed objects or arrays
- Use `useCallback` for function props passed to memoized children

## Insight

React.memo is only as effective as the referential stability of the props you pass. Wrapping a component in memo without also stabilizing its props with useMemo and useCallback is a false optimization — you get the overhead of memo checks with none of the savings.
