# Hands-On Labs and Sample Repositories

> Curated, executable references for AZ-104 administrator topics. Most modules include free Microsoft Learn sandboxes - no Azure subscription required.

## Microsoft Learn - Sandbox-Backed Modules

| Path / Module | What you build |
| --- | --- |
| [Microsoft Azure Administrator (AZ-104) path](https://learn.microsoft.com/training/courses/az-104t00) | Full AZ-104 learning path. |
| [Manage identities and governance in Azure](https://learn.microsoft.com/training/paths/az-104-manage-identities-governance/) | Entra ID users/groups, RBAC, policy, subscriptions. |
| [Implement and manage storage in Azure](https://learn.microsoft.com/training/paths/az-104-manage-storage/) | Storage accounts, blobs, files, lifecycle, security. |
| [Deploy and manage Azure compute resources](https://learn.microsoft.com/training/paths/az-104-manage-compute-resources/) | VMs, scale sets, App Service, Container Instances. |
| [Implement and manage virtual networking](https://learn.microsoft.com/training/paths/az-104-manage-virtual-networks/) | VNets, subnets, NSGs, peering, private endpoints. |
| [Monitor and back up Azure resources](https://learn.microsoft.com/training/paths/az-104-monitor-backup-resources/) | Azure Monitor, Log Analytics, Backup, Site Recovery. |
| [Configure secure access to your applications](https://learn.microsoft.com/training/modules/configure-secure-access-using-azure-active-directory/) | Entra ID, Conditional Access, MFA. |
| [Configure Azure Files](https://learn.microsoft.com/training/modules/configure-azure-files-file-sync/) | Azure Files + File Sync, hybrid file shares. |
| [Implement Azure Bastion](https://learn.microsoft.com/training/modules/connect-vm-with-azure-bastion/) | Secure RDP/SSH without public IPs. |
| [Configure VPN and ExpressRoute](https://learn.microsoft.com/training/paths/az-104-manage-virtual-networks/) | Hybrid connectivity. |
| [Implement Azure load balancing](https://learn.microsoft.com/training/modules/load-balance-non-http-s-traffic-azure/) | Standard Load Balancer for non-HTTP. |
| [Implement Azure Application Gateway](https://learn.microsoft.com/training/modules/configure-application-gateway/) | L7 with WAF. |

## Service Quickstarts Worth Running

| Quickstart | Outcome |
| --- | --- |
| [Create a Linux VM with Bicep](https://learn.microsoft.com/azure/virtual-machines/linux/quick-create-bicep) | First IaC VM deployment. |
| [Create a Windows VMSS with autoscale](https://learn.microsoft.com/azure/virtual-machine-scale-sets/quick-create-portal) | Autoscale rules and scaling profiles. |
| [Configure Azure File Sync](https://learn.microsoft.com/azure/storage/file-sync/file-sync-deployment-guide) | Hybrid file tiering. |
| [Set up site-to-site VPN](https://learn.microsoft.com/azure/vpn-gateway/tutorial-site-to-site-portal) | Azure <-> on-prem connectivity. |
| [Hub-and-spoke with Bicep](https://learn.microsoft.com/azure/architecture/networking/architecture/hub-spoke) | Reference network topology. |
| [Backup an Azure VM](https://learn.microsoft.com/azure/backup/quick-backup-vm-portal) | Recovery Services vault + policy. |
| [Configure Azure Bastion](https://learn.microsoft.com/azure/bastion/tutorial-create-host-portal) | RDP/SSH over HTTPS. |
| [Deploy Azure Firewall](https://learn.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) | Centralized network filtering. |

## Azure-Samples and Reference Repositories

| Repository | Purpose |
| --- | --- |
| [Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) | Hundreds of ARM/Bicep reference templates. |
| [Azure/bicep](https://github.com/Azure/bicep) | Bicep language repo with examples. |
| [Azure/azure-policy](https://github.com/Azure/azure-policy) | Policy definitions reference. |
| [Azure/Enterprise-Scale](https://github.com/Azure/Enterprise-Scale) | Enterprise-scale landing zones (full reference IaC). |
| [Azure/ALZ-Bicep](https://github.com/Azure/ALZ-Bicep) | Azure Landing Zones in Bicep. |
| [Azure-Samples/azure-files-samples](https://github.com/Azure-Samples/azure-files-samples) | Files + File Sync scripts. |
| [Azure/azure-monitor-baseline-alerts](https://github.com/Azure/azure-monitor-baseline-alerts) | Curated alert rule baseline per service. |
| [Azure/Network-Security](https://github.com/Azure/Network-Security) | NSG + Firewall reference scripts. |

## CLI / PowerShell References

| Reference | Use |
| --- | --- |
| [Azure CLI reference](https://learn.microsoft.com/cli/azure/) | `az` command index. |
| [Azure PowerShell reference](https://learn.microsoft.com/powershell/azure/) | `Az` module index. |
| [Cloud Shell quickstart](https://learn.microsoft.com/azure/cloud-shell/quickstart) | Browser shell with credentials, CLI, Bicep, kubectl. |

## Recommended Practice Path

1. Spin up a sandbox subscription via Microsoft Learn modules - no cost.
2. Deploy a 3-tier reference: VNet + 2 subnets + NSGs + a VM scale set behind a load balancer + Azure Files.
3. Add **Bastion** for VM access and **private endpoints** for storage/Key Vault.
4. Apply an **Azure Policy initiative** (MCSB) and remediate findings.
5. Configure **diagnostic settings** to a Log Analytics workspace; build one alert rule + action group.
6. Configure **Azure Backup** for the VMs; perform a test restore.
7. Practice cost analysis: tag resources, set a **budget**, view cost by tag.
8. Replace any keys with **managed identity** + RBAC.
