# Flashcards: Active Recall

> Click any card to reveal the answer. Use the **Domain pager bottom-right** to switch between exam areas. ~50 cards across 6 domains.

<section class="fc-section" data-fc-title="Identity & Governance">
<h2>1 - Identity & Governance</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Where does an RBAC role assignment take effect?</div><div class="fc-a">At the assigned <strong>scope and below</strong> (management group -> subscription -> resource group -> resource). Inherits downward.</div></div>

<div class="flashcard"><div class="fc-q">Owner vs Contributor vs User Access Administrator?</div><div class="fc-a"><strong>Owner</strong> = Contributor + manage access. <strong>Contributor</strong> = full management except role assignment. <strong>UAA</strong> = manage access only.</div></div>

<div class="flashcard"><div class="fc-q">Difference between a security group and a Microsoft 365 group?</div><div class="fc-a"><strong>Security</strong> = grant resource access / RBAC. <strong>M365</strong> = collaboration (mailbox, Teams, SharePoint).</div></div>

<div class="flashcard"><div class="fc-q">What does an Azure Policy <code>deny</code> effect do?</div><div class="fc-a">Blocks creation/update of non-compliant resources. <code>audit</code> only logs; <code>deployIfNotExists</code> remediates.</div></div>

<div class="flashcard"><div class="fc-q">What is a management group used for?</div><div class="fc-a">Group multiple subscriptions to apply governance (RBAC, Policy, Blueprints) at scale. Up to 6 levels deep.</div></div>

<div class="flashcard"><div class="fc-q">When do you use a resource lock?</div><div class="fc-a"><code>CanNotDelete</code> prevents deletion; <code>ReadOnly</code> prevents modification. Survives RBAC - even Owners blocked.</div></div>

<div class="flashcard"><div class="fc-q">Conditional Access vs MFA?</div><div class="fc-a">CA is the policy engine (signals -> grant/block/MFA). MFA is one possible <em>control</em> CA can require.</div></div>

<div class="flashcard"><div class="fc-q">Tags vs Resource Groups for cost tracking?</div><div class="fc-a">Tags work <strong>across</strong> RGs/subs and don't inherit by default. RGs group lifecycle. Use both: RG for lifecycle, tags for chargeback.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Storage">
<h2>2 - Storage</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">LRS vs ZRS vs GRS vs GZRS?</div><div class="fc-a"><strong>LRS</strong> = 3 copies, 1 DC. <strong>ZRS</strong> = 3 zones. <strong>GRS</strong> = LRS + async to paired region. <strong>GZRS</strong> = ZRS + async paired region.</div></div>

<div class="flashcard"><div class="fc-q">Hot vs Cool vs Cold vs Archive tier?</div><div class="fc-a">Hot: frequent access, low access cost. Cool: 30+ days. Cold: 90+ days. Archive: 180+ days, hours to rehydrate.</div></div>

<div class="flashcard"><div class="fc-q">Block blob vs append blob vs page blob?</div><div class="fc-a">Block: most files. Append: log streaming (append-only). Page: random read/write - backs unmanaged VHDs.</div></div>

<div class="flashcard"><div class="fc-q">SAS token vs Stored Access Policy vs Shared Key?</div><div class="fc-a">SAS = scoped time-limited URL. Stored access policy = revocable SAS template. Shared Key = full account key (avoid).</div></div>

<div class="flashcard"><div class="fc-q">When use Azure Files vs Blob Storage?</div><div class="fc-a">Files = SMB/NFS shared mount (lift-and-shift apps). Blob = object/REST for unstructured data.</div></div>

<div class="flashcard"><div class="fc-q">What does AzCopy do that Storage Explorer can't?</div><div class="fc-a">High-throughput scripted bulk copy with resume; ideal for migrations and pipelines.</div></div>

<div class="flashcard"><div class="fc-q">Soft delete vs versioning on blobs?</div><div class="fc-a">Soft delete = recover deleted blobs within retention. Versioning = automatic version snapshot on every write.</div></div>

