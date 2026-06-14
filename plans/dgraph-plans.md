# Dgraph Plans

Dgraph Cloud offers managed hosting plans alongside a fully open-source self-hosted option. The cloud service (now under Istari Digital following its October 2025 acquisition from Hypermode) provides three tiers targeting different scale and reliability requirements.

## Free Tier

- **Cost:** $0/month
- **Backend type:** Shared infrastructure
- **Storage:** ~1 MB (suitable for evaluation and development only)
- **Data transfer:** Limited
- **High availability:** No
- **Use case:** Prototyping, learning, and personal experiments
- **Notes:** Free backends may be paused after periods of inactivity. Not recommended for production workloads.

## Shared Plan

- **Cost:** ~$39.99/month (pricing subject to change; verify at [site.dgraph.io/pricing](https://site.dgraph.io/pricing))
- **Backend type:** Shared cluster (multi-tenant)
- **Data transfer:** 5 GB/month included; $2.00 per additional GB
- **Storage:** Included (no additional storage charge under standard use)
- **High availability:** Yes (shared HA)
- **Cloud provider:** AWS (us-west-2 and other regions)
- **Use case:** Small production apps, MVPs, and low-to-medium traffic workloads
- **Notes:** Multiple tenants share the same underlying cluster nodes. Query performance may vary based on co-tenant activity.

## Dedicated Plan

- **Cost:** Starting at ~$199/month (single node); production-grade HA configurations start at approximately $219/month per node
- **Backend type:** Dedicated cluster (single-tenant)
- **Storage:** Up to 1 TB per node
- **High availability:** Yes (replication and sharding across nodes)
- **Cloud provider:** AWS and GCP
- **Features:**
  - Access Control Lists (ACLs)
  - Multi-tenancy within your backend
  - Audit logging
  - Change Data Capture (CDC)
- **Use case:** Enterprise, high-traffic, or data-sensitive production deployments
- **Notes:** Pricing scales with node count and storage. Contact Dgraph/Istari Digital sales for custom configurations.

## Enterprise / Self-Hosted

- **Cost:** Custom (contact sales)
- **Deployment:** On-premise or private cloud (AWS, GCP, Azure)
- **Licensing:** Open-source (Apache 2.0) for self-managed; enterprise license for additional support and features
- **Features:** Full control over infrastructure, custom SLAs, dedicated support
- **Helm charts:** Available at [github.com/dgraph-io/charts](https://github.com/dgraph-io/charts) for Kubernetes deployments
- **Docker:** Official images at [hub.docker.com/r/dgraph/dgraph](https://hub.docker.com/r/dgraph/dgraph)
- **Use case:** Organizations requiring data sovereignty, custom security configurations, or extremely large-scale deployments

## Plan Comparison

| Feature | Free | Shared | Dedicated | Self-Hosted |
|---|---|---|---|---|
| Cost | $0 | ~$40/mo | ~$199+/mo | Infrastructure cost |
| Storage | ~1 MB | Included | Up to 1 TB/node | Unlimited |
| Data Transfer | Limited | 5 GB/mo | Custom | Unlimited |
| High Availability | No | Yes | Yes | Configurable |
| Multi-tenancy | No | No | Yes | Yes |
| Audit Logging | No | No | Yes | Yes |
| CDC | No | No | Yes | Yes |
| ACLs | No | Limited | Yes | Yes |
| Subscriptions (WebSocket) | Yes | Yes | Yes | Yes |
| Custom Domain | No | No | Yes | Yes |

## Pricing Resources

- Official Pricing Page: [https://site.dgraph.io/pricing](https://site.dgraph.io/pricing)
- Cloud Console Pricing Calculator: [https://cloud.dgraph.io/pricing](https://cloud.dgraph.io/pricing)
- Community Pricing Discussion: [https://discuss.dgraph.io/t/dgraph-cloud-pricing/16850](https://discuss.dgraph.io/t/dgraph-cloud-pricing/16850)
