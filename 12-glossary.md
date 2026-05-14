# Glossary and Acronym Reference

> Authoritative definitions for the operations terms, acronyms, and product names that appear in AZ-104 scenarios.

## Identity

| Term | Definition |
| --- | --- |
| **Entra ID** | Microsoft's cloud identity service (formerly Azure AD). |
| **Tenant** | An Entra ID instance. |
| **Directory role** | Tenant-level role (e.g., Global Administrator, User Administrator). |
| **Azure role / RBAC role** | Resource-scoped role (Owner, Contributor, Reader, plus 100+ built-ins). |
| **Custom role** | Role you author when no built-in fits; defined as JSON. |
| **Group types** | Security (RBAC), Microsoft 365 (collaboration). |
| **Dynamic group** | Membership computed from user/device attributes. |
| **Conditional Access** | Policy engine for sign-in (signal -> enforce: MFA, block, compliant device). |
| **Self-service password reset (SSPR)** | User-initiated password reset with verification methods. |
| **Entra Connect** | Tool to sync on-prem AD to Entra ID. |
| **B2B** | Invite external users (guests). |
| **B2C** | Consumer-facing identity (separate Entra B2C tenant). |

## Governance

| Term | Definition |
| --- | --- |
| **Management group** | Container above subscriptions for inheritance of policy and RBAC. |
| **Subscription** | Billing/quota/network boundary. |
| **Resource group** | Logical container; resources share lifecycle and region for management. |
| **Tag** | Key/value metadata; used for cost allocation, automation, governance. |
| **Azure Policy** | Declarative engine; effects: Audit, Deny, Append, Modify, DeployIfNotExists, AuditIfNotExists. |
| **Initiative** | Group of related policies. |
| **Resource Lock** | `ReadOnly` or `Delete` lock; prevents accidental change. |
| **Cost Management** | Budgets, alerts, cost analysis. |

## Compute

| Term | Definition |
| --- | --- |
| **VM** | Azure Virtual Machine; IaaS compute. |
| **VM Size** | SKU family (B/D/E/F/N etc.) with vCPU, memory, storage profile. |
| **Availability Set** | Fault and update domain grouping within one datacenter. |
| **Availability Zone** | Physically separate datacenter within a region. |
| **VMSS** | Virtual Machine Scale Set; identical VMs with autoscale. |
| **Custom Script Extension** | Runs scripts on VM at deploy/post-deploy. |
| **App Service plan** | Hosting tier for App Service apps. |
| **Deployment slot** | Staging slot of an App Service app (warm-up, swap). |
| **Container Instance (ACI)** | Single-container serverless. |
| **Container Apps** | Serverless container runtime with KEDA-based autoscale. |
| **AKS** | Azure Kubernetes Service. |

## Storage

| Term | Definition |
| --- | --- |
| **Storage account** | Top-level resource for blobs, files, queues, tables. |
| **Blob types** | Block (general), Append (logs), Page (VHDs). |
| **Access tier** | Hot, Cool, Cold, Archive. |
| **Lifecycle management** | Rules to transition or delete blobs by age/access. |
| **Redundancy** | LRS, ZRS, GRS, GZRS, RA-GRS, RA-GZRS. |
| **Azure Files** | Managed SMB / NFS file share. |
| **File Sync** | Tier files between on-prem servers and Azure Files. |
| **Queue Storage** | Simple async messaging. |
| **Table Storage** | NoSQL key/value (legacy; consider Cosmos DB Table API). |
| **Shared access signature (SAS)** | Delegated access token; user-delegation SAS preferred (uses Entra). |
| **Static website hosting** | Serve static content from `$web` container. |
| **Soft delete** | Retain deleted blobs/containers/file shares for a retention period. |

## Networking

| Term | Definition |
| --- | --- |
| **VNet** | Private IP space in Azure. |
| **Subnet** | Address range within a VNet. |
| **NSG** | Network Security Group; stateful 5-tuple rules at subnet or NIC. |
| **ASG** | Application Security Group; named groups of NICs used in NSG rules. |
| **Route table** | User-defined routes (UDR) override system routes. |
| **VNet peering** | Private high-throughput link between VNets. |
| **VPN Gateway** | IPsec site-to-site or point-to-site. |
| **ExpressRoute** | Private connectivity via partner. |
| **Private endpoint** | Private IP for an Azure PaaS resource in your VNet. |
| **Service endpoint** | Subnet-level access to PaaS over public endpoint. |
| **Load Balancer** | L4 regional load balancer (Basic/Standard). |
| **Application Gateway** | L7 with WAF; HTTP only. |
| **Front Door** | Global L7 + CDN + WAF. |
| **Azure Firewall** | Stateful firewall as a service. |
| **DNS zones** | Public Azure DNS, Private DNS zones for VNet name resolution. |
| **Network Watcher** | Connection monitor, NSG flow logs, IP flow verify, packet capture. |

## Monitoring

| Term | Definition |
| --- | --- |
| **Azure Monitor** | Metrics, logs, alerts, autoscale. |
| **Log Analytics workspace** | KQL-queryable log store. |
| **Diagnostic settings** | Resource-side route for platform logs to LA workspace, Storage, or Event Hubs. |
| **Action group** | Reusable set of alert recipients/automations. |
| **Activity log** | Subscription-level control-plane events. |
| **Application Insights** | APM, workspace-based. |
| **Service Health** | Portal blade for Azure incidents and planned maintenance. |
| **Resource Health** | Health view of a specific resource. |

## Backup and Recovery

| Term | Definition |
| --- | --- |
| **Recovery Services vault** | Vault for Backup and Site Recovery. |
| **Azure Backup** | Backup for VMs, files, SQL in VM, Azure Files, SAP HANA. |
| **Soft delete (vault)** | 14-day retention of deleted backups. |
| **Snapshot** | Point-in-time disk copy. |
| **Site Recovery (ASR)** | Replication-based DR for VMs. |

## Tools

| Term | Definition |
| --- | --- |
| **Azure Resource Manager (ARM)** | The deployment and management plane. |
| **ARM template** | JSON IaC. |
| **Bicep** | DSL that transpiles to ARM JSON. |
| **Azure CLI** | `az` cross-platform CLI. |
| **Azure PowerShell** | `Az` PowerShell module. |
| **Cloud Shell** | Browser-hosted shell with credentials, CLI, and Bicep preinstalled. |
