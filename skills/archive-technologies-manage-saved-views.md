---
name: Save a search as a view and organise the workspace sidebar
description: Turn an ad-hoc Archive search into a saved Content, Creator or Social Profile View, group views and Collections in the sidebar, and read them back — including the two documented cases where a view's filters are stored but not applied.
api: https://app.archive.com/api/v2
operations: [contentViews, creatorViews, socialProfileViews, viewGroups, collections, createContentView, updateContentView, deleteContentView, createCreatorView, createSocialProfileView, createViewGroup, moveContentViewToGroup, moveCollectionToGroup, reorderViewsInGroup]
---

# Save a search as a view, and organise the sidebar (Archive API)

Archive's saved views — Content Views (media decks), Creator Views and Social Profile Views —
are the same objects the Social Listening sidebar shows, and since 2026-07 they are fully
manageable from code. View groups and Collections are grouped by the same mechanism.

## Setup

- Endpoint: `POST https://app.archive.com/api/v2`.
- Headers: `Authorization: Bearer <arch_live_ token>` + `WORKSPACE-ID: <workspace uuid>`.
- A mutation costs ~55 credits and a write is a single operation, so page size does not apply.
  Startup plans sustain about one write per second; see
  `rate-limits/archive-technologies-rate-limits.yml`.

## Steps

1. **Compose the search first.** Build and test the filter with `items(filter:, sorting:,
   customAttributeConditions:)` or `creators(...)` until it returns what you want. The filter
   inputs used for reading are the same ones the view takes, so a working search saves in one call.
2. **Save it.**
   - `createContentView` for a media deck over items.
   - `createSocialProfileView` for a view over social profiles.
   - `createCreatorView` for a view over creators — this one narrows by
     `customAttributeConditions`, **not** by a free-form `filters` blob.
3. **Read views back** with `contentViews`, `creatorViews`, `socialProfileViews` — each returns
   the full filter definition plus the id you feed to `items(presetId:)` /
   `creators(presetId:)`.
4. **Update partially.** `updateContentView` / `updateCreatorView` / `updateSocialProfileView`
   are partial updates — omitted fields keep their values. Do not read-modify-write the whole
   object.
5. **Group the sidebar.** `createViewGroup`, then `moveContentViewToGroup` /
   `moveCreatorViewToGroup` / `moveSocialProfileViewToGroup` to move views in or out, and
   `reorderViewsInGroup` to order them.
6. **Group Collections too** — `moveCollectionToGroup(collectionId: ID!, groupId: ID)`. Note it
   takes a `collection_id`, not a `view_id`. Verify the move by reading `Collection.group` or
   `ViewGroup.collections`. Pass `groupId: null` to move a Collection out of its group.
7. **Delete** with `deleteContentView` / `deleteCreatorView` / `deleteSocialProfileView` /
   `deleteViewGroup`. A delete removes the saved view or group only — never the underlying
   content, creators or media.

## Two documented traps

- **A Social Profile View's `filters` blob is stored but ignored on read.** Its
  `customAttributeConditions` (labels, post date, other CRM conditions, including the view's
  stored timezone) apply correctly, but a view narrowing by `followersCount` or `verified`
  returns **every** profile in the workspace. There is no inline workaround —
  `SocialProfileFilterInput` accepts only `platform`. Archive lists the fix under "Coming soon".
- **`filterPresets` today returns only Content Views and Collections**, not Social Profile or
  Creator views. Read those from `socialProfileViews` / `creatorViews` instead.

## Error handling

- Mutation business errors come back in `userErrors` inside the mutation result on **HTTP 200**.
  `"userErrors": []` is the success signal — check it on every write.
- `items(presetId:)` rejects a preset with the wrong accessor, cross-workspace, or unknown, with
  `extensions.code = "WRONG_VIEW_TYPE_FOR_ITEMS"`.
- Query errors arrive in the top-level `errors[]` array on HTTP 200; `undefinedField` means the
  field is not in the schema — check `graphql/archive-technologies.graphql`, since introspection
  is disabled in production.

Full contract: `conventions/archive-technologies-conventions.yml`,
`errors/archive-technologies-problem-types.yml`.
