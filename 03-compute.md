# 3. Compute (20-25%)

> Virtual Machines, Scale Sets, App Service, Containers, and ARM/Bicep.

---

## Domain mind map

```mermaid
mindmap
  root((Compute))
    Virtual Machines
      Sizing families
        General B Dsv5
        Compute F
        Memory E M
        Storage L
        GPU N
      Disks
        OS disk
        Data disks
        Disk types
          Standard HDD
          Standard SSD
          Premium SSD
          Premium SSD v2
          Ultra Disk
      Availability
        Availability Set (FD/UD)
        Availability Zones
        VM SLA
      Backup
        Snapshot
        Recovery Services Vault
      Extensions
        Custom Script
        DSC
        Diagnostics
    VM Scale Sets
      Uniform vs Flexible
      Autoscale rules
      Upgrade policy
        Manual
        Automatic
        Rolling
    App Service
      Plan tiers
        Free / Shared / Basic
        Standard / Premium v3
        Isolated v2
      Slots
      Scale up vs out
      Auth/Authz (Easy Auth)
      Custom domains + TLS
    Containers
      Container Instances (ACI)
      Container Apps
      AKS basics
      Container Registry (ACR)
    ARM and Bicep
      Templates
      Parameters / variables
      Outputs
      Deployment scopes
        sub / RG / mg / tenant
      What-if
```

---

## VM size decision tree

```mermaid
flowchart TD
    A[What workload?] --> Web[Web / dev / burst]
    A --> CPU[CPU-bound batch]
    A --> Mem[In-memory DB / SAP]
    A --> Disk[Big local disk IO]
    A --> GPU[ML / rendering]
    Web --> Bs[B-series<br/>burstable]
    Web --> Dsv5[Dsv5/Ddsv5<br/>balanced]
    CPU --> Fsv2[Fsv2/Fsv3]
    Mem --> Esv5[Esv5 / Edsv5]
    Mem --> M[M / Mv2 series]
    Disk --> Lsv3[Lsv3]
    GPU --> NC[NC / ND / NV]
```

| Family | Letter clue | Use case |
|---|---|---|
| General | A, B, D | Mixed workloads, web, dev/test |
| Compute | F | High CPU per RAM (batch, gaming servers) |
| Memory | E, M | Databases, in-memory analytics, SAP |
| Storage | L | Big Data, NoSQL, low-latency local SSD |
| GPU | N | Training, inference, visualization |

---

## Disk types

```mermaid
flowchart LR
    Q[Need IOPS / latency?] --> Std[Standard HDD<br/>backup, cold]
    Q --> SSD[Standard SSD<br/>web, low IO]
    Q --> Prem[Premium SSD<br/>prod databases, P-tier]
    Q --> P2[Premium SSD v2<br/>granular IOPS, no caching]
    Q --> Ultra[Ultra Disk<br/>microsecond latency, top tier DBs]
```

Memorize: **Premium SSD** is required to get the **single-instance VM SLA**. **Ultra Disk** + **Premium SSD v2** can independently dial IOPS and throughput.

---

## VM availability options

```mermaid
flowchart TD
    Goal[Avoid downtime] --> One{Single VM?}
    One -- Yes + premium disks --> SLA1[99.9% single-VM SLA]
    One -- No multiple VMs --> Set{Same data center OK?}
    Set -- Yes --> AS[Availability Set<br/>fault + update domains<br/>99.95%]
    Set -- No DC failure must survive --> AZ[Availability Zones<br/>distributed across AZs<br/>99.99%]
    AZ --> Region{Region failure must survive?}
    Region -- Yes --> ASR[Azure Site Recovery<br/>cross-region replication]
```

| Construct | Protects against | SLA |
|---|---|---|
| Single VM (Premium) | Hardware failure of disks | 99.9% |
| Availability Set (FD + UD) | Rack / host updates | 99.95% |
| Availability Zone | Datacenter outage | 99.99% |
| Region pair / ASR | Whole-region outage | n/a - depends on RPO/RTO |

You **cannot** add an existing VM to an Availability Set after creation. You must redeploy.

---

## VM Scale Set quick reference

```mermaid
flowchart LR
    VMSS[VM Scale Set] --> Uni[Uniform<br/>identical VMs<br/>up to 1,000]
    VMSS --> Flex[Flexible<br/>mixed VMs<br/>spans AZs<br/>recommended for new deployments]
    Auto[Autoscale rules] --> Metric[Based on metrics<br/>CPU / queue length]
    Auto --> Sched[Based on schedule]
    Up[Upgrade policy] --> Man[Manual]
    Up --> AutoU[Automatic]
    Up --> Roll[Rolling<br/>batch by batch]
```

