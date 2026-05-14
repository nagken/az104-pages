# AZ-104 Visual Study Guide - Master Index

> **Microsoft Azure Administrator**
> Visual concept layer aligned to the Microsoft Learn AZ-104 [skills measured](https://learn.microsoft.com/credentials/certifications/resources/study-guides/az-104). Diagrams, decision trees, and original summaries only - no exam questions reproduced.

---

## How to use this guide

```mermaid
flowchart LR
    A[Start Here<br/>Master Index] --> B[Read Mind Map<br/>below]
    B --> C[Pick a Domain<br/>1->5]
    C --> D[Study Diagrams]
    D --> E[Memorize<br/>Decision Trees]
    E --> F[Decision Reference<br/>final review]
    F --> G[Exam Ready]
```

---

## The 5 Exam Domains - Mind Map

```mermaid
mindmap
  root((AZ-104))
    Identity and Governance
      Microsoft Entra ID
        Users and Groups
        Self-Service Password Reset
        Tenant Properties
        Bulk Operations
        Guest Users B2B
      RBAC
        Built-in roles
        Custom roles
        Scope hierarchy
      Subscriptions and Mgmt Groups
        Cost Mgmt and Budgets
        Tags
        Resource Locks
      Azure Policy
        Initiatives
        Effects DeployIfNotExists
    Storage
      Storage Accounts
        Performance Standard Premium
        Replication LRS ZRS GRS
        Access tiers Hot Cool Archive
        Firewalls and endpoints
      Blob
        Containers
        Lifecycle Mgmt
        Soft delete versioning
        Object Replication
      Azure Files
        SMB NFS
        File Sync
        Snapshots
      Tools
        AzCopy
        Storage Explorer
        Import Export
    Compute
      Virtual Machines
        Sizing and family
        Disks and types
        Availability Sets and Zones
        Backup and Snapshots
        VM extensions
      VM Scale Sets
        Autoscale rules
        Upgrade policy
      App Service
        Plans tiers
        Slots
        Scale up vs out
      Containers
        ACI Container Instances
        Container Apps
        AKS basics
      ARM and Bicep
        Templates
        Parameters
    Virtual Networking
      VNets and Subnets
        Address spaces
        Service endpoints
        Subnet delegation
      NSGs and ASGs
        Rules priority
        Effective rules
      Connectivity
        VNet Peering
        VPN Gateway
        ExpressRoute
        Virtual WAN
      Name Resolution
        Azure DNS
        Private DNS Zones
      Load Balancing
        Load Balancer SKU
        Application Gateway WAF
        Traffic Manager
        Front Door
      Network Security
        Azure Firewall
        Bastion
        Route Tables UDR
    Monitor and Maintain
      Azure Monitor
        Metrics
        Logs
        Alerts and Action Groups
        Workbooks
      Log Analytics
        Workspaces
        KQL basics
      Backup
        Recovery Services Vault
        Backup policies
        VM and File Share backup
      Site Recovery
        VM replication
        Failover and failback
      Network Watcher
        Connection Monitor
        IP flow verify
        NSG flow logs
```

---

## Official Skills Weighting

```mermaid
pie title AZ-104 Skills Measured Midpoints
  "Identity and Governance" : 22.5
  "Storage" : 17.5
  "Compute" : 22.5
  "Virtual Networking" : 17.5
  "Monitor and Maintain" : 12.5
```

| Slice | Weight | Jump to chapter |
| --- | --- | --- |
| Identity and Governance | **20-25%** | [01 Identity & Governance](01-identity-governance.md) |
| Storage | **15-20%** | [02 Storage](02-storage.md) |
| Compute | **20-25%** | [03 Compute](03-compute.md) |
| Virtual Networking | **15-20%** | [04 Virtual Networking](04-virtual-networking.md) |
| Monitor and Maintain | **10-15%** | [05 Monitor & Maintain](05-monitor-maintain.md) |

Coverage note: this guide follows the official Microsoft Learn AZ-104 [skills measured](https://learn.microsoft.com/credentials/certifications/resources/study-guides/az-104) weights - Identity & Governance **20-25%**, Storage **15-20%**, Compute **20-25%**, Virtual Networking **15-20%**, Monitor & Maintain **10-15%**.

---

## Service emphasis map

```mermaid
flowchart LR
    CASES[Common AZ-104 task types] --> ID[Manage users, groups, RBAC]
    CASES --> STORE[Storage accounts and blob features]
    CASES --> VM[Provision and resize VMs]
    CASES --> NET[VNets, peering, NSGs]
    CASES --> MON[Alerts, backup, ASR]

    ID --> IDA[Entra ID, custom roles, Azure Policy]
    STORE --> STOREA[LRS/ZRS/GRS, lifecycle, AzCopy, File Sync]
    VM --> VMA[VM sizes, disks, scale sets, availability]
    NET --> NETA[Peering, VPN, App Gateway, Front Door]
    MON --> MONA[Log Analytics, alerts, Recovery Services]
```

Core services to know across the five measured skills: Microsoft Entra ID, Azure RBAC, Azure Policy, Storage Accounts (Blob, Files, Queues, Tables), Azure Virtual Machines, VM Scale Sets, App Service, Container Apps, AKS basics, Virtual Networks, NSGs, Application Gateway, Load Balancer, VPN Gateway, ExpressRoute, Azure Firewall, Bastion, Azure Monitor, Log Analytics, Application Insights, Azure Backup, Azure Site Recovery, Network Watcher.

---

## Domain files in this guide

| # | Domain | File | Focus |
|---|--------|------|-------|
| 1 | Identity & Governance | [01-identity-governance.md](01-identity-governance.md) | Entra ID, RBAC, Subscriptions, Policy |
| 2 | Storage | [02-storage.md](02-storage.md) | Storage accounts, Blob, Files, AzCopy |
| 3 | Compute | [03-compute.md](03-compute.md) | VMs, VMSS, App Service, Containers, ARM |
| 4 | Virtual Networking | [04-virtual-networking.md](04-virtual-networking.md) | VNets, NSGs, Peering, VPN, LB, App GW |
| 5 | Monitor & Maintain | [05-monitor-maintain.md](05-monitor-maintain.md) | Monitor, Log Analytics, Backup, ASR |
| | **Exam Decision Reference** | [06-exam-cheatsheet.md](06-exam-cheatsheet.md) | Decision trees + scenario keyword map |
| + | **Extra Concepts** | [07-extra-az104-concepts.md](07-extra-az104-concepts.md) | Edge-case concepts, gotchas, CLI patterns |
| + | **Architectures - AZ-104** | [10-arch-az104.md](10-arch-az104.md) | Reference architectures mapped to each skill area |

---

## The 7 Core Question Patterns in AZ-104

```mermaid
flowchart TD
    Q[Any AZ-104 Question] --> P1
    Q --> P2
    Q --> P3
    Q --> P4
    Q --> P5
    Q --> P6
    Q --> P7

    P1["1 Which RBAC role gives least privilege?"]
    P2["2 Which replication / tier meets durability + cost?"]
    P3["3 Which VM size / disk / availability option?"]
    P4["4 How do I connect VNets / on-prem?"]
    P5["5 Which alert / metric / log answers this?"]
    P6["6 Which backup / ASR option meets RPO/RTO?"]
    P7["7 Which CLI / PowerShell / portal step first?"]

    P1 --> R1[Built-in over custom; assign at smallest scope]
    P2 --> R2[GRS for region failure; Cool/Archive for rarely accessed]
    P3 --> R3[VMSS for elasticity; AZ for HA; Premium SSD for IOPS]
    P4 --> R4[Peering inside region; VPN cheap; ExpressRoute SLA]
    P5 --> R5[Metric alert vs log alert; Action Group for notify]
    P6 --> R6[Backup = days/weeks RPO; ASR = minutes RPO]
    P7 --> R7[Read order: lock first, then RBAC, then policy effect]
```

---

## The "Magic Words" Translator

```mermaid
flowchart LR
    subgraph Triggers
      T1["'least privilege'"]
      T2["'cheapest, rarely accessed'"]
      T3["'survive region failure'"]
      T4["'connect on-prem to Azure'"]
      T5["'no public internet'"]
      T6["'auto scale'"]
      T7["'guest user from partner'"]
      T8["'prevent accidental delete'"]
      T9["'force HTTPS / WAF'"]
      T10["'flow logs / packet capture'"]
    end
    subgraph Answers
      A1["Built-in RBAC role at smallest scope"]
      A2["Cool or Archive blob tier"]
      A3["GRS / RA-GRS storage; ASR for VMs"]
      A4["S2S VPN (cheap) or ExpressRoute (SLA)"]
      A5["Private Endpoint + service endpoint policies"]
      A6["VM Scale Set or App Service autoscale"]
      A7["Entra ID B2B guest invitation"]
      A8["Resource Lock (CanNotDelete or ReadOnly)"]
      A9["Application Gateway with WAF SKU"]
      A10["Network Watcher (NSG flow logs / Packet Capture)"]
    end
    T1 --> A1
    T2 --> A2
    T3 --> A3
    T4 --> A4
    T5 --> A5
    T6 --> A6
    T7 --> A7
    T8 --> A8
    T9 --> A9
    T10 --> A10
```

---

## Recommended study order

```mermaid
gantt
    title Suggested 8-day plan
    dateFormat X
    axisFormat Day %d
    section Core
    Identity & Governance :a1, 0, 1d
    Storage :a2, after a1, 1d
    Compute :a3, after a2, 2d
    section Network & Ops
    Virtual Networking :b1, after a3, 1d
    Monitor & Maintain :b2, after b1, 1d
    section Final
    Decision reference + practice :c1, after b2, 2d
```

**Next:** open [01-identity-governance.md](01-identity-governance.md)
