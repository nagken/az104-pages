# Azure Architecture Center

> The **[Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)** is Microsoft's official catalog of reference architectures, design patterns, and Well-Architected workload guidance. Browse the full catalog at [learn.microsoft.com/azure/architecture/browse](https://learn.microsoft.com/en-us/azure/architecture/browse/) and filter by product, category, or scenario.

## How to use it for AZ-104

1. When you're asked to **operate** a service, find the matching reference architecture and read the *operations* and *cost* sections.
2. Use the **decision guides** as quick lookups during scenario-style questions.
3. Adapt the **Bicep / ARM** in linked samples for hands-on practice.
4. Follow the **landing zone** guidance to understand the broader context your subscription sits in.

## Top entry points

| Resource | Why it matters for AZ-104 |
| --- | --- |
| [Architecture Center home](https://learn.microsoft.com/en-us/azure/architecture/) | Curated landing page; start here. |
| [Browse architectures](https://learn.microsoft.com/en-us/azure/architecture/browse/) | Filterable catalog of every reference architecture. |
| [Cloud Adoption Framework](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/) | Governance, landing zones, migration. |
| [Well-Architected Framework](https://learn.microsoft.com/en-us/azure/well-architected/) | Five pillars context for operations decisions. |
| [Best practices for cloud applications](https://learn.microsoft.com/en-us/azure/architecture/best-practices/) | API design, autoscaling, caching, data partitioning. |

## Reference architectures by AZ-104 topic

### Identity and governance

- [Azure landing zones](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/)
- [Hybrid identity options](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/identity/azure-ad)
- [Management groups + RBAC at scale](https://learn.microsoft.com/en-us/azure/governance/management-groups/overview)

### Networking

- [Hub-and-spoke topology](https://learn.microsoft.com/en-us/azure/architecture/networking/architecture/hub-spoke)
- [Site-to-site VPN](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/vpn)
- [ExpressRoute private peering](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/hybrid-networking/expressroute)
- [DNS for hybrid environments](https://learn.microsoft.com/en-us/azure/architecture/networking/architecture/azure-dns-private-resolver)

### Compute

- [N-tier app on Azure VMs](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/n-tier/n-tier-sql-server)
- [Multi-region N-tier](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/n-tier/multi-region-sql-server)
- [App Service production hosting](https://learn.microsoft.com/en-us/azure/architecture/web-apps/app-service/architectures/baseline-zone-redundant)
- [VMSS autoscale design](https://learn.microsoft.com/en-us/azure/architecture/best-practices/auto-scaling)

### Storage

- [Storage account redundancy options](https://learn.microsoft.com/en-us/azure/storage/common/storage-redundancy)
- [Hybrid file services with Azure File Sync](https://learn.microsoft.com/en-us/azure/architecture/hybrid/azure-file-share)
- [Lifecycle management for blob tiers](https://learn.microsoft.com/en-us/azure/storage/blobs/lifecycle-management-overview)

### Monitoring and operations

- [Azure Monitor enterprise rollout](https://learn.microsoft.com/en-us/azure/azure-monitor/best-practices)
- [Log Analytics workspace design](https://learn.microsoft.com/en-us/azure/azure-monitor/logs/workspace-design)
- [Backup and DR strategy](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/manage/protect)
- [Update management with Azure Update Manager](https://learn.microsoft.com/en-us/azure/update-manager/overview)

## Decision guides worth memorizing

- [Choose a compute service](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/compute-decision-tree)
- [Choose an Azure storage service](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/storage-options)
- [Choose a load-balancing service](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/load-balancing-overview)
- [Choose an Azure messaging service](https://learn.microsoft.com/en-us/azure/service-bus-messaging/compare-messaging-services)
- [Choose a containerization option](https://learn.microsoft.com/en-us/azure/architecture/guide/choose-azure-container-service)

## Patterns useful for AZ-104 scenarios

| Pattern | When to apply |
| --- | --- |
| **Retry / Circuit Breaker** | App calling Azure services through transient errors. |
| **Throttling** | Protect a workload from runaway load. |
| **Cache-Aside** | Reduce read load on a database. |
| **Health Endpoint Monitoring** | Probes for Load Balancer / App Gateway / Front Door. |
| **Queue-Based Load Leveling** | Smooth spiky producer traffic before VM/AKS workers. |
