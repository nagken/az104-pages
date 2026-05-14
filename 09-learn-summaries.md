# Microsoft Learn Summaries (AZ-104)

> Tight summaries of every major service covered in this AZ-104 guide, organized the Microsoft Learn way: **Overview -> Components -> Key concepts -> Integrations**. Use this page as a 60-second refresher before diving into the domain pages.

## Microsoft Entra ID

- **Overview** - Cloud identity provider for Microsoft 365 and Azure. Issues tokens, brokers SSO, enforces conditional access.
- **Components** - Tenant, Directory, Users, Groups, Service Principals, Managed Identities, App Registrations.
- **Key concepts** - Tenant != subscription; user types (member, guest); group types (security, M365); dynamic membership rules.
- **Integrations** - Conditional Access, MFA, PIM, RBAC, Application Proxy.
- Source: [Entra fundamentals](https://learn.microsoft.com/entra/fundamentals/whatis).

## Azure RBAC

- **Overview** - Authorization layer that decides who can do what on which Azure resource.
- **Components** - Security principal, Role definition, Role assignment, Scope.
- **Key concepts** - Scope hierarchy (MG -> Sub -> RG -> Resource); inheritance; Owner vs Contributor vs User Access Administrator; built-in vs custom roles; deny assignments.
- **Integrations** - Entra ID, Azure Policy, Privileged Identity Management.
- Source: [Azure RBAC overview](https://learn.microsoft.com/azure/role-based-access-control/overview).

## Azure Policy

- **Overview** - Configuration/compliance engine. Audits or denies non-compliant resource shapes; can also remediate.
- **Components** - Policy definition, Policy assignment, Initiative (set of definitions), Compliance, Remediation task.
- **Key concepts** - `audit` vs `deny` vs `append` vs `modify` vs `deployIfNotExists`; effects evaluated at resource creation/update; exemptions.
- **Integrations** - Management groups, Resource Manager, Defender for Cloud.
- Source: [Azure Policy overview](https://learn.microsoft.com/azure/governance/policy/overview).

## Storage account

- **Overview** - Container for blobs, files, queues, tables. Endpoint per service; redundancy chosen at account level.
- **Components** - General-purpose v2, Premium block blob, Premium file shares, Premium page blobs.
- **Key concepts** - Access tiers (Hot/Cool/Cold/Archive); redundancy (LRS/ZRS/GRS/RA-GRS/GZRS); access keys vs SAS vs Entra ID auth; lifecycle policies; soft delete + versioning.
- **Integrations** - Azure Files (SMB/NFS), AzCopy, Azure Backup, Defender for Storage.
- Source: [Storage account overview](https://learn.microsoft.com/azure/storage/common/storage-account-overview).

## Virtual machine

- **Overview** - IaaS compute primitive. Linux or Windows on Hyper-V hosts.
- **Components** - VM, OS disk + data disks, NIC + public IP, NSG, availability set, availability zone, VMSS.
- **Key concepts** - VM SKU families (B/D/E/F/M/N/H); Standard SSD vs Premium SSD vs Ultra disk; spot instances; reserved instances; bursting.
- **Integrations** - VNet, Backup, Monitor, Update Manager, Site Recovery.
- Source: [VM sizes](https://learn.microsoft.com/azure/virtual-machines/sizes).

## App Service

- **Overview** - Managed PaaS for web apps, REST APIs, mobile backends.
- **Components** - App Service Plan (compute), Web App, Deployment slots, Custom domain + cert.
- **Key concepts** - Plan tier defines scale + features (Free/Shared/Basic/Standard/Premium V3/Isolated V2); slots for blue-green; built-in auth (EasyAuth); VNet integration.
- **Integrations** - Application Gateway, Front Door, Key Vault references, Managed Identity.
- Source: [App Service overview](https://learn.microsoft.com/azure/app-service/overview).

## Container Apps and AKS

- **Overview** - Managed container platforms. Container Apps for serverless containers + KEDA scaling; AKS for full Kubernetes.
- **Components (ACA)** - Environment, App, Revision, Ingress, Dapr.
- **Components (AKS)** - Cluster, Node pools (system + user), Add-ons (CSI, monitoring, ingress), Virtual nodes.
- **Key concepts** - When to choose which (managed simplicity vs full k8s control); cluster autoscaler + KEDA; private clusters; node pool taints.
- Sources: [Container Apps](https://learn.microsoft.com/azure/container-apps/overview) | [AKS](https://learn.microsoft.com/azure/aks/intro-kubernetes).

## Virtual network

- **Overview** - Layer-3 isolated network in Azure. Subnets carve up the address space.
- **Components** - VNet, Subnet, NSG, ASG, Route table, Service endpoint, Private endpoint, Peering.
- **Key concepts** - Address space + non-overlapping CIDRs; default routes (system); user-defined routes; service tags; flow logs.
- **Integrations** - All IaaS resources, App Service VNet integration, Private Link.
- Source: [Virtual network overview](https://learn.microsoft.com/azure/virtual-network/virtual-networks-overview).

## Load balancing services

- **Overview** - Four primary load balancers, each at a different OSI layer / scope.
- **Components** - Azure Load Balancer (L4 regional), Application Gateway (L7 regional, WAF), Front Door (L7 global, WAF + CDN), Traffic Manager (DNS-level global).
- **Key concepts** - When to pick which by traffic type (TCP/UDP/HTTP) and scope (regional/global); health probes; SSL termination; sticky sessions.
- Source: [Load-balancing decision tree](https://learn.microsoft.com/azure/architecture/guide/technology-choices/load-balancing-overview).

## Azure Monitor

- **Overview** - Single pane for metrics, logs, alerts, dashboards across every Azure resource.
- **Components** - Metrics store (time series), Log Analytics workspace (KQL), Alerts (metric/log/activity), Action Groups, Workbooks, Insights (VM, Container, App).
- **Key concepts** - Diagnostic settings route logs/metrics to LA, Storage, Event Hub; KQL fundamentals; alert evaluation frequency; action group types (email, SMS, webhook, function, ITSM).
- **Integrations** - Every Azure resource, Application Insights, Defender for Cloud.
- Source: [Azure Monitor overview](https://learn.microsoft.com/azure/azure-monitor/overview).

## Backup and disaster recovery

- **Overview** - Two services: Azure Backup for snapshot-based backup, Site Recovery for replicate-and-fail-over DR.
- **Components** - Recovery Services Vault, Backup policies, Replication policies, Recovery plans.
- **Key concepts** - RPO + RTO targets; per-VM vs per-disk backup; cross-region restore; soft delete on RSV; SLA differences.
- Sources: [Azure Backup](https://learn.microsoft.com/azure/backup/backup-overview) | [Site Recovery](https://learn.microsoft.com/azure/site-recovery/site-recovery-overview).
