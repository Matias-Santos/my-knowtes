---
id: "0ef076eb-704e-4c92-936b-2b2a08086109"
title: "React useEffect Dependency Array"
tl_dr: "The dependency array tells React which reactive values trigger re-synchronization of an effect when they change."
created_at: "2026-08-23T23:23:39.533087+00:00"
updated_at: "2026-08-24T12:07:23.378865+00:00"
source: "web"
---

# React useEffect Dependency Array

## React `useEffect` Dependency Array

The dependency array is the second argument passed to `useEffect`. It serves as a contract between your code and React, declaring which reactive values the effect relies on.

### How it works
- **React tracks** the values listed in the dependency array.
- **When any value changes** between renders, React treats the effect as "out of sync" and re-runs it.
- **When nothing changes**, React skips the effect entirely, avoiding unnecessary work.

### Variants
| Dependency Array | Behavior |
|---|---|
| `[a, b]` | Re-runs when `a` or `b` changes |
| `[]` | Runs once after the initial render |
| *(omitted)* | Runs after every render |

### What counts as a reactive value?
- Props
- State variables
- Variables and functions declared inside the component body

### Key principle
Every reactive value used inside the effect **must** be listed in the dependency array. Missing dependencies can cause stale closure bugs where the effect reads outdated values.

## Insight

The dependency array is not just a performance optimization — it is a correctness mechanism. Treating it carelessly leads to subtle bugs where effects operate on stale data. Always let the React linter (eslint-plugin-react-hooks) enforce exhaustive dependencies, and rethink your effect's design if the dependency list feels overwhelming.
