---
id: "800e23db-f87c-4065-a85c-4cd13b2b2391"
title: "RTK Query: Core Concepts & How It Works"
tl_dr: "RTK Query is Redux Toolkit's built-in data fetching and caching layer that eliminates boilerplate for server state management."
created_at: "2026-09-03T15:03:10.742203+00:00"
updated_at: "2026-09-03T15:07:03.366664+00:00"
source: "web"
---

# RTK Query: Core Concepts & How It Works

## What is RTK Query?

RTK Query is a data fetching and caching tool built into Redux Toolkit. It removes the need to hand-write thunks, reducers, and loading/error state logic for common server-state use cases.

## Core Concepts

### 1. `createApi`
The central building block. Define one API slice per service, specifying:
- **`reducerPath`** — the Redux store key for this API's state
- **`baseQuery`** — typically `fetchBaseQuery`, which wraps the Fetch API around a `baseUrl`
- **`endpoints`** — a builder function declaring individual queries and mutations

### 2. Endpoints
- **`builder.query`** — reads data (GET); results are cached automatically by endpoint + argument
- **`builder.mutation`** — writes data (POST, PUT, DELETE); can invalidate cached queries to trigger refetches

### 3. Auto-generated Hooks
RTK Query generates a React hook for every endpoint:
- `useGetXQuery()` for queries
- `useUpdateXMutation()` for mutations

Both return `{ data, isLoading, isError, error }` and manage the full request lifecycle.

### 4. Cache Invalidation with Tags
- Use **`providesTags`** on queries to label their cached data.
- Use **`invalidatesTags`** on mutations to bust matching caches, automatically triggering refetches.

### 5. Store Setup
Add the generated reducer and middleware to `configureStore`:

```js
configureStore({
  reducer: {
    [api.reducerPath]: api.reducer,
  },
  middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(api.middleware),
})
```

## Key Benefits
- Eliminates manual loading/error/data state boilerplate
- Built-in caching, request deduplication, and background refetching
- Integrates directly into an existing Redux store with minimal setup

## Insight

Because RTK Query generates hooks, reducers, and middleware from a single `createApi` definition, the highest-leverage habit is designing your tag schema (`providesTags`/`invalidatesTags`) carefully upfront — poor tag design is the most common source of stale data bugs and unnecessary network requests.
