# Dgraph Rate Limits

Dgraph does not publish a rigid, publicly documented rate-limit table in the same style as REST-centric APIs. Instead, throughput is governed by a combination of cluster resources, plan tier, and Dgraph's internal concurrency controls. The notes below reflect what is known from official documentation, community discussions, and the behavior of Dgraph Cloud.

## Dgraph Cloud — Shared Tier

On shared backends, resources (CPU, memory, network) are pooled across tenants. Effective limits include:

- **Concurrent GraphQL connections:** Constrained by shared node capacity; no hard-documented number, but expect lower concurrency than dedicated tiers.
- **Data transfer:** 5 GB/month included. Overage charged at $2.00/GB. High-volume query result payloads count toward this limit.
- **Query complexity:** Very large or deeply nested queries may time out or be throttled to protect shared cluster health.
- **WebSocket subscriptions:** Supported, but the number of simultaneous subscription connections per backend is subject to shared resource limits.

## Dgraph Cloud — Dedicated Tier

On dedicated backends, limits are driven by provisioned node resources rather than a fixed API rate cap:

- **Concurrent queries:** Configurable via `--limit` flags; defaults allow hundreds of concurrent operations per node.
- **Data transfer:** Per the contracted allocation; additional GBs are billed at overage rates.
- **Subscription connections:** Limited by available memory and file descriptor limits on the dedicated node (thousands of concurrent WebSocket connections are achievable on adequately provisioned nodes).

## Self-Hosted / Open Source

Rate limiting is entirely operator-controlled. Dgraph exposes several configuration flags relevant to throughput:

| Flag | Default | Description |
|---|---|---|
| `--limit` | Varies | Max concurrent queries per Dgraph Alpha node |
| `--mutations` | `allow` | Mutation mode: `allow`, `disallow`, or `strict` |
| `--query-edge-limit` | 1,000,000 | Max edges traversed in a single query |
| `--normalize-node-limit` | 10,000 | Max nodes returned by a normalize query |
| `--max-retries` | 5 | Max retries for failed operations |
| `--query-timeout` | 0 (unlimited) | Timeout per query in seconds |

To enforce rate limiting in self-hosted deployments, operators typically place a reverse proxy (nginx, Envoy, Kong) or a GraphQL gateway (Apollo Router, Hasura) in front of Dgraph Alphas.

## Admin API Limits

The `/admin` GraphQL endpoint used for schema management does not impose separate documented rate limits but should not be called at high frequency. Schema updates trigger reindexing and can be resource-intensive.

## GraphQL Query Limits

Dgraph imposes soft limits on query complexity to protect cluster stability:

- **Max query depth:** No fixed hard limit; extremely deep recursive queries will exhaust memory before hitting a code-level cap.
- **Result set size:** Queries use `first` and `offset` for pagination. Without a `first` parameter, Dgraph may return the full result set, which for large datasets can be very large.
- **Timeout:** Queries running beyond the configured `--query-timeout` are cancelled and return an error.

## Recommendations

- Always paginate large queries using `first` and `offset` (or cursor-based pagination via `@cascade` + `ID`).
- Use `@search` indexes appropriate to your query patterns to avoid full-graph scans.
- For production workloads on shared tiers, implement client-side retry logic with exponential backoff.
- Monitor data transfer usage in the Dgraph Cloud Console to avoid unexpected overage charges.
- For high-concurrency applications, upgrade to a Dedicated backend or self-host on sufficiently provisioned Kubernetes nodes.

## Resources

- [Dgraph Alpha Configuration Flags](https://docs.dgraph.io/deploy/dgraph-alpha/)
- [GraphQL Overview](https://docs.dgraph.io/graphql/overview/)
- [Dgraph Cloud Console](https://cloud.dgraph.io/)
- [Community: Dgraph Performance Tuning](https://discuss.dgraph.io/)
