---
id: "830a7f75-8a5e-4c50-8eb5-31c05f9d68f4"
title: "useState vs useRef: Choosing the Right Hook"
tl_dr: "Use useState for values that drive UI output; use useRef for values that must persist across renders without triggering re-renders."
created_at: "2026-08-24T00:06:16.676072+00:00"
updated_at: "2026-08-24T00:39:10.454232+00:00"
source: "web"
---

# useState vs useRef: Choosing the Right Hook

A core decision in React is knowing which persistence mechanism to reach for:

**useState**
- Use when the value is directly tied to what the user sees
- Any change should cause the component to re-render and reflect updated UI
- Examples: form input values, toggle states, fetched data displayed in the DOM

**useRef**
- Use when the value needs to survive across renders, but changing it should NOT cause a re-render
- Mutations are immediate and synchronous via `.current`, but React is unaware of them
- Examples: tracking a timer ID, storing a previous value, holding a DOM reference

**The decision rule:**
> If the UI needs to react to it → `useState`. If it just needs to remember it → `useRef`.

## Insight

Conflating these two hooks is a common source of bugs: using useState for a value that doesn't need to trigger re-renders causes unnecessary renders and performance degradation; using useRef for a value that should update the UI causes stale or invisible updates. Applying this rule explicitly at the point of variable declaration keeps components lean and correct.
