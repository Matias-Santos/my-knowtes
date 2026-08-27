---
id: "b90c3455-bbd4-4976-9059-5971c2deb0ce"
title: "useRef: Persistent Mutable Value Without Re-renders"
tl_dr: "useRef persists a mutable value across renders via .current without triggering re-renders when it changes."
created_at: "2026-08-24T00:02:28.628383+00:00"
updated_at: "2026-08-27T21:20:20.381412+00:00"
source: "web"
---

# useRef: Persistent Mutable Value Without Re-renders

A `useRef` returns a plain object `{ current: ... }` that React keeps stable across renders. Unlike state, mutating `.current` does **not** cause a re-render — React simply doesn't track it.

**Key characteristics:**
- The ref object itself is stable (same reference every render)
- `.current` is mutable and can hold any value
- Changes to `.current` are synchronous and immediate
- No re-render is scheduled when `.current` changes

**Common use cases:**
- Storing a previous value of props or state
- Holding a timer ID (e.g., `setTimeout` / `setInterval`)
- Keeping a flag (e.g., `hasMounted`, `isFetching`)
- Accessing DOM elements directly

```jsx
const intervalRef = useRef(null);

useEffect(() => {
  intervalRef.current = setInterval(() => {
    console.log('tick');
  }, 1000);

  return () => clearInterval(intervalRef.current);
}, []);
```

The golden rule: if a value needs to **drive the UI**, use `useState`. If it just needs to be **remembered** without affecting rendering, use `useRef`.

## Insight

The distinction between state and refs maps directly to the distinction between reactive and imperative data. Reaching for useRef when a value doesn't need to affect the UI avoids unnecessary re-renders and keeps your component's reactive surface area minimal — which simplifies reasoning and improves performance.
