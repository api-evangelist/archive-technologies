---
name: Discover a creator's UGC and engagement
description: Find a creator in an Archive workspace, resolve their social profiles, and pull their UGC items with engagement metrics.
api: https://app.archive.com/api/v2
operations: [creators, socialProfiles, items, engagementHistory]
---

# Discover a creator's UGC and engagement (Archive API)

Use the Archive GraphQL API to locate a creator and retrieve the content and engagement Archive has tracked for them.

## Setup

- Endpoint: `POST https://app.archive.com/api/v2` (GraphQL).
- Auth: send `Authorization: Bearer arch_live_...` and a `WORKSPACE-ID: <workspace-uuid>` header on every request. The `WORKSPACE-ID` goes in the header, never the body.
- Rate limit: 5 requests/second per workspace. Shape traffic client-side; a 429 (`RATE_LIMIT_EXCEEDED`) means you exceeded it.
- Max query depth is 10; introspection is disabled in production.

## Steps

1. **Find the creator** — query `creators` filtered by name, email, or custom attribute. Page with `first` (max 100) and `after` using `pageInfo.endCursor`.
2. **Resolve social accounts** — from the creator, read `socialProfiles` (or query `socialProfiles` by handle/id/url) to get their Instagram/TikTok/YouTube accounts.
3. **Pull UGC** — query `items` scoped to those social profiles to list posts, reels, stories, and videos with captions.
4. **Read engagement** — query `engagementHistory` for the items/profiles to get likes, views, comments, shares, EMV, and virality over time. These BigInt fields arrive as **strings** — cast them to numbers explicitly.

## Error handling

- Query errors appear in the top-level `errors[]` array even on HTTP 200 (`extensions.code` e.g. `undefinedField`, `UNAUTHORIZED`). Inspect it before using `data`.
- On 429, wait the seconds named in the error message, then retry.

See `conventions/archive-technologies-conventions.yml` and `errors/archive-technologies-problem-types.yml` for the full contract.
