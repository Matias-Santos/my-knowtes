---
id: "86947e86-8367-4f1d-8c15-fa18d67b5bd8"
title: "React Native Testing Library: Functions & Utilities"
tl_dr: "A practical reference for all React Native Testing Library functions and utilities, and how to apply them in any test."
created_at: "2026-09-03T13:52:17.388062+00:00"
updated_at: "2026-09-03T13:53:52.204206+00:00"
source: "web"
---

# React Native Testing Library: Functions & Utilities

## Overview
React Native Testing Library (RNTL) provides a set of utilities designed to test React Native components in a way that resembles how users interact with the app.

## Core Functions

### Rendering
- **`render(component)`** — Mounts a component and returns query and utility helpers scoped to it.
- **`rerender(component)`** — Re-renders the mounted component with new props.
- **`unmount()`** — Unmounts the component, useful for testing cleanup logic.

### Querying Elements
Queries follow the pattern `getBy`, `queryBy`, `findBy` (and their `AllBy` variants):
- **`getBy*`** — Returns the element or throws if not found. Use when the element must exist.
- **`queryBy*`** — Returns the element or `null`. Use to assert absence.
- **`findBy*`** — Returns a Promise. Use for elements that appear asynchronously.

Common selectors: `ByRole`, `ByText`, `ByPlaceholderText`, `ByDisplayValue`, `ByTestId`, `ByLabelText`.

### User Events & Interactions
- **`fireEvent(element, eventName)`** — Fires a synthetic event on an element.
- **`fireEvent.press(element)`**, **`fireEvent.changeText(element, value)`** — Shorthand helpers for common events.
- **`userEvent`** (RNTL v12+) — Higher-level API that simulates realistic user interactions, including pointer and keyboard events.

### Async Utilities
- **`waitFor(callback)`** — Retries the callback until it passes or times out. Essential for async state updates.
- **`waitForElementToBeRemoved(callback)`** — Waits until the queried element disappears from the tree.

### Debugging
- **`screen.debug()`** — Prints the current component tree to the console.
- **`logRoles()`** — Lists all ARIA roles present in the rendered output, helpful for choosing the right query.

### Accessibility Queries (recommended approach)
Prefer `ByRole` and `ByLabelText` queries — they align with accessibility semantics and produce more resilient tests.

## General Usage Pattern
```js
import { render, screen, fireEvent } from '@testing-library/react-native';

test('shows confirmation on submit', () => {
  render(<MyForm />);
  fireEvent.changeText(screen.getByPlaceholderText('Name'), 'Santos');
  fireEvent.press(screen.getByRole('button', { name: /submit/i }));
  expect(screen.getByText('Confirmed!')).toBeTruthy();
});
```

## Insight

Prioritising `ByRole` and `ByLabelText` queries over `ByTestId` keeps tests tied to accessibility semantics, making them less brittle to implementation changes and surfacing a11y issues as a side effect of writing tests.
