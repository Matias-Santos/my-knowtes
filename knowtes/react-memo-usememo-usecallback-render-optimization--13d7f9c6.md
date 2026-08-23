---
id: "13d7f9c6-846b-4a44-9d42-84ebec6f6db8"
title: "React.memo, useMemo & useCallback: Render Optimization"
tl_dr: "React.memo prevents unnecessary child re-renders, while useMemo and useCallback preserve stable references across renders."
created_at: "2026-08-23T23:45:12.614371+00:00"
updated_at: "2026-08-23T23:46:17.099012+00:00"
source: "web"
---

# React.memo, useMemo & useCallback: Render Optimization

React provides three complementary tools for render optimization:

**React.memo**
- A higher-order component that wraps a child component
- Performs a shallow comparison of props before re-rendering
- If props haven't changed, the child render is skipped entirely
- Best used for pure components that receive the same props frequently

**useMemo**
- Preserves the reference of a computed **value** (e.g., an object or array) between renders
- Without it, a new object is created on every render, breaking shallow equality checks
- Works as a natural companion to React.memo — stabilize prop values before passing them down

**useCallback**
- Preserves the reference of a **function** between renders
- Without it, a new function instance is created on every render
- Also pairs with React.memo — if you pass a callback as a prop, useCallback prevents the child from seeing it as a new prop

## Insight

These three tools form a system: React.memo sets the gate, while useMemo and useCallback supply the stable keys that keep that gate from triggering unnecessarily. Using React.memo alone is often ineffective if object or function props are recreated on every render — all three must be used together intentionally.
