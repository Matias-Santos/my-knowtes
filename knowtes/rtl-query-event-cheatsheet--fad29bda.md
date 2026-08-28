---
id: "fad29bda-2e21-4af4-98ed-a945c804308f"
title: "RTL Query & Event Cheatsheet"
tl_dr: "Match the right RTL query or event utility to the scenario: getBy for immediate presence, findBy for async appearance, queryBy for absence, userEvent for interactions, and waitFor for deferred expectations."
created_at: "2026-08-28T13:08:42.445901+00:00"
updated_at: "2026-08-28T13:08:42.445918+00:00"
source: "claude-sonnet-4-6"
---

# RTL Query & Event Cheatsheet

## React Testing Library: Query & Async Strategy

### Query Methods
- **getBy** — use when the element must be in the DOM *right now*. Throws immediately if not found.
- **findBy** — use when the element appears *after* an asynchronous operation (e.g., data fetch, state update). Returns a promise.
- **queryBy** — use when asserting an element is *absent*. Returns `null` instead of throwing, making it safe for negative assertions.

### Interaction & Async Utilities
- **userEvent** — simulates realistic user interactions (click, type, etc.). Prefer over `fireEvent` for async flows; works well when testing successful async outcomes.
- **mockResolvedValue / mockRejectedValue** — use `mockRejectedValue` to simulate async errors (e.g., failed API calls) and test error states.
- **waitFor** — use when you need to wait for an *expectation* (e.g., `expect(...).not.toBeInTheDocument()`) to pass, not just for an element to appear.

### Decision Flow
1. Element present now? → `getBy`
2. Element appears after async? → `findBy`
3. Element should be gone? → `queryBy` + `waitFor` if timing is involved
4. Testing async success? → `userEvent` + `mockResolvedValue`
5. Testing async error? → `userEvent` + `mockRejectedValue`
6. Waiting on a complex assertion? → `waitFor`

## Insight

Choosing the wrong query type is one of the most common RTL mistakes — using getBy for async elements causes flaky tests, while using findBy for absence checks gives false confidence. Mapping each scenario to its correct utility makes tests both reliable and expressive.
