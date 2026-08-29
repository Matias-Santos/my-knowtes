---
id: "c71e75f1-53aa-4de4-a25e-a3c11c057be5"
title: "useState vs useRef: Choosing the Right Hook"
tl_dr: "Use useState for values that drive UI output; use useRef for values that must persist across renders without triggering re-renders."
created_at: "2026-08-24T00:06:18.279762+00:00"
updated_at: "2026-08-29T14:50:09.388293+00:00"
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
