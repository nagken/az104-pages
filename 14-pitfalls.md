# Common Pitfalls and Distractor Patterns

> Mistakes that look right on the AZ-104 exam but lose points. Each entry pairs the wrong choice candidates pick with the correct one and the rule.

## Identity and RBAC

### Owner where Contributor would do

**Pitfall**: Granting Owner "to be safe."

**Reality**: Owner includes role assignments. Least-privilege rule: pick the **most narrowly-scoped built-in role** that satisfies the requirement. Often the answer is a resource-type role (VM Contributor, Storage Blob Data Contributor) at the resource group, not Contributor at the subscription.

### Confusing directory roles with Azure roles

**Pitfall**: Assigning Global Administrator for "manage Azure subscriptions."

**Reality**: **Directory roles** (Global Admin, User Admin) act inside Entra ID. **Azure roles** (Owner, Contributor) act on Azure resources. To manage Azure subscriptions you grant Azure RBAC at the subscription, not Global Admin. Global Admin can elevate to manage subscriptions but should not be the daily role.

### Adding users directly to RBAC instead of groups

**Pitfall**: Assigning roles to individual users.

**Reality**: Assign to **security groups**; manage membership in the group. Reduces sprawl and auditing surface.

## Governance

### Resource Lock for "compliance"

**Pitfall**: Assuming a Delete lock satisfies a "prevent non-compliant configuration" requirement.

**Reality**: Locks prevent delete or all writes. They do not enforce specific configuration. Use **Azure Policy** (deny/append/modify) for configuration enforcement.

### `Audit` policy when `Deny` is required

**Pitfall**: Using `audit` when the requirement is to *block*.

**Reality**: `audit` only logs noncompliance. `deny` blocks. `deployIfNotExists` remediates after creation (good for tag application or DCR association).

### Subscription per team for governance

**Pitfall**: Splitting subscriptions per team to apply different rules.

**Reality**: Use **management groups** to inherit policy/RBAC. Subscriptions are billing/quota/network boundaries.

## Storage

### LRS where ZRS is required

**Pitfall**: LRS "for cost savings" on production storage.

**Reality**: LRS keeps three copies in one zone - a zone outage takes data offline. Use **ZRS** for production; LRS only for dev/test or easily recreated data.

### Account key for app access

**Pitfall**: Embedding the storage account key in app config.

**Reality**: Use **managed identity** with a Storage Blob Data Reader/Contributor role. If a token is required, use **user-delegation SAS** (signed with Entra), not an account-key SAS.

### Archive tier for "cheap and quickly retrievable"

**Pitfall**: Archive for cool data needing on-demand reads.

**Reality**: Archive is offline; rehydration takes hours. **Cold** is the cheapest *online* tier.

### Soft delete is not backup

**Pitfall**: Treating blob soft delete as a backup solution.

**Reality**: Soft delete recovers individual deletions for a retention window. **Azure Backup for Blob Storage** is the answer for point-in-time and operational backup with policy.

## Networking

### Service endpoint when private endpoint is required

**Pitfall**: Using service endpoints to "lock down" a storage account.

**Reality**: Service endpoints route subnet traffic to the *public endpoint* over backbone - public access remains. **Private endpoints** assign a private IP in your VNet; with public access disabled, the resource has no public surface.

### App Gateway for non-HTTP

**Pitfall**: App Gateway for SQL or SSH balancing.

**Reality**: App Gateway is HTTP/HTTPS only. Use **Standard Load Balancer** for L4.

### Two NSGs blocking each other

**Pitfall**: NSG at NIC + NSG at subnet with conflicting rules.

**Reality**: Both must allow traffic. Inbound: subnet NSG first, then NIC NSG. Outbound: NIC first, then subnet. Easier to apply NSGs at one level.

### Forgetting the /27 minimum on certain gateways

**Pitfall**: Sizing a VPN gateway subnet `/29`.

**Reality**: **GatewaySubnet** must be at least `/29` (recommended `/27`). **AzureFirewallSubnet** is `/26`. **AzureBastionSubnet** is `/26` (or `/27` for older deployments).

### Custom UDR breaks Azure-managed traffic

**Pitfall**: Adding `0.0.0.0/0 -> next hop NVA` then losing access to PaaS.

**Reality**: Many Azure services rely on system routes. Combine UDRs with **service tags** and explicit routes to AzureCloud, AzureMonitor, etc., or use Azure Firewall with proper allow rules.

## Compute

### Availability set in a zone-capable region

**Pitfall**: Choosing availability set when zones are available.

**Reality**: **Availability zones** are stronger (separate datacenters vs separate racks). Prefer zones when the region supports them.

### B-series for steady CPU workloads

**Pitfall**: Selecting B-series for a CPU-bound API.

**Reality**: B-series is **burstable** with credit accumulation; sustained CPU drains credits. Use D/F series for steady CPU.

### App Service Free tier in production

**Pitfall**: Free or Shared plan for tier-1 hosting.

**Reality**: No SLA, no custom domain TLS, no scale, no slots. Use **Standard** or higher in production.

### Slot swap doesn't move app settings flagged "deployment slot"

**Pitfall**: Expecting connection strings to follow the swap.

**Reality**: App settings marked **"deployment slot setting"** stick to their slot. Useful for staging-only secrets; understand the swap semantics for tests.

## Monitoring and Backup

### Diagnostic logs not flowing because diagnostic settings are missing

**Pitfall**: Assuming logs appear automatically.

**Reality**: Each resource needs explicit **diagnostic settings** to send logs to Log Analytics, Storage, or Event Hubs. Use Azure Policy `deployIfNotExists` to enforce.

### Application Insights without Log Analytics workspace

**Pitfall**: Creating classic App Insights resources.

**Reality**: Classic mode is retired. Use **workspace-based App Insights** linked to a Log Analytics workspace.

### Backup vault default RA-GRS without compliance need

**Pitfall**: Leaving default geo-redundancy when a single-region requirement applies.

**Reality**: Vault redundancy can be set at creation only (LRS/GRS/ZRS for backup). Choose deliberately based on RPO/RTO and data residency.

### Treating Azure Backup as DR

**Pitfall**: Relying on Backup for region failure recovery.

**Reality**: Azure Backup is point-in-time recovery; restores can be slow. Pair with **Azure Site Recovery** for low-RTO regional DR of VM workloads.
