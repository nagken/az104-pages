# AZ-104 - Reference Architectures

> Real reference architectures from the [Azure Architecture Center](https://learn.microsoft.com/azure/architecture/browse/) and [Cloud Adoption Framework](https://learn.microsoft.com/azure/cloud-adoption-framework/) that map directly to AZ-104 exam topics. Each entry calls out which **skills-measured area** it reinforces.

## Domain 1 - Identity, governance, and resources (20-25%)

| Architecture | Topic it reinforces |
| --- | --- |
| [Azure landing zone conceptual architecture](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/) | Management groups, subscriptions, RBAC, policy at scale |
| [Hybrid identity with Microsoft Entra Connect](https://learn.microsoft.com/entra/identity/hybrid/connect/whatis-azure-ad-connect) | On-prem AD <-> Entra ID sync, password hash / pass-through auth |
| [Azure RBAC and Azure Policy enterprise reference](https://learn.microsoft.com/azure/governance/policy/concepts/policy-for-kubernetes) | Inheritance, custom roles, deny / audit / deploy-if-not-exists |
| [Resource organization at scale](https://learn.microsoft.com/azure/cloud-adoption-framework/ready/azure-best-practices/resource-naming) | Naming standards, tagging, resource groups |

## Domain 2 - Storage (15-20%)

| Architecture | Topic it reinforces |
| --- | --- |
| [Hybrid file services with Azure File Sync](https://learn.microsoft.com/azure/architecture/hybrid/azure-file-share) | SMB shares, tiering, DFS replacement |
| [Choose a data store](https://learn.microsoft.com/azure/architecture/guide/technology-choices/data-store-decision-tree) | Blob vs Files vs Queues vs Tables vs SQL |
| [Blob storage lifecycle and access tiers](https://learn.microsoft.com/azure/storage/blobs/access-tiers-overview) | Hot / Cool / Cold / Archive, lifecycle policy |
| [Azure Storage encryption and security baseline](https://learn.microsoft.com/azure/storage/common/security-baseline) | CMK, private endpoints, firewall rules, SAS hygiene |
| [AzCopy / Storage Mover migration patterns](https://learn.microsoft.com/azure/storage-mover/service-overview) | Bulk migration, throttling, resumable copy |

## Domain 3 - Compute (20-25%)

| Architecture | Topic it reinforces |
| --- | --- |
| [Run a Windows VM on Azure](https://learn.microsoft.com/azure/architecture/reference-architectures/n-tier/windows-vm) | Single VM hardening, managed disks, availability options |
| [Run an N-tier app with VMSS](https://learn.microsoft.com/azure/architecture/reference-architectures/n-tier/n-tier-sql-server) | VM Scale Sets, load balancer, autoscale rules |
| [App Service multi-region](https://learn.microsoft.com/azure/architecture/web-apps/app-service/architectures/multi-region) | Web Apps, deployment slots, Front Door |
| [Container Apps baseline](https://learn.microsoft.com/azure/architecture/example-scenario/serverless/microservices-with-container-apps) | Containerized workloads without AKS |
| [AKS baseline architecture](https://learn.microsoft.com/azure/architecture/reference-architectures/containers/aks/baseline-aks) | When to graduate to Kubernetes |

## Domain 4 - Virtual networking (15-20%)

| Architecture | Topic it reinforces |
| --- | --- |
| [Hub-and-spoke network topology](https://learn.microsoft.com/azure/architecture/networking/architecture/hub-spoke) | VNet peering, shared services, transitive routing |
| [Azure Firewall in hub-spoke](https://learn.microsoft.com/azure/architecture/example-scenario/firewalls/) | Centralized egress, FQDN filtering, forced tunneling |
| [Private endpoints for PaaS](https://learn.microsoft.com/azure/architecture/example-scenario/private-web-app/private-web-app) | Private DNS zones, service tags, NSG rules |
| [Connect on-prem with VPN Gateway](https://learn.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/vpn) | S2S VPN, BGP, active-active |
| [ExpressRoute reference](https://learn.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/expressroute) | Private circuits, peering types, FastPath |
| [Azure Application Gateway with WAF](https://learn.microsoft.com/azure/architecture/example-scenario/gateway/application-gateway-before-azure-firewall) | Layer-7 LB, path-based routing, WAF v2 |

## Domain 5 - Monitor and maintain Azure resources (10-15%)

| Architecture | Topic it reinforces |
| --- | --- |
| [Monitoring a hybrid environment](https://learn.microsoft.com/azure/architecture/hybrid/hybrid-perf-monitoring) | Azure Monitor, Log Analytics, agents |
| [Azure Monitor baseline alerts](https://learn.microsoft.com/azure/azure-monitor/best-practices) | Metric vs log alerts, action groups |
| [Backup and disaster recovery for Azure VMs](https://learn.microsoft.com/azure/architecture/framework/resiliency/backup-and-recovery) | Recovery Services vault, soft delete, restore tiers |
| [Azure Site Recovery for VMs](https://learn.microsoft.com/azure/site-recovery/azure-to-azure-architecture) | Cross-region failover, RPO / RTO planning |
| [Update Manager for VMs](https://learn.microsoft.com/azure/update-manager/overview) | Patch management, maintenance windows |

## How to study with these

1. Read the matching domain page in this guide first.
2. Open the architecture; trace **identity -> resource org -> networking -> workload -> backup/monitor**.
3. Re-answer the [exam decision reference](06-exam-cheatsheet.md) with that architecture as the worked example.
