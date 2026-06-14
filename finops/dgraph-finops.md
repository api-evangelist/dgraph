# Dgraph FinOps

This document covers cost management, optimization strategies, and financial governance for teams running Dgraph Cloud or self-hosted Dgraph deployments.

## Cost Drivers

### Dgraph Cloud

| Cost Component | Shared | Dedicated |
|---|---|---|
| Base compute | ~$39.99/mo flat | ~$199+/mo per node |
| Data transfer | 5 GB included, then $2.00/GB | Contracted allocation |
| Storage | Included in plan | Included up to 1 TB/node |
| Additional nodes | N/A | Per-node billing |
| Enterprise features (ACL, CDC, audit) | Not available | Included |

### Self-Hosted

Cost is driven entirely by your own infrastructure:

- **Compute (EC2, GKE, AKS, etc.):** Dgraph Alpha nodes are CPU and memory-intensive. Minimum recommended for production: 4 vCPU / 8 GB RAM per Alpha node.
- **Storage (block volumes):** Dgraph uses RocksDB for storage; fast NVMe/SSD is strongly recommended. Budget for 2–3x your raw data size due to RocksDB amplification and replication.
- **Network egress:** Graph queries can return large result sets. Monitor egress costs from cloud providers.
- **Kubernetes overhead:** If using the official Helm chart, account for load balancer, persistent volume, and Dgraph Zero (coordinator) node costs.

## Cost Optimization Strategies

### Minimize Data Transfer (Cloud)

- Use `first` and `offset` to paginate results rather than fetching full datasets per query.
- Apply `@cascade` or `filter` in queries to reduce payload size.
- Avoid returning large arrays of nested objects when a count or aggregate is sufficient — use `aggregateProduct { count }` instead of fetching all records.
- Compress responses: Dgraph Cloud supports gzip compression; enable it in your HTTP client (`Accept-Encoding: gzip`).

### Efficient Schema Design

- Index only the fields you query; excess `@search` directives add write amplification and storage overhead.
- Use `@dgraph(type: "...")` to share predicates across types and reduce index duplication.
- Avoid storing large blobs (images, documents) in Dgraph predicates; store references (URLs, S3 keys) instead.

### Right-Sizing

- **Free → Shared:** Use the free tier only for non-production work. The 1 MB storage cap makes it impractical for any real application.
- **Shared → Dedicated:** Upgrade when you need consistent performance SLAs, ACLs, CDC, or storage beyond what the shared tier provides.
- **Dedicated scaling:** Start with a single dedicated node and add nodes (sharding) only when query latency or storage limits require it.

### Self-Hosted Savings

- Use spot/preemptible instances for non-HA dev/staging Dgraph clusters; use on-demand or reserved instances for production Zeros and Alphas.
- Enable RocksDB compression (`--badger compression=zstd`) to reduce storage volume size and I/O costs.
- Use a Dgraph cluster with replication factor of 3 only for production; development clusters can run with replication factor of 1.
- Autoscale Dgraph Alpha read replicas horizontally during peak traffic using Kubernetes HPA, then scale down during off-peak hours.

## Monitoring and Alerting

### Dgraph Cloud

- **Console Dashboard:** Monitor storage usage, data transfer, and query performance in the [Dgraph Cloud Console](https://cloud.dgraph.io/).
- **Data Transfer Alerts:** Set up billing alerts if your cloud provider charges for data transfer overages; Dgraph Cloud charges $2.00/GB above the 5 GB monthly included allowance.

### Self-Hosted

Dgraph exposes Prometheus metrics on all Alpha and Zero nodes (port 8080 by default):

Key metrics to monitor for cost control:

| Metric | Why It Matters |
|---|---|
| `dgraph_memory_inuse_bytes` | Memory pressure; over-provisioning waste |
| `dgraph_disk_bytes_read_total` | Storage I/O; correlates with IOPS cost |
| `dgraph_active_mutations_total` | Write throughput; guides node sizing |
| `dgraph_pending_proposals_total` | Raft lag; indicates undersized Zero nodes |
| `process_resident_memory_bytes` | Actual vs. allocated memory for right-sizing |

Use Grafana dashboards from the [Dgraph community](https://github.com/dgraph-io/dgraph/tree/main/contrib/config/monitoring) for visual monitoring.

## Budget Estimation

### Small App (Shared Cloud)

- 1 shared backend: ~$40/month
- Data transfer (estimated 10 GB/month): $40 + (5 GB × $2) = ~$50/month
- **Total: ~$50/month**

### Medium Production App (Dedicated Cloud, 1 node)

- 1 dedicated node: ~$200–$220/month
- Storage and transfer included in plan
- **Total: ~$200–$220/month**

### Large Enterprise (Dedicated, 3-node cluster)

- 3 dedicated nodes: ~$600–$660/month
- Custom storage and support contract
- **Total: Contact sales for quote**

### Self-Hosted (AWS, 3-node production cluster)

- 3× r6i.xlarge EC2 (4 vCPU, 32 GB, reserved 1yr): ~$300/month
- 3× 500 GB gp3 EBS volumes: ~$45/month
- Network egress (estimated 50 GB/month): ~$4.50/month
- Load balancer + misc: ~$20/month
- **Total: ~$370/month** (excluding operational overhead)

## Resources

- [Dgraph Cloud Pricing](https://site.dgraph.io/pricing)
- [Cloud Console](https://cloud.dgraph.io/)
- [Self-Hosted Deployment Guide](https://docs.dgraph.io/deploy/)
- [Helm Charts](https://github.com/dgraph-io/charts)
- [Community Pricing Discussions](https://discuss.dgraph.io/t/dgraph-cloud-pricing/16850)