- Flexible orchestration is the **default** for new scale sets and is preferred unless you specifically need Uniform features.
- Autoscale: define **scale-out** rule (e.g. avg CPU > 70%) and a **scale-in** rule (e.g. avg CPU < 30%). Always include both.
- Cooldown prevents flapping. Default 5 minutes.

---

## App Service plan tiers

| Tier | Use case | Custom domain + TLS | Scale out | Slots | VNet integration |
|---|---|---|---|---|---|
| Free | Learning | Default `azurewebsites.net` only | No | No | No |
| Shared | Light dev | Custom domain only | No | No | No |
| Basic | Test | y | Manual up to 3 | No | No |
| Standard | Prod small | y | Auto up to 10 | 5 slots | y |
| Premium v3 | Prod | y | Auto up to 30 | 20 slots | y |
| Isolated v2 (ASE) | Regulated | y | Up to 100 | 20 slots | Dedicated VNet |

---

## App Service deployment slots

```mermaid
flowchart LR
    Dev[Code commit] --> Stage[Staging slot<br/>app-myapp/staging]
    Stage --> Smoke[Smoke test]
    Smoke --> Swap[Slot swap]
    Swap --> Prod[Production slot<br/>app-myapp]
    Swap -.warm-up.-> Prod
```

- Slot swap performs **warm-up** of the staging slot first, then swaps DNS/traffic.
- **Slot settings** (sticky settings) stay with the slot during a swap (e.g. connection strings).
- Auto swap can swap automatically once warm-up succeeds.

---

## Containers in AZ-104 scope

```mermaid
flowchart TD
    Need[I want to run a container] --> Q1{One-off / batch?}
    Q1 -- Yes --> ACI[Azure Container Instances<br/>seconds to start, per-second billing]
    Q1 -- No long-running web/api --> Q2{Need full Kubernetes?}
    Q2 -- No --> CA[Container Apps<br/>serverless, KEDA scale, ingress]
    Q2 -- Yes --> AKS[Azure Kubernetes Service<br/>full control, node pools]
    ACR[Azure Container Registry] --> ACI
    ACR --> CA
    ACR --> AKS
```

ACR tiers:

- **Basic / Standard** differ mostly by storage and throughput.
- **Premium** adds geo-replication, content trust, private link, customer-managed keys, retention policy.

---

## ARM templates and Bicep

```mermaid
flowchart LR
    Bicep[Bicep file .bicep] -->|build| ARM[ARM template JSON]
    ARM --> Deploy[az deployment group create]
    Deploy --> Scope{Scope}
    Scope --> RG[--resource-group]
    Scope --> Sub[az deployment sub create]
    Scope --> MG[az deployment mg create]
    Deploy --> WhatIf[--what-if<br/>preview changes]
```

Key concepts:

- Templates are **idempotent** - running the same template twice converges to the same state.
- `dependsOn` is implicit when you reference another resource by symbolic name in Bicep.
- `outputs` expose values for nested or pipeline consumption.
- Use `what-if` before `deploy` to preview create / modify / delete actions.

Sample Bicep snippet:

```bicep
param location string = resourceGroup().location
param vmName string

resource nic 'Microsoft.Network/networkInterfaces@2023-09-01' = {
  name: '${vmName}-nic'
  location: location
  properties: { /* ... */ }
}

resource vm 'Microsoft.Compute/virtualMachines@2024-03-01' = {
  name: vmName
  location: location
  properties: {
    hardwareProfile: { vmSize: 'Standard_D2s_v5' }
    networkProfile: { networkInterfaces: [ { id: nic.id } ] }
    /* osProfile, storageProfile ... */
  }
}
```

---

## Compute exam clue patterns

| Phrase | Likely answer |
|---|---|
| "auto scale based on CPU" | VMSS autoscale rule (or App Service autoscale) |
| "no downtime during host maintenance" | Availability Set |
| "survive datacenter failure" | Availability Zones |
| "test in production safely" | App Service deployment slot |
| "burst CPU, idle most of the time" | B-series VM |
| "preview infra changes before deploy" | `az deployment ... --what-if` |
| "lightweight, run a container 10 minutes" | Azure Container Instances |
| "serverless container web app" | Azure Container Apps |
| "minimum admin overhead for K8s" | AKS (managed control plane) |

---

**Next:** [04-virtual-networking.md](04-virtual-networking.md)
