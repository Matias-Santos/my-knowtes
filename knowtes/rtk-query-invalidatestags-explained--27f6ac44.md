---
id: "27f6ac44-2b3f-46c9-8154-7b46003438e7"
title: "RTK Query: invalidatesTags Explained"
tl_dr: "invalidatesTags triggers automatic refetching by marking cached data as stale when a mutation succeeds."
created_at: "2026-09-03T15:08:19.727443+00:00"
updated_at: "2026-09-03T15:09:15.011491+00:00"
source: "web"
---

# RTK Query: invalidatesTags Explained

## How invalidatesTags Works

In RTK Query, cache entries are tagged using `providesTags` on query endpoints. When a mutation endpoint defines `invalidatesTags`, RTK Query will automatically refetch any active query whose `providesTags` overlap with the invalidated tags — as soon as the mutation completes successfully.

## Basic Pattern

```ts
const api = createApi({
  endpoints: (builder) => ({
    getUsers: builder.query({
      query: () => '/users',
      providesTags: ['User'],
    }),
    addUser: builder.mutation({
      query: (newUser) => ({
        url: '/users',
        method: 'POST',
        body: newUser,
      }),
      invalidatesTags: ['User'],
    }),
  }),
});
```

Here, calling `addUser` invalidates the `'User'` tag, which causes `getUsers` to refetch automatically.

## Granular Invalidation

Tags can include an `id` for fine-grained control:

- `providesTags: (result) => result.map(user => ({ type: 'User', id: user.id }))` — tags each individual item.
- `invalidatesTags: (result, error, arg) => [{ type: 'User', id: arg.id }]` — only refetches the specific item that changed, not the entire list.

Using `{ type: 'User', id: 'LIST' }` as a sentinel tag is a common convention to separately track the list vs. individual items.

## Key Behaviours

- Invalidation only triggers refetch if the query is currently **subscribed** (i.e., actively rendered).
- If no component is subscribed, the cache entry is simply marked stale and will refetch on next use.
- Errors in the mutation do **not** trigger invalidation by default.

## Insight

Using granular `{ type, id }` tags instead of broad string tags prevents unnecessary network requests — only the affected data is refetched. Audit your mutations to ensure you're not over-invalidating (e.g., refetching entire lists when only one item changed), as this is a common performance pitfall when first adopting RTK Query.
