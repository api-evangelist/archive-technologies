---
name: Export a workspace's content or creators without burning the credit budget
description: Page a Content View, Collection or creator set out of Archive at the lowest credit cost, self-throttling against the ratelimit response headers instead of reacting to 429s.
api: https://app.archive.com/api/v2
operations: [filterPresets, contentViews, collections, items, creators, socialProfiles, engagementHistory]
---

# Export from Archive without burning the credit budget

Archive enforces **two** limits at once — a flat **5 requests/second** per workspace and a
**per-plan credit budget** that refills every second. Whichever you reach first stops you. One
credit is one millisecond of backend compute, so cost follows the work a request causes, not the
size of the response. This skill is the export path that respects both.

## Setup

- Endpoint: `POST https://app.archive.com/api/v2` (GraphQL, single endpoint).
- Headers on every request: `Authorization: Bearer <arch_live_ token>` and `WORKSPACE-ID: <workspace uuid>`. The workspace goes in the **header**, never the body.
- Schema of record: `graphql/archive-technologies.graphql`. Introspection is disabled in production — you cannot discover fields at runtime.
- Max query depth 10; max page size 100.

## Read your own budget first — do not guess it

Every response carries the IETF rate-limit headers. Read them off the **first** call and plan the job:

```
ratelimit-policy: "growth";q=15000;w=60      # q = burst, w = refill window; q/w = 250 credits/sec
ratelimit: "growth";r=14203;t=1785409320     # r = credits remaining now
```

The quoted name is the plan. Throttle against `r`. Discovering the ceiling with a 429 is the
failure mode these headers exist to prevent.

## Steps

1. **Resolve the preset id.** Query `filterPresets` (Content Views + Collections with their ids),
   or `contentViews` / `collections` for the full definition. `items(presetId:)` only accepts a
   preset whose accessor is `media_deck` or `collections` — anything else raises
   `extensions.code = "WRONG_VIEW_TYPE_FOR_ITEMS"`.
2. **Page at `first: 100`. Always.** Every request costs at least 5 credits however little it
   returns. An `items` page of 100 costs 60 credits; the same 100 posts fetched one at a time cost
   500. Same data, eight times the price.
3. **Page the content** — `items(first: 100, presetId: "...")`, following
   `pageInfo.endCursor` into `after` while `pageInfo.hasNextPage`. Select only the relations you
   need: a full `items` page with every relation costs ~183 credits against 60 for a lean one.
4. **Or page the creators** — `creators(first: 100)` costs 90 credits per page. Adding
   `customAttributeConditions` costs **340**. Passing a `presetId` incurs the same surcharge, so a
   saved view does not avoid it. Keep the filter server-side only when it returns a small share of
   the workspace — the documented crossover is 90 ÷ 340, about one in four. Above that, fetch
   everything and filter locally.
5. **Fetch engagement deliberately.** `engagementHistory` takes a single `itemId`, not a batch, so
   it fans out into one request per item. At 6 credits/page it is the cheapest call available —
   here the **5-requests-per-second ceiling** binds, not credits. Budget it as request count.
6. **Batch what can be batched.** `mediaContents(itemIds: [...])` and `itemIdsByUrl(urls: [...])`
   both take up to 100 ids/urls per call. Use them instead of looping.
7. **Pace the run.** Run a token-bucket limiter on your side set to your plan's refill rate
   (`q ÷ w`), shared across every process touching that workspace. Retries with backoff add
   latency without raising your rate.

## Error handling

- Most failures return **HTTP 200** with a top-level `errors[]` array — check the body, not just
  the status. Only four cases move the status: 401 (bad token, `errors` is a **string**), 404 (bad
  `WORKSPACE-ID`, also a string), 429, and 5xx.
- On 429, honour `Retry-After` (or the seconds in the message). A batch sync that honours it takes
  longer and loses nothing; only a client that ignores 429 fails.
- Dates must be UTC — ending `Z`, `+00:00` or `-00:00`. A real offset like `-03:00` is rejected,
  and arrives as `INTERNAL_SERVER_ERROR` even though the input is at fault.
- BigInt engagement fields (likes, views, comments, shares, EMV) arrive as **strings**. Cast them.

Full contract: `conventions/archive-technologies-conventions.yml`,
`rate-limits/archive-technologies-rate-limits.yml`, `errors/archive-technologies-problem-types.yml`.
