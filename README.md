# Dgraph

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

Dgraph is a distributed graph database with a native GraphQL API, automatic type enforcement, real-time subscriptions, horizontal scaling, and built-in authorization directives. It is the only open-source, complete graph database designed for terabyte-scale, real-time use cases.

Originally developed by Dgraph Labs and acquired by Hypermode in 2023, Dgraph was subsequently acquired by Istari Digital in October 2025. The project remains open-source under the Apache 2.0 license.

## Key Features

- **Native GraphQL:** Schema-driven API that auto-generates queries, mutations, and subscriptions — no resolver boilerplate required.
- **Distributed Architecture:** Horizontal scaling via sharding and replication across Dgraph Alpha nodes, coordinated by Dgraph Zero.
- **ACID Transactions:** Full ACID compliance with consistent replication and linearizable reads.
- **Real-Time Subscriptions:** Native WebSocket subscriptions without additional infrastructure.
- **Built-In Authorization:** Row-level security using `@auth` directives and JWT claims directly in the schema.
- **DQL Access:** Low-level Dgraph Query Language (DQL) for advanced graph traversals beyond the GraphQL layer.

## Links

- **Website:** [https://site.dgraph.io/](https://site.dgraph.io/)
- **Documentation:** [https://docs.dgraph.io/](https://docs.dgraph.io/)
- **GitHub Organization:** [https://github.com/dgraph-io](https://github.com/dgraph-io)
- **Cloud Console:** [https://cloud.dgraph.io/](https://cloud.dgraph.io/)
- **Pricing:** [https://site.dgraph.io/pricing](https://site.dgraph.io/pricing)
- **LinkedIn:** [https://www.linkedin.com/company/dgraph-labs](https://www.linkedin.com/company/dgraph-labs)
- **Community:** [https://discuss.dgraph.io/](https://discuss.dgraph.io/)

## APIs

| API | Description |
|---|---|
| Dgraph GraphQL API | Native GraphQL layer with auto-generated CRUD operations, subscriptions, and authorization |

## Plans

Dgraph Cloud offers three tiers:

| Plan | Cost | Notes |
|---|---|---|
| Free | $0/month | ~1 MB storage; dev/prototyping only |
| Shared | ~$39.99/month | 5 GB data transfer included; shared HA cluster |
| Dedicated | ~$199+/month/node | Single-tenant; ACLs, CDC, audit logging, 1 TB storage |

Self-hosted deployments are available via Docker or Helm charts with no licensing cost (Apache 2.0).

See [plans/dgraph-plans.md](plans/dgraph-plans.md) for full plan details.

## Rate Limits

Dgraph does not publish hard API rate limits. On shared cloud tiers, throughput is constrained by pooled cluster resources. On dedicated tiers and self-hosted deployments, limits are governed by node capacity and operator-configured flags such as `--query-edge-limit` and `--query-timeout`.

See [rate-limits/dgraph-rate-limits.md](rate-limits/dgraph-rate-limits.md) for details.

## FinOps

Key cost drivers are compute (node count), data transfer (shared tier: $2/GB over 5 GB/month), and storage (up to 1 TB/node on dedicated). Self-hosted deployments shift costs to infrastructure and operational overhead.

See [finops/dgraph-finops.md](finops/dgraph-finops.md) for cost optimization strategies and budget estimates.

## Repository Structure

```
dgraph/
  apis.yml                        - APIs.json 0.19 catalog entry
  README.md                       - This file
  graphql/
    dgraph-graphql.md             - Dgraph GraphQL API reference
  plans/
    dgraph-plans.md               - Pricing plan details
  rate-limits/
    dgraph-rate-limits.md         - Rate limit documentation
  finops/
    dgraph-finops.md              - Cost management guidance
```

## Maintainer

Kin Lane — kin@apievangelist.com
