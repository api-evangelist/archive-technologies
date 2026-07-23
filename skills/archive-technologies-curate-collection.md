---
name: Curate a collection of UGC items
description: Create an Archive collection and populate it with UGC items, including items ingested from a URL.
api: https://app.archive.com/api/v2
operations: [items, itemIdsByUrl, uploadItemFromUrl, createCollection, addItemToCollections]
---

# Curate a collection of UGC items (Archive API)

Use the Archive GraphQL API to build and populate a collection of UGC for a campaign or reporting workflow.

## Setup

- Endpoint: `POST https://app.archive.com/api/v2` (GraphQL).
- Auth: `Authorization: Bearer arch_live_...` plus a `WORKSPACE-ID: <workspace-uuid>` header on every request.
- Rate limit: 5 requests/second per workspace.

## Steps

1. **Create the collection** — run the `createCollection` mutation with a name. Read the returned collection id. Mutations return HTTP 200 even on failure — always inspect the `userErrors` field on the result before proceeding.
2. **Find items to add** — query `items` (filter/search) for existing UGC, or resolve a known post URL to Archive item ids with `itemIdsByUrl`.
3. **Ingest new UGC from a URL** — if the post is not yet in the workspace, run the `uploadItemFromUrl` mutation to ingest it, then take the resulting item id. Handle `userErrors`.
4. **Add items** — run the `addItemToCollections` mutation with the item id(s) and the target collection id(s). To remove, use `removeItemFromCollections`.

## Error handling

- Every mutation may return business errors on `userErrors` while HTTP status is 200 — handle it (e.g. "Item not found", "Collection not found").
- On 429 (`RATE_LIMIT_EXCEEDED`), back off for the seconds named in the message.

See `conventions/archive-technologies-conventions.yml` and `errors/archive-technologies-problem-types.yml` for the full contract.