<div class="flashcard"><div class="fc-q">What is a private endpoint for storage?</div><div class="fc-a">Maps the storage account to a private IP in your VNet - traffic stays on Microsoft backbone, public access can be disabled.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Compute">
<h2>3 - Compute</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">When choose VMSS Flexible over Uniform?</div><div class="fc-a"><strong>Flexible</strong>: heterogeneous SKUs, FD spread across zones, IaaS workloads needing AS-like control. <strong>Uniform</strong>: identical VMs at scale (1000+).</div></div>

<div class="flashcard"><div class="fc-q">Availability set vs availability zone?</div><div class="fc-a">AS = fault/update domains <em>within one DC</em> (99.95%). AZ = separate datacenters in a region (99.99%).</div></div>

<div class="flashcard"><div class="fc-q">App Service zero-downtime deployment?</div><div class="fc-a"><strong>Deployment slots</strong> - deploy to staging slot, warm up, swap with production.</div></div>

<div class="flashcard"><div class="fc-q">VMSS scale on CPU - which method?</div><div class="fc-a">Autoscale rule on the VMSS using <strong>Percentage CPU</strong> metric (or custom metric via App Insights).</div></div>

<div class="flashcard"><div class="fc-q">Managed disk types and use cases?</div><div class="fc-a">Standard HDD (backup), Standard SSD (web/dev), Premium SSD (prod DBs), Ultra SSD (extreme IOPS/latency).</div></div>

<div class="flashcard"><div class="fc-q">Difference between B-series and D-series VMs?</div><div class="fc-a">B = burstable (CPU credits, cheap baseline). D = general purpose, sustained performance.</div></div>

<div class="flashcard"><div class="fc-q">App Service Plan tier needed for VNet integration + autoscale?</div><div class="fc-a"><strong>Standard or higher</strong>. Free/Shared/Basic don't support VNet integration or autoscale.</div></div>

<div class="flashcard"><div class="fc-q">When do you use ARM template vs Bicep?</div><div class="fc-a">Bicep = preferred authoring language (cleaner). ARM JSON = the underlying engine. Bicep transpiles to ARM.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Networking">
<h2>4 - Networking</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Smallest valid VNet subnet size?</div><div class="fc-a"><strong>/29</strong> (8 addresses, 3 usable - Azure reserves 5). Common minimum is /28.</div></div>

<div class="flashcard"><div class="fc-q">NSG rule evaluation order?</div><div class="fc-a">By <strong>priority (lowest number first)</strong>. First match wins. Default rules are last.</div></div>

<div class="flashcard"><div class="fc-q">NSG vs Azure Firewall?</div><div class="fc-a">NSG = stateful L3/L4 ACL on subnet/NIC, free. Firewall = managed L3-L7 stateful firewall with FQDN/threat intel, paid.</div></div>

<div class="flashcard"><div class="fc-q">When use VPN gateway vs ExpressRoute?</div><div class="fc-a">VPN = encrypted over internet, lower cost, <=1.25 Gbps. ExpressRoute = private circuit, high bandwidth/SLA.</div></div>

<div class="flashcard"><div class="fc-q">Network Watcher tool to diagnose RDP unreachable?</div><div class="fc-a"><strong>Connection Troubleshoot</strong> (or <strong>IP Flow Verify</strong> to find the blocking NSG rule).</div></div>

<div class="flashcard"><div class="fc-q">Application Gateway vs Load Balancer?</div><div class="fc-a">App Gateway = L7 (URL routing, WAF, SSL term). Load Balancer = L4 (TCP/UDP, ultra-low latency).</div></div>

<div class="flashcard"><div class="fc-q">Service endpoint vs private endpoint?</div><div class="fc-a">Service endpoint = VNet identity over Microsoft backbone (still public IP). Private endpoint = private NIC IP in your VNet (preferred).</div></div>

