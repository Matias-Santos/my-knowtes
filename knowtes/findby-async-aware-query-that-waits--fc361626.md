---
id: "fc361626-9c1a-4eac-b009-dcb9a2d35121"
title: "findBy: Async-Aware Query That Waits"
tl_dr: "findBy waits for an element to appear in the DOM after an asynchronous operation, unlike getBy which checks immediately."
created_at: "2026-08-27T23:34:50.218905+00:00"
updated_at: "2026-08-29T14:49:15.287699+00:00"
source: "web"
---

# findBy: Async-Aware Query That Waits

## findBy vs getBy: Timing Matters

`findBy` is the async counterpart to `getBy`. It returns a **Promise** that retries the query until the element appears or a timeout is reached.

### When to use each:
- **getBy** — element is already in the DOM at query time (synchronous)
- **findBy** — element will appear *after* an async operation (e.g., API call, state update, animation)

### Example:
```js
// ❌ Fails if element isn't there yet
const el = screen.getByText('Welcome');

// ✅ Waits for element to appear
const el = await screen.findByText('Welcome');
```

`findBy` internally combines `waitFor` + `getBy`, polling until the element is found or the timeout expires (default 1000ms).

## Insight

If your test is querying for something that depends on a fetch, state update, or any async side effect, always reach for findBy — using getBy in those cases will cause flaky or false-negative tests because it checks the DOM before the update has landed.
