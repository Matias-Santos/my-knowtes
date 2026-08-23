---
id: "655cb51b-4368-4ce7-87a3-e37432b6e32e"
title: "useEffect Lifecycle & Cleanup Timing"
tl_dr: "A useEffect runs after mount and on dependency changes; its cleanup runs before each re-run and on unmount to prevent resource leaks."
created_at: "2026-08-23T23:34:35.811560+00:00"
updated_at: "2026-08-23T23:34:48.172277+00:00"
source: "web"
---

# useEffect Lifecycle & Cleanup Timing

## useEffect Lifecycle

**When it runs:**
- After the component **mounts** (initial render)
- After any subsequent render where a **dependency value has changed**

## Cleanup Function

If the effect creates an external resource — such as a **timer**, **event listener**, or **subscription** — it should return a cleanup function.

**When cleanup runs:**
1. **Before the effect runs again** (if a dependency changed and the effect is about to re-execute)
2. **When the component unmounts**

```js
useEffect(() => {
  const timer = setInterval(() => console.log('tick'), 1000);

  return () => clearInterval(timer); // cleanup
}, [someDependency]);
```

This pattern ensures stale resources are torn down before new ones are created, and that nothing persists after the component is removed from the DOM.

## Insight

Forgetting to return a cleanup function is one of the most common sources of memory leaks and stale-closure bugs in React. Every effect that acquires a resource should release it — treat cleanup as mandatory, not optional.
