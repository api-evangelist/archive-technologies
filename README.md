# Archive Technologies

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Archive (Archive Technologies, Inc.) is an AI-powered creator and community marketing platform for e-commerce brands, founded in 2021 and headquartered in Miami, FL. Archive automates social listening across TikTok, Instagram, and YouTube, creator discovery, UGC rights and repurposing, campaign tracking, and reporting. Its public developer surface is the Archive API — a GraphQL API at POST https://app.archive.com/api/v2 that gives teams programmatic access to workspace data (creators, social profiles, UGC items, engagement history, content views, collections, campaigns, and competitor brands) authenticated with a workspace-scoped bearer token plus a WORKSPACE-ID header. The schema is published as a full reference (53 operations — 29 queries and 24 mutations — over 135 types) even though runtime introspection is disabled. Archive also operates an OAuth-protected remote MCP server at https://app.archive.com/api/v2/mcp whose 53 tools map 1:1 to those GraphQL operations, and both surfaces draw on the same per-workspace credit budget alongside a flat 5 requests/second ceiling, signalled at runtime with IETF ratelimit headers. Archive is backed by Battery Ventures among others.

**Areas:** Company · Creator Marketing · Influencer Marketing · Social Listening · User Generated Content · E-Commerce · GraphQL · MCP

## Archive Technologies on APIs.io

Everything profiled for Archive Technologies is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/archive-technologies/](https://apis.io/providers/archive-technologies/)** — or go straight to one of the APIs or properties below.

### APIs (1)

- [Archive API](https://apis.io/apis/archive-technologies/archive-api/) — Archive's public GraphQL API for programmatic access to creator and UGC workspace data — creators, social profiles, items (posts/reels/stories/videos),…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [archive](https://apis.io/servers/archive-technologies/archive/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Archive Technologies Authentication](https://apis.io/security/archive-technologies/archive-technologies-authentication/)
- [Archive Technologies Domain Security](https://apis.io/security/archive-technologies/archive-technologies-domain-security/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Archive Technologies Scopes](https://apis.io/scopes/archive-technologies/archive-technologies-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Archive Technologies Plans Pricing](https://apis.io/plans/archive-technologies/archive-technologies-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Archive Technologies Rate Limits](https://apis.io/rate-limits/archive-technologies/archive-technologies-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.archive.com/)
- [Documentation](https://api-docs.archive.com)
- [Getting started](https://api-docs.archive.com/getting-started)
- [API reference](https://app.archive.com/api/v2/docs)
- [Pricing](https://archive.com/pricing)
- [Support](https://help.archive.com/en/)
- [Changelog](https://feedback.archive.com/changelog)
- [Blog](https://archive.com/blog)
- [Terms of service](https://archive.com/legal/terms)
- [Privacy policy](https://archive.com/legal/privacy)
- [Status](https://archive.instatus.com/)

## API Evangelist rating

**Kin Score 53.3** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 75.0 |
| Contract quality | 44.6 |
| Contract governance | 4.5 |
| Operational transparency | 42.1 |
| Developer ergonomics | 58.9 |
| Access clarity | 69.7 |

**Agent readiness 47.8** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | verified |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | documented (partial credit) |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | na |

**Regulatory alignment 34.3** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/archive-technologies/](https://apis.io/providers/archive-technologies/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Archive Technologies: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/archive-technologies/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
