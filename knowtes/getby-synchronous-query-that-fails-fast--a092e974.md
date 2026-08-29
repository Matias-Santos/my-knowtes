---
id: "a092e974-4e59-41e8-85f4-0fb3f4c38922"
title: "getBy: Synchronous Query That Fails Fast"
tl_dr: "getBy queries assert immediate DOM presence — if the element isn't there right now, the test throws."
created_at: "2026-08-27T22:29:50.305094+00:00"
updated_at: "2026-08-29T14:49:13.853997+00:00"
source: "web"
---

# getBy: Synchronous Query That Fails Fast

In Testing Library, `getBy*` queries are **synchronous** and **immediately assertive**. When you call `getByText`, `getByRole`, or any other `getBy` variant, it looks for the element in the current DOM state at that exact moment.

- ✅ **Use getBy when** the element should already be rendered — no waiting, no async
- ❌ **It throws immediately** if the element is absent or if more than one match is found
- This makes it ideal for asserting things that are **unconditionally present** after render

```js
// Element must be on screen right now
const heading = getByRole('heading', { name: /welcome/i });
```

Contrast this with `findBy` (async, returns a Promise) and `queryBy` (returns null instead of throwing).

## Insight

Choosing `getBy` is itself a test assertion — you're declaring the element must exist at this moment. If a test is flaky or throws unexpectedly, it's often a sign you used `getBy` for something that renders asynchronously and should use `findBy` instead.
