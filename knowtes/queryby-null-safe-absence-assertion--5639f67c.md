---
id: "5639f67c-134d-4ccc-95aa-93965fb38263"
title: "queryBy: Null-Safe Absence Assertion"
tl_dr: "queryBy returns null when an element is not found, making it the right choice for asserting something is absent from the DOM."
created_at: "2026-08-27T22:30:33.756026+00:00"
updated_at: "2026-08-27T22:30:33.756037+00:00"
source: "claude-sonnet-4-6"
---

# queryBy: Null-Safe Absence Assertion

When you need to verify that an element does **not** exist in the rendered output, use `queryBy` instead of `getBy`. The key difference is what happens on failure:

- `queryBy` — returns `null` if the element isn't found (no throw)
- `getBy` — throws immediately if the element isn't found

Because `queryBy` returns `null` gracefully, you can write assertions like:

```js
expect(screen.queryByText('Error message')).not.toBeInTheDocument();
```

Using `getBy` for absence checks would cause the test to throw before your assertion even runs, giving you a misleading failure.

## Insight

Choosing the wrong query type inverts your test's failure signal — using `getBy` for absence checks makes the test fail for the wrong reason. Reserve `getBy` for asserting presence, and `queryBy` for asserting absence.