<div class="flashcard"><div class="fc-q">VNet peering vs VNet-to-VNet VPN?</div><div class="fc-a">Peering = backbone, low-latency, no encryption overhead. VPN = IPsec tunnel, slower, useful across regions/clouds w/ encryption.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Monitoring & Backup">
<h2>5 - Monitoring & Backup</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">Where to store diagnostic logs cheaply for 1 year?</div><div class="fc-a"><strong>Storage account</strong> (cool/cold tier). Log Analytics is for query; Event Hub is streaming.</div></div>

<div class="flashcard"><div class="fc-q">Restore a backup deleted within 14 days - what feature?</div><div class="fc-a"><strong>Soft delete</strong> on the Recovery Services vault (default 14 days, deleted backups retained free).</div></div>

<div class="flashcard"><div class="fc-q">Azure Monitor data sources?</div><div class="fc-a">Metrics (numeric, near real-time), Logs (Log Analytics, KQL), Activity Log (control plane), Diagnostics (resource).</div></div>

<div class="flashcard"><div class="fc-q">What does Azure Backup back up?</div><div class="fc-a">VMs, Azure Files, SQL/SAP HANA in VM, on-prem via MARS agent / DPM / MABS. Stored in Recovery Services vault.</div></div>

<div class="flashcard"><div class="fc-q">Action group vs alert rule?</div><div class="fc-a">Alert rule defines the condition; <strong>action group</strong> defines who/how to notify (email, SMS, webhook, runbook).</div></div>

<div class="flashcard"><div class="fc-q">Log Analytics workspace retention vs archive?</div><div class="fc-a">Interactive 30-730 days. Archive tier extends to 12 years at lower cost; query requires restore/search jobs.</div></div>

<div class="flashcard"><div class="fc-q">Azure Site Recovery (ASR) vs Azure Backup?</div><div class="fc-a">ASR = replication for DR (RPO seconds, failover). Backup = point-in-time restore for data loss/corruption.</div></div>

<div class="flashcard"><div class="fc-q">What is Activity Log retention by default?</div><div class="fc-a"><strong>90 days</strong>, free. Export to Log Analytics / Storage / Event Hub for longer retention.</div></div>

</div>
</section>

<section class="fc-section" data-fc-title="Automation & Operations">
<h2>6 - Automation & Operations</h2>

<div class="flashcard-grid">

<div class="flashcard"><div class="fc-q">ARM/Bicep <code>incremental</code> vs <code>complete</code> mode?</div><div class="fc-a">Incremental (default) adds/updates. Complete <strong>deletes</strong> resources in the RG not in the template.</div></div>

<div class="flashcard"><div class="fc-q">When use Run Command vs Custom Script Extension?</div><div class="fc-a">Run Command = ad-hoc one-off via portal/CLI. Custom Script Extension = declarative, repeatable provisioning script.</div></div>

<div class="flashcard"><div class="fc-q">Update Manager vs old Update Management?</div><div class="fc-a">Update Manager = native Azure service (no Log Analytics agent). Replaces deprecated Automation-based Update Management.</div></div>

<div class="flashcard"><div class="fc-q">System-assigned vs user-assigned managed identity?</div><div class="fc-a">System = lifecycle tied to resource, 1:1. User-assigned = standalone, share across many resources.</div></div>

<div class="flashcard"><div class="fc-q">Move resource between subscriptions - what to check?</div><div class="fc-a">Source/target same tenant, resource type supports move, no locks, validate via <code>az resource move --validate</code> first.</div></div>

<div class="flashcard"><div class="fc-q">Azure Automation runbook types?</div><div class="fc-a">PowerShell, PowerShell Workflow, Python, Graphical. Hybrid Runbook Worker for on-prem execution.</div></div>

<div class="flashcard"><div class="fc-q">Quickly find non-compliant policies across subscription?</div><div class="fc-a">Azure Policy -> <strong>Compliance</strong> blade. Filter by initiative/scope. Drill into resource for reason.</div></div>

<div class="flashcard"><div class="fc-q">CLI to list all VMs across subscription with size?</div><div class="fc-a"><code>az vm list --query "[].{name:name, size:hardwareProfile.vmSize}" -o table</code></div></div>

</div>
</section>