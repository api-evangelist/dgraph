# Dgraph

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
