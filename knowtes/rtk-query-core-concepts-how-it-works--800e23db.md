---
id: "800e23db-f87c-4065-a85c-4cd13b2b2391"
title: "RTK Query: Core Concepts & How It Works"
tl_dr: "RTK Query is Redux Toolkit's built-in data fetching and caching layer that eliminates boilerplate for server state management."
created_at: "2026-09-03T15:03:10.742203+00:00"
updated_at: "2026-09-03T15:04:03.564314+00:00"
source: "web"
---

# RTK Query: Core Concepts & How It Works

## What is RTK Query?

RTK Query is a powerful data fetching and caching tool built into Redux Toolkit (RTK). It is designed to simplify the common cases of loading data in a web app, eliminating the need to hand-write thunks, reducers, and loading/error state logic.

## Core Concepts

### 1. `createApi`
The central piece of RTK Query. You define a single API slice using `createApi`, specifying:
- **`reducerPath`** — the key under which the API state lives in the Redux store
- **`baseQuery`** — typically `fetchBaseQuery`, which wraps the Fetch API and accepts a `baseUrl`
- **`endpoints`** — a builder function where you define individual queries and mutations

### 2. Endpoints
- **Query** (`builder.query`) — for fetching/reading data (GET). Results are cached automatically.
- **Mutation** (`builder.mutation`) — for writing/changing data (POST, PUT, DELETE). Can invalidate cached queries to trigger refetches.

### 3. Auto-generated Hooks
For each endpoint, RTK Query automatically generates React hooks:
- `useGetXQuery()` for queries
- `useUpdateXMutation()` for mutations

These hooks return `{ data, isLoading, isError, error }` and handle the full request lifecycle.

### 4. Caching & Invalidation
- Query results are cached by endpoint + argument.
- **Tags** (`providesTags` / `invalidatesTags`) control cache invalidation: a mutation can invalidate a query's cache, causing it to refetch automatically.

### 5. Store Setup
The generated API reducer and middleware must be added to the Redux store:
```js
store.reducer[api.reducerPath] = api.reducer
configureStore({ middleware: (getDefault) => getDefault().concat(api.middleware) })
```

## Key Benefits
- Eliminates manual loading/error/data state boilerplate
- Automatic caching, deduplication, and background refetching
- Seamless integration with existing Redux store

## Insight

RTK Query's tag-based cache invalidation is the most important pattern to internalize — getting it right means mutations automatically keep UI in sync with the server without manual refetch calls, which is the primary source of bugs in hand-rolled Redux data fetching.
